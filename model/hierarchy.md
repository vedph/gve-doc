---
title: Entities Hierarchy
layout: default
parent: Model
nav_order: 5
---

- [Entities Hierarchy](#entities-hierarchy)
  - [Overview](#overview)
  - [Cadmus for VEdition](#cadmus-for-vedition)
    - [Items](#items)
      - [Snapshot](#snapshot)
      - [Carrier](#carrier)
      - [Epigram Version and Lost Lines](#epigram-version-and-lost-lines)
      - [Epigram](#epigram)
      - [Collection](#collection)
      - [Parts Matrix](#parts-matrix)

# Entities Hierarchy

The snapshot is just the lowest level entity in our hierarchy. Being closest to the material supports, and representing a highly complex multiple-versions text, it's the most complex one. Yet, above it there are other entities we want to represent in our data.

> ⚠️ This document reflects an early stage of the modeling stage for those entities, representing the base for a Cadmus-based editor.

## Overview

The diagram below summarizes the main entities in this project:

![alt text](img/entities.png)

Starting from the material side (top of the diagram), everything starts with our **carriers**. A carrier is defined as "the concrete, physical object that contains, among other things, the textual witnesses and is then digitally reproduced and described in the edition". In the diagram I am representing two notebook carriers, named H55 and H54.

These notebooks contain many texts. In the diagram we focus on a single epigram, which happens to be found in both these notebooks. At the lowest level of our model hierarchy there are the [snapshots](textual), a highly structured and computable digital model representing the **textual situation** specific to that carrier. The "textual situation" is defined as "the transmitted sum of all variants of textual versions on a text carrier". The snapshot is not just a container of variations of a text, but rather a compact model capable of generating all of them with their metadata, via operations which represent the scholars' interpretation of the annotations chaotically scattered on the carrier. As such, it can also be used to animate the history of the making of this specific text across all its supposed stages, as hinted by all the annotations on the carrier.

So, a carrier will usually contain multiple snapshots, each referred to a specific text. Typically, scholarly critical judgement usually ends up defining a final **epigram version** from each snapshot. Let us provide a less abstract example: the below picture was taken from a spreadsheet collecting all the epigram versions from all the carriers:

![alt text](img/xls-epigrams.png)

As you can see, each column starting from C corresponds to a carrier (H55, M1, M2, H54, etc.). All the epigram versions present in various carriers are aligned on the same row: for instance, at row 14 what can be considered the same epigram is attested by two different epigram versions, one in H55 and another in H54, both with _incipit_ "In dem engsten der".

Now, the very fact that we are aligning the same texts in the same row, and assigning them a shared numeric identifier, implies that we are recognizing them as instances of the same class, the abstract **epigram**. The epigram is defined as "all textual versions that are “related to each other through textual identity and distinguishable through textual variance” (GGA 224)". In our model, the "epigram" is thus an abstraction, which is conveniently used to hold metadata shared among all the epigrams: it corresponds to the row in the spreadsheet table, which is identified by your epigram identifier. "Real" epigrams (=epigram versions) are in columns; but they are all lined up in the same row because we consider all these texts as alternatives of a single unit. This unit is our abstraction, the primary reason for lining them up in the same row; so all what we say about the row is meant to apply to all the columns in it.

So, looking again at our diagram, each snapshot is connected to an epigram version, which is the text critically reconstructed as the final stage of the creative process detailed by the snapshot. This is the second (mid) layer in our diagram.

In turn, multiple epigram versions are instances of an abstraction, the epigram, which appears in yet another layer (bottom). In our diagram example, the two epigram versions from snapshots in carriers H55 and H54 are linked to a single epigram. It should be noticed that in this project, due to the peculiar nature of Venetian Epigrams, the epigram has no text at all: it is just an abstract container for metadata shared among epigram versions, each arising from a different carrier.

Finally, on this abstract layer we find a **collection**. This is a more specialized IT term fit to the entity represented in the digital model, and essentially corresponding to what is defined as an **order or sequence** ("the arrangement of all textual witnesses as found on a textual carrier"[^1]). The term "collection" here is used with a purely IT sense, meaning any number of items belonging to an _ordered_ set. What corresponds to this collection may vary: it might just be an idea of the author for organizing some epigrams, derived from what he writes about them elsewhere; or it might be what emerges from marks added to the notebooks (e.g. numbers) for each epigram, hinting at some plan for building an ordered collection of them; or something material, like the physical sequence in which they appear. Whatever the specific nature, we adopt a single, more abstract model for them.

⚙️ A collection contains texts in a given order. The order is represented by a sequence. From a purely IT standpoint, a _collection_ is an abstract data type that groups multiple data elements, which can be of the same or different types. That's the most generic, umbrella term and can include sets, lists, arrays, etc. A "set" is a specialization which differs because its elements are all unique. Among collections, some are ordered: these are "sequence", "list", and "array". A sequence can be finite or infinite (e.g. the sequence of numbers). Lists and arrays are both ordered, but lists are dynamically resized, while arrays are static but more memory-efficient. So, "collection" is more abstract: it's a conceptual container that doesn't enforce rules about order, uniqueness, or mutability. It's like saying "a vehicle" without specifying whether it's a car, bike, or spaceship. Instead:

- a set is a collection with constraints: no duplicates, no order.
- a sequence is a collection with structure: order matters, duplicates allowed.
- a list is a sequence with flexibility (dynamic size).
- an array is a sequence with rigidity (fixed size).

In this generic model, a "collection" is not necessarily ordered; or it might be ordered in multiple ways. From the point of view of a modular architecture, this means separating these two notions: the collection is the overarching entity, and it can sort its items in no way (when not sorted), or in just 1 way, or in multiple ways. This implies that the model for zero or more ordering's assigned to the items of a collection will be repeated for each different order. This ensures we have a consistent model without redundancies (i.e. repetitions), and it is also the practical reason for which the collection is a Cadmus item, while sequences are parts of that item, even if the details of this implementation are still to be defined. So, we can say that those items belong to a collection, and say this only once; then, we can add that this collection can be ordered in zero or more different sequences. Anyway, this can be easily regarded as a modeling detail. We can adopt a synecdoche and name these collections "sequences": from the user's point of view, nothing changes. Yet, internally we distinguish between the collection in the technical sense from how we can arrange items in it.

## Cadmus for VEdition

Given that our modeling is highly complex, though necessarily open to changes, especially at this early stage; and that we need a quick and effective infrastructure to lean our snapshot on, we are going to adopt [Cadmus](https://vedph.github.io/cadmus-doc) to represent all our entities in a single database, with a uniform data architecture.

Essentially, you can think of Cadmus records (called _items_) as boxes, where you can put any number and type of objects (called _parts_). Each of these objects has its own self-contained model, and is typically designed for reuse, so that you can put the same type of objects in many different boxes. For instance, an object representing a structured datation (with all the nuances for years, months, days, centuries, termini ante and post, etc.) can be put into any box representing an item which requires a date. The data model is thus dynamic and built by composition, and so is the web-based editor corresponding to it: the model of a box is just its content, which varies whenever a new object is put into it or an existing one is removed from it. This modularity allows for highly structured and scalable dynamic models, which fit to a lot of different scenarios, including text with all its annotations, whatever their complexity.

In fact, text in Cadmus is just an object, like any other datum, and so are its annotations. For instance, if you have a text with a critical apparatus, a comment, and paleographic annotations, you might have a box with an object for the plain text; an object for the apparatus annotations; another one for the comments; and yet another one for paleographic annotations. Each of these objects has its own model, so that annotating a text essentially means linking an object of any type to a specific portion of it.

This produces a sort of layered annotation system, where each layer contains a set of annotations belonging to a specific knowledge domain, and thus having its own model. For highly complex or highly frequent annotations this has many benefits:

- it allows for a highly _scalable_ scenario, where you can add as many annotations (layers) as you want, without affecting the existing text and its other annotations. You just add another annotation to the layer object, or a new layer object for annotations belonging to a different knowledge domain, without having to change neither the text or its existing annotations. This is not true for annotations systems like XML, where a single tree-based structure holds all the metadata attached to portions of the text; there, adding many heterogeneous structures on top of it means struggling to tackle a complex game of interlocking pieces to build a tree with all the required tags woven together. Additionally, this is not always practical or even possible, and eventually ends up hitting the barrier of overlap, which is not allowed in XML. The typical solution in this case is standoff, which in fact is one of the typical outputs of Cadmus when exporting a subset of its data into TEI; yet, that's right its complexity which calls for an automatic generation of it.
- it allows designing the model of each annotation _without constraints_ from the physical model. You can define a highly structured object for each type of annotation, without caring about having to interlock its parts with those of other models into a single, predefined structure. Also, instead of just attaching "flat" tags (as element names or attribute name/value pairs) to a portion of text, you can attach a _fully structured_ object of any depth, where each property is either a scalar value or yet another object, without limits.
- it allows abstracting from a specific physical model (e.g. XML) thus producing an _easy user experience_, requiring no IT skills to create digital content; you just have to fill a web form.
- it allows using the same abstract source model to _generate multiple outputs_, whether it is a TEI document (in one or more different schemas), an RDF graph, etc.

[^1]: Strictly speaking, for a collection it is not necessary that the witnesses belong to a single textual carrier. In theory, one could even envisage a case where someone is planning a collection by picking texts from different carriers. The definition quoted here anyway refers to the most typical case.

### Items

Here we list the Cadmus items with their parts, as defined for the GVE editor in the API backend profile.

#### Snapshot

- _identity_:
  - [metadata](https://github.com/vedph/cadmus-general/blob/master/docs/metadata.md)
  - [external IDs](https://github.com/vedph/cadmus-general/blob/master/docs/external-ids.md)
  - [links](https://github.com/vedph/cadmus-general/blob/master/docs/pin-links.md)
- _content_:
  - snapshot (GVE: [code](https://github.com/vedph/gve-core/blob/master/Cadmus.Gve.Parts/GveSnapshotPart.cs)):
    - `snapshot` (`Snapshot`):
      - `size` (`Size`):
        - `width` (`double`)
        - `height` (`double`)
      - `style` (`string`)
      - `defs` (`string`)
      - `image` (`SnapshotImage`):
        - `url`\* (`string`)
        - `canvas` (`Rectangle`):
          - `x` (`double`)
          - `y` (`double`)
          - `width` (`double`)
          - `height` (`double`)
        - `opacity` (`double`)
      - `text` (`CharChainNode[]`):
        - `id` (`int`)
        - `index` (`int`)
        - `label` (`string`)
        - `data` (`char`)
        - `sourceTag` (`string`)
        - `features` (`Feature[]`):
          - `name`\* (`string`)
          - `value` (`string`)
          - `setPolicy`\* (`int`)
      - `textStyle` (`string`)
      - `textOptions` (`SvgBaseTextOptions`):
        - `lineHeightOffset` (`double`)
        - `charSpacingOffset` (`double`)
        - `spcWidthOffset` (`double`)
        - `offset` (`Point`):
          - `x` (`double`)
          - `y` (`double`)
        - `minLineHeights` (dictionary of doubles keyed by shorts)
  - [comment](https://github.com/vedph/cadmus-general/blob/master/docs/comment.md)
- _history_:
  - [historical dates](https://github.com/vedph/cadmus-general/blob/master/docs/asserted-historical-dates.md)
- _editorial_:
  - [references](https://github.com/vedph/cadmus-bricks/blob/master/docs/doc-reference.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md)

#### Carrier

- _identity_:
  - [metadata](https://github.com/vedph/cadmus-general/blob/master/docs/metadata.md)
  - [shelfmarks](https://github.com/vedph/cadmus-codicology/blob/master/docs/cod-shelfmarks.md)
  - [external IDs](https://github.com/vedph/cadmus-general/blob/master/docs/external-ids.md)
  - [categories](https://github.com/vedph/cadmus-general/blob/master/docs/categories.md):`carrier`
- _material_:
  - [measurements](https://github.com/vedph/cadmus-general/blob/master/docs/physical-measurements.md)
  - [preservation states](https://github.com/vedph/cadmus-general/blob/master/docs/physical-states.md)
- _content_:
  - [comment](https://github.com/vedph/cadmus-general/blob/master/docs/comment.md)
- _history_:
  - [historical dates](https://github.com/vedph/cadmus-general/blob/master/docs/asserted-historical-dates.md)
  - [historical events](https://github.com/vedph/cadmus-general/blob/master/docs/historical-events.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md):`hist`
- _editorial_:
  - [references](https://github.com/vedph/cadmus-bricks/blob/master/docs/doc-reference.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md)

#### Epigram Version and Lost Lines

For epigram version the group ID is the epigram's EID.

- _identity_:
  - [metadata](https://github.com/vedph/cadmus-general/blob/master/docs/metadata.md)
  - [links](https://github.com/vedph/cadmus-general/blob/master/docs/pin-links.md) 🔗 carrier
  - [links](https://github.com/vedph/cadmus-general/blob/master/docs/pin-links.md):`auth`
- _material_:
  - [measurements](https://github.com/vedph/cadmus-general/blob/master/docs/physical-measurements.md)
  - [preservation states](https://github.com/vedph/cadmus-general/blob/master/docs/physical-states.md)
- _content_:
  - [categories](https://github.com/vedph/cadmus-general/blob/master/docs/categories.md):`topic`
  - [flags](https://github.com/vedph/cadmus-general/blob/master/docs/flags.md):`txt`
  - [token-based text](https://github.com/vedph/cadmus-general/blob/master/docs/token-text.md)
  - [apparatus layer](https://github.com/vedph/cadmus-philology/blob/master/docs/fr.apparatus.md)
  - [comment layer](https://github.com/vedph/cadmus-general/blob/master/docs/fr.comment.md)
  - `hands` (GVE):
    - `hands` (`GveHand[]`):
      - `eid` (`string`)
      - `ownerId` (`AssertedCompositeId`)
      - `tag` (`string` 📚 `gve-hand-tags`)
      - `tool` (`string` 📚 `gve-hand-tools`)
      - `color` (`string` 📚 `gve-hand-colors`)
      - `notes` (dictionary of strings)
- _history_:
  - [historical dates](https://github.com/vedph/cadmus-general/blob/master/docs/asserted-historical-dates.md)
  - [historical events](https://github.com/vedph/cadmus-general/blob/master/docs/historical-events.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md):`hist`
- _editorial_:
  - [references](https://github.com/vedph/cadmus-bricks/blob/master/docs/doc-reference.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md)

#### Epigram

- _identity_:
  - [metadata](https://github.com/vedph/cadmus-general/blob/master/docs/metadata.md)
  - [external IDs](https://github.com/vedph/cadmus-general/blob/master/docs/external-ids.md)
- _content_:
  - [categories](https://github.com/vedph/cadmus-general/blob/master/docs/categories.md):`topic`
  - [comment](https://github.com/vedph/cadmus-general/blob/master/docs/comment.md)
- _editorial_:
  - [references](https://github.com/vedph/cadmus-bricks/blob/master/docs/doc-reference.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md)

#### Collection

- _identity_:
  - [metadata](https://github.com/vedph/cadmus-general/blob/master/docs/metadata.md)
  - [external IDs](https://github.com/vedph/cadmus-general/blob/master/docs/external-ids.md)
- _content_:
  - [categories](https://github.com/vedph/cadmus-general/blob/master/docs/categories.md):`seq`
  - [links](https://github.com/vedph/cadmus-general/blob/master/docs/pin-links.md) 🔗 version
  - [comment](https://github.com/vedph/cadmus-general/blob/master/docs/comment.md)
- _editorial_:
  - [references](https://github.com/vedph/cadmus-bricks/blob/master/docs/doc-reference.md)
  - [note](https://github.com/vedph/cadmus-general/blob/master/docs/note.md)

#### Parts Matrix

| part         | snapshot | carrier | version | lines  | epigram | collection |
| ------------ | -------- | ------- | ------- | ------ | ------- | ---------- |
| categories   |          | carrier | topic   | topic  | topic   | seq        |
| comment      | X        | X       |         |        | X       | X          |
| dates        | X        | X       | X       | X      |         |            |
| events       |          | X       | X       | X      |         |            |
| external IDs | X        | X       |         |        | X       | X          |
| flags        |          |         | txt     | txt    |         |            |
| hands (GVE)  |          |         | X       | X      |         |            |
| links        | X        |         | X auth  | X auth |         | X          |
| measurements |          | X       | X       | X      |         |            |
| metadata     | X        | X       | X       | X      | X       | X          |
| note         | X        | X hist  | X hist  | X hist | X       | X          |
| references   | X        | X       | X       |        | X       | X          |
| shelfmarks   |          | X       |         |        |         |            |
| states       |          | X       | X       | X      |         |            |
| text         |          |         | X       | X      |         |            |
| apparatus=   |          |         | X       | X      |         |            |
| comment=     |          |         | X       | X      |         |            |
