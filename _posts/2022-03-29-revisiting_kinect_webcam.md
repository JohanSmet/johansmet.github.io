---
classes: wide
title: Revisiting Kinect WebCam in 2022.
excerpt: Another walk down memory lane. Let's compile and test Kinect WebCam on Windows 10.
---

A few weeks ago, I received a nice e-mail asking about Xbox 360 Kinect support in [Kinect WebCam](/projects/kinect_webcam) on Windows 10. Now, I haven't touched this project in years; I certainly haven't tried running it on Windows 10. I wrote a nice e-mail back, explaining that DirectShow was already on its way out in 2014 and being replaced by shinier, newer things.

![Kinect WebCam in action]({{ site.url }}{{ site.baseurl }}/assets/images/kinect_webcam_example.png){: .align-center}
<figcaption class="text-center">Rough Green Screen effect in Kinect WebCam doing hand tracking.</figcaption>

But in the back of my head, it kept nagging me. Last weekend, I finally broke down and I dug out my Kinect v2; the original Kinect was somewhere in the attic. First order of business, get this thing compiling on my PC again. I could use the binaries I have available, but where's the fun in that?

Blogs are so last decade (or is it the decade before that?) so I decided to try something new and I recorded my screen during the entire ordeal. The result is available on my [YouTube channel](https://www.youtube.com/channel/UCUkXCU_GZgW2s4jbmHNkCcg/).

The biggest problem was going to be installing the required dependencies. The Kinect SDKs are still available from Microsoft. As well as the DirectShow Base Classes, these are now even available on GitHub. Qt5 and OpenCV were going to be the biggest hurdles. To make things go as smooth as possible, I decided to use a C++ package manager. I tried out [vcpkg](https://vcpkg.io) first. I liked it, but it compiles everything from source. Usually, that's not a problem, I kinda prefer it that way, but I *really* didn't want to compile Qt5 and its dependencies myself. So I switch to [Conan](https://conan.io).

Basically no modifications were necessary to get things up and running. I did change the CMake project a bit to make building against the Kinect SDKs optional; to allow me to compile and test incrementally. I also added decent build instructions to the project's readme.

The final thing I wanted to achieve was adding continuous integration; something I try to do for all my projects. The biggest hurdle here were the Kinect SDKs. The other dependencies are retrieved by Conan. I decided to make a private GitHub repository with the required parts of the Kinect SDKs and also included the DirectShow Base Classes. Only the headers files are required for the build because the filter loads the Kinect DLLs dynamically at run-time. The only one capable of accessing that private repo is the CI workflow,, so I don't think I'm breaking any licensing restrictions by doing this.

Using binary dependencies from Conan worked great for the 64-bit build, but not for the 32-bit build. These binaries aren't available anymore, which shouldn't be surprising. In the future, I'll have to think about moving to a cached build-from-source solution. I'll probably check out the export functionality of vcpkg, next. 

Another option would be to upgrade the dependencies to newer versions, that have more binary packages available. But I'm not sure if I want to invest the required time into a project that's basically reached the end of its lifetime.

Looking back at the project, I'm still quite satisfied with it. There are a few things I would do differently or would like to improve, though. I'd split the filter into a separate v1 (Xbox 360) and v2 (Xbox One) Kinect WebCam filter, so they can be used at the same time. And maybe spent some more time on the green screen effect; a smoothing filter might make things a lot more tolerable.

And what about DirectShow being on its way out to be replaced by better things? Well, apparently that's still the case in 2022 ... Funny how that happens, right?
