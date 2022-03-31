---
title: Dromaius
classes: wide
excerpt: <b>Personal Open Source Project</b><br/>Dromaius, a hobby project emulator of retro hardware.
order: 40
header:
  teaser: assets/images/project_teasers/dromaius_th.png
sidebar:
  - text: Visit the project's repo at
  - text: <i class="fab fa-fw fa-github" aria-hidden="true"></i> [GitHub](https://github.com/JohanSmet/dromaius)
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=dromaius&type=watch&count=true&v=2" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=dromaius&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=dromaius&type=fork&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
---

![Dromaius]({{ site.url }}{{ site.baseurl }}/assets/images/dromaius/demo_01.gif){: .align-center}

## Background
This project was started as an attempt to improve my knowledge of the inner workings of computers. Especially early computers with an architecture that can still be understood by a simple human brain as mine. Dromaius is the genus of [ratite](https://en.wikipedia.org/wiki/Dromaius) present in Australia, of which the one extant species is the Dromaius novaehollandiae or **emu**. Yes, naming things is hard.

## Goals
Emulate late seventies, early eighties computers at the component level. The internal workings of IC's are abstracted, we're not interested at emulating down to the transistor level. The emulated signals at the pins of the IC's should mimic their real-life counterparts, allowing for some simplification (e.g. fixed propagation delays).

## Features
- Emulation of several vintage MOS parts:
	- the 6502 CPU
	- the 6520 Peripheral Interface Adapter
	- the 6522 Versatile Interface Adapter
- Emulation of a selection of the 7400 series logic ICs
- Emulation of various ROM and RAM chips
- a simple computer (a cpu, a rom, and some ram) to be able to run a 6502 binary.
- an implementation of the Commodore PET 2001N 8-bit personal computer
	- includes a 1530/C2N Datassette
	- includes a read-only (for now) 2031 Floppy Drive
- a, basic, cross-platform UI
- a browser-based (JS and Wasm) schematic visualisation of the Commodore PET
- written in C11/C++ (and a smidgen of Python and JavaScript)

## Platform support
Dromaius is primarely developed on Linux, but should run fine on modern Windows. It compiles for Mac OS X, but hasn't been tested due to lack of hardware.

You can play around with a [demo](https://justcode.be/dromaius/htmlview/) of the current version of the schematic viewer. The full GUI-version of dromaius also runs in a browser, and you can try it out [here](https://justcode.be/dromaius/gui/). These should work on any recent browser, tested regularly with Firefox and Chromium.

![HtmlView]({{ site.url }}{{ site.baseurl }}/assets/images/dromaius/htmlview_animated.gif){: .align-center}

## Potentially Asked Questions

#### Yet another emulator?
Yes, but the intention isn't to compete with existing emulators. If you're looking to run some Commodore PET software from back in the day, you're better of with a more established emulator, like [VICE](https://vice-emu.sourceforge.io/).

I started working on Dromaius as a way to learn how computers work, and to hopefully provide a tool that's useful for others. With Dromaius you can see how the signals propagate through the computer without expensive equipment.

Besides, if you love looking at blinkenlights, you're going to love Dromaius.

#### Why emulating all those details? Isn't that slow?
Yes, but that's beside the point. Running at this level of detail does 'waste' a lot of energy on things that aren't visible to the end-user. But a higher level emulator isn't going to provide you with a nice visual of the flip-flops in the display section of the PET working together to generate the horizontal and vertical sync signals from the 16 MHz clock signal.


