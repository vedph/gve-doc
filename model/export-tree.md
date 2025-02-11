<!-- ---
title: Exporting Tree
layout: default
parent: Model
nav_order: 6
--- -->

# Exporting Tree

⚠️ This is preliminary and incomplete documentation!

Let us consider the versions produced by our [mock example](sample-arzdc.md):

- v0: ARZDC
- v1: ARDC
- v2: AVDC
- v3: ABDC
- v4: APDC
- v5: APCD
- v6: ABCD

As you can see from its chain, the graph contains 7 different paths we can follow to build all the versions from v0 to v6:

![chain](img/arzdc-gviz.png)

Versions in this graph are multi-dimensional: each path defining a version defines a different direction. The graph is a multi-dimensional representation of linear combinations representing versions.

When exporting data, we often need to reduce these dimensions to provide a more linear output. As a privileged target is XML, and XML is tree-based, this implies reducing the multiple dimensions of a graph into a line which can branch into other lines on a bidimensional space.

This will allow using e.g. a TEI `<app>` element with `<lem`> and `<rdg>` as the branching points, opening two different lines each with all its nodes up to the end of each version. To do this we will have to duplicate nodes; but we will end up projecting the multiple dimensions of the graph into a bidimensional tree and thus export into an XML tree, even if a quite nested and complex one.

As an example, consider the above graph. Let us start from the latest version, v6, which is the "final" stage of our transformations in our reconstruction. This final stage will be our main text line, and each departure from it will produce a branching with two or more lines stemming from it (initially the lines will be two, but others may be added later). Let us follow this procedure, starting from a blank root node to ensure that we have a single root (arrow points to child nodes). So, at start we have a tree with a single root node:

```mermaid
graph LR;

root
```

(1) **v6**: ABCD: this version is represented by a single lineage: A is child of root, B is child of A, C is child of B, and D is child of C:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D
```

(2) **v5**: APCD: again, starting from the root we first find an A child; this is equal to our first A node of APCD, so we keep going along the existing lineage until we find a match. This will reduce nodes redundancy in our projected tree. As soon as we step down to B, we have a mismatch (with P): this defines the parent of this mismatching node as the branching point.

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D
A --> P
P --> C5
C5[C] --> D5[D]
```

(3) **v4**: APDC: starting from the root, we follow matches up to the P node of APCD; this will be our branching node. The new branch will thus be DC:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D6[D]
A --> P
P --> C5
C5[C] --> D5[D]
P --> D4[D]
D4 --> C4[C]
```

(4) **v3**: ABDC: starting from the root, we follow matches up to the B node of AB; this will be our branching node, adding to it the child branch DC:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D6[D]
A --> P
P --> C5
C5[C] --> D5[D]
P --> D4[D]
D4 --> C4[C]
B --> D3[D]
D3 --> C3[C]
```

(5) **v2**: AVDC: starting from the root, we follow matches up to its A child node. We will thus add a third branch to it, VDC:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D6[D]
A --> P
P --> C5
C5[C] --> D5[D]
P --> D4[D]
D4 --> C4[C]
B --> D3[D]
D3 --> C3[C]
A --> V
V --> D2[D]
D2 --> C2[C]
```

(6) **v1**: ARDC: starting from the root, we follow matches up to its A child, as above; its new branch will be RDC:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D6[D]
A --> P
P --> C5
C5[C] --> D5[D]
P --> D4[D]
D4 --> C4[C]
B --> D3[D]
D3 --> C3[C]
A --> V
V --> D2[D]
D2 --> C2[C]
A --> R
R --> D1[D]
D1 --> C1[C]
```

(7) **v0**: ARZDC: starting from the root, we follow matches up to its R child, which becomes a new branching node. To this we will add the branch ZDC:

```mermaid
graph LR;

root --> A
A --> B
B --> C
C --> D6[D]
A --> P
P --> C5
C5[C] --> D5[D]
P --> D4[D]
D4 --> C4[C]
B --> D3[D]
D3 --> C3[C]
A --> V
V --> D2[D]
D2 --> C2[C]
A --> R
R --> D1[D]
D1 --> C1[C]
A --> R0[R]
R0 --> Z
Z --> D0[D]
D0 --> C0[C]
```

We have now considered all the outputs of the chain graph, building a full path for each, in a trie-like structure. Starting from the root we can follow branches, each corresponding to a version. We are thus representing the same linear combinations as the chain graph, but in a tree-shaped structure, which implies that we have to duplicate nodes. Yet, this structure is now fit to an XML-based rendition.

In fact, if we traverse the tree depth-first, we get:

- ABCD (v6, as defined by the last, leaf node in this traversal)
- ABDC (v3)
- APCD (v5)
- APDC (v4)
- AVDC (v2)
- ARDC (v1)
- ARZDC (v0)
