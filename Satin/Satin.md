---
title: Satin
layout: default
---

# About Satin

Satin is a Metal GPU rendering engine for Apple platforms, inspired by Three.js.

Satin is written by [Reza Ali](http://github.com/rezaali). 

[Read more about Satin's origins on Reza's portfolio site](https://www.syedrezaali.com/#/satin/) 

Satin is written in Swift, and designed to be flexibe, portable and embeddable into existing code bases. 

Satin provides a scene graph, mesh, geometry, materials, lighting, shaders, camera, animation, parameterization and rendering infrastructure out of the box.

## Satin Fundamentals

The core base classes of Satin are:

| Class        | Description         | Subclasses |
|:-------------|:------------------|:------|
| `Context`   | Provides control over the GPU, visual fidelity    | N/A  |
| `Object`    | Provides the basis of the scene graph, allowing for placement of and nesting of other objects to compose a scene. | `Camera` `Mesh` `Light` `IBLScene` |
| `Camera`    | Provides camera controls for rendering the scene | `PerspectiveCamera` and `OrthographicCamera`  |
| `Mesh`      | An `Object` subclass that renders `Geometry` with `Material`s  | `InstancedMesh`, `TessellationMesh` `TextMesh`  |
| `Geometry`   | Provides drawing of primitives (triangles, lines, points etc) via buffers of vertices and indices | See `Satin/Geometry/*`  |
| `Material`   | Provides color, shading, texturing of Meshes | See `Satin/Materials/*`  |
| `Shader`   | Provides low level control and customization of `Materials` and the rendering, including live coding of shaders. | See `Satin/Shaders/*`  |
| `Renderer`   | Manages low GPU resources, to render a Scene `Object` with one or many `Camera` | N/A  |






