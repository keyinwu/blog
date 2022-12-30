---
layout: post
title: "Unity C# Scripting Tips"
date:   2022-12-30
tags: [unity C#]
comments: false
author: keyinwu
---

- Automatic Component Dependencies
- OnDrawGizmos()
- tbc

## Automatic Component Dependencies

```c#
[RequireComponent(typeof(MeshFilter), typeof(MeshRenderer))]
public class Grid : MonoBehaviour {}
```

## OnDrawGizmos()

Gizmos can be drawn inside an ***OnDrawGizmos*** method, which is automatically invoked by the Unity editor. An alternative method is ***OnDrawGizmosSelected***, which is only invoked for selected objects.

Gizmos are drawn directly in **world space**, not in the object's local space.

OnDrawGizmos methods are also invoked while Unity is in **edit mode**.

```c#
	private void OnDrawGizmos () {
		if (vertices == null) {
			return;
		}
		Gizmos.color = Color.black;
		for (int i = 0; i < vertices.Length; i++) {
			Gizmos.DrawSphere(transform.TransformPoint(vertices[i]), 0.1f);
			// gizmos will move with the object
		}
	}
```