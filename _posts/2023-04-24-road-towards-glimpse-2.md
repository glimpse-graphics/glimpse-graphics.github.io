---
title: "Road Towards Glimpse 2"
mobileTitle: "Road Towards Glimpse 2"
layout: post
abstract: Plans for new major release 2.0.0 and the reasons behind it.
keywords: glimpse
author: sczerwinski
tags: [glimpse]
---

I decided the time has come to start working on the next major release
of Glimpse. It is a huge step for such a big project, maintained by a single
person. However, there are a few reasons why Glimpse 2.0 needs to come to life
pretty soon.

### OpenGL ES 3.1

One of the reasons behind Glimpse was a single OpenGL interface for both
desktop JVM and Android. Currently, the project no longer uses Java (it uses
Kotlin Multiplatform), but the philosophy behind it is still valid.

Using OpenGL ES 2.0 allowed for minimum API level 8 (Glimpse uses 14 due to
other reasons), which ensured maximum Android versions range, while still
providing shaders functionality.

But the times have changed. Android 5.0 (API level 21), which is the minimum
version supporting OpenGL ES 3.1, is now available on over 99% of Android
devices. Coincidentally, it is also the minimum version required by Jetpack
Compose. Nowadays, it is hard to find an ongoing Android project that would
target API levels before 21.

So why not harness the features provided by OpenGL ES 3.1 in Glimpse?

### Less Focus On Android

In the early days of the project, Android was the driving force behind
new features of Glimpse. As time flew by, however, I put more and more focus
on desktop JVM in my projects.

Does this mean that Android will no longer be supported? Absolutely no.
The vision behind Glimpse is do be a multiplatform framework. And I can
reassure that **Android will continue to be fully supported by Glimpse**.

Maintenance of Android codebase does not require much effort anyway. Android
code is limited to several core classes and a few platform-specific extensions.

However, Android will no longer be the main focus of the framework. Some new
complex and resource-consuming features can be implemented, although lower-end
Android devices might not be powerful enough to handle them.

### High-Level Features

The core scaffolding of Glimpse is pretty much solidified. It will undergo
changes, in order to harness OpenGL ES 3.1, but the overall structure will
stay similar.

This means more time being spent on higher-level features, which will
facilitate development of OpenGL applications.

### Breaking Changes In 1.3.0-SNAPSHOT

As I was working on new features planned for version 1.3.0, I realised that
I needed to introduce some breaking changes in the upcoming release. Core types
of the framework—angles, vectors, matrices, cameras, lenses—are becoming
generic.

Migration to the next version will require a lot of manual updates in the code.
I realised, these changes were hard to justify without a general major update.

### What Will Not Change

Glimpse is a free, open-source project, published under Apache 2.0 licence.
However, I will greatly appreciate even small contributions. If you would
like to support my efforts, you can do it via [GitHub Sponsors] or [Ko-fi].

Glimpse is still multiplatform. It will continuously support both desktop JVM
and Android. I would like to eventually extend supported platforms by iOS
and JS, but I can't promise anything.

There are currently no plans to use Vulkan. Maybe in Glimpse 3.0?


[GitHub Sponsors]: https://github.com/sponsors/sczerwinski
[Ko-fi]: https://ko-fi.com/sczerwinski
