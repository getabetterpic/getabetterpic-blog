---
layout: post
title: React from an Angular Dev Perspective
tags:
  - react
  - angular
author: Daniel Smith
published: true
date: '2019-01-05'
---

I've been working with Angular for over a year now, and worked with AngularJS before that. Lately I've been trying out the other major JS frameworks, React and Vue. These are my thoughts on React so far.

First off, why do I have to `.bind(this)` on everything?!? That's my biggest annoyance with React. Coming from Angular/TypeScript land, you simply define a method on the component class and it just works. No `.bind(this)` in the constructor, or creating special `foo = () => 'something'` functions. You just build classes like they were meant to be and Angular handles the scoping of `this` for you.

And I'm still not sold on JSX. Maybe it's because I've worked exclusively with View/Controller paradigms (I worked with Ember before switching to Angular) but I like my HTML to be in a separate section. I do like Vue's approach where you have your `<template>` section separate from your `<script>` section even though they live in the same file.

So what do I like about React? I like the ability to create a simple component using just a function. So nice! And I _think_ I like CSS-in-JavaScript even though it feels a bit of a bastardization of CSS. I feel bad for CSS, it gets a bad rap even though it's a very capable language in its own right.

I also really like the TypeScript support for React and the type checking it makes possible in the template. If you're gonna have JSX, at least it gives you types for stuff in your templates.

React is obviously a very popular library, just not sure if I'd pick it for a new project. Unless it was a mobile project, the React Native stuff is top notch.
