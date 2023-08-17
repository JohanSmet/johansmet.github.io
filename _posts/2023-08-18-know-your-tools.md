---
classes: wide
title: Know your tools
excerpt: Story time, and thoughts about software development.
---

Oh my, it has been a while since I've written anything for this blog. I should do something about that... time for a story.

A couple of years ago, I wasn't driving a lot, and the reason will be familiar to you. The battery of my car was getting older and couldn't bridge the time between trips to the grocery store anymore. Replacing the battery would probably have been the best course of action, but it required interaction with other humans – something that not only introverts were trying to avoid at the time. So, I bought a cheap jump start battery.

It worked fine the first time I used it. The battery includes a short cable with two clamps, and the procedure to start the car is simple: open the hood, connect the positive lead to a terminal in the fuse box (the battery is located in the trunk in my car model), connect the negative lead to the chassis, and start the car. I encountered no problems starting the car.

A week later, I needed to repeat the procedure, and now unexpectedly, it did not work. I thought maybe the battery pack was empty, so I took it back inside to measure it. The battery pack also has a rather bright LED built-in to provide a flashlight-like function. I played with that while I was walking inside, and the light shone brightly. I connected my multimeter, and the voltage was fine at a little over 12V. Puzzled, I went back outside, hooked it all up again, and now the car started. I was intrigued but decided to go shopping while the car was running.

Fast forward a few weeks, and I had the same problem again. Everything was hooked up in the same manner as earlier, but the car did not start. This time I decided to investigate. I measured the voltage between the positive terminal in the fuse box and the chassis, with the battery pack connected; it was about 8V. No wonder the car would not start. I disconnected the battery pack and measured its voltage, giving a result of 13V. Hmm, maybe a bad connection somewhere? I beeped out the connector cable, and it was fine. I connected the positive lead and measured the voltage between the terminal and the negative lead: 8V. Wait, what? I touched my multimeter probe to the positive connector, and the voltage reading jumped to 13V. Ok.

The positive terminal in the fuse box is on the edge of the box, and the copper is only reachable on one side. The clamp of the battery pack has copper on both prongs, but it turns out that only one side is connected to the cable, and the prongs are not electrically connected to each other. So it only works when the connector is on the terminal in one way but not when it is reversed.

![The aformentioned clamps]({{ site.url }}{{ site.baseurl }}/assets/images/various/charger_clamps.jpg){: .align-center}
<figcaption class="text-center">The clamps in question.</figcaption>

And now, that brings me to the moral of the story: I would have saved myself a lot of trouble if I had checked my assumptions about the cable and connector of the battery pack. Because there was copper on both sides of the clamp, I assumed both would be connected to the wire. Not an unreasonable assumption, but still, it appears, a wrong one.

This story has some interesting parallels in software development. I'd like to talk about two: "Know your tools" and "Check your assumptions."

"Know your tools" does not only refer to the development tools you use to build your software product, but also to the components making up the software. Taking some time to get familiar with all features will teach you how to use them effectively and, maybe more importantly, to consider the caveats and avoid the shortcomings. Learn how to quickly navigate through the source code, jumping to definitions or declarations. Using conditional or data breakpoints can bring a debugging session to a much quicker conclusion. Taking a minute to (re)read the documentation of a standard library feature can save you a lot of headache later on in the project.

I'm not saying you should learn the C++ standard by heart, unless you're a language implementer. Just don't be afraid to take the time to look something up or ask a colleague for their opinion. It's not a waste of time if it prevents bugs from being introduced into the project. Try to avoid getting mixed up in an hour-long "discussion" about proper placement of braces or, even worse, a heated tab vs. spaces debate.

"Checking your assumptions" is one of the most important rules of debugging, at least in my opinion. Fellow coders will most likely have experienced this situation before: you're trying to figure out a problem, but "nothing makes sense" or "what's happening shouldn't be possible." Take a step back, list your assumptions about the situation, and try to check if they hold. Either set a breakpoint and verify the behavior or check the documentation of a function you're calling to see if you're not mistaken about its preconditions. It can spare you a lot of frustrating hours going down the wrong rabbit hole.

Or maybe I just should have bought a higher quality battery pack ;-)
