---
layout: post
title: Cornell Box Scene Render
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

Before moving on to implementing more materials and a bounding volume hierarchy for triangle meshes, not to mention cleaning up the triangle mesh implementation itself, I decided to render the original Cornell box scene properly. I took the [measured data for the scene](http://www.graphics.cornell.edu/online/box/data.html) and put it in the scene specification of my renderer. The positions of the camera and all the triangle vertices correspond completely with the data, but getting the correct color values for the scene components and the roof light was done by trying to come as close to the reference picture as possible. The field of view of the camera is also not completely correct. Below is a comparison between my rendering and the reference image. Can you see which is which?

![]({{ site.url }}/assets/cornell1.png)

The colors match quite well but there's some tweaking to be done to get it completely right. Below is the rendering in its full resolution. The image was rendered with 65k samples per pixel with a maximum ray depth of 10, each pass taking around 82ms to complete, resulting in a total rendering time of approximately 1,5 hours. The convergence was very slow due to the relatively dim lighting and my hope is to implement more sophisticated rendering methods such as bidirectional path tracing or metropolis light transport in the future to improve the sampling.

![]({{ site.url }}/assets/cornell2.png)