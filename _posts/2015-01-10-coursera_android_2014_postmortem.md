---
title: Coursera.org 2014 Android Specialization Capstone Post-Mortem
classes: wide
---
# Introduction
Massive Open Online Courses (MOOCS) have been a hobby of mine for a while now. Since 2012 I've participated in a few courses related to computer sciences on several websites. When Coursera.org announced their first specializations, a grouping of multiple courses with a final capstone project, I was particularly interested in the Android specialization.

Coursera describes the specialization as : "This sequence of courses examines mobile cloud computing on the Android platform, starting with user-facing applications, through the middleware and services running on Android devices, all the way to integration with network-accessible cloud services." I had no real prior experience with mobile app development (not counting the few hours spend toying with the NDK) and was looking forward to learning more about it.

I completed the first three parts with distinction, a requirement for entering the capstone, with minimal effort and decided to go ahead and participate in the final part. 

![Mutibo Game]({{ site.url }}{{ site.baseurl }}/assets/images/mutibo_game.jpg){: .align-center}

In this blog post I'm going to look back on the two months of free time I invested in the capstone and try to provide an honest view of the end result. I used to be subscribe to Game Developer Magazine back in the day and the first article I read was the post-mortem where a developer/producer looks back on a completed project and goes over things they thought went right and, more interestingly, what went wrong. I'm going to format this post in the same way.

Out of 3 possible projects I chose a movie quiz app because that resembled my personal interests (games) the most. In the app the user is presented with 4 movies and has to pick the one that does not fit in with the others. The game ends when three incorrect answers have been given.

You can check out the full source of the app and the accompanying server on [GitHub](https://github.com/JohanSmet/coursera_mutibo). As a reference a short video demonstration of the submitted app is available on YouTube:
<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/-nSTSuENN5M?controls=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

My submission was voted in the top 10 of its category by the other participants and the Coursera staff.

# What went right 
## 1. Integrated Google+ / Facebook authentication early on.
My initial intent was to start with basic authentication (as demonstrated in the third course) and only integrate Google+ and/or Facebook at the end of the project if time allowed. 

I started researching various options during lunch breaks and it seemed to me that integrating Google+ would not be as though as it was made out to be on the Coursera-forums. I decided to drop my original plan and implemented the Google+ authentication flow during a weekend, about the same amount of time I had budgeted for basic authentication. Facebook was easy to add in later on.

![Login Screeb]({{ site.url }}{{ site.baseurl }}/assets/images/mutibo_login.jpg){: .align-center}

## 2. Dropping multi-player at the end of the capstone.

I really wanted to add a few of the suggested bonus features and I scheduled multi-player matches as the last feature before starting doing final UI polishing.

I started working on the multi-player feature in the final week before the deadline. That meant I had a few hours each evening to work on it because I reserved the final week-end to make the final deliverables, including the video presentation. 

During the second night it became clear I could either finish multi-player or polish the UI/UX of the app. I made the call to drop multi-player and started polishing the general look and feel of the app.

In the end I think this decision played a big part in getting the app voted in the top 10 best submission of the capstone.

## 3. Detailed Design Document
As any professional software developer, hopefully, will confirm time invested in a well thought out design document will pay back a lot later on in the project. I have to admit though that I usually skip this for personal/hobby projects. I start with a general idea and just go with the flow. 

For me there were quite a few unknowns in the capstone and so I set out to construct a comprehensive document. For the server component I focused on the API I wanted to expose to the client and for the app I limited myself to screen mockups and a way of storing the required data on the device.

I ended up with a document just shy of 30 pages. This helped me keep my focus during the implementation phase.

## 4. Basic Web-based UI to manage the questions 
Early on in the development I used a week-end to build a basic management interface. It has a web front-end (using jQuery/handlebars) and the server fetches all the required movie information, including posters, from the Internet.

This allowed me to add questions to the game without wasting much time later in the development cycle. It also allowed me to develop the first parts of the server and test them in a environment I was more familiar with at the time.
<br />  

# What went wrong
## 1. Wasted time at the beginning of the capstone.
I started at a leisurely pace in the beginning. The first week was spent goofing around with a few prototypes to check out the viability of some of my ideas. I started on the design document in the second week and finished it just before the mid-term deadline. I hadn't written any code for the final product at that time.

Admittedly I made a silly mistake when I first budgeted my available time. I'm used to thinking in (work) days and have a general idea how much I can accomplish in a day. So I figured writing the design document in under a week should be easy. Ofcourse when you can only spend about 2 hours each evening working on it, it takes quite a bit longer.

I realized this in time and ramped up my speed as much as possible. If I had started at this speed I probably would have finished multi-player in time for the deadline. Ah well.

## 2. Overengineered back-end
This ties in to the previous point. I overestimated the work I was going to be able to do in the alloted time. So I spend quite a bit of time designing and implementing, some would say 'overengineering', a versatile solution that wasn't really required for the app as it was implemented by the deadline.

I tried to focus on robustness and scalability in the design and implementation of the server. While this is great for production software, this project will never go that far. Again, I could have used the time invested in this to finish multi-player before the dead-line.

## 3. Unit tests
This might seem as a weird thing to put in the 'what went wrong'-category. Unit tests are great, right ? A lot of developers would claim they are indispensable for quality software development. And they are right, ofcourse. 

But they are not for a project of this scope, especially with a such a limited time budget. I spent quite a bit of time in the beginning of the project setting up the unit test framework and writing tests for the first few Spring controllers of the server.

I don't feel that time was an entire waste, but I don't feel like they yielded enough benefit to be worth it (for a project of this scope).

## 4. Demo-video was too limited
The demonstration video I made for the final submission only showed the Android app itself. The server and the management UI were only referenced indirectly. A lot of work went in these parts and they deserved some screen time as well.
<br />

# Final thoughts on the capstone and specialization
I really enjoyed myself during the entire specialization and learned a lot, especially during the capstone. The first three courses were fine as an introduction for the capstone. But they were rather easy to complete, even for someone who hadn't touched Java for about 15 years. 

The capstone itself was different though. Implementing the server and application from scratch really forces one to focus and understand each part of the solution. I proved to be a real challenge to complete in the allotted timf and that provides a good sense of accomplishment.

I do feel that the final grading rubric could have been more detailled. As it was now an app with a barely functioning UI could received the same score as a fully polished, robust app.

In the end I'm glad I participated in the specialization. I could have learned much of it on my own but I would have never come into contact with some of the technologies used in the capstone project (the Spring framework comes to mind).

The capstone also serves as a frame of reference for my abilities. I feel comfortable now including Java and Android experience on my resume and feel I can back this up with the results of my capstone submission.

