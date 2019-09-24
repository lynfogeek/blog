---
layout: post
title: "A Tale of Two Platforms: Keeping Android & iOS in Sync"
categories: [Android, iOS, Video]
tags: [androiddev, conference, iosdev]
fullview: false
comments: true
---

Over its 4 first years, Impraise’s mobile applications have grown with various updates, refactoring and feature changes. While being a young startup allowed us to “move fast and break things”, we grew to a stage where we had to invest more on testing, stability and future proof implementations.
We evaluated the pros and cons of various architectures & frameworks, for both our Android and iOS applications.

Our goal was to find an Architecture:
– Easily testable and maintainable
– Easily adaptable to Android and iOS
– Adoptable without massive refactoring needed
– With an unidirectional flow of events

We adopted `Clean Swift` as a base for our iOS architecture, and adapted it to Android. In this talk, I share with Guilherme, our lead mobile developer, the similarities and differences of our implementations, as well as their advantages and drawbacks.


<center>
<iframe src="https://player.vimeo.com/video/267250251" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
</center>

The video is hosted on <a href="https://vimeo.com/267250251">Vimeo</a> by <a href="https://vimeo.com/appdevcon">Appdevcon</a> .
