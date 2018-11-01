---
title: Kinect Webcam
layout: single
classes: wide
---
A DirectShow-filter that turns your Kinect into a webcam. Works with Kinect for Windows v2 and the original Kinect for Windows.


## Background
In the summer of 2014 I was following a Massive Open Online Course which required webcam verification when my trusty (read: old) Logitech webcam broke. At the time I was also messing around with a Kinect for Windows v2 so I thought why not try to use its camera as a webcam. Not because I'm cheap (I promise) but it seemed like a fun little project.

Turns out it's not that little a project when you don't have any prior experience with DirectShow (who would have guessed) . But the basics were working pretty soon, over the next few weeks of spare time I fleshed it out more. October and November were filled with other spare time work and then the holidays happened. But now finally at the dawn of 2015 I've cleaned it up and packaged it for public release.


## Features
- Multiple output resolutions
	- Camera video is not scaled
    - Output is a sub-window of the original video stream
- Head Tracking
	- Uses Kinect body tracking to center the output sub-window around the tracked joint (by default around your head).
    - Only works when the output resolution is smaller than the native resolution of the camera.
- Background removal ("green screen")
	- Removes the background to only show people detected by the Kinect sensor
    - More a gimmick than a feature: partly due to the lower resolution of the depth sensor the resulting image quality is less than ideal.
- Includes simple application to manage the settings of the filter


## Support and Limitations
This filter should work with most applications that support DirectShow. I've supplied versions for 32-bit and 64-bit applications. For Kinect for Windows v2 you'll need to run at least Windows 8. It has been tested on Windows 8.1 with : 

- GraphStudioNext 32-bit and 64-bit
- Adobe Flash in Firefox, Chrome and Waterfox (64-bit)
- VLC
- Skype for Desktop
- Yahoo Messenger (yes, really)


It does not work with Internet Explorer on Windows 8+. Most probably because DirectShow isn't supported in Metro-style apps (in favor of Media Foundation). It has not been tested on Windows 10.


## Installation and usage instructions
1. Download the ZIP-archive for your platform and intented application
	- use the 32-bit version even on a 64-bit Windows when the intended host application is 32-bit (e.g. Firefox).
2. Extract the ZIP-archive to a local directory
3. Run kw_config.exe with administrator privileges by right-clicking and choosing "Run as Administrator"
	- only required for the first time
4. Go to the "Advanced" tab and click on "Register Filter"
5. The Kinect will show up under the name "KinectWebCam" in supported applications.
6. Profit !


## Downloads
32-bit Windows : [ZIP-archive]({{ site.url }}{{ site.baseurl }}/assets/downloads/kinect_webcam_x86_20160319.zip) (MD5 checksum: 850509a44d28125335d19b95a4c70abe)<br>
64-bit Windows : [ZIP-archive]({{ site.url }}{{ site.baseurl }}/assets/downloads/kinect_webcam_x64_20160319.zip) (MD5 checksum: 6aeb9ebe9e3c83cbf9ad515abe07215a)

**Updated 2016-03-19**. No longer requires both Kinect v1.8 and v2.0 runtimes to be installed to function. Filter will detect available libraries at runtime.

## Source code
The source code is available under the MIT-license on [GitHub](https://github.com/JohanSmet/kinect_webcam).

### Requirements
- CMake : version 2.8 or newer
- Visual Studio 2013 : developed with the Professional but Express will most like work as well. Older versions will probably not work because the code uses some C++11 features.

### Dependencies
- The Microsoft DirectShow Base Classes (strmbase)
	- extract to a "strmbase" subdirectory of the source root
	- more information [here](http://msdn.microsoft.com/en-us/library/windows/desktop/dd407279%28v=vs.85%29.aspx)
- OpenCV : tested with version 3.0.0-beta
	- extract to a "opencv" subdirectory of the source root

