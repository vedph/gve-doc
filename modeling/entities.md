# Entities Overview

⚠️ This is part of an ongoing discussion, so that's just a draft.

## Sheets

At the bottom level we have sheets. Sheets provide what I call "snapshots" of a text together with all the variants on top of it, in the form of written annotations. Here I'm already designing a comprehensive model, which I partially showed at the DiSCePT meeting, and once I've completed it I will show you in detail. Meanwhile, I know enough of it to be able to provide a first model draft and I'm currently working on its implementation and refinement. The model includes:

- on the philological side, the text and its variants in the form of operations which change it. So it's a generative process, you have 1-N recipes to build a text starting from the ingredients in the snapshot, i.e. the "base" text and its annotations.

- on the diplomatic side, each operation has its diplomatic metadata, including their visual representation. This allows us animating them, though in a different way with respect to your presentation because here we're below the level you dealt with there.

At level 1, the identity of each sheet is granted by its materiality. So one sheet is one sheet, and has some standard identifier(s).

Apart from all the metadata defined by model for representing the various philological and diplomatic aspects of each sheet, which are other metadata for each sheet? We will e.g. have standard ID(s) in the literature, maybe date(s) and place(s), physical description (e.g. size, ink...), etc.

## Epigram

Above level 1, everything is immaterial. The first abstraction we encounter above the sheets level is the epigram. The epigram is a reconstructive hypothesis of the text. In a traditional apparatus you summarize the differences between your witnesses, usually manuscripts. In our case, each sheet provides 1-N witnesses, so the apparatus here will summarize the differences among them. Formally that's not so different from a traditional apparatus descending from collation; the main difference is that our witnesses come from a single sheet and its interpretations rather than from different manuscripts.

At level 2, the identity of each sheet is to be defined. Genetically it's defined by the sheet which generated the witnesses of that epigram. Is this only a single sheet, or are there 1-N of them?

Which are the metadata at this level? Apart from the ID, title, date(s), etc.

## Collection

above level 2 we have collections. A collection is an ordered set of epigrams.

At level 2, how we define the identity of each collection? I suppose there is some standard scholarly scheme in the literature.

Also, which are the metadat for collections? And can a collection have different orderings for its epigrams, or just one? This of course depends on whether the collection is just an abstraction or it comes from a physical carrier. A printed book has a single order, but two versions of the same collection (two editions of the same collection) might change it.
