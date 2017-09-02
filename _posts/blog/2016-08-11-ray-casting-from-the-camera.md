---
layout: post
title: Ray Casting from the Camera
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

Having tested the accumulation buffer and random number generation on the GPU, the next step is to cast rays from the camera origin into the scene through each pixel.

The camera is described my the HCamera class which holds data such as resolution, position, view direction, aperture radius etc. in a HCameraData struct. This data is passed to the renderer which allocates memory and copies the data for use on the GPU. The renderer also allocates memory on the GPU for an array of HRay objects that will be used to store the rays. The initial ray casting is done in the InitCameraRays kernel which assigns each ray to a pixel in parallel on the GPU and computes a direction from the camera origin through the pixel. Each ray direction is offset by a random jittering within the pixel, and each ray origin is offset from the camera origin within the aperture. This allows us to perform anti-aliasing essentially for free and adds a depth of field effect.

To verify that the ray casting from the camera is working as intended, the ray directions for each pixel is normalized to have each component in the [0.0f, 1.0f] range and is then drawn as a color to the OpenGL buffer, generating the image below:

![]({{ site.url }}/assets/raycasting.png)

Having this step completed now allows us to set up a scene of spheres and implement a sphere intersection routine to get a basic path tracing test scenario up and running. I have already done this with some errors still present which I will post about shortly.