---
title: Model
layout: default
parent: Home
nav_order: 1
---

# Model - Introduction

- 🎥 [video](https://www.youtube.com/watch?v=6fdIUpomJZw)

Let us start by introducing the essential terminology of the digital model, while describing our edition's scenario.

Goethe's Venetian Epigrams (here VE for short) are a very peculiar work, attested by a moltitude of epigrams scattered and often repeated, with or without variations, across multiple supports, here named 🔖 **text carriers**, whatever their material reality (notebooks, single sheets, letters, etc.). Given this situation, there is no possible notion of the text of each epigram; we rather have a constellation of texts, without any authoritative reference edition.

When dealing with autograph manuscripts, which attest the evolution of a text mostly by the hand of the author himself, the starting point is of course the text carriers, our material evidence. Each carrier is a document with the potential of representing multiple ver­sions of a text, as it contains _annota­tions_ which imply _chan­ges_ to it. In fact, graphically each text on a carrier does not just represent a text, but it includes many corrections, rethinkings, or refactorings of it, in the form of various types of 🔖 **annotations** (deletions, insertions, transpositions, side notes, etc.) on top of a text.

Such a document is thus a sort of picture taken in relation to the text being composed at a given point in time; this is why we name the digital model of this document a 🔖 **snapshot**. This does not necessarily imply that this is the end stage of the creative process underlying the text. The process may well have gone further on; but this is not reflected by the snapshot, which just represents a picture which froze an instant of this process at a given time.

The snapshot is thus far from being the representation of an undisputed linear text; it's an even chaotic bunch of text and annotations on some carrier. It is up to the reader to make sense of these annotations, and try to deduce one or more versions of the text from his interpretation.

These annotations represent changes on the text; but there often is no way to know their relative order, or even their selection when they represent several competing versions of the text. _It is like having all the ingredients_ (our text with annotations), _when we have lost the recipe_ (how to select and order the annotations to generate the intended version(s)).

When for whatever reason an authoritative version of the final outcome of all these operations on the text is lacking, in a traditional (paper) edition we might be forced to forge one, representing the best effort in interpreting our carrier's content. Of course, that would just be a best effort; and it might even be the case that we are not sure in deciding among competing interpretations. In the end, we come up with our best recipe.

In a digital edition instead, we have the power to handle all the rich and complex data preserved by our snapshot, without resorting to compromises. We can represent all the ingredients (text and annotations), all the recipes (annotation selection and ordering), and all their outcomes (resulting versions), while still keeping each as a separate layer, so that everyone can evaluate our text reconstruction, and even provide his own alternatives.

In the case of VE, the situation is even more complex, because:

- various carriers contain multiple, often different versions of each epigram.
- in turn, each epigram version, when attested by such a manuscript, potentially contains multiple versions of itself, virtually attested by the annotations which hint at changes in it.

In a traditional approach we tend to adapt philological methods and tools based on the comparison of multiple witnesses and usually targeted to the reconstruction of a critical text. In this case, the multiple texts are functionally equivalent to those witnesses, and markup is used to "align" them via a systematic comparison. The product of this comparison is a sort of diffing result, where for each version we can tell what has been added or replaced, and thus represent the transformations of a text over time.

While this is perfectly fine, and works in most cases, in our scenario this is difficult, both for the higher scale of complexity and for the conceptual nature of a representation which is more static than dynamic. In our scenario, a traditional, comparison-based approach among static documents is more difficult to implement, requiring a deep annotation, and harder to scale, especially where versions are multiplied twice, like in VE. In a similar approach, the transformation process of the text emerges from the results of comparisons, properly encoded with markup. Here instead we somehow reverse the strategy, focusing on the cause of these text versions, the process which created them, rather than on aligning its effects for comparison. We focus on the text-making process, which generates all the versions we virtually find in each snapshot, multiplied by all the epigram versions scattered across our carriers.

A process essentially being a sequence of actions, we can model our text making process as a sequence of editing 🔖 **operations**, like for instance adding, deleting, replacing, moving, or just annotating text. This process is usually linear: it starts with a 🔖 **"base" text**, which simply is the input of the first operation, and transforms it operation after operation, accumulating changes. The effect of this time-driven thread is thus the generation of a progressive alteration of the base text.

Along the path defined by this often long sequence of operations, only some of them can be assumed to be representative of a specific stage in the alteration of the text. For instance, we might find that a single hand, clearly recognizable from a distinct ink, applies a set of changes, like adding a comma, turning a semicolon into a dot, and replacing a word with another one. These are 3 operations, each having as input the output of the previous one (except for the first one, whose input is the base text); yet, it is only once we consider all these operations together that we can fully represent the sum of alterations which generated the stage associated with that hand.

In other terms, given that each operation produces a different version of the previous text, we can say that not all these versions are eligible to represent a sort of waypoint along the transformation path, i.e. a specific 🔖 **alteration stage**. So, only some of these operation-generated versions are the terminal point of an alteration stage.

In the digital model, the snapshot is thus essentially the sum of the _base text_ and the _operations_ transforming it. These are enough to let the system automatically generate all the versions, either staged or not, along the transformation path, together with all the annotations we might want to attach to portions of their text or to the versions as a whole.
