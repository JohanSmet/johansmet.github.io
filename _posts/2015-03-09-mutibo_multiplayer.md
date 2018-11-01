---
title: Multi-player added to Mutibo (Coursera Capstone)
---
I felt kind of bad about not being able to implement all the features I had planned for my submission to the first Coursera.org Android Capstone before the deadline (for more details see my [previous post]({{ site.baseurl }}{% post_url 2015-01-10-coursera_android_2014_postmortem %}). So I revisited the code base and finished up the missing multi-player bits, and fixed some minor flaws along the way.

I put a quick demonstration video on YouTube (watch below!) to show off the functionality and the code is up on [GitHub](https://github.com/JohanSmet/coursera_mutibo). 

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/_-eFssPL0Lw?controls=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>


For simplicity's sake I did end up implementing it differently than I had originally envisioned.
My original design would allow players to challenge their Facebook/Google+ friends to a multi-player match. I didn't want to spend too much time on this so I only implemented a simple match-making procedure: when you start a multi-player match you wait for the next player to come along, if there isn't already one waiting for you. 

Only two players compete in a match and they answer the same question simultaneously until one of them gets three questions wrong. During the course of the match the server sends messages to the players using Google Cloud Messaging. I had written most of this code during the capstone with the intent of using it send challenges to other players. I decided to reuse this code for the entire multi-player system because it only requires small messages with embedded payload. 

So what's next for Mutibo? I'm thinking of experimenting with other mobile platforms and porting Mutibo might be a fun way of getting my feet wet. Haven't yet decided if it will be Windows Phone or iOS. I am leaning towards Windows Phone, mostly because of the Mac OS X requirement to iOS development.

We'll see what happens ... Until then, stay frosty !
