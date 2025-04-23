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

## Examples

### A simple scene

1. Create a subclass of `MetalViewRenderer` with entrypoints for `setup`, `update` and `draw`

```swift
import Satin

final class Renderer3D: MetalViewRenderer {

    override func setup() {
    }

    override func update() {
    }

    override func draw(renderPassDescriptor: MTLRenderPassDescriptor, commandBuffer: MTLCommandBuffer) {
    }
}
```

This is generally the template of all Satin setups. Now we will leverage the some specific subclasses of Geometry and Material to make and customize a scene 


2. Create a `Mesh` and associated `Geometry` and `Material`
```swift
import Satin

final class Renderer3D: MetalViewRenderer {

    let mesh = Mesh(
        label: "Sphere",
        geometry: IcoSphereGeometry(radius: 0.5, resolution: 0),
        material: BasicDiffuseMaterial(hardness: 0.7)
    )

    override func setup() {
    }

    override func update() {
    }

    override func draw(renderPassDescriptor: MTLRenderPassDescriptor, commandBuffer: MTLCommandBuffer) {
    }
}
```


3. To render our Mesh, we need a Renderer, a Camera, and optionally a base Object which acts as our scene.
```swift
import Satin

final class Renderer3D: MetalViewRenderer {

    let mesh = Mesh(
        label: "Sphere",
        geometry: IcoSphereGeometry(radius: 0.5, resolution: 0),
        material: BasicDiffuseMaterial(hardness: 0.7)
    )

    lazy var scene = Object(label: "Scene", [mesh])
    lazy var renderer = Renderer(context: defaultContext)
    lazy var camera = PerspectiveCamera(position: [0, 0, 5], near: 0.1, far: 100.0, fov: 30)

    override func setup() {
    }

    override func update() {
    }

    override func draw(renderPassDescriptor: MTLRenderPassDescriptor, commandBuffer: MTLCommandBuffer) {
    }
}
```

4. Lets add the final missing pieces to draw:

```swift
import Satin
final class Renderer3D: MetalViewRenderer {

    let mesh = Mesh(
        label: "Sphere",
        geometry: IcoSphereGeometry(radius: 0.5, resolution: 0),
        material: BasicDiffuseMaterial(hardness: 0.7)
    )

    lazy var scene = Object(label: "Scene", [mesh])
    lazy var renderer = Renderer(context: defaultContext)
    lazy var camera = PerspectiveCamera(position: [0, 0, 5], near: 0.1, far: 100.0, fov: 30)

    override func setup() {

        camera.lookAt(target: .zero)
        renderer.setClearColor(.zero)

    }

    override func update() {
    }

    override func draw(renderPassDescriptor: MTLRenderPassDescriptor, commandBuffer: MTLCommandBuffer) {

        renderer.draw(renderPassDescriptor: renderPassDescriptor,
                      commandBuffer: commandBuffer,
                      scene: scene,
                      camera: camera)
    }
}
```

Thats all we need for our own `MetalViewRenderer` class, but we need a user interface to draw our `Renderer3D`. Satin provides `SatinMetalView` in SwiftUI.

Here is a simple SwiftUI view that will drive our Renderer3D

```swift

import SwiftUI
import Satin

struct ContentView: View {

    var body: some View {
        SatinMetalView(renderer: Renderer3D() )
    }
}
```







 




