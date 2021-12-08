---
layout: post
title: Crawl, Walk, Run... 
subtitle: If only it were that easy
---

Since my last entry, quite a bit of work has been acomplished. I have managed to re-familiarize myself with Marmoset Hexels 3 (my software of choice for the art in this project), I have also been doing a **lot** of reading surrounding OpenGL.

<p align="center">
    <img src="/img/2021-12-08-Camp_Fire.gif" />
</p>

Below are some of the important notes I've been attempting to implement to create a basis for the Crimson Engine.

I need to do a lot more reading surrounding the OpenGL GLSL (OpenGL Shading Language) and the GLSL Shader. As of right now, I've been following a tutorial and created a `default.glsl` file that contains default data for the vertex and fragment shaders.

OpenGL reads from two sets of buffers to be able to render any object on screen: A vertex buffer and element buffer. The vertex buffer object (VBO) is initialized from a vertex array object (VAO) that contains a list of each vertex with their associated RGB values. The format for each vertex is:
`[x, y, z, r, g, b]`. The element buffer object (EBO) is initialized from a list of indices that correspond to the vertices in the VBO. The indices are used to draw the triangles in the VBO (it is important to note that OpenGL draws the triangles COUNTER-CLOCKWISE, this becomes important when specifying the order of the indices).

[//]: # (TODO: FINISH THIS BLOGPOST)