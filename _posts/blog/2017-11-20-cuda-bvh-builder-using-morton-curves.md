---
layout: post
title: CUDA BVH Builder Using Morton Curves
tags: [CUDA, GPU Path Tracer]
author: Henrik Dahlberg
---

For the PDC summer course Introduction to High Performance computer, [Martin Biel](https://martinbield.aerobatic.io/) wrote a BVH builder in CUDA based on a [HPG paper](http://research.nvidia.com/sites/default/files/publications/karras2012hpg_paper.pdf) by Tero Karras. The work uses Morton codes, 3D space-filling curves, to construct the bounding volume hierarchy very quickly on the GPU.

The main idea is to represent the BVH as a binary search tree, where each internal node forms an axis-aligned bounding box around its children nodes. The leaf nodes are the bounding volumes around each individual triangle. To construct the tree the triangles are first sorted according to their spatial locality, and then converted into a binary tree which is finally traversed to construct the intermediate bounding volumes in the hierarchy.

Morton codes constitute a mapping between 3D coordinates and 1D coordinates that preserves the spatial locality. The Morton code of a given 3D coordinate is determined by interleaving the bit representation of each xyz coordinate into one binary number. A consequence of this representation is that a sorted list of Morton codes naturally form a data structure where adjacent entries are in close proximity in 3D space. The sorted list can be used to construct a binary search tree, which will form the basis of the bounding volume hierarchy.

Two adjacent Morton codes form the children of one internal node. Four adjacent Morton codes form a sub-tree of two internal nodes corresponding to two adjacent pairs, which in turn form the children of another internal node. The construction of the tree is made by splitting the sorted list of Morton codes recursively. If the split is made at the point where the highest bit differs between codes at the left and right side of the split, then a binary structure that preserves the locality is obtained. Each internal node in the tree is defined by the bit prefix shared by all of its children nodes. Consequently, the resulting structure is a binary radix tree.

Here is a visualization of the hierarchy on the Stanford Dragon model.
![](https://raw.githubusercontent.com/henrikdahlberg/GPUPathTracer/master/Images/DragonBVH.png)


There are lots of technicalities in how to prevent race conditions between GPU threads when constructing the BVH from the radix tree, as well as how to traverse the tree on the GPU once it is built. I advise anyone interested in the details to read the paper, as well as the [follow-up paper](http://research.nvidia.com/sites/default/files/pubs/2013-07_Fast-Parallel-Construction/karras2013hpg_paper.pdf) by Tero Karras and Timo Aila on how to improve trees build by this method. The BVH introduced here is not necessarily superior to a BVH built according to SAH or a similar heuristic, but since it is generated on the GPU it builds hierarchies very quickly, allowing it to be used in interactive applications such as games that require fast BVH generation and intersection tests for things such as collisions. In short, the implementation allows me to generate a BVH for a scene in the matter of milliseconds, and as with any acceleration structure the number of intersection tests is reduced dramatically. To summarize, below are some images rendered in my CUDA path tracer that would have been very time consuming if not for the implementation of a BVH.

A model of a Star Destroyer from my favourite franchise Star Wars. I haven't implemented texture mapping yet so it is missing some detail, looking a bit dull and perhaps not doing the thing from the films justice. The stars are modeled by small, bright spherical light sources at a randomized distance from the model center. The model is lit by a larger sun-like object from the side.
![]({{ site.url }}/assets/StarDestroyer.png)

And here is the model currently featuring the front page of the blog. It's the well known Stanford Bunny and Dragon rendered with the brute-force subsurface scattering implementation of my renderer.
![]({{ site.url }}/assets/dragonbunny.png)