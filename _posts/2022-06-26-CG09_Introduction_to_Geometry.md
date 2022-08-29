---
layout: post
title: "CG09 Introduction to Geometry"
date:   2022-06-26
tags: [lecture notes, computer graphics]
comments: false
author: keyinwu
---


## Implicit Representations in Graphics

- algebraic surfaces f(x,y,z) = 0
- constructive solid geometry
- level set methods
- blobby surfaces
- fractals
- ...
- Pros
  - description can be very compact (e.g., a polynomial)
  - **easy to determine** if a point is in our shape (just plug it in!)
  - other queries may also be easy (e.g., distance to surface)
  - for simple shapes, exact description/no sampling error
  - easy to handle changes in topology (e.g., fluid)
- Cons
  - **expensive to find** all points in the shape (e.g., for drawing)
  - very difficult to model complex shapes

## Explicit Representations of Geometry

- triangle meshes
- polygon meshes
- subdivision surfaces
- NURBS
- point clouds
- ...
- (All points are given directly)
- Explicit surfaces make some tasks easy (like sampling), make other tasks hard (like inside/outside tests).

## Algebraic Surfaces (Implicit)

- Surface is zero set of a polynomial in x, y, z

## Constructive Solid Geometry (Implicit)

- Build more complicated shapes via Boolean operations
- Can chain together expressions

## Blobby Surfaces (Implicit)

- Instead of Booleans, gradually blend surfaces together
- <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/blobby_surfaces.jpeg" width="70%"/>

## Blending Distance Functions (Implicit)

- A distance function gives distance to closest point on object
- Can blend any two distance functions d1, d2
- Appearance depends on how we combine functions
- Q: How do we implement a Boolean union of d1(x), d2(x)?   
A: Just take the minimum: f(x)
min(d1(x), d2(x)) (get zeros on both shapes)
- [Scene of pure distance functions](http://iquilezles.org/)

## Level Set Methods (Implicit)

- <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/level_set_methods.jpeg" width="70%"/>
- Level Sets from Medical Data (CT, MRI, etc.)
- Level Sets in Physical Simulation: [encodes distance to air-liquid boundary](http://physbam.stanford.edu)
- Level Set Storage
  - Drawback: storage for 2D surface is now O(n3)
  - Can reduce cost by storing only a narrow band around surface

## Fractals (Implicit)

- exhibit self-similarity, detail at all scales
- new "language" for describing natural phenomena
- hard to control shape
- Mandelbrot Set  
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/mandelbrot_set.jpeg" width="70%"/>
  - Can color according to how quickly each point diverges/converges.
- [Iterated Function Systems](http://electricsheep.org)

## Point Cloud (Explicit)

- list of points (x,y,z)
- Often augmented with normals
- Easily represent any kind of geometry
- Easy to draw dense cloud (>>1 point/pixel)
- Hard to interpolate undersampled regions
- Hard to do processing / simulation / ...

## Polygon Mesh (Explicit)

- Store vertices and polygons (most often triangles or quads)
- Easier to do processing/simulation, adaptive sampling
- More complicated data structures
- Irregular neighborhoods
- Triangle Mesh (Explicit)
  - Store vertices as triples of coordinates (x,y,z)
  - Store triangles as triples of indices (i,j,k)
  - Use barycentric interpolation to define points inside triangles
  - (linear interpolation of point cloud)

## Bézier Curves (Explicit)

- Bernstein Basis  
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/bernstein_basis.jpeg" width="70%"/>
- A Bézier curve is a curve expressed in the Bernstein basis  
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/bezier_curves.jpeg" width="70%"/>
- High-degree Bernstein polynomials don't interpolate well (ex. n=10)

## Piecewise Bézier Curves (Explicit)

- -->Alternative idea: piece together many Bézier curves
- Widely-used technique (Illustrator, fonts, SVG, etc.)
- <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/piecewise_bezier_curves.jpeg" width="70%"/>
- Tangent Continuity
  - want endpoints of each segment to meet
  - want tangents at endpoints to meet
  - Each curve is cubic: u^3p0 + 3u^2(1-u)p1 + ...
  - 2 constraints < 4 vector degrees of freedom --> solvable
  - Can also do this with quadratic Bézier and linear Bézier
- **Higher-order curve**

## More on Bézier (higher-order surface)

- Tensor Product
  - Can use a pair of curves to get a surface
  - Tensor Product: Value at any point (u,v) given by product of a curve f at u and a curve g at v
- Bézier Patches  
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/bezier_patches.jpeg" width="70%"/>
  - Bézier patch is sum of (tensor) products of Bernstein bases
- Bézier Surface
  - connect Bézier patches to get a surface
  - Very easy to draw: just dice each patch into regular (u,v) grid!
  - Tangent continuity (Think: how many constraints? How many degrees of freedom?)
  - To make interesting shapes (with good continuity), need patches that allow more interesting connectivity (not only 4)
- Spline patch schemes  
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/spline_patch.jpeg" width="70%"/>

## Rational B-Splines (Explicit)

- Bézier can't exactly represent conics-not even the circle!
- Solution: interpolate in homogeneous coordinates, then project back to the plane: result is called a rational B-spline

## NUBS (Explicit)

- (N)on-(U)niform (R)ational (B)-(S)pline
  - knots at arbitrary locations (non-uniform)
  - expressed in homogeneous coordinates (rational)
  - piecewise polynomial curve (B-Spline)
- Homogeneous coordinate *w* controls "strength" of a vertex

## NUBS Surface (Explicit)

- <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/nubs_surface.jpeg" width="70%"/>

## Subdivision Surfaces (Explicit)

- NUBS Alternative: **Subdivision** (explicit)
  - Start with "control curve"
  - Repeatedly split, take weighted average to get new positions
  - For careful choice of averaging rule, approaches nice limit curve
    - Often exact **same curve** as well-known spline schemes!
  - One possible scheme: **Lane-Riesenfeld**
    - insert midpoint of each edge
    - use row k of Pascal's triangle (normalized to 1) as weights for neighbors
    - e.g., k = 2, (1,2,1) get weights (1/4,1/2,1/4)
    - limit is B-spline of degree k + 1
- Subdivision Surfaces
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/subdivision_surfaces.jpeg" width="70%"/>


## Summary

- Two major categories:
  - Implicit: "tests" if a point is in shape
  - Explicit: directly "lists" points