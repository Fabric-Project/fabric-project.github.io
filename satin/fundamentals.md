---
title: Satin Fundamentals
layout: default
parent: Satin
nav_order: 2
---

{:toc}

# Satin Fundamentals

Satin follows the common paradigm of creative coding toolkits which you may be familiar with:

- `setup()` - setup your scene, load assets, etc
- `update()` - prior to every frame draw, update is called giving you an opportunity to tick animations, etc.
- `draw(renderPassDescriptor: MTLRenderPassDescriptor, commandBuffer: MTLCommandBuffer)` allows you to submit rendering commands to the GPU.

- `MetalViewRenderer` is the class which provides the rendering infrastructure in Satin.

Within your own `MetalViewRenderer` subclass - you will be working with the core base classes of Satin:


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

To display your rendered scene, Satin provides:

- `SatinMetalView` for SwiftUI
- `SatinImmersiveSpace` for visionOS
- `MetalViewController` along with `MetalView` for AppKit
- `MetalViewRendererDelegate` protocol for custom drawing

These views all set up a the drawing loop, MTLDevices associated with the view, command queues, antialiasing, pixel formats, etc. 