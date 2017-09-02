---
layout: post
title: Rendering Errors from RNG Bias
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

This post will be about some errors I remedied this week that had to do with bad random number generation. I realized there was something wrong with the rendering when I approached some scene objects closer than usual. I'm not sure why this is, but after stepping inside the Cornell box I had set up for the scene, artefacts popped up that hadn't been there earlier. Below are some pictures displaying these errors.

![]({{ site.url }}/assets/rngbias1.png)

![]({{ site.url }}/assets/rngbias2.png)

![]({{ site.url }}/assets/rngbias3.png)

![]({{ site.url }}/assets/rngbias4.png)

![]({{ site.url }}/assets/rngbias5.png)

![]({{ site.url }}/assets/rngbias6.png)

I tried to change the hemisphere sampling kernel without any success, after which I started playing around with the seed that's passed to the random number generators in the kernels. After each ray bounce, I was not reseeding the random number generator based on the ray depth, causing the seed to be the same at each bounce for the individual rays. I changed the seed to take the ray depth into account, after which I also started seeding everything form the system time in milliseconds resulting in a different seed each time the scene is reset, for instance when the camera is moved.

After having done this the renderings of the Cornell box scene still exhibited strong bias like in the upper leftmost picture above, shown in the leftmost picture below. I tried increasing the maximum ray depth, thinking that this might have been the source of the bias, but it barely changed anything. Switching to a sphere light instead of the square light changed the bias slightly as can be seen in the middle picture below, but that's obviously not an acceptable solution since a rendering system ideally shouldn't have to take such details of the scene into account.

Having looked around the web for similar projects I know that some - like me up until this point - generate random numbers using the cuRAND library, and some use the thrust library. I tried switching the cuRAND implementation for a corresponding implementation using the thrust library and voilá, the bias was gone, shown in the rightmost image below. I'm sure there was something wrong in the way I implemented the cuRAND function calls and fixing this would probably have remedied the problem. That would have required me to identify why the implementation wasn't working however and as I am also using thrust for stream compaction I will stick to this method for random number generation as well.

![]({{ site.url }}/assets/rngbias7.png)

![]({{ site.url }}/assets/rngbias8.png)

![]({{ site.url }}/assets/rngbias9.png)

While this error was incredibly annoying to chase down, it's interesting in retrospect to see the effects of bad random number generation. My first thought when seeing the errors at the top of this post was certainly not that it was the RNG seeding that caused it. I spent a lot of time going through the camera implementation to see if it was the camera movement causing it, or if there was any self-intersection going on in the tracing kernels, so this has been very educational.