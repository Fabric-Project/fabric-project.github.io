---
title: Tutorial 1
layout: default
parent: Satin
nav_order: 3
---


# A Simple Scene with Satin


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

