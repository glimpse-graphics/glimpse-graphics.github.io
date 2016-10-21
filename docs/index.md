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

## 1. Project Setup

### 1.1 Desktop Applications

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

### 1.2 Android Applications

## 2. Basic API

### 2.1 Points

### 2.2 Vectors

### 2.3 Matrices

### 2.4 Angles

### 2.5 Colors

### 2.6 Texture Coordinates

### 2.7 Buffers

### 2.8 Resources

## 3. Constructing A Scene

### 3.1 Basic Configuration

### 3.2 Meshes

#### 3.2.1 Vertices And Faces

#### 3.2.2 Curves

#### 3.2.3 Polygons

#### 3.2.4 Prisms

#### 3.2.5 Surfaces of Revolution

#### 3.2.6 Platonic Solids

#### 3.2.7 OBJ Files

### 3.3 Models

#### 3.3.1 Transformations

### 3.4 Materials

#### 3.4.1 Shaders

#### 3.4.2 Textures

#### 3.4.3 Predefined Materials

### 3.5 Lights

#### 3.5.1 Direction Lights

#### 3.5.2 Point Lights

#### 3.5.3 Spotlights

### 3.6 Cameras

#### 3.6.1 Camera Projection

#### 3.6.2 Camera Position

### 3.7 Lights, Camera, Action

## 4. Desktop Applications

### 4.1 Glimpse Frame

### 4.2 Rendering

### 4.3 Menus

### 4.4 Input

#### 4.4.1 Mouse

## 5. Android Applications

### 5.1 Glimpse View

### 5.2 Assets And Resources

### 5.3 Activity

## 6. Advanced Concepts

### 6.1 GLES Facade

### 6.2 Disposables

### 6.3 Custom Implementations
