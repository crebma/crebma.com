---
layout: post
title: "It's OK to Cheat Sometimes!"
date: 2013-07-06 12:54
comments: true
categories: [legacy, tdd, refactor, ios]
---

Lots of people have strong opinions about what you should and shouldn't do in testing. For the most part, those rules tend to be right. But deep in the heart of a huge legacy codebase, striving for tdd perfection is often a self-defeating goal. Should you be on a path to refactor and test that codebase towards that perfect tdd goal? Of course! But when you first inherit an enormous untested codebase, or are new to tdd and want to start testing your codebase, it is perfectly ok to take some shortcuts. Here are a couple of sticking points I've seen people hit and then give up entirely.

**DISCLAIMER:** These all assume Kiwi use, because that's all I've used so far. I'm sure there's an easy translation, so long as your chosen testing framework has a robust mocking/stubbing framework in it.


#### "I have business logic in viewWillAppear, and it's heavily tangled with the rest of my controller!"
Spent the last two hours trying to untangle that code so you can extract it to one or more collaborators? It's ok. That's a good goal that should be met, but it doesn't make sense to string out several days and possibly create several bugs in the process of refactoring untested code. It can be worthwhile to "boy scout" that code little by little. What that means is that you leave that code a little better than you found it. Perhaps not sparkling clean, but a little better every time you touch it. A shortcut you can use to start heading in that direction is to extract everything that's not [super viewWillAppear] into a separate method, or even just the logical block that contains the logic you are touching. In your spec, use a class extension to declare that method in a place where your specs can see them, but the rest of your production code cannot. Now you can write characterization tests around that private method that will help build a foundation for safe future refactoring.


#### "This controller uses ivars all over the place that I can't modify!"
This one is not a shortcut, but it is an easy fix. Convert that ivar to a property. If it's a private ivar, here's where the same afore mentioned shortcut comes in: declare that property in a class extension in your specs.


#### "This chunk of code makes a UIAlertView but I'm not actually touching that code so I don't want to deal with it!"
Stub the alloc/init methods. I know, it's hard to hear. You might even be wincing right now. But seriously. Factory type methods might be better, but if you've already chosen a different spot in that chunk of code to boy scout, it can be helpful to stub so as not to invoke UI code.

Again, I just want to reiterate that these should be used as stop gap measures. You know you're doing it right (and should feel AWESOME) as soon as you can remove these dirty little workarounds. But iOS code had been around long enough now that legacy codebases not only exist, but have grown into unmaintainable masses.