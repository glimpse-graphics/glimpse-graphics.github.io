---
title: Glimpse Documentation
linkTitle: Documentation
layout: toc-page
abstract: Glimpse Documentation
keywords: docs,documentation,glimpse,opengl,3d
---

{% include warn.html content="This page is out-of-date." %}

## Introduction

Glimpse is an open-source project aimed to make [OpenGL][opengl] simple.

The source code is written in [Kotlin][kotlinlang] and it is intended
to be used with Kotlin.

Glimpse is distributed under [Apache License Version 2.0][apache-2].

### Glimpse Philosophy

Glimpse aims to solve 2 major issues when it comes to implementing OpenGL
applications:

1. **The initial setup is usually the most painful part of working with
   OpenGL.** Most problems are caused by:
   *  missing or incorrectly ordered calls to OpenGL functions,
      when no exceptions are thrown, but the visual effect is not as
      expected;
   *  miscalculated transformation matrices, which are virtually impossible
      to debug.
2. **Different platforms use different bindings for the OpenGL API, which
   are not compatible,** e.g. Android implementation provides static methods,
   while JOGL requires using an instance of OpenGL interface.

The first issue is solved by adding another layer of abstraction on top of
OpenGL. Glimpse implements typical use cases, providing a much more natural
interface to the application developer.

To solve the second issue, Glimpse is implemented as a Kotlin Multiplatform
library, with the common code independent of the underlying OpenGL
implementation. In principle, it even allows for non-OpenGL implementations.

Currently, Glimpse supports the following implementations:
*  Android
*  Desktop Java (JOGL):
   *  Windows
   *  macOS
   *  Linux

### What Glimpse Is NOT

**Glimpse is NOT a game engine.** Even though it builds another layer of
abstraction on top of OpenGL, it is still a relatively low-level library.

## Gradle Setup

When implementing a Glimpse application, include the following dependencies
in your `build.gradle.kts`:

```kotlin
// CORE LIBRARY:

implementation("graphics.glimpse:glimpse-core:[GLIMPSE_VERSION]")

// ANNOTATION PROCESSOR:

// If you want Glimpse Processor to generate Java code:
kapt("graphics.glimpse:glimpse-processor-java:[GLIMPSE_VERSION]")

// If you want Glimpse Processor to generate Kotlin code:
kapt("graphics.glimpse:glimpse-processor-kotlin:[GLIMPSE_VERSION]")
// Read about limitations of generated Kotlin sources:
//     https://kotlinlang.org/docs/kapt.html#generating-kotlin-sources

// UI COMPONENTS:

// If you want to use platform-specific UI components:
implementation("graphics.glimpse:glimpse-ui:[GLIMPSE_VERSION]")

// If you want to use Compose multiplatform UI components:
implementation("graphics.glimpse:glimpse-ui-compose:[GLIMPSE_VERSION]")

// ADVANCED FEATURES:

// If you want to load meshes from Wavefront OBJ files:
implementation("graphics.glimpse:glimpse-obj:[GLIMPSE_VERSION]")

// If you want to render images offscreen:
implementation("graphics.glimpse:glimpse-offscreen:[GLIMPSE_VERSION]")

```

If you are still using Groovy scripts in your project, include the following
dependencies in your `build.gradle`:

```groovy
// CORE LIBRARY:

implementation 'graphics.glimpse:glimpse-core:[GLIMPSE_VERSION]'

// ANNOTATION PROCESSOR:

// If you want Glimpse Processor to generate Java code:
kapt 'graphics.glimpse:glimpse-processor-java:[GLIMPSE_VERSION]'

// If you want Glimpse Processor to generate Kotlin code:
kapt 'graphics.glimpse:glimpse-processor-kotlin:[GLIMPSE_VERSION]'
// Read about limitations of generated Kotlin sources:
//     https://kotlinlang.org/docs/kapt.html#generating-kotlin-sources

// UI COMPONENTS:

// If you want to use platform-specific UI components:
implementation 'graphics.glimpse:glimpse-ui:[GLIMPSE_VERSION]'

// If you want to use Compose multiplatform UI components:
implementation 'graphics.glimpse:glimpse-ui-compose:[GLIMPSE_VERSION]'

// ADVANCED FEATURES:

// If you want to load meshes from Wavefront OBJ files:
implementation 'graphics.glimpse:glimpse-obj:[GLIMPSE_VERSION]'

// If you want to render images offscreen:
implementation 'graphics.glimpse:glimpse-offscreen:[GLIMPSE_VERSION]'
```

## Core Features

### Rendering Callback

To use Glimpse for rendering, you must create your own implementation of
[`GlimpseCallback`][apidocs:GlimpseCallback].

Run all operations that should be executed only once in `onCreate()`.
In most cases, this includes:
*  basic configuration (clear color, depth test, face culling, etc.),
*  building meshes,
*  loading textures,
*  compiling shaders and linking programs.

Handle viewport changes in `onResize()`, including updates to cameras
and lenses. At this stage, `GlimpseAdapter.glViewport()` should be called.

Run all per-frame operations in `onRender()`, including rendering itself.
Don't forget to call `GlimpseAdapter.glClear()`.

Call all `dispose()` methods in `onDestroy()`

### OpenGL Adapter

Abstraction layer is built on top of
[`GlimpseAdapter`][apidocs:GlimpseAdapter], which provides a common OpenGL
interface on all supported platforms.

In most cases, `GlimpseAdapter` is merely passed as a parameter to other
components, but it needs to be called directly for most basic operations:

```kotlin
override fun onCreate(gl: GlimpseAdapter) {
    gl.clearColor(Vec4(0f, 0f, 0f, 1f))
    gl.glClearDepth(1f)
    gl.glDepthTest(DepthTestFunction.LESS_OR_EQUAL)
    gl.glCullFace(FaceCullingMode.DISABLED)
    gl.glEnableBlending()
    gl.glBlendingFunction(
        BlendingFactorFunction.SOURCE_ALPHA,
        BlendingFactorFunction.ONE_MINUS_SOURCE_ALPHA
    )
}
```

### Basic Types

#### Angles

To avoid the confusion with angle measurement units, Glimpse uses a data
class [`Angle`][apidocs:Angle], containing both `deg` and `rad` values.

To create an angle, call one of the available factory methods:

```kotlin
Angle.fromDeg(45f)
Angle.fromRad(0.785f)
```

Glimpse will automatically use correct measurement unit required in each
supported use case.

#### Vectors

Glimpse defines 2D, 3D and 4D vector types: `Vec2`, `Vec3` and `Vec4`.

`Vec2` can also be used as texture coordinates, while `Vec3` and `Vec4`
can act as colors without or with alpha channel respectively.

For more information, visit the [API docs][apidocs:core:types].

#### Matrices

There are 2×2, 3×3 and 4×4 matrices defined in Glimpse—`Mat2`, `Mat3`
and `Mat4`—implementing all sorts of operations, such as multiplication,
transposition or inversion.

A number of utility functions provide an easy way to create matrices
for various affine transformations.

For more information, visit the [API docs][apidocs:core:types].

### Buffers

[Buffers][apidocs:Buffer] provide data arrays to shaders. They are
the foundation, meshes and models are built on top of. In most cases,
there is no need to use buffers explicitly.

To allow future support for non-JVM platforms, Glimpse introduces wrappers
for `java.nio` buffers:
*  [`FloatBufferData`][apidocs:FloatBufferData]
*  [`IntBufferData`][apidocs:IntBufferData]

### Meshes And Models

A [`Mesh`][apidocs:Mesh] defines a set of vertices the way they are rendered.
Currently, only meshes of triangles are supported out-of-the-box, but it is
possible to create a custom mesh that consists of points, lines, quads, etc.

#### Building Mesh Data

_**Note:** This section describes manual mesh data creation.
In most cases, however, the desired way to go is to use
[Wavefront OBJ files](#wavefront-obj-files)._

To create a mesh, [`MeshData`][apidocs:MeshData] is required, which can be
built with a [`MeshDataBuilder`][apidocs:MeshDataBuilder]:

```kotlin
val triangleMeshData = MeshDataBuilder()
    .addVertex(Vec3(-1f, -1f, 0f))
    .addVertex(Vec3(1f, 1f, 0f))
    .addVertex(Vec3(0f, 0f, 1f))
    .addTextureCoordinates(Vec2(0f, 0f))
    .addTextureCoordinates(Vec2(2f, 0f))
    .addTextureCoordinates(Vec2(1f, 1f))
    .addNormal(Vec3(0f, -1f, 0f))
    .addNormal(Vec3(1f, 0f, 0f))
    .addNormal(Vec3(0.7f, -0.7f, 0f))
    .addFace(
        listOf(
            MeshDataBuilder.FaceVertex(0, 0, 0),
            MeshDataBuilder.FaceVertex(1, 1, 1),
            MeshDataBuilder.FaceVertex(2, 2, 2),
        )
    )
    .buildArrayMeshData()
```

Method `buildArrayMeshData()` builds a non-indexed mesh. Indexed mesh data
implementations are currently not available.

#### Creating A Mesh

With an instance of `ArrayMeshData` built with a `MeshDataBuilder`,
a `Mesh` can now be created:

```kotlin
override fun onCreate(gl: GlimpseAdapter) {
    val meshFactory = Mesh.Factory.newInstance(gl)
    val mesh = meshFactory.createMesh(meshData)
}
```

To draw different meshes, create a custom implementation of `Mesh`
interface. Custom implementations of `MeshData` are not supported.

#### Models

You can implement [`Model`][apidocs:Model] interface to combine a `Mesh`
and a model transformation matrix in a single object.

### Textures

#### Building A Texture Image Source

An instance of  [`TextureImageSource`][apidocs:TextureImageSource] can be
built with a [`TextureImageSourceBuilder`][apidocs:TextureImageSourceBuilder],
which implements a set of platform-specific methods and extension functions
that allow using different texture image sources.

The desktop implementation allows for loading textures from:
*  `InputStream`s,
*  `File`s,
*  resources.

For example:
```kotlin
TextureImageSource.builder()
    .fromResource(this, "bricks.png")
    .build()
```

The Android implementation provides the following texture image sources:
*  `Bitmap`s,
*  `File`s,
*  assets.

For example:
```kotlin
TextureImageSource.builder()
    .fromAsset(context, "bricks.png")
    .build()
```

#### Prepared Texture Image Sources

Instead of using `build()` method of `TextureImageSourceBuilder`, you may
call `buildPrepared()`, which returns a `TextureImageSource` containing
a pre-loaded texture image.

Creating a texture from a prepared texture image source is quicker,
and therefore reduces the workload of OpenGL thread. On the other hand,
a prepared image source consumes much more memory, and thus it should be
used with caution.

For example, on Android:
```kotlin
TextureImageSource.builder()
    .fromAsset(context, "bricks.png")
    .buildPrepared()
```

Desktop implementation of `buildPrepared()` additionally requires the
`GLProfile` used by the `GLJPanel`:
```kotlin
TextureImageSource.builder()
    .fromResource(this, "bricks.png")
    .buildPrepared(glimpsePanel.glProfile)
```

#### Building Textures

Once a `TextureImageSource` is built, it can be used to create a
[`Texture`][apidocs:Texture].

[Texture `Builder`][apidocs:Texture:Builder] allows for building textures
from multiple sources in a convenient way:
```kotlin
override fun onCreate(gl: GlimpseAdapter) {
    val textures = Texture.Builder.getInstance(gl)
        .addTexture(textureSource1)
        .addTexture(textureSource2)
        .addTexture(textureSource3)
        .generateMipmaps()
        .build()
}
```

### Cameras And Lenses

#### Cameras

Implementations of [`Camera`][apidocs:Camera] interface help to calculate
a view matrix.

*  `FreeCamera` is not bound to any specific target. Its orientation is
   determined by its [roll, pitch and yaw][wiki:pitch-yaw-roll] rotation
   angles.
*  `TargetCamera` looks at a specific `target` point in space.
*  `RelativeTargetCamera` also looks at a specific point in space,
   but its position is defined using spherical coordinates relative to
   the `target`.

#### Lenses

Projection matrix can be calculated using [`Lens`][apidocs:Lens] interface.

*  `OrthographicLens` provides orthographic projection matrix.
*  `FrustumLens` provides a perspective projection matrix defined by
   a given frustum.
*  `PerspectiveLens` provides a perspective projection matrix defined by
   a field of view angle.

### Shaders And Programs

#### Annotations

### Logging

## UI Components

### Android

### Java Swing

### Multiplatform Compose

<video controls loop poster="/assets/video/glimpse_triangle_example_480.png">
  <source src="/assets/video/glimpse_triangle_example_480.mov" type="video/quicktime" />
  <source src="/assets/video/glimpse_triangle_example_480.webm" type="video/webm" />
  Your browser does not support HTML5 videos.
</video>

## Advanced Features

### Wavefront OBJ Files

### Offscreen Rendering

## API Docs

Visit [API Docs website][apidocs] to see the latest documentation.


[apidocs]: https://glimpse.graphics/docs/
[apidocs:GlimpseAdapter]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse/-glimpse-adapter/index.html
[apidocs:GlimpseCallback]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse/-glimpse-callback/index.html
[apidocs:core:types]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.types/index.html
[apidocs:Angle]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.types/-angle/index.html
[apidocs:Buffer]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.buffers/-buffer/index.html
[apidocs:FloatBufferData]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.buffers/-float-buffer-data/index.html
[apidocs:IntBufferData]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.buffers/-int-buffer-data/index.html
[apidocs:Mesh]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.meshes/-mesh/index.html
[apidocs:MeshData]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.meshes/-mesh-data/index.html
[apidocs:MeshDataBuilder]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.meshes/-mesh-data-builder/index.html
[apidocs:Model]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.models/-model/index.html
[apidocs:TextureImageSource]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.textures/-texture-image-source/index.html
[apidocs:TextureImageSourceBuilder]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.textures/-texture-image-source-builder/index.html
[apidocs:Texture]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.textures/-texture/index.html
[apidocs:Texture:Builder]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.textures/-texture/-builder/index.html
[apidocs:Camera]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.cameras/-camera/index.html
[apidocs:Lens]: https://glimpse.graphics/docs/v1.0.0/glimpse/core/core/graphics.glimpse.lenses/-lens/index.html

[opengl]: https://www.opengl.org/
[kotlinlang]: https://kotlinlang.org/
[apache-2]: http://www.apache.org/licenses/LICENSE-2.0

[wiki:pitch-yaw-roll]: https://en.wikipedia.org/wiki/Aircraft_principal_axes
