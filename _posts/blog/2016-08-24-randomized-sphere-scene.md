---
layout: post
title: Randomized Sphere Scene
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

When no actual productive work is being done it's easy to add unnecessary features. I made a sphere scene randomizer that generates a number of spheres and assigns diffuse color and emission values.

![]({{ site.url }}/assets/randomizedspheres.png)

Also, below is a rendering of a low poly stanford bunny from the current mesh loading implementation. It's not pretty at the moment and I hope to build a better framework for that soon. Sorting out the geometry and camera transformations is of higher priority at the moment, as well as adding specular and transmissive materials.

![]({{ site.url }}/assets/lowpolyrabbit.png)