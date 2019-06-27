---
title: SOLID - Building scalable software
date: "2019-06-27"
description: Building software is messy. Following certain principles and patterns can help remove some of the mess. SOLID is where to start.
---

If there's anything that I've realized over my time developing software, it's that all code is trash. Code is written by humans. Humans are smart (mostly) and don't write bad code on purpose. As we develop software, many times, the requirements change. As we iterate, it's natural that we find that our product is all together different from our stakeholder's expectations. So we deviate from the plan - which is okay, we're agile. But let me share a mind-shaking statistic. The average tenior of a software developer at any given company, is two years. This means at any given time, your colleagues are just as clueless about major design decisions as you (I'm not sure if that's terrifying or a relief). When we combine these two problems, with problem-solving techniques that vary from developer to developer, our codebase moves from the Mona Lisa, to spaghetti, to spaghetti that someone put in a blender then the dishwasher. Software is like any system. As a codebase ages, it naturally descends into [chaos](https://en.wikipedia.org/wiki/Chaos_theory). Not all hope is lost. If you implement a few design principles in your codebase, you can certainly alleviate these pains.

## Code Rot - How do you know you have a problem?

When something smells in your house, it's pretty easy to find the cause. You open the door and wrinkle your nose. After that first whiff, you walk to the kitchen. Sniffing twelve times a second, you try to identify the culprit. Is it the trash? No, it smells like old pizza but you've been immune to old pizza smell since college. Is it the sink? The fridge? Is there a rotten potato that fell into a hidden corner of your pantry? Whatever the case, finding the issue is usually harder than removing it. Sometimes our codebase smells the same. When a new developer clones the repo, what will she smell? Will there be a whiff of rotten spaghetti in the corner? Maybe even overwhelmed because she just walked into a binary garbage can? Or will she walk into a pristine system? Will your codebase still have that _new code smell_ in three or four years? Like a house, no codebases are pefect. Trust me, one of the primary reasons that my wife and I have friends over is because we'd never clean without them (I'm not sure if I'm joking here or not). So let's look at some of the ways that you can detect these smells in your code.

#### Rigidity

> Rigidity is the tendency for software to be difficult to change - Robert C. Martin

Does your codebase have files, folders, or classes that you just don't touch? Every change to one class or function thunders forth with an avalanche of bugs and defects. It may have been an easy fix, I mean - You did tell everyone at standup that it would only take a couple days.

## SOLID - Engineering for the future

Introduced by Robert C. Martin, the SOLID design principles are set of basic ideas that are meant to make your code scalable and maintainable. These are widely applied to the Object Oriented Programming paradigm, but the underlying principles have a place in every codebase - even an HTML/JQuery web component. The most basic ideas include things like - _don't make your class or function do things that it shouldn't_, and _don't depend too much on implementation detail of a class or function you can't change_. The ideas that I'll be presenting in this article, may include heavy tech jargon. I'll try to make it relatable, I swear.

#### (S)ingle Responsibility Principle

> Do one thing and do it well

When I was in college, I had a coffee maker. But I was really into coffee, so it was an expensive coffee maker. There was a deluxe steaming wand dangling off the side. It had a grinder up top and a drip tray below. It could dispense hot water into medium grind beans and I could attach the espresso head and in just a few minutes, have a hobbit's cup worth of Cafe Bustelo (the rich man's Lavazza).
