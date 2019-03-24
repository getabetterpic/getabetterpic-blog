---
layout: post
title: Rocket Flight Recorder
tags:
  - mobile
  - android
  - firefox os
  - cordova
  - ember
author: Daniel Smith
published: true
date: '2015-05-17'
---

One of the side projects I've been working on lately is a mobile app for recording info about a rocket flight. The original idea was to create something that would record GPS coordinates throughout the flight, let me know how high the rocket went, and, ultimately, send me a text with the current GPS coordinates when the rocket landed.

So far I've gotten the GPS working, along with the accelerometer data, and displaying that to the screen. I'm also going to be recording the flight on video, so it also launches the camera.

I've also decided that I want to output the GPS coordinates and altitude readings to a file for post-processing until I can decide exactly what I want to do with it in- and after-flight. I originally was thinking of outputting CSV, but decided it would be easier to output JSON since Ember Data can output JSON natively (and I don't have to format it).

Speaking of Ember Data, I'm currently using the LocalForage adapter for it, which utilizes IndexedDB and WebDB to persist data. Since these are web technologies, Cordova supports them. The technologies I'm using to build this is Cordova and Ember, coupled together with the ember-cli-cordova addon for Ember-CLI
