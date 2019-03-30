---
layout: post
title: What Does Being a Tech Lead Mean?
tags:
  - leadership
  - tech
  - tech lead
  - leading a team
  - high performance development
author: Dan Smith
date: '2019-03-29'
---

Lately I've been thinking about what it means to be a tech lead on a team, partly triggered by [this post](https://www.no.lol/2019-03-09-should-i-become-a-tech-lead/) by Lauren Tan. I've been a tech lead for a little over a year now and in that time I feel like I've done a decent job at it. But what can I do to help take my team to the "next level"? Right now I'm working on what questions I should be asking. A couple I've come up with so far are, 1) what makes a development team high-functioning?, and 2) how does that translate into actinable goals I can give my devs?

As to the first question, one of the measurables I look at are how much code can we get shipped. How many PRs can my team get merged and in production in a given amount of time. Not a perfect metric to be sure, but it should give us an idea of how much we're contributing. If we're continually shipping PRs on a regular cadence, that's a good sign things are going well.

Of course that's only part of the story since if we ship a bunch of shit and it doesn't work or is riddled with bugs, then we obviously are doing something wrong. So the other night when I couldn't sleep I started thinking about some aspirational and actionable goals we could use to ensure our team is one of the best.

## Writing Quality Software

I like this phrase because it touches on a few key points that are key to producing results. First, "Writing" says we're doing more than just coding/shipping. Writers take the time to put together something they want to share with the world. Then they edit it. And edit it some more. Then send it to an editor who makes further suggestions. Then it goes to a proofreader to make sure there aren't any spelling/gramatical errors. And there's probably a lot of in-between there that I don't know about since I've never written professionally.

Translating that into programming, we should be approaching our code in the same way a writer does a story. Asking does it do what it's supposed to is fundamental. But after that, does it do what it's supposed to in the best way possible? There should be editing going on (we call it refactoring) before anyone else gets involved. Then it goes to the editor/code reviewer. The job of the code reviewer is to think about and validate the approach, not just look for bugs. Finally, after all that has passed, it should go to proofreading/QA to find any mistakes that were missed. But above all, the end product should be readable. A writer may have the best story to tell but if no one can read it, then no one is going to put the time and effort into understanding the story.

Second, "Quality" points to a certain level of dedication to our craft where the code is as bug-free as possible. But as much as being bug-free, it should also be understandable and followable. Stealing from the writing analogy above, a story told with perfect grammar and spelling can still be difficult to follow if not executed properly. Plot points need to be in the correct order, characters have to be believable, and settings easy to visualize. In the same way, a piece of code can work correctly but if you have to jump between 12 different files to figure out what's going on, then maybe there's something amiss.

And "Software" is more than just code. We aren't writing code in a vacuum. It has to work well with the other code that's already there. We could crank out the most beautiful piece of code ever written, but if it doesn't do its job in the part of the whole it fits into, then it's useless. Even if it does its job but is fighting the rest of the system to do it, then it shouldn't be there. Any code written should fit with what's going on around it.

## Be the Best Team

This one sounds a bit more flighty, but the idea is to be the team that everyone else in the org points to and says "Have you seen that team? They know what they're doing!" This is less about an ego boost than it is building trust with other people that our team can and will accomplish the projects given it. And not just getting things done, but do them in a high quality way.

One of the highest compliments my team has gotten was when our QA person told me "I don't have to worry as much about your team's stuff since it's usually pretty solid." That's what I want, where we build trust from other people so that QA can focus on testing more scenarios and edge cases rather than nitpicking tiny things that we got wrong.

## Smaller PRs

And finally the actionable piece. Smaller PRs allow us to accomplish the previous two points by getting us to focus on small bits of the story at a time, make sure they're perfect, then ship them quickly. As the features develop, they will often have to change from the original implementation since all the pieces of software change, so the piece you shipped out 2 weeks ago may not make as much sense now that you have more context and have been working on this feature for a bit. But that's ok. Software is going to change over time. As long as we aren't starting from scratch and throwing away perfectly usable code, then our code becomes better from what we've learned. And those times where we do have to throw out code that doesn't pass muster makes the codebase stronger.

## Summary

Edit your code so that it is readable and works well with existing code before putting it in front of other people. And make smaller pull requests.
