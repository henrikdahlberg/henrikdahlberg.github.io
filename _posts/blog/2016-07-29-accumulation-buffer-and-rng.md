---
layout: post
title: Accumulation Buffer and RNG
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

I have just made some progress on the project and uploaded the latest to GitHub. The OpenGL and CUDA interoperability is now fully set up and the HRenderer class can render an image with the help of CUDA for OpenGL to display. A GL buffer is mapped to CUDA which is then manipulated in the HRenderer::Render() function call which launches a CUDA kernel. The kernel generates an image pass, gamma corrects and converts the colors to a type that OpenGL can display.

For now, the purpose was to demonstrate the accumulation buffer that will be used to update the display after each frame in order to give an interactive viewport. The kernel generates a random HDR color uniformly between (0.0f, 0.0f, 0.0f) and (1.0f, 1.0f, 1.0f) and stores it in the accumulation buffer. For each frame that passes, the buffer is then appended with a new color and the result is averaged over the number of passes. This way we can demonstrate how the color converges to the expected color value (0.5f, 0.5f, 0.5f). Below are three images after 1, 10 and 500 frames.

![1 frame]({{ site.url }}/assets/accumulationbuffer1.png)
![10 frames]({{ site.url }}/assets/accumulationbuffer2.png)
![500 frames]({{ site.url }}/assets/accumulationbuffer3.png)

This buffer will be needed in order for the Monte Carlo rendering simulation to converge to the desired image result, so the accumulation buffer step is crucial to an interactive path tracing engine.

Next I will work on Camera and Scene setup, probably using basic shapes such as spheres and boxes and get a basic render up and running before I work on the more advanced mesh loading and bounding volume hierarchy implementation.