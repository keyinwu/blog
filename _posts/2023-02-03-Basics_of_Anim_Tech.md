---
layout: post
title: "Basics of Animation Technology (WIP)"
date:   2023-02-03
tags: [animation]
comments: false
author: keyinwu
---

## 2D Animation

- Sprite animation
- Sprite-like animation in pseudo-3D game
  - Doom
- Sprite-like animation in modern game
  - 2D character
  - Game effect
- Live2D

## 3D Animation

- DOF: degrees of freedom
  - 6 DoFs per rigid objects
- Rigid Hierarchical Animation
- Per-vertex Animation
  - Most flexible (3 DoFs per vertex)
  - Mostly implemented by Vertex Animation Texture (VAT)
  - suitable for complex morphing
  - Need massive data
  - Cloth/Fluid Vertex Animation
- Morph Target Animation
  - A variation on Per-vertex Animation
  - Use **key frames with Lerp** instead of sequence frames
  - Suitable for facial expression
- 3D Skinned Animation
  - Advantages
    - Need less data than per-vertex animation
    - Mesh can be animated in a natural way (like human skin)
  - Animation is continuous and smmoth at joints
- Physics-based Animation
  - Ragdoll (in GTA5)
  - Cloth and fluid simulation
  - Inverse Kinematics (IK)

## Skinned Animation Implementation

- Different Spaces
  - Local Space
  - Model Space
  - World Space
- Humanoid Skeleton in Real Game
  - Normally 50-100 joints
  - May more than 300+ joints including **facial joints** and **gameplay joints** (weapon joint & mount joint, need to attach weapon to mount point)
- Root joint
  - The center of feet
  - Convenient to touch the ground
- Pelvis joint
  - The first child joint of the root joint
  - Human upper and lower body separation

## Math of 3D Rotation

- 3D Orientation Math
  - Euler Angle: Yaw, Pitch, Roll angle
  - Order Dependency
  - 3D-Rotation combined by x, y, z sequentially: R = RxRyRz
- Problems of Euler Angle
  - Gimbal Lock (because of the loss of one DoF)
  - Hard to interpolate (singularity problem)
  - Difficult for rotation combination
  - Hard to rotate by certain axis
- Quaternion
  - q = a + bi + cj + dk

## Animation Compression

## Animation DCC
