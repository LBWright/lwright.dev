---
title: SOLID - Building scalable software
date: "2019-06-27"
description: Building software is messy. Following certain principles and patterns can help remove some of the mess. SOLID is where to start.
---

If there's anything that I've realized over my time developing software, it's that all code is trash. Code is written by humans. Humans are smart (mostly) and don't write bad code on purpose. As we develop software, many times, the requirements change. As we iterate, it's natural that we find that our product is all together different from our stakeholder's expectations. So we deviate from the plan - which is okay, we're agile. But let me share a mind-shaking statistic. The average tenior of a software developer at any given company, is two years. This means at any given time, your colleagues are just as clueless about major design decisions as you (I'm not sure if that's terrifying or a relief). When we combine these two problems, with problem-solving techniques that vary from developer to developer, our codebase moves from the Mona Lisa, to spaghetti, to spaghetti that someone put in a blender then the dishwasher. Software is like any system. As a codebase ages, it naturally descends into [chaos](https://en.wikipedia.org/wiki/Chaos_theory). Not all hope is lost. If you implement a few design principles in your codebase, you can certainly alleviate these pains.

## Code Rot - How do you know you have a problem?

When something smells in your house, it's pretty easy to find the cause. You open the door and wrinkle your nose. After that first whiff, you walk to the kitchen. Sniffing twelve times a second, you try to identify the culprit. Is it the trash? No, it smells like old pizza but you've been immune to old pizza smell since college. Is it the sink? The fridge? Is there a rotten potato that fell into a hidden corner of your pantry? Whatever the case, finding the issue is usually harder than removing it. Sometimes our codebase smells the same. When a new developer clones the repo, what will she smell? Will there be a whiff of rotten spaghetti in the corner? Maybe even overwhelmed because she just walked into a binary garbage can? Or will she walk into a pristine system? Will your codebase still have that _new code smell_ in three or four years? Like a house, no codebases are pefect. Trust me, one of the primary reasons that my wife and I have friends over is because we'd never clean without them (I'm not sure if I'm joking here or not). So let's look at some of the ways that you can detect these smells in your code.

#### Rigidity

> Rigidity is the tendency for software to be difficult to change - Robert C. Martin (RCM)

Does your codebase have files, folders, or classes that you just don't touch? Every change to one class or function thunders forth with an avalanche of bugs and defects. It may have been an easy fix, I mean - You did tell everyone at standup that it would only take a couple days. Small modules turn into behemoths and simple changes to marathons. When software behaves this way, managers fear to allow engineers to fix non-critical problems. But even if managers let their devs fix those issues, the codebase can turn into a roach motel. Engineers may check in but don't check out.

When these fears become so accute that the team refuses to allow changes to software, rigidity sets in. We have officially moved from design defficiency to issue aversion.

#### Fragility

> Fragility is the tendency of the software to break in many places every time it is changed. - RCM

It seems like we hear it all the time, "I'm not sure how this is broken, I didn't touch anything over here". As time moves on, the more fragile the codebase becomes. Every fix only seems to break something else, or introduces new issues. The team is tired of fixing each other's bugs and the code review becomes much more intensive. Developers lose credibility. Managers pull their hair out. Releases get pushed back.

#### Immobility

> Immobility is the inability to reuse software from other projects or from parts of the same project - RCM

We've written the same thing over and over again but it never quite satisfies the requirements of this project. We want to build reusable tool but just don't have the time. So we extend and mangle and finagle previously written code into our new system but it has too much baggage to be dependable. Or we find that it's too dependent on another chunk of code. After a few weeks of trying to make it work, we simply just rewrite it.

#### Viscosity

This comes in two flavors: design and environment. When _design_ is viscous, changes to the codebase are slow and executing minute tasks are tedious. It's not clear what variables are named so it takes a while to understand code for maintainence. Maybe modules are hidden. Or a too strict style guide is implemented. Awkward file structures can even lead to design viscosity. The _environment_ is viscous when the reflected changes take ages to be reflected in the codebase. Sometimes this happens through poor code review or a lack of collaboration. Sometimes developers may have to copy files from one directory to another, then run a series of manual build commands to see a change locally. Long compile times may also result in a viscous environment.

These four symptoms are the red flags of poor architecture. When our applications suffer from one (or several) of them, we know that it's a matter of time before they rot into obscurity. As mentioned before, these aren't typically caused by idiots or monkeys typing code. There are a whole host of reasons why a system degrades. By no means do I mean to lash the individuals responsible for powering the future. While these problems may be introduced unknowingly or by the changing landscape of our development environments, it is up to the developers - the command-line heroes - to make the world right again. But how?

## SOLID - Engineering for the future

Introduced by Robert C. Martin, the SOLID design principles are set of basic ideas that are meant to make your code scalable and maintainable. These are widely applied to the Object Oriented Programming paradigm, but the underlying principles have a place in every codebase - even an HTML/JQuery web component. The most basic ideas include things like - _don't make your class or function do things that it shouldn't_, and _don't depend too much on implementation detail of a class or function you can't change_. The ideas that I'll be presenting in this article, may include heavy tech jargon. I'll try to make it relatable, I swear.

#### (S)ingle Responsibility Principle

> Do one thing and do it well

When I was in college, I had a coffee maker. But I was really into coffee, so it was an expensive coffee maker. There was a deluxe steaming wand dangling off the side. It had a grinder up top and a drip tray below. It could dispense hot water into medium grind beans and I could attach the espresso head and in just a few minutes, have a hobbit's cup worth of Cafe Bustelo (the rich man's Lavazza).
