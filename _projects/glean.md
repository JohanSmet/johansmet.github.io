---
title: Glean
excerpt: "<b>Open Source Contributions</b><br/>Glean: An OpenGL Test and Benchmarking Suite"
header:
  teaser: assets/images/project_teasers/glean_th.png
order: 5
classes: wide
---

## Background
From the projects website: *glean* is a suite of tools for evaluating the quality of an OpenGL implementation and diagnosing any problems that are discovered. glean also has the ability to compare two OpenGL implementations and highlight the differences between them.

## Contributions
I did the initial port of glean from Linux to Windows in the late 1999s / early 2000s. Around that time my day job started demanding more and more of my attention and I stepped away from the project.

## Experiences
I've always been pragmatic in my choice of operating system and I've oscillated between using Windows and Linux for the last two decades. Glean was the first time I took a project designed for one operating system and tried to make it run on another. And it was a fun experience.

Time has made my memories of this a little hazy. I remember using Visual C++ 6.0 to build the sources and that CVS was used for source revision control. I might have even submitted patches by e-mail for a bit; I can't confirm this because I don't have my e-mails from back then anymore.

I do remember one of the bigger issues being the difference in rendering context and window creation. With Wgl (on Windows) you need a window handle earlier in the process than with GlX (on Linux). Running on Windows required either a massive restructuring of glean or a clever solution. We ended up creating a temporary (invisible) window, and device context (HDC), for creating the OpenGL context. All based on a single line in the Win32 API documentation that stated that the HDC used for context creation did not have to be the same as the one used for SwapBuffers; only their pixel formats have to be the same.
