---
title: Setup Gradle project
mobileTitle: Setup Gradle project
subtitle: for desktop applications
layout: post
abstract: Setup Glimpse Framework in a Gradle project
keywords: glimpse,framework,opengl,3d,gradle,setup
---

**Latest Glimpse Framework release:** {{ site.glimpse_version }}

**Minimum Kotlin version:** {{ site.kotlin_version }}

Desktop applications require following dependencies:

* `org.glimpseframework:glimpse-framework-api`—Glimpse Framework API
* `org.glimpseframework:glimpse-framework-jogl`—JOGL implementation of Glimpse Framework
* `org.glimpseframework:glimpse-framework-materials`—Standard materials to be used with Glimpse Framework

## Minimum Gradle configuration

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
