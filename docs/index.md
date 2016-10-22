---
title: Glimpse Framework Documentation
linkTitle: Documentation
layout: toc
abstract: Glimpse Framework Documentation
keywords: docs,documentation,glimpse,framework,opengl,3d
---

## Introduction

Glimpse Framework is an open-source project aimed to make [OpenGL](https://www.opengl.org/) simple.

The source code is written in [Kotlin](https://kotlinlang.org/) and it is intended to be used with Kotlin.

* **Latest Glimpse Framework release:** {{ site.glimpse_version }}
* **Required Kotlin version:** {{ site.kotlin_version }}

Glimpse Framework is distributed under [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

## Project Setup

### Desktop Applications

Desktop applications require at least following dependencies:

* `org.glimpseframework:glimpse-framework-api`—Glimpse Framework API
* `org.glimpseframework:glimpse-framework-jogl`—JOGL implementation of Glimpse Framework
* `org.glimpseframework:glimpse-framework-materials`—Standard materials to be used with Glimpse Framework

```gradle
buildscript {
	ext.kotlin_version = '{{ site.kotlin_version }}'
	ext.glimpse_version = '{{ site.glimpse_version }}'

	repositories {
		jcenter()
	}

	dependencies {
		classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:${kotlin_version}"
	}
}

apply plugin: 'kotlin'

repositories {
	jcenter()
}

dependencies {
	compile "org.glimpseframework:glimpse-framework-api:${glimpse_version}"
	compile "org.glimpseframework:glimpse-framework-jogl:${glimpse_version}"
	compile "org.glimpseframework:glimpse-framework-materials:${glimpse_version}"
}
```

### Android Applications

## Basic API

### Points

### Vectors

### Matrices

### Angles

### Colors

### Texture Coordinates

### Buffers

### Resources

## Constructing A Scene

### Basic Configuration

### Meshes

#### Vertices And Faces

#### Curves

#### Polygons

#### Prisms

#### Surfaces of Revolution

#### Platonic Solids

#### OBJ Files

### Models

#### Transformations

### Materials

#### Shaders

#### Textures

#### Predefined Materials

### Lights

#### Direction Lights

#### Point Lights

#### Spotlights

### Cameras

#### Camera Projection

#### Camera Position

### Lights, Camera, Action

## Desktop Applications

### Glimpse Frame

### Rendering

### Menus

### Input

#### Mouse

## Android Applications

### Glimpse View

### Assets And Resources

### Activity

## Advanced Concepts

### GLES Facade

### Disposables

### Custom Implementations
