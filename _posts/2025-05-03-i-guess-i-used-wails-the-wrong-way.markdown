---
layout: post
title: "I Guess I Used Wails the Wrong Way"
date: 2025-05-03 00:00:00
description: And so, I finally went with C++
tags: SoftwareEngineering Random
---

Recently, I was looking at [Wails](https://wails.io/) as an option to build a hobby project I was thinking about. I planned to build a desktop application that can take a webcam input stream, and based on the movement of the person captured in the camera, then move some objects rendered by the application on the screen.

At work, my team has been using Wails to develop a desktop GUI client for one of our internal platforms that needs to alter some settings in the client machine when used. Many of the users of this platform aren't technical users, hence we need to provide GUI. Also, the users are using different operating systems on their machines, so we need to be able to support Linux, Mac OS, and Windows.

Initially, we built the front-end using [Electron](https://www.electronjs.org/). But as we started using Golang more for our back-end, we decided to move to Wails so we could reuse the server code written in Golang in our client implementation.

Wails can also use React front-end with TypeScript, which makes it a relatively interesting choice for me to use in my hobby project since I haven't been coding much at work as my work becomes heavier on managing)(and across multiple domains so by itself it already involves a lot of context-switching), and I have never been very up-to-date with front-end development technologies.

Sounds like an obvious choice for me to use while improving my knowledge of the framework my team uses at work, right?

...

Hoo boy, was it a mistake.

While I didn't really encounter meaningful problems doing the regular React and Golang parts of Wails, I think the way it was built for workflows similar to client-server communication between web browsers and web servers (after all, Wails front-end is just a web app rendered on a browser engine) made it a poor fit for the project I had in mind.

Continuously sending processed webcam frame images from the back-end to the front-end to be rendered by the React front-end was pretty slow and eventually caused the application to hang. Also, the front-end was a bit laggy when updating the webcam stream images because I was using the `<img>` tag to render the webcam frames and updating the image with a set interval from the front-end.

Probably I should've used the `<video>` tag instead, which I've never used before, but then I might need to adjust the implementation of the webcam image capture and processing flow to allow it to be streamed as video for the React front-end.

I ended up deciding to just drop the idea of using Wails for the project and just use C++ with OpenCV instead. And [it immediately worked](https://github.com/sdsdkkk/camface). Also, I ended up using OpenGL to draw the object to be controlled with the webcam input [and make it 3D](https://github.com/sdsdkkk/vobject-controller) instead of 2D as I initially planned to do with Wails.

I had this idea of changing the direction of [the project](https://github.com/sdsdkkk/camface) into a robot that can automatically aim and shoot people who are close enough to the camera instead (I saw a YouTube video of someone building this stuff a few years ago). But then I might need to build my own custom robotic parts, which might be a bit too complicated for me to want to invest my time in it for the time being, so I decided to just move forward with my original plan of making an object that's controllable via camera input.

Anyway, I think this is the first time I touched C++ since I graduated from university. I believe last time I used C++ was in my Computer Graphics course where we got assignments to draw things with OpenGL using C++, and I never came back to it after that. 

I did use C occasionally, such as when contributing a minor feature to the [dump1090_sdrplus](https://github.com/sdsdkkk/dump1090_sdrplus) project to allow it to export the data captured by my [RTL-SDR](https://www.rtl-sdr.com/) dongle into a CSV file. I believe that was the only time I contributed to any software project written in C though, the rest of my C usage was just me making toy Linux kernel modules for my own purposes.

# References

[The Wails Project](https://wails.io/)

[Electron](https://www.electronjs.org/)

[GitHub - sdsdkkk/camface](https://github.com/sdsdkkk/camface)

[GitHub - sdsdkkk/vobject-controller](https://github.com/sdsdkkk/vobject-controller)

[GitHub - sdsdkkk/dump1090_sdrplus](https://github.com/sdsdkkk/dump1090_sdrplus)

[RTL-SDR](https://www.rtl-sdr.com/)
