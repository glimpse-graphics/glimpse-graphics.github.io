---
title: "Glimpse: Reactivation"
mobileTitle: "Glimpse: Reactivation"
layout: post
abstract: |
  Glimpse development starts again, this time as a Kotlin
  multiplatform project.
keywords: glimpse
author: sczerwinski
tags: glimpse
image: 2021-01-04-glimpse-reactivation.png
---

I started Glimpse 5 years ago, as an attempt to simplify the use of OpenGL.
The very first version was written in Java, and it provided a unified
object-oriented interface for both Android and desktop Swing applications.

While Kotlin was becoming more and more popular, I decided to rewrite
the entire Glimpse codebase in the new language, using a mixed OOP and FP
approach. The attempt was much nicer to use, yet still art for art’s sake
in many ways.

I had too many plans for improvements and further development of the project,
and… for the next 4 years, I didn’t touch Glimpse at all. Maybe because
I didn’t have time, maybe because I didn’t need it at the moment, and maybe
because I didn’t really believe any of my ideas on how to proceed was
actually good.

So why did I “resurrect” the project now?

I’ve been working on Glimpse over the last few months, although you will
not see any commits in the repository until recently. I developed it as
a part of another project, and that allowed me to take a more pragmatic
approach to the library. I got rid of a lot of fancy code, and replaced it
with what actually worked, and was useful.

I’ve also been looking into some new technologies that helped me better
organise my code, and made some elements of the library obsolete.
To mention just a few:
* Glimpse is now a [Kotlin multiplatform project][kotlin_multiplatform],
  maintained in a single GitHub repository.
* I replaced [Travis CI][travis_ci] with much more powerful
  [GitHub Actions][gh_actions], so I can focus on the coding instead of
  maintaining releases and documentation.
* Thanks to [Jetpack Compose][jetpack_compose] on Android and
  [Compose for Desktop][jetbrains_compose], a bleeding edge library from
  Jetbrains, there is no need for building a UI framework from scratch.

I’m pretty confident, I’m finally going in the right direction.
I can already see first stable release of Glimpse on the horizon.


[kotlin_multiplatform]: https://kotlinlang.org/docs/reference/multiplatform.html
[travis_ci]: https://travis-ci.org/
[gh_actions]: https://github.com/features/actions
[jetpack_compose]: https://developer.android.com/jetpack/compose
[jetbrains_compose]: https://www.jetbrains.com/lp/compose/
