---
layout: post
title: Heroku as a Server
date: '2013-04-02T10:32:00.002-04:00'
author: Daniel Smith
tags:
---

When I originally started learning Rails, one of the things I ran across early on was <a href="http://www.heroku.com/" target="_blank">Heroku</a>. It is a service that functions similar to git from the command line, allowing easy deployment of an app to their service. I signed up for an account but didn't really use it that much for my actual development. However, recently I have been having a difficult time getting the proxy module in Apache to work like I need it to.<br /><br />(My setup has two virtual servers running on my main server, with all of my sites directed to the one address. The main Apache server should then recognize which site is trying to be accessed, and forward to the appropriate server. That has not been happening and all of my sites are going directly to the main one.)<br /><br />So I tried giving Heroku a shot again. I must say it was very straight forward and took me maybe 15 minutes to get everything up and running, and most of that was just redirecting my <a href="http://invoiceapp.getabetterpic.com/" target="_blank">invoiceapp</a> site to Heroku's servers. So now I have the main site I needed (just an issue tracker for stuff at work), and my project site is also up and running.<br /><br />One of the cool things about Heroku is it is free to host for smaller projects like this one. They charge based on usage levels, so a small personal project isn't going to use enough resources to even round up to a penny.
