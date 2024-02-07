# Introduction

This book is a collection of articles that discuss ways to use vertex and pixel shaders to implement a variety of effects. The following provides a brief overview of these articles.

## Section I-- Geometry Manipulation Tricks

This section starts with a DX9 sequel to Dean Calver's vertext comprehenssion article in *Direct3D ShaderX: Pixel and Vertex Shader Tis and Tricks*.

 The vs_3_0 vertex shader model has a feature called vertex texturing, which Kristof Beets uses to create a very realistic-looking cloth animation in his article “Cloth Animation with Pixel and Vertex Shader 3.0.” In “Collision Shaders,” Takashi Imagire, who is known for the example programs on his web site (www.t-pot.com), uses shaders to calculate collisions, something that has never been shown before. The final article in this section covers using displacement mapping as a method of geometry compression. The main aim of Tom Forsyth’s article is to allow people to take data from the industry’s current mesh and texture authoring pipelines, and to derive displacement map data from them.

## Section II--Rendering Techniques

The section starts with the article by Greg James that presents a convenient ad flexible technique for rendering ordinary polygon objects of any shape as trick volumes of light scattering or light absorbing material with ps_1_3.

O’dell Hicks shows in his article, “Screen-aligned Particles with Minimal Vertex Buffer Locking,” how to create screen-aligned particles with a vertex shader, bringing us one step closer to the goal of having almost everything done by the GPU.

## Section III -- Software Shaders and Shader Programming Tips


## Section IV -- Image Space

