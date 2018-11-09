---
classes: wide
title: Revisiting Mutibo
excerpt: Revisiting a software project from four years ago.
---

In 2014 I enrolled in an online course teaching Android app development at [Coursera](https://coursera/org). For the capstone project I wrote a small movie quiz app and an accompanying web service. It's been almost 4 years since I've touched, or even looked at, the source code. 

I thought it could be interesting to dig up the sources and try to get everything running again. I briefly entertained the idea of making it into a YouTube video, or even live-stream the process, but ultimately decided it would be too boring for anyone to watch. Looking back I'm glad I didn't try because most of my time was spend fiddling with certificates and app keys that I wouldn't want to show on screen anyway.

## The web service
About the only thing I remember about the web service, without looking through the code, was that it is written in Java using the [Spring](https://spring.io/) framework (a course requirement). It was hosted on [OpenShift](https://www.openshift.com) for a bit but I didn't upgrade the instance when RedHat moved to version 3 of the platform.

I originally developed the web service using NetBeans and gradle, a build tool. I created a new, debian based, virtual machine for the webservice and installed JDK 8. Getting the service to run was surprisingly easy.  All it took was installing the same, now ancient, gradle version on the machine and make it download all the dependencies and build the project. Then it just started up with no other issues.

I tried the latest stable gradle version but it won't work without some modifications to the build scripts. Rather understandably I'd say because I'm running version 1.11 and they're up to 4.10.2 at the moment. 

## The android app

I downloaded the latest version of [Android Studio](https://developer.android.com/studio/) and opened the project. This version of Android studio doesn't work with the version of gradle I used originally so it took some fiddling with the build scripts to get it to compile. 

I'm running android studio in an KVM virtual machine. As a side effect this prevents Android Virtual Devices (AVDs) from running properly because they also require KVM. I decided to test with my old Nexus 5 phone but had to disable "Instant Run" in Android Studio to get it to work properly.

At this point I got to the login screen of the app but logging in through either Google or Facebook was not working. After a while I figured out that due to the new Android Studio install the app was being signed with a new debug key and that I had to update the Facebook App settings and the Google API certificates.

After a [small update](https://github.com/JohanSmet/coursera_mutibo/commit/0a55eecad3730fa8c93642dddf02c7449640e189) to the Facebook login code (The Graph API doesn't return the first name by default anymore) everything was up and running.

## Modernizing

Upgrading to the latest gradle would be the first step in getting the service up-to-date. Subsequently we'd have to go over the dependencies and upgrade them one by one. The biggest time sink will probably be upgrading to spring boot 2.0 (from 1.1.8) and spring 5.0 (from spring 4.0). The server also consumes data from other, external, webservices (notably [The Movie Database](https://www.themoviedb.org/?language=en-US)) and uses [RetroFit](https://square.github.io/retrofit/) which is now at version 2.4 (1.7 is used in the project).

The app doesn't have much dependencies, basically only RetroFit (version 1.7 / latest is 2.4) and OkHttp (version 2.1 / latest is 3.11) and the Facebook and Google support libraries. But modernizing it will take more work than just updating these dependencies. 

One issue is that the Google API being used to get authentication tokens has been deprecated and will have to be replaced. We also need to look at supporting newer Android versions. The app targets SDK version 20 (Android 4.4 KitKat) but according to the [Android Distribution dashboard](https://developer.android.com/about/dashboards/) more that 90% of devices in use run a newer version of android. 

I'll have to get the AVD working with my setup and try it out on newer Android version to see what needs to be updated.

## Conclusion

In the end getting this 4 year old project to run again was easier than I expected. Granted I am still using mostly the same tools and dependencies. It was fun to play with this again but I don't think getting everything modernized will be worth the effort. It's still only an old school project after all.

