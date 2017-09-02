---
layout: post
title: Code Skeleton and Project Outline
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

After a hardware failure I have received a new computer and began working on the project. An initial push with the project skeleton code has been made to [the GitHub repository](https://github.com/henrikdahlberg/GPUPathTracer). 

The CUDA computations will be carried out on a GTX 980 card. The CPU is an i7-6700K. Target features of the finished engine are:


- Object loading
- Boundary volume hierarchy generation
- Boundary volume hierarchy GPU traversal
- Path tracing on the GPU, implying GPU random number generation
- Accumulation buffer on the GPU for interactive viewport display
- Moveable camera with mouse and keyboard interaction
- An as general as possible material description

I aim at coding a modular and extendable rendering engine in an object-oriented way. I am not a great programmer but I hope to become better in the progress. Most examples of similar projects online run their code in one large main-file but I hope to define a better structure so that the code is more modular. I have begun setting up the structure for the project. Below is an outline of the main components and classes for the rendering engine and what their purpose is.

**Camera:** Class with position, rotation and lens properties and methods to move the object around. It has an internal **CameraData** struct which will be used to store positioning data of the camera that the GPU can access through CUDA. It will be updated for each frame on the CPU and then copied.

**Image:** Class with information about the image to be displayed in the OpenGL context window. Has information such as the image resolution and pixel color values.

**Scene:** Class with information about the scene to be rendered. Not yet decided is if this class will contain functions to load models or if that will be handled by a **Loader** class. Also not yet decided is if the boundary volume hierarchy will be handled here or in a separate class.

**Renderer:** Class that handles rendering. Has methods for sampling (may be handled by a **Sampler** class) and rendering on the GPU. The details of how this will be carried out hasn't been decided yet but the idea is to have it take a **Scene** object, copy traverse the boundary volume hierarchy and performing the light simulation on the GPU, and sending the result to an **Image** object ready for display.

In addition to these there will be classes for vectors, rays, triangles, materials etc. depending on how big the scope of the project will end up being, as well as extra mathematical utility. One of the main supporting arguments for structuring a project like this is that it allows for easier extendability if one would like to add or change features of some components, for instance the Renderer.

This blog and the GitHub repository  will be updated regularly from now on. The next step in the project is to get the random number generation going properly after which I will move on to the accumulation buffer. Another big obstacle is to figure out an efficient workflow with OpenGL and CUDA. I am also working on revising the project specification as I have had some new ideas over the months since I first initiated the project, before the hardware failure.