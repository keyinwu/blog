---
layout: post
title: "CG004: Drawing a Triangle"
date:   2022-06-24
tags: [lecture notes, computer graphics]
comments: false
author: keyinwu
---


Two major techniques for "getting stuff on the screen"

- Rasterization
- Ray tracing

Pipeline(s): inputs -> stages -> outputs

## Rasterization Pipeline

- Input & Output
  - 3D "primitives" -essentially all triangles! (possibly w/ color...) 
  - bitmap image (possibly w/ depth, alpha, ...)
- Why triangles?
  - can approximate any shape
  - always planar, well-defined normal
  - easy to interpolate data at corners - "barycentric coordinates"
  - Key reason: can focus on making an extremely well-optimized pipeline for drawing them

## Sampling

- Sampling = measurement of a signal
- Reconstruction 
  - via nearest: piecewise constant / "nearest neighbor"
  - via linear interpolation: piecewise linear
  - ...

## Aliasing

- Audio: high frequencies in the original signal masquerade as low frequencies after reconstruction (due to undersampling)
- Spatial aliasing (sin(x^2+y^2))
- Temporal aliasing (wagon wheel effect)
- Nyquist-Shannon theorem
  - Consider a **band-limited** signal: has no frequencies above some threshold w0
  - The signal can be perfectly reconstructed if sampled with period **T=1/2w0**
  - and if interpolation is performed using a **"sinc filter"** (ideal filter with no frequencies above cutoff (infinite extent!))
- Challenges in computer graphics
  - often not band-limited
  - infinite extent of "ideal" reconstruction filter (sinc) is impractical for efficient implementations.
- Aliasing artifacts in images   
  - moiré pattern    
  <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/aliasing_artifacts.jpeg" width="70%"/>
  

## Reduce Aliasing

- Supersampling
  - When having **infinite** frequencies in the signal, no matter how many samples are taken, there're always some errors in reconstruction. (Nyquist-Shannon theorem)
  <!-- - > More: Inigo Quilez, "Filtering the Checkerboard Pattern" & Apodaca et al, "Advanced Renderman" (p273) -->

- Resampling
  - Resample to display's pixel resolution (box filter)
  - Displayed result (with anti-aliased edges: 100%, 50%, 25% of a pixel)
  - Final coverage signal


## **Coverage** 

- Coverage via sampling
    - test a collection of sample points
    - with enough points & smart choice of sample locations, can start to get a good estimate
  - Sample coverage(x, y) = 1 / 0
- Breaking Ties
  - Edge cases: the sample is classified as within triangle if the edge is a "top edge" or "left edge"
  - Top edge: horizontal edge that is above all other edges
  - Left edge: an edge that is not exactly horizontal and is on the left side of the triangle. (triangle can have one or two left edges)
- Evaluate coverage(x,y) for a triangle
  - **Point-in-triangle test** (LA)
  - Traditional approach: incremental traversal  
    <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/incremental_traversal.jpeg" width="50%"/>
    - also visits pixels in an order that improves memory coherence: backtrack, zig-zag, Hilbert/Morton curves...
  - Modern approach: parallel coverage tests
    - modern hardware is highly parallel
    - test all samples in triangle "bounding box" in parallel
    - can be very wasteful in some cases
  - Hybrid approach: tiled triangle traversal
    <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/tiled_triangle_traversal.jpeg" width="50%"/>
    - Idea: work "coarse to fine"
  - Hierarchical strategies  
    <img src="https://github.com/keyinwu/blog/raw/main/images/ComputerGraphics/hierarchical_strategies.jpeg" width="80%"/>
    - recursive coarse to fine

<!-- ## **Occlusion**

<br> -->

## **Summary**

- Sampling & reconstruction
  - sampling: turn a *continuous* signal into *digital* information
  - reconstruction: turn *digital* information into a *continuous* signal
  - aliasing occurs when the reconstructed signal presents a false sense of what the original signal looked like
- Frame rasterization as sampling problem
  - sample **coverage function** into pixel grid
  - reconstruct by emitting a "little square" of light for each pixel
  - aliasing manifests as jagged edges, shimmering artifacts,...
  - reduce aliasing via supersampling
- Triangle rasterization is basic building block for graphics pipeline
  - amounts to three half-plane tests
  - atomic operation--make it fast!
  - several strategies: incremental, parallel, blockwise, hierarchical...
