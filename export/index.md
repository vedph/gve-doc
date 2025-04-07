---
title: Export
layout: default
parent: Home
nav_order: 3
---

⚠️ This is preliminary draft documentation subject to change!

# GVE Export

Exporting GVE data is a complex topic, because it includes many different scenarios and for efficiency reasons it also shares logic with different projects like Cadmus.

Whatever the scenario, for the most part the export flow is covered by a common rendering pipeline, which constitutes the core of any rendering process.

## Pipeline

Exporting data from a GVE model to some text-based output like TEI uses a refactored version of the [Cadmus rendering subsystem](https://vedph.github.io/cadmus-doc/migration/rendering/rendition.html). The main data flow follows a pipeline leading from the [GVE snapshot](../model/textual.md) to the final result, whether it's some form of TEI or any other format:

```mermaid
graph LR;

SNAPSHOT --> tree-builder
tree-builder --> tree-filters
tree-filters --> tree-renderer
tree-renderer --> text-filters
text-filters --> RESULT
```

Except for the first step, which adapts GVE data to the rendering pipeline, this is shared with the Cadmus rendering subsystem, typically (but not only) used to export multi-layered text into various forms of TEI.

We can devise 4 main stages in the pipeline flow:

1. **preprocessing**: adapt the input to the model used by the first part of the pipeline (in this case using a _tree builder_), i.e. a _linear tree_. This is a tree structure having a single branch and starting with a blank root node. Every text segment being processed is represented by a node; and each node is child of the node with the previous segment, and parent of the node with the next one. So, at start this tree is functionally like an array: we just have a sequence of segments. Yet, in the next stages the tree may branch and [transform](export-trees.md) as required by the desired output.
2. **tree filtering**: the tree passes through any number of _tree filters_, which can freely transform it. Each filter has a tree as its input, and the same tree or a new one as its output. The output of each filter will thus be the input for the next one.
3. **tree rendering**: the tree structure is materialized into some text-based output format, using a _tree renderer_. This receives the filtered tree as its input, and outputs an object whose type depends on the renderer: it might be a string, an object representing an XML element, etc.
4. **text filtering**: the output is optionally further refined by passing through a sequence of _text filters_. Despite their name, these filters are free to represent their object in the way it best fits their task. For instance, some of them just look at the text as a string, while others may look at it as an XML element using a structured object to represent it.

The pipeline approach has many **benefits**:

- it splits monolithic and complex logic into smaller and simpler parts, each implemented by an independent software component. Complex logic is thus attained via composition of more manageable and self-contained modules.
- such components are designed for reuse, so that we can generate virtually unlimited outputs by just chaining and configuring them in different ways.
- leveraging reuse allows to easily export completely different outputs without having to write any code. On the other hand, whoever writes code to add some more specialized logic is not only serving his own project, but also contributing to a repository of pipeline modules which could be used in many other scenarios.
- defining a pipeline is a completely declarative process, so no programming is required: a pipeline is fully configured by creating a JSON document with all the settings required to configure and concatenate each module.

## Scenarios

To start with, we can envisage at least 3 export scenarios in this context:

1. _exporting Cadmus text_. This is an [independent project](https://vedph.github.io/cadmus-doc/migration/rendering/rendition.html), but GVE borrows a good portion of its logic, because with reference to text rendition they target similar problems.

2. [exporting GVE snapshots into text documents](export-tei). For instance, you might want to generate some kind of TEI document(s) representing a subset of source data.

3. [exporting GVE snapshots into Cadmus items](export-item). This might be useful to ease UX when we start dealing with higher-order entities in our hierarchy, as we will be using Cadmus to edit them.
