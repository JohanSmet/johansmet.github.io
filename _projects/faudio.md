---
title: FAudio
excerpt: "<b>Open Source Contributions</b><br/>FAudio is an XAudio reimplementation that focuses solely on developing fully accurate DirectX Audio runtime libraries for the FNA project, including XAudio2, X3DAudio, XAPO, and XACT3."
header:
  teaser: assets/images/project_teasers/faudio_th.png
order: 15
classes: wide
---

## Background
From the FAudio project page: "This is FAudio, an XAudio reimplementation that focuses solely on developing fully accurate DirectX Audio runtime libraries for the FNA project, including XAudio2, X3DAudio, XAPO, and XACT3."

In the spring of 2018 flibitijibibo created [flibitBounties](https://github.com/flibitijibibo/flibitBounties), a repository hosting various mini-projects available to work on in exchange for some money and real-world experience. I wanted to add some other real-world experience to my resume, and these bounties proved a very nice way to start contributing to open-source projects. The bounties have precisely delineated goals, and Ethan provides quick and encouraging feedback. 

## Contributions
I did various mini-projects for FAudio:

- low/high/band-pass filters ([commit](https://github.com/FNA-XNA/FAudio/commit/7e42256c88f6389ed2aff0ece4a83391bf51fa5f)).
- COM-wrapper / XAudio2 Wine DLLs ([pull request](https://github.com/FNA-XNA/FAudio/pull/11)). This encompasses a wrapper that mimics the XAudio2 COM interface and allows FAudio to be used in place of XAudio2 without changing the target application. 
- Reverb effect ([pull request](https://github.com/FNA-XNA/FAudio/pull/12)). A reverb effect was needed for the Nintendo Switch port of **Dust: An Elysian Tail**. There is minimal information about the reverb implementation used in XAudio2, so we tried to make an effect that sounds decent with a wide range of input parameters.
- Experimental WMA decoder using FFmpeg ([pull request](https://github.com/FNA-XNA/FAudio/pull/46)). Andrew Eikum contributed the FFmpeg code he wrote for Wine, and I finished its integration into FAudio and added some tweaks.

During these, I also contributed various bug fixes and miscellaneous improvements. The COM-wrapper, and the subsequent usage as the XAudio2 implementation for Wine, allowed applications from outside the FNA ecosystem to be used which exposed previously unseen problems.

## Experiences
Working on FAudio was a fun and interesting experience. It went from working on a small isolated piece (the low/high/band-pass filter is basically just a function) to trying to figure out why Jet Set Radio Future hangs after a few seconds or why Skyrim has no sound. Without having access to the source of those games, of course. The feeling when you figure it out and JSRF finally runs without a hitch, is undescribable.

The work on the reverb effect was especially rewarding, but also frustrating. Digital signal processing is a large and complex field; with which I had little experience. There's a lot of information out there; but a lot of papers only mention high level information and almost no reproducible details. I'm glad the results we got sound rather convincing. I also ended up in the credits of the Switch port of Dust: An Elysian Tail; so that's also nice.

I know I only played a small part in the story of FAudio and FNA, but it's nice to know that I contributed a little to some of the ports that Ethan did and that I had a hand in a very tiny part of Proton.

But don't ask me about WMA, unless you have the time to spare ...
