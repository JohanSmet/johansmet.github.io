---
permalink: /resume/opensource
title: Resume - Overview of Open Source contributions
classes: wide
---

## Open Source Contributions

A noncomprehensive overview of my, humble, contributions to open source projects. 

### [Glean](http://glean.sourceforge.net/)

From the glean project page: "*glean* is a suite of tools for evaluating the quality of an
OpenGL implementation and diagnosing any problems that are discovered".

In the early 2000's I did the initial port of glean to Microsoft Windows (from Linux).

### [FAudio](https://github.com/FNA-XNA/FAudio)

From the FAudio project page: "This is FAudio, an XAudio reimplementation that focuses solely on developing fully accurate DirectX Audio runtime libraries for the FNA project, including XAudio2, X3DAudio, XAPO, and XACT3."

In the spring of 2018 flibitijibibo created [flibitBounties](https://github.com/flibitijibibo/flibitBounties), a repository hosting various mini-projects available to work on in exchange for some money and real-world experience. I wanted to add some other real-world experience to my resume and these bounties proved a very nice way to start contributing to open-source projects. The bounties have precisely delineated goals and Ethan provides quick and encouraging feedback. 

I did various mini-projects for FAudio:

- low/high/band-pass filters ([commit](https://github.com/FNA-XNA/FAudio/commit/7e42256c88f6389ed2aff0ece4a83391bf51fa5f)).
- COM-wrapper / XAudio2 Wine DLLs ([pull request](https://github.com/FNA-XNA/FAudio/pull/11)). This encompasses a wrapper that mimics the XAudio2 COM interface and allows FAudio to be used in place of XAudio2 without changing the target application. 
- Reverb effect ([pull request](https://github.com/FNA-XNA/FAudio/pull/12)). A reverb effect was needed for the Nintendo Switch port of Dust: An Elysian Tail. There is minimal information about the reverb implementation used in XAudio2 so we tried to make an effect that sounds decent with a wide range of input parameters.
- Experimental WMA decoder using FFmpeg ([pull request](https://github.com/FNA-XNA/FAudio/pull/46)). Andrew Eikum contributed the FFmpeg code he wrote for Wine and I finished its integration into FAudio and added some tweaks.

During these, I also contributed various bugfixes and miscellaneous improvements. The COM-wrapper, and the subsequent usage as the XAudio2 implementation for Wine, allowed applications from outside the FNA ecosystem to be used which exposed previously unseen problems. Diagnosing these issues without only the source for FAudio and not the troubled application proved to be interesting but also rewarding.

### [My own projects](/projects)

The code for most of my hobby projects has been released under a permissive license on my [GitHub](https://github.com/JohanSmet) page.

[Return to resume](/resume)