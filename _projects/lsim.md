---
title: LSim
classes: wide
excerpt: <b>Personal Open Source Project</b><br/>LSim (Logical SIMulator) is a tool to simulate digital logic circuits.
order: 30
header:
  teaser: /assets/images/project_teasers/lsim_th.png
sidebar:
  - text: Visit the project's repo at
  - text: <i class="fab fa-fw fa-github" aria-hidden="true"></i> [GitHub](https://github.com/JohanSmet/lsim)
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=lsim&type=watch&count=true&v=2" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=lsim&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=lsim&type=fork&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
---
LSim (or Logical SIMulator) is a tool to simulate digital logic circuits. It's a hobby/toy project I started to help me learn about digital logic design.

![LSim]({{ site.url }}{{ site.baseurl }}/assets/images/lsim.gif){: .align-center}

## Background
You might be asking yourself: Why the h#$! did you spend hours and hours building LSim when there are more functional alternatives already available? And that would be a perfectly valid question. To provide a satisfying answer, we have to go to the beginning and tell a (not so) little story.

tl;dr: I wanted to write a script to test an adder circuit, and things spiraled a bit out of control. But I had fun and learned a lot along the way.

I've been a software engineer for more than 20 years, depending on when you start counting. I've always had an interest in how the hardware worked, but mostly on a higher level, not at the electronic component level. I knew about caches, instruction pipelines, branch predictors and how to parallelize a fdiv instruction with integer instructions on a Pentium CPU to optimize your perspective correct texture mapper. Yeah, showing my age here a little.

Like many recently, I got bitten by the retro-bug and started tinkering around with games consoles and computers from my youth. I have a special fondness for the coin-op arcade machines. Circuit boards for these are readily available, but are either very expensive or broken. So you end up staring at a bunch of 74-series logic chips with a logic probe in hand trying to figure out why half of the sprites aren't drawn properly on the screen... Time to learn more about digital logic circuit design and debugging.

I started following online tutorials and courses and building circuits with Logisim (and derivatives). It's an awesome resource, but I found the support for automated testing of circuits rather confusing, at least to me.

So I thought I'll write a small program that can load the Logisim circuit, run the simulation, and take input, that I can control procedurally. So the first version of LSim was born: a C++ library (with unit tests, of course) and Python bindings to script the test fixtures. I didn't support all the features of Logisim, but enough to run the kind of circuits I was building at the time. At the time, another potential goal was to take a schematic (or a net list or something) from KiCad and run a digital simulation of the circuit.

For more complicated circuits, it was hard to debug issues with LSim and see where it was messing up. So I wrote a graphical wrapper to display the circuit and step through the simulation. This was ever only intended to be a visualization, not an editor. 

Unfortunately, the way Logisim stores the circuits makes it hard to load them without a lot of inside information. So I just approximated the position of the components and replaced the wires with splines.  As you can imagine, this became an indecipherable mess rather quickly. So I thought, how hard can it be to turn the visualization into a basic editor? Famous last words ;-)

Most of the time on this project was spent working on lsim_gui . I ended rewriting LSim to better segregate the construction/editing of the circuit and the simulation itself. The simulation engine itself underwent a few major revisions.

Along the way, I not only learned a lot about digital (I now have a basic understanding of how flip-flops and binary counters work) but also about the way digital circuits work electrically to be able to simulate them properly. It might have been a strange and long, winding path to take, but I learned quite a bit and had a lot of fun along the way.

## Current status

As it stands now (in September 2019) LSim is able to simulate basic digital circuits (e.g. flip-flops and counters) and more complex circuits. As an example, LSim includes a basic 8-bit computer (inspired by Ben Eater's work [here](https://eater.net/8bit)). It's probably capable of more, but the UI is a major limiting factor. 

With the Python bindings, it's not hard to write test scripts for the designed circuits. It's also possible to procedurally generated complex repetitive circuits (e.g. ROMs) with a Python-script.

LSim runs on multiple platforms. It compiles natively for modern Windows, Linux and MacOS X machines. A WebAssembly-version can run straight in the browser. You can try a demo over [here](http://justcode.be/lsim). The demo is fully functional except for saving changes.


## Design decisions

#### Why C++ with Python bindings?

From a productivity standpoint, it might have made more sense to write LSim in straight Python. Or some other fancy, state-of-the-hype [sic], hip language. But this being a hobby project changes the decision-making process. I'm one of those people who enjoy programming in C and C++. My previous hobby project was written in C99, so it seemed obvious to me to use C++. 

But I didn't want to write (and compile) the test cases for circuits, so adding Python bindings was an easy decision. Plus, extra points because it was something I hadn't done before.

#### Why use Dear ImGui and not Qt/wxWidgets/'insert cool UI library'?

I've used Dear ImGui before for small test programs and I fell in love with a bit. The immediate mode design makes it easy to thrown together a quick UI without too much fuss. And using SDL2 as the backend ensures it runs on multiple platforms, including a browser with WebAssembly.

The UI wasn't the main focus at the beginning, so I didn't want to add a dependency on a big UI library. I feel like it worked out okay, as long as you don't mind a non-platform native look-and-feel.

#### Single object with dynamic functions in favor of an inheritance hierarchy

LSim has a single class to handle all the digital logic components. The difference in behavior is obtained by setting a function pointer to the appropriate free function at creation time. A more traditional C++ approach would be to have a base class that handles input and output connections and derive a class for each type of logic component with a virtual function to handle the simulation.

And that was what LSim started out with. When you nest circuits or copy-and-paste components, you have to have a way of cloning the entire derived class from a pointer to the base class. That's certainly doable but gets overly complicated, especially for objects where the only difference is that one virtual function.

