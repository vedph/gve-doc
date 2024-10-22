# User Experience

This section of the documentation refers to the frontend GUI designed and implemented to provide a user-friendly, collaborative and web-based content creation system for data shaped according to our [model](../model/index.md).

In general, given the complexity of a genetic model, I tried to imagine an easy UX which mostly mimicks the traditional process of a scholar in editing an autograph text.

When faced with an annotated text like this, the first approach is right trying to make a sense of it: we start from the text, and then explore all the annotations stemming from it, as represented by a combination of visual hints and additional portions of text. In most cases, such annotations represent changes of the start text, in terms of editing operations: the author (or anyone else acting on the text) deletes something, or adds something, or moves something around. This process accumulates many changes on the text, until it reaches what the scholar interprets as a target version.

Of course, this is not necessarily a linear process; it may well happen that there are many different lineages stemming from the start text, and that annotations are added on top of other annotations. In such cases our line branches into many different lines, like in a tree.

So, ideally we can imagine to implement a description of this process in these steps:

1. define the _text_ to start with, which is our "base text".
2. describe each single _editing operation_ directly or indirectly performed on it. In the simplest case, we have a linear progression where each operation's output is the input to the next operation; but we may well start from the output of any of the previous operations in our reconstructed sequence, thus producing various branches in a tree-shaped structure.
3. optionally, also describe the _visuals_ corresponding to each operation. For instance, a slash on top of a letter meaning its delection, a V-like sign with some letters above it meaning their insertion in a specific place, etc.
4. optionally, define _animation timelines_ for presenting your visuals for any specific versions.

## Architecture

Technically, the UI has been designed into a set of **layers and modules**, with the primary purpose of providing a reusable tool for this and other projects. Currently, there are 4 layers, each corresponding to a different implementation technology:

1. **snapshot core** (TS library, packaged with NPM: `gve-snapshot-core`): this is a pure Typescript library, framework-agnostic, containing just the definitions of the parts of a snapshot model. These definitions are provided in a separate library so that everyone can reuse them even without importing any piece of logic, which is rather provided by upper layers.
2. **snapshot view** (Web Component, packaged with NPM: `gve-snapshot-view`): this is a framework-agnostic, pure web component which can be directly integrated in any HTML page or application to visualize a snapshot. Integrating this component is as simple as typing the name of its tag in the HTML page, and passing it a JS object with data of the snapshot to visualize. Starting from this object, the view takes care of all the complex details relative to generating an interactive SVG-based visualization of the snapshot. As such, this component can be used both in an editing environment, where it works as a preview (and that's how we use it in our editor), and in a presentation environment for end users. This is why it has been implemented with a highly neutral web-based technology with no dependencies from a specific web framework like Angular, React, or the like: this is a pure W3C standard web component.
3. **snapshot editor** (Angular 18+ library, packaged with NPM: `gve-shell`): this is the core component used to fully edit the snapshot model. This is implemented as an Angular library, so that it can be easily integrated into any Angular based web application. In turn, this editor uses the snapshot view to preview the content being edited.
4. **VEdition editor** (Angular 18+ [Cadmus](https://myrmex.github.io/overview/cadmus)-based editor, distributed in containerized Docker images: `cadmus-gve-app`): this is the editor planned for the full content of VEdition, which is not limited to snapshots, but extends to a whole tree of entities based on it. In this case we adopt [Cadmus](https://myrmex.github.io/overview/cadmus) for its modularity and extensibility, which allows us to build a full web editor on top of an already mature tool, backed by a full-fledged database. In turn, this editor uses the snapshot editor library for editing snapshots, and more libraries for the other entities involved.

This layered design allows to reuse the logic included in each component without having to introduce unwanted dependencies, according to the purposes and environment of reuse.
