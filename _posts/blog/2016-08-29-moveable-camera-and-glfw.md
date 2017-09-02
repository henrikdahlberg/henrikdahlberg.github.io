---
layout: post
title: Moveable Camera and GLFW
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

I've made lots of progress this week. I decided to switch out the GLUT library functionality for GLFW to prepare for a full move to modern OpenGL. Right now I'm using some deprecated OpenGL functions to draw my image to the window which is not ideal, but it works for now. I read a great series of tutorials on modern OpenGL at [Learn OpenGL](http://www.learnopengl.com/) and I implemented a modified version of the described camera system into the HCamera class. At the moment all the controls are set up in the main.cpp file and passed to the camera, but in the future I would like to use a proper controller class but for now I will focus on further developing the rendering.

The camera set up to move in its local coordinate system by pressing the WASD keys, as well as moving up and down in the world coordinate system by pressing the space and left ctrl buttons respectively. The camera can also be rotated by pressing and holding down the right mouse button like in many CAD programs. The camera speed can be increased or decreased by scrolling the mouse wheel up or down and I plan to add more controls for things like aperture radius and focal distance soon.

Below a YouTube video showcasing the camera movement. It's obvious that the application is not ready for real-time usage due to the noise, but it's nice to be able to move around the scene to find a good angle for a rendering.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=_TkD4HVctVA
" target="_blank"><img src="http://img.youtube.com/vi/_TkD4HVctVA/0.jpg" 
alt="Moveable Camera Showcase" width="480" height="360" border="10" /></a>