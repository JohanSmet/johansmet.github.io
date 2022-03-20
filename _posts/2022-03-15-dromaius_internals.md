---
classes: wide
title: "Dromaius: the simulator core"
excerpt: The internals of Dromaius exposed.
---

This blog post will explain how the internal workings of the Dromaius emulator changed while evolving from a single CPU emulator to a Commodore PET emulator. It's the fourth part in the series about the Dromaius emulator. I'll apologize in advance, it might be a rather long post.

Other parts in this series:
- Part 1: [I accidentally wrote a 6502 emulator.](/2019/12/12/6502_emulator)
- Part 2: [Growing the 6502 emulator.](/2022/03/01/minimal_6502)
- Part 3: [Dromaius: playing with PETs](/2022/03/08/minipet)

## Initial Design
As mentioned in a previous post in the series, the most basic parts of the emulator are chips and signals. Chips emulate the workings of the individual components of the schematic diagram. Signals are the connections that carry information between chips. In the first revision of Dromaius signals can be multiple bits wide, an attempt to optimize reading and writing eight or sixteen bit busses. 

Signals are managed by a *signal pool*. The signal pool creates and manages the signals, and has functions to fetch and change the value of a signal. The signal pool is double buffered. One buffer stores the current state of the signals and is read-only. The other buffer is write-only and receives the changes made by chips during the current simulation step. When the signal pool is *cycled*, the new state is compared with the original state and changed signals are marked. Afterwards, the old state is replaced with the new state. With the double-buffering, changes are atomic and chips always read a consistent value even though they are processed sequentially.

The *simulator* component owns the signal pool and all chips. When chips are created, they inform the simulator on which signals they depend. When that signal changes value, the chips gets marked as *dirty* and will be processed in the next simulation step. Normally, only dirty chips are processed, but chips can also schedule a forced execution with the simulator. This allows chips to function without depending on external input, e.g. an oscillator changes its output on its own.

This model works great for sequential, or clocked, logic. But the PET also contains lots of combinational logic chips, which produce results after a small delay when their inputs change. This means that the results of a dirty combinational chip should already be available before the next clock edge when the simulation is run. For this to work, the combinational chips are processed in between simulation step, using the write-only signal pool as both input and output.

## Order! Order!
This model started breaking down quickly, even when only a limited subset of the PET was implemented. Combinational logic can depend on others, making the order in which they are processed is important. It gets even worse when these dependencies are cyclical. This made the emulator very easy to break by adding or moving a chip. Something had to be done.

I decided to drastically reduce the length of a time step in the simulator. Instead of jumping from one clock edge to the next, the emulator now took hundreds of small steps in between, allowing the combinational logic chips to be simulated just like the clocked logic chips. No special handling was required anymore. As an optimization, the simulator skips ahead to the next scheduled wake-up event when the combinational chain settles and there are no more dirty chips.

## Breaking up is hard to do
Signals containing multiple sequential bits seemed like an obvious optimization when implementing the 8-bit data bus or the 16-bit address bus. This started to become problematic as more and more 74-family logic chips were implemented to replace the high-level memory addressing logic routine (that determines which chip should be enabled for a particular value of the address bus). Chips depend on a particular bit of a byte-signal or write to specific non-sequential bits of a multi-bit bus.

The drawbacks started to outweigh the benefits of this optimization. Signals were changed to be booleans and only represent one bit. A *SignalGroup* structure was introduced to facilitate reading or writing to a multiple signals/bits at once. The performance impact did not turn out to be as bad as expected.

## Let's do multiple things at once.
By this point, almost the entire PET schematic diagram was emulated, except the IEEE-488 interface. Performance was a real problem. For the fully emulated PET, most effort was being spent on the 16 MHz circuitry for the display and the DRAM refresh. It ran at about a tenth of the real speed. A 'light' PET emulator, which handles the display and RAM at a higher emulation level, ran at about half the speed of the real PET.

While Dromaius uses a separate thread for the presentation layer, the *simulator* itself uses only a single thread. Going multithreaded would be the logical next step, right? 

In the first, naive, approach the simulator processed the chips using a few worker threads and access to the signals was coordinated using locking. As expected, that didn't work very well; more time was spent in the synchronization code than was saved by processing the chips in parallel.

A second approach queued the updates to signals in thread-specific changelogs. At the end of the time step, the simulator merged these changes into the state for the next time step. This was better, but still too slow. Merging the data at the end was the bottleneck. Too little work was done in each processing step to offset the cost of coordinating multiple threads.

## Less is more
Profiling showed that, besides a few high-frequency components around the 16Mhz clock, most of the time was being spent by the simulator checking for changed signals, marking dirty chips, and just copying data around preparing for the next simulation step. After a brief but unsuccessful detour through SSE-land, another approach was devised. This led to the design that is in use today and works as follows:

- Signals are no longer separate booleans (a byte each) but a just a bit in a 64-bit integer array.
- The smaller footprint allows us to define several *layers*, so each chip can write to a separate layer without interfering with other chips.
- This also means less data to move around by the simulator, with some bit-shuffling 64 signals can be handled at once
- The bitwise operations when reading/writing signals mean more work for the chips but by moving the focus from the simulator to multiple chips might yield better results when multi-threading.
- This simplifies handling the situations where multiple chips write to the same signal at once. Previously, all the other chips had to be processed again to determine the new output value.

After a few optimizations, performance was doubled from the previous implementation. The 'light' version now runs at full speed, the full version at about a quarter. (Measured on an 8-year-old Intel quad-core.)

The optimizations included reduces the number of signal layers by being smart with the allocations and avoiding unnecessary writes to signals. Chips don't just blindly write the same value in each time step, but only when the value changes.

## Limitations
The number of supported chips in the simulator is currently limited to 64 due to the fact a 64-bit bit mask is used to track dirty chips. With the smaller glue-logic ICs (think AND/OR gates etc.) already combined into one bigger 'custom' chip, the full PET currently weighs in at... 64 chips.  Several potential solutions exist and this should get implemented in the near future. Hopefully without detrimental effects to performance.

Another, maybe surprising, limitation is the difficulty of emulating jumpers or physical switches that can be changed while the emulator is running. This has to do with the way signals are defined. One signal definition represents all the places it is used in the schematic diagram. Even if it's used on multiple pages, if there's a direct electrical connection it's one signal. Supporting switches means being able to split or merge signals on the fly. This also affects things like loopback connectors to run diagnostics.

## Conclusion.
Ok, I've rambled long enough; I'll wrap it up here. Hope you liked this look inside Dromaius. I don't have a next post planned for this series, but I'll be sure to add one when there's more to talk about. Maybe we'll finally take the next step and add a new computer ...
