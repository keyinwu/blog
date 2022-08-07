---
layout: post
title: "Compositing With Non-premultiplied or Premultiplied Alpha"
date:   2022-07-13
tags: [computer graphics, opengl]
comments: true
author: keyinwu
---


Assume we composite image B with opacity alphaB **over** image A with opacity alphaA, there're two ways of calculating the alpha channel:

- Non-premultiplied alpha
- Premultiplied alpha

## Non-premultiplied alpha

Composite Color:

- **C = alphaB * B + (1 - alphaB) * alphaA * A**

  where alphaB represents the appearance of semi-transparent B, and (1 - alphaB) represents what B lets through

Composite Alpha:

- **alphaC = alphaB + (1 - alphaB) * alphaA**


## Premultiplied alpha

Premultiplied alpha means multiplying color by alpha first, and then compositing, where

- **A' = alphaA * (Ar, Ag, Ab, 1)**

- **B' = alphaB * (Br, Bg, Bb, 1)**

- **C' = B' + (1 - alphaB) * A'**

Final Color:

- **C = C' / alphaC'**


<!-- ## Fringing -->

<!-- example -->

<!-- - Premultiplied alpha also performs better when pre-filtering (downsampling) a texture with an alpha matte -->

<!-- calculation example -->



## Advantages of premultiplied alpha

1. Compositing operation treats all channels the same (color and a)
2. Fewer arithmetic operations for "over" operation
3. Closed under composition (repeated "over" operations)
4. Better representation for filtering (upsampling/downsampling) images with alpha channel
5. Fits naturally into rasterization pipeline (as homogeneous coordinates)

> Closed: if taking two elements of the set and applying the operation on them always yields an element within that set