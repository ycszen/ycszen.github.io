---
layout: post
title: "Graphics: Ray Tracing"
comments: True
date: 2015-03-24
categories: "graphics"
---

In this post, I targets to share my understanding towards ray tracing pipeline and will give an intuitive structure for ray tracing program design

<!--more-->

### Introduction

As wiki-pdia defines, [*Ray tracing*](https://en.wikipedia.org/wiki/Ray_tracing_(graphics)) is __a procedure of object rendering__, which differs from Rasterization-based rendering in the whole pipeline. It is pixel by pixel rather than primitive by primitive. And to reflect inter-reflection, a usual method is to allow recursion in this process, so as to get a realism image.

### Pipeline Overview

![alt pl](/images/posts/2015-04-06-ray-tracing-pipeline.png){:height="320px"}
{: .text-center }

In the above graph, different from other computer graphic rendering pipeline, ray tracing is a procedure that __mimics the imaging process in the real world__. It simulates the procedure of catching direct ray from light source, reflected ray from object surface, refracted ray through transparent object, etc, and thus achieves a superior realism effect.

In the following section, I will introduce the general ray tracing pipeline based on my personal practice, and will give a graph to help you with your [__ray tracer design__](#ray-tracer-design).

### Pipeline Details

#### Start from image plane and camera ray
{: .text-center}

![alt model](/images/posts/2015-04-06-ray-tracing-model.png){:height="320px"}
{: .text-center}

We first define a image plane at a particular space, consists of several pixels. The whole process goes in a __pixel by pixel__ manner. At each pixel, we produce a ray shot from the camera center towards a particular point in current pixel. And we trace that ray through calling a recursive function __trace()__, so as to get the result color of this camera ray.

#### Find Ray-Scene intersection
{: .text-center}

As addressed before, in the ray tracing pipeline, one important and determining step is to find the intersection point of our camera ray and the objects in the scene.

An natural and intuitive method is to define lots of object primitives in the scene, call __intersect(ray, scene)__ which iterating through the long list of all primitive objects in the scene and see what is the closest intersection point. So the problem is sub-divided into one that intersect ray and primitive 3D shapes.

In the following part, I would like to address some intersection formula with major shape primitives.

__Ray representation__

To ease the computation with intersection, we use a single ray origin along with ray direction and a variable __t__ stands for distance to represent a ray.

$$
\begin{align}
  &ray = ray_{origin} + ray_{direction} \cdot t \\
  \text{where } &ray_{origin} = (x_{s}, y_{s}, z_{s}), \\
  \text{and } &ray_{direction} = (x_{d}, y_{d}, z_{d})
\end{align}
$$

So that finding closest ray-object intersection is equivalent with finding the closest __t__ value from solving ray object intersection equation.

__Sphere intersection__

For a sphere object, we could represent it by its sphere center and its radius as following:

$$
(x - x_o)^2 + (y - y_o)^2 + (z - z_o)^2 = r^2
$$

And thus we can find the intersection point by __solving following equation set__:

$$
\begin{gather*}
&(x - x_o)^2 + (y - y_o)^2 + (z - z_o)^2 = r^2 \\
&x = x_s + t \cdot x_d \\
&y = y_s + t \cdot y_d \\
&z = z_s + t \cdot z_d
\end{gather*}
$$

This is a typical quadratic equation group, which we can solve by a closed form equation:

$$
\begin{gather*}
A = x_d^2 + y_d^2 + z_d^2 \\
B = 2 \cdot (x_d \cdot (x_s - x_o) + y_d \cdot (y_s - y_o) + z_d \cdot (z_s - z_o)) \\
C = (x_s - x_o)^2 + (y_s - y_o)^2 + (z_s - z_o)^2 - r^2 \\
t_1 = \frac{-B - \sqrt{ B^2 - 4 \cdot A \cdot C}}{2 \cdot A} \\
t_2 = \frac{-B + \sqrt{ B^2 - 4 \cdot A \cdot C}}{2 \cdot A}
\end{gather*}
$$

The resulted $t_1$ and $t_2$ will have different value condition according to the sign of $\Delta = B - 4 A C$. We need to __include__ the nearest positive solution and __exclude__ imaginary solutions as well as negative solutions (Since that a ray shooting out will not intersect a point that is behind the ray origin).  

By the way, it is really easy to calculate _normal for a intersection point_ on the sphere since we only need to calculate __$$\vec{N} = p_{intersect} - p_{origin}$$__
{: .maxim}

__Plane intersection__

To represent a infinite plane, we can use its surface normal $$\vec{N} = (A, B, C)$$, so that we would have $$A \cdot x + B \cdot y + C \cdot z + D = 0$$. And in this case, the solution for a ray surface intersection would be

$$t = - \frac{A \cdot x_s + B \cdot y_s + C \cdot z_s + D}{ A \cdot x_d + B \cdot y_d + C \cdot z_d }$$


__Box intersection__



__Triangle intersection__




#### Adding __local illuminance effect__ at intersection point
{: .text-center}

As I wrote in [__another post__](/graphics/2015/03/22/local-reflectance-model/), we could use the phong local reflectance model to simulate the material property of objects in the scene at each pixel. And to do so, we need to calculate the surface normal $\vec{N}$ at the intersection point __p__ , the light source direction vector $$\vec{L} = P_{intersection} - L_{source}$$ , the reflected light direction $\vec{R} = 2 \cdot \vec{N} - \vec{L}$ as well as the view direction $\vec{V} = -\vec{Ray_{direction}}$.

#### Shadow test
{: .text-center}

#### Reflection Ray
{: .text-center}

#### Refraction Ray
{: .text-center}

#### Fine-graining scene through __shooting more ray__ at each pixel
{: .text-center}

### Accelerate the process

#### Direction 1: Fewer Ray
{: .text-center}

#### Direction 2: Faster Intersection
{: .text-center}

#### Direction 3: Generalized Ray
{: .text-center}


### A Sample Ray Tracing Program Structure
{: #ray-tracer-design}

#### __Program Structure__
{: .text-center}

![alt struct](/images/posts/2015-04-06-ray-trace-program.png){:height="512px"}
{: .text-center}

Above graph is a basic structure for a simple ray tracer. The __blue__ part represents the primitive classes and object classes that together form the ray trace scene. The __Red__ part gives the logic for tracing a single pixel's effective color in the scene. You would also need a simple image utility function to deal with image format, so as to store output image to disk.

#### __Output Result__
{: .text-center}

![alt scene](/images/posts/2015-04-06-traced-scene.png){:height="256px"}
{: .text-center}


Here is an _output image_ from my __ray tracer__.
{: .maxim .text-center}


### References

[1] [**SFU - CMPT 361 Introduction To Computer Graphics by Prof. Ping Tan**](http://www.cs.sfu.ca/~pingtan/)

[2] [**Ray Tracing Acceleration Techniques**](http://www.cs.virginia.edu/~gfx/Courses/2003/ImageSynthesis/scribed_notes/03_acceleration.pdf)

[3] [**Berkley X - Foundation of computer graphics**](https://courses.edx.org/courses/BerkeleyX/CS-184.1x/2013_October/info)

[4] [**Scratchpixel - writing a simple raytracer**](http://www.scratchapixel.com/old/lessons/3d-basic-lessons/lesson-1-writing-a-simple-raytracer/)
