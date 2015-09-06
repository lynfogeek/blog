---
layout: post
title: Open Sourcing the Droidcon Netherlands app
categories: [General, Android, Apps, Open Source]
tags: [androiddev, open source, Droidcon]
fullview: false
comments: true
---

[Droidcon](http://droidcon.com/) is a global developer conference series which takes place every year in a dozen of different cities around the world. The quality of the talks is really impressive, every time I go to one of these events I learn a lot on various technical and design points.

This year, I had the chance to make the conference app for the Dutch edition. [Droidcon NL](http://www.droidcon.nl/) took place in Amsterdam between the 23rd and 25th of November.

## The application

The goal of this app was not to be the best conference app ever but a simple and easy to use application. I chose to follow the [Material Design guidelines](http://www.google.com/design/spec/) which give a really nice look & feel, and allows doing some nice effects really efficiently.

The app lists the schedule in the home screen, and you can get more information for each conference, such as the description, location and speaker. I also implemented a simple [Floating Action Button](http://www.google.com/design/spec/components/buttons.html#buttons-floating-action-button) to favorite a talk, this simply highlights it on the main screen.

![The App]({{ site.url }}/assets/media/screenshot_droiconnl.png)

## The source code

During the event, I talked to many people about the app, and besides some tips for improvement, few developers asked me if the source code will be available. So I decided to clean it, refactor a few classes and here we go. [You can browse / fork / download the source code from my github.](https://github.com/lynfogeek/droidconNL-2014) It's released under [Apache v2 license](https://github.com/lynfogeek/droidconNL-2014/blob/master/LICENSE).

## The app

If you just want to try it, [the application is still available on the Google Play](appbox-octopress), maybe I will update it for next year ;)
