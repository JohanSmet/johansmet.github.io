---
title: ShaderSim
classes: wide
excerpt: An instruction-level simulator for GPU shaders, written in C99.
order: 20
sidebar:
  - text: Visit the project's repo at
  - text: <i class="fab fa-fw fa-github" aria-hidden="true"></i> [GitHub](https://github.com/JohanSmet/shader_sim)
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=shader_sim&type=watch&count=true&v=2" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=shader_sim&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
  - text: <iframe src="https://ghbtns.com/github-btn.html?user=JohanSmet&repo=shader_sim&type=fork&count=true" frameborder="0" scrolling="0" width="170px" height="20px" style="margin:0.5em 0"></iframe>
---
ShaderSim is an instruction-level simulator for GPU 'shaders'. The primary intention is to help debug/validate a shader and to visualize and help understand the execution of the shader. More attention has been given to the clarity and maintainability of the code, run-time performance comes secondary in this respect.

At the moment, only (a subset of) SPIR-V has been implemented. Support for other, compiled, shader formats (HLSL, Metal) might be added in the future.

## Background
A few months ago, I was playing around with [`ARB_gl_spirv`](https://www.khronos.org/registry/OpenGL/extensions/ARB/ARB_gl_spirv.txt) and SPIR-V shaders as a prototype for an unrelated project. I found it somewhat difficult to understand what exactly was happening when a shader was executing. Looking around for a debugger that supported SPIR-V didn't yield any usable results.

I was watching [bitwise](https://github.com/pervognsen/bitwise) building computer systems from scratch, implementing his own programming language and RISC-V back-end. That was probably a big part of why I thought writing an SPIR-V simulator would be a fun way to spend some free time.

The goals I set for this project were:
- implement an instruction-level simulator; I'm not looking to build a full-fledged GPU simulator
- support SPIR-V at first but possibly extend to other shading languages (notably HLSL)
- the core of the project should be easily integrated with other projects, so:
	- portable
	- self-contained, minimal external dependencies
	- no esoteric programming language

So I dusted off my trusty C99 compiler (not really) and started coding.

## Current status
At the moment, the simulator successfully runs shaders that use a subset of SPIR-V. Notably, image types and atomic operations aren't supported yet.

I started with a command line interface program to feed the simulator. It supports a unit test like operation, you specify input data and the expected output in a config file and the CLI runs the shader and compares the state of the output variables with the expected values.

But that does not really help with understanding or debugging a shader. I wanted to build something that behaves like a debugger, but I didn't really feel like messing with curses or a cross-platform UI library (hello, dependency hell). I was starting to implement a web service (with [civetweb](https://github.com/civetweb/civetweb) to use with an in-browser front-end. But I didn't want the hassle (and potential legal ramifications) of hosting the web service for a public demo. That's when it struck me, why not use [Emscripten](https://github.com/kripken/emscripten) to run the entire thing in the browser on the client side.

And after some messing around with Emscripten (and dredging up my JavaScript skills), you can now try the ShaderSim UI [here](https://justcode.be/shader_sim).

## Design decisions

#### Instruction-level simulator written in plain C
I chose to build an instruction-level simulator because that seemed the fastest way to achieve the goals for the project. ShaderSim is simulating the perceived effect of an instruction. It's not simulating a GPU at the hardware level.

Speed of execution is also a secondary concern, ease of implementation and maintenance is primary. An interpreter written in plain C checks those boxes best. I'm aware there are more performant ways of implementing the simulator, but these are not in the scope of the project ATM.

#### WebAssembly + JavaScript UI
I wanted a debugger-like UI that could easily be used be third-parties to check out the project. Compiling the ShaderSim library to WebAssembly with Emscripten allows me to keep all dependencies on the build side. At the client side, you only need a modern browser. And it provided a nice excuse to try out Emscripten, something I hadn't come across earlier.

#### Implementing low-level primitives ourselves.
To keep the external dependencies to a minimum, I decided to implement the simple containers used in the project myself. Naturally, I first looked at the alternatives, but the number of available C libraries seemed too complicated for my purposes. At the moment there are two containers in the project: a dynamic array and an associative container using open addressing with linear probing.


## Plans for the future
- replace the JSON config files for the CLI with something more concise and code-like.
- support HLSL
- support missing SPIR-V features

