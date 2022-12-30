---
layout: post
title: "Procedural Grid"
date:   2022-12-30
tags: [unity C#]
comments: false
author: keyinwu
---


## Creating the Mesh

```c#
  private Mesh mesh;

	private IEnumerator Generate () {
		WaitForSeconds wait = new WaitForSeconds(0.05f);
		
    // Define Mesh
		GetComponent<MeshFilter>().mesh = mesh = new Mesh();
		mesh.name = "Procedural Grid";

		vertices = new Vector3[(xSize + 1) * (ySize + 1)];
		for (int i = 0, y = 0; y <= ySize; y++) {
			for (int x = 0; x <= xSize; x++, i++) {
				vertices[i] = new Vector3(x, y);
				yield return wait;
			}
		}
    // Define Mesh Vertices
		mesh.vertices = vertices;
	}
```

Which side a triangle is visible from is determined by the orientation of its vertex indices. By default, if they are arranged in a **clockwise** direction the triangle is considered to be **forward-facing and visible**. Counter-clockwise triangles are discarded so we don't need to spend time rendering the insides of objects, which are typically not meant to be seen anyway.

<img src="https://github.com/keyinwu/blog/raw/main/images/Unity/pg-triangle-sides.jpeg" width="50%"/>
<em>The two sides of a triangle</em>

```c#
  private IEnumerator Generate () {
		…

		int[] triangles = new int[6];
		triangles[0] = 0;
		triangles[3] = triangles[2] = 1;
		triangles[4] = triangles[1] = xSize + 1;
		triangles[5] = xSize + 2;
    // Define Mesh Triangles
		mesh.triangles = triangles;
	}
```

<img src="https://github.com/keyinwu/blog/raw/main/images/Unity/pg-quad.jpeg" width="30%"/>
<figcaption>A quad made with two triangles</figcaption>

### Mesh Code
```c#
  private void Awake () {
		Generate();
	}

	private void Generate () {
		GetComponent<MeshFilter>().mesh = mesh = new Mesh();
		mesh.name = "Procedural Grid";

		vertices = new Vector3[(xSize + 1) * (ySize + 1)];
		for (int i = 0, y = 0; y <= ySize; y++) {
			for (int x = 0; x <= xSize; x++, i++) {
				vertices[i] = new Vector3(x, y);
			}
		}
		mesh.vertices = vertices;

		int[] triangles = new int[xSize * ySize * 6];
		for (int ti = 0, vi = 0, y = 0; y < ySize; y++, vi++) {
			for (int x = 0; x < xSize; x++, ti += 6, vi++) {
				triangles[ti] = vi;
				triangles[ti + 3] = triangles[ti + 2] = vi + 1;
				triangles[ti + 4] = triangles[ti + 1] = vi + xSize + 1;
				triangles[ti + 5] = vi + xSize + 2;
			}
		}
		mesh.triangles = triangles;
	}
```

## Generating Additonal Vertex Data

### Normals

Normals are defined per vertex. The default normal direction is (0, 0, 1). The Mesh.RecalculateNormals method computes the normal of each vertex by figuring out which triangles connect with that vertex, determining the normals of those flat triangles, averaging them, and normalizing the result.

```c#
  // Define Mesh Normals
  mesh.RecalculateNormals();
```

### Texture

UV Coordinates

```c#
  Vector2[] uv = new Vector2[vertices.Length];
  ...
    uv[i] = new Vector2((float)x / xSize, (float)y / ySize); // use float
  ...
  // Define Mesh UV
  mesh.uv = uv;
```

Tiling Settings

<img src="https://github.com/keyinwu/blog/raw/main/images/Unity/pg-uv-tiling.jpg" width="70%"/>
<figcaption>Correct UV coordinates, tiling 1,1 vs. 2,1</figcaption>

The texture's wrap mode: **clamp** or **repeat**. By settings tiling to (2, 1) the U coordinates will be doubled. If the texture is set to repeat, then we'll see two square tiles of it.

### Normal Maps & Tagent Vectors

Normal maps are defined in **tangent space**. This approach allows us to apply the same normal map in different places and orientations.

The cross product of them yields the third direction needed to define 3D space. So a tangent is a 3D vector, but Unity actually uses a 4D vector. Its fourth component is always either −1 or 1, which is used to control the direction of the third tangent space dimension – either forward or backward. This facilitates mirroring of normal maps, which is often used in 3D models of things with bilateral symmetry, like people. The way Unity's shaders perform this calculation requires us to use −1.

```c#
  Vector4[] tangents = new Vector4[vertices.Length];
	Vector4 tangent = new Vector4(1f, 0f, 0f, -1f);
  ...
    tangents[i] = tangent;
  ...
  // Define Mesh Tagents
  mesh.tangents = tangents;
```

<img src="https://github.com/keyinwu/blog/raw/main/images/Unity/pg-tagent-space.jpeg" width="30%"/>
<figcaption>Tagent Space</figcaption>