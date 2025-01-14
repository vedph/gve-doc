---
title: User Experience
layout: default
parent: Home
nav_order: 2
---

# User Experience

This section of the documentation refers to the frontend GUI designed and implemented to provide a user-friendly, collaborative and web-based content creation system for data shaped according to our [model](../model/index).

In general, given the complexity of a genetic model, I tried to imagine an easy UX which mostly mimicks the traditional process of a scholar in editing an autograph text.

When faced with an annotated text like this, the first approach is right trying to make a sense of it: we start from the text, and then explore all the annotations stemming from it, as represented by a combination of visual hints and additional portions of text. In most cases, such annotations represent changes of the start text, in terms of editing operations: the author (or anyone else acting on the text) deletes something, or adds something, or moves something around. This process accumulates many changes on the text, until it reaches what the scholar interprets as a target version.

Of course, this is not necessarily a linear process; it may well happen that there are many different lineages stemming from the start text, and that annotations are added on top of other annotations. In such cases our line branches into many different lines, like in a tree.

💡 So, ideally we can imagine to implement a description of this process in these **steps**:

1. define the _text_ to start with, which is our "base text".
2. describe each single _editing operation_ directly or indirectly performed on it. In the simplest case, we have a linear progression where each operation's output is the input to the next operation; but we may well start from the output of any of the previous operations in our reconstructed sequence, thus producing various branches in a tree-shaped structure.
3. optionally, also describe the _visuals_ corresponding to each operation. For instance, a slash on top of a letter meaning its delection, a V-like sign with some letters above it meaning their insertion in a specific place, etc.
4. optionally, define _animation timelines_ for presenting your visuals for any specific versions.

>You can look at this procedure this very simple [real-text example](../model/limerick). Also, the [more abstract example](../model/snapshot-sample) for a [snapshot](../model/snapshot) contains the description of all its data parts.
