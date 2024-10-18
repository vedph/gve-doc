# Entities Overview

⚠️ This is part of an ongoing discussion, so that's just a draft.

- [reference documentation](http://www.fusisoft.it/docs/gve/): main documentation. Please refer to the [conceptual documentation](http://www.fusisoft.it/docs/gve/docs/autograph.html) in it for the definitions and discussions of snapshots, carriers, and versions.
- [GVE demo app](https://gve-demo.fusi-soft.com/): chain data structure demo.

## L1 - Snapshot

At the bottom level we have the snapshot. In the case of GVE, the essential source of epigrams is represented by autograph sheets from handbooks. Whatever its physical support(s), we call the physical support of an autograph text 🔖 carrier. In the case of our workbooks, a portion, one, or even more physical sheets referring to each single composition represent a carrier.

The main challenge in modeling the content of these carriers is that they do not just represent a linear text; rather, they represent a sort of 🔖 snapshot of the creative process which in the author's mind would have led to the intended text. So, the carrier is the material support of the snapshot, which potentially contains many versions of a text.

In fact, graphically the snapshot does not just represents a text, but it includes many corrections, rethinkings, or refactorings of it, in the form of various types of 🔖 annotations (deletions, insertions, transpositions, side notes, etc.).

### Snapshot Model

The model of level 1 is quite complex, as it must represent in separate layers:

- the base text;
- all the annotations on top of it, giving rise to different variants according to our interpretations. The annotations are represented as operations which change the text they refer to;
- our interpretations of the annotations, producing many readings. Interpretations select and order operations;
- metadata about each operation;
- diplomatic rendition of each operation;
- animation timeline for each version of the text resulting from interpretations.

![level 1](../img/level1.png)

At the core of this model is the chain data structure. This data structure consists of a linear sequence of nodes, each connected to the previous and the next node by a link, and in our scenario representing a single character. Additionally, each link has a version tag, which adds the dimension of time to this structure.

Internally, this structure collects nodes and links, and once they are added, they cannot be removed (unless we are using a single version). Any modification is done by adding new nodes and links, which connect or disconnect or reorder the existing nodes.

So, a chain is a sort of time-enabled string, capable of representing multiple versions of the same text as defined by multiple subsets of the links connecting text nodes. You just follow the links with the desired version to build the corresponding text.

A set of operations target this chain structure and add new links and nodes to represent each successive change, even in different branches, as each operation starts from a specific chain state and produce another chain state. Operations are selected and ordered by interpretations, and include philologic and diplomatic metadata.

The chain contains all the states for our text, which are the basis for producing many text versions. For each produced version we can provide a timeline, which is in charge of orchestrating animations of the visual renditions of each visible operation.

### Snapshot Metadata

Q: Apart from all the metadata defined by model for representing the various philological and diplomatic aspects of each sheet, which are other metadata for each carrier? We will e.g. have standard ID(s) in the literature, maybe date(s) and place(s), physical description (e.g. size, ink...), etc.

A: as a first overview, see the [inventory of Goethe's poems by Klassik Stiftung Weimar](https://www.klassik-stiftung.de/). There are entries having properties like (this is by no means a complete list):

- location: this is like a geolocation CSV address, from the widest to the narrowest area. It usually ends with an institution.
- sheets count
- title
- title on MS
- dating on MS
- incipit
- transmission form
- context
- many identifiers, like:
  - signature(s)
  - WA sigle
  - WA column
  - WA paralipomena
  - WA printing location
  - Goethe letter repertory
- persons (names with comma separating last from first)

TODO: here we need to decide which metadata to include, and what's their exact meaning and form.

## L2 - Version

Above level 1, everything is immaterial except for some collections at level 4.

From each snapshot our interpretations extract 1 or more versions.

The **identity** of each version is given by the source sheet plus a reading identifier, unique in the scope of each sheet.

The **relationship** between carrier and versions is 1-to-many: each single carrier generates 1 or more versions.

### Version Metadata

Metadata are among the output of the operations defined by each interpretation. Dealing with the details of such metadata at the carrier level will automatically define the metadata inherited by L2.

## L3 - Epigram

The epigram is a reconstructive hypothesis of the text, based on any number and combination of versions. From this point of view, think of versions as the witnesses in a traditional apparatus.

In a traditional apparatus, you summarize the differences between your witnesses, usually manuscripts. In our case, each carrier provides 1-N witnesses (the versions), so the apparatus here will summarize the differences among them. Formally that's not so different from a traditional apparatus descending from collation; the main difference is that our witnesses come from a single carrier and its interpretations rather than from different manuscripts.

Based on the interpretation of the intradocument (within the single manuscript) variants, each carrier can produce one or more versions of an epigram. Those variants, along with all the interdocument (between the witnesses) variants are then collected under the umbrella of the metaepigram, which stands for the abstract entity that contains all epigrams that can be group together in force of their constants.

At any rate, from the point of view of the model an epigram is just an abstraction derived from reconstruction of a text; whether this is based on a single carrier or on many of them it's a detail.

The **identity** of each epigram should be probably defined against an official registry.

### Epigram Metadata

Q: Which are the metadata at this level? Apart from the ID, title, date(s), etc.

## L4 - Collection

A collection is an ordered set of epigrams, and can be either material or immaterial. An immaterial collection can be based on material collections, but also on other evidence; or it may even rest on other evidence only. For this reason, material and immaterial collections are siblings and rest at the same level; they represent the same hierarchical level, even though by historical accidentes some of them were materialized and others did not.

The **identity** of each collection should be probably defined against an official registry.

The **relationship** between epigrams and collections is many-to-many, because:

- many epigrams are included in a collection;
- a single epigram may be included in many collections.

### Collection Metadata

Q: Which are the metadata at this level? Apart from the ID, title, date(s), etc.
