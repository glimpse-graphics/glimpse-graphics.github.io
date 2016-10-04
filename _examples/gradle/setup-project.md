---
title: Setup Gradle project
mobileTitle: Setup Gradle project
layout: default
abstract: Setup Glimpse Framework in a Gradle project
keywords: glimpse,framework,opengl,3d,gradle,setup
---

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
