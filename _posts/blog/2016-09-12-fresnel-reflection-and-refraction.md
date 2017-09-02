---
layout: post
title: Fresnel Reflection and Refraction
tags: [DH2323 Project, GPU Path Tracer]
author: Henrik Dahlberg
---

I have implemented pure reflective and transmissive materials into the renderer as well as glossy materials, using the Fresnel equations to compute the reflectance R and transmittance T. When a ray intersects a primitive in the scene, the reflection and transmission directions are computed from the incident direction, the surface normal and the refractive indices of the mediums on each side of the boundary. For specularly transmissive materials, one approach is to trace both the reflected and the refracted ray through the scene. Another approach that gives the same expected value for the computed radiance is to take a specular sample with probability equal to the computed Fresnel reflectance. This approach is known as russian roulette and is commonly used in various statistical scenarios. We sample a uniform random number and if this number is less than the computed R, we take a reflected ray sample. Otherwise we take a transmitted ray sample If the material is transmissive or a diffuse sample if it is not.

Below are a few renders showing these effects. The glossy materials are have reflected and diffuse samples, the mirror materials are purely reflective, and the transparent materials have reflected and transmitted samples.

![]({{ site.url }}/assets/fresnel1.png)

![]({{ site.url }}/assets/fresnel2.png)

![]({{ site.url }}/assets/fresnel3.png)

The Lambertian diffuse samples, sampled by randomizing a cosine-weighted direction in the hemisphere, is actually just an approximation to the more general phenomenon of subsurface scattering, where light enters a material and is scattered around and absorbed before it exits the material. Such a phenomenon is modelled by the bidirectional subsurface reflectance distribution functions (BSSRDF) that encapsulates these volumetric scattering properties. When a material is modelled by a physically based BSSRDF that expresses low tranmissive properties and when the refractive index between the adjacent media is similar, the material will exhibit a diffuse appearance that the Lambertian model is trying to mimic. The next blog post will be about an attempt at implementing this into the renderer.