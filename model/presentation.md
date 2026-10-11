# Presentation

## Entities Graph

An essential aspect of the model devised for VEdition is that it reflects the **"constellation"** of texts which goes under the name of "Venetian epigrams", where there is no plain linear evolution. Many versions of the "same" text (the abstraction we name "epigram", here `E`) coexist, scattered across multiple manuscripts or prints (the "text carriers", `C`), variously grouped under multiple collections (material or immaterial; `L`).

Each of these documents (named "snapshots", `S`) mostly contains multiple "versions" of the text (`V`), as its manuscript annotations hint at various transformations of the text first jotted down on paper. So, these versions are not written at length, but they are the result of interpreting the annotations on the base text which hint at them. Such documents represent a "snapshot" of the creative process behind the transformations of a text, generating its multiple versions.

So, among these entities:

- `C` (carriers) contain `S` (snapshots);
- `E` (epigrams) contain `S` (snapshots);
- `S` (snapshots) contain (they actually generate) `V` (versions);
- `L` (collections) contain (they actually collect) `V` (versions);
- `V` (versions) are the only entities representing text.

> Among these entities, `C` is material; `S` is an abstraction but in a sense it is partially material, because it is based on a unitary set of signs on paper; `E` is an abstraction; `V` is (mostly) a reconstruction; and `L` can be either an abstraction (the collection resulting from numbering some versions) or a material entity (e.g. the carrier collecting snapshots in a given order).

The result is a **graph** of nodes (=entities) which can be browsed starting from any node and following the links to any other node.

The diagram in Figure 1 exemplifies this graph:

```mermaid
flowchart TD;

C1[C] --> S1[S]
C1[C] --> S2[S]
C1[C] --> S3[S]
C2[C] --> S4[S]
S1 --> V1[V]
S1 --> V2[V]
E1[E] --> S1
E1[E] --> S2
E2[E] --> S3
E2[E] --> S4
L1[L] --> V1
L2[L] --> V1
L2[L] --> V2
```

- _Figure 1 - Top-level entities links_

In presenting these data we should try to reflect this complex set of relationships among different entities, allowing users to seamlessly hop from node to node and get a sense of their connection in the constellation.

If we consider the hierarchy intrinsic in the above defined entities, we can define 3 levels:

1. `C` and `E`
2. `S` and `L`
3. `V`

Here each upper level contains some or all entities of the lower level (Figures 2-4):

```mermaid
flowchart LR;

CE[C/E] --> S1[S]
CE[C/E] --> S2[S]
```

- _Figure 2: carriers and epigrams contain snapshots_

---

```mermaid
flowchart LR;

S --> V1[V]
S --> V2[V]
```

- _Figure 3: snapshots contain (generate) versions_

---

```mermaid
flowchart LR;

V1[V] --> L
V2[V] --> L
```

- _Figure 4: versions are grouped into collections_

---

## Presenting the Graph

In a presentation we should clearly present entities, while still allowing users to walk across their whole graph.

The 3 levels identified in the previous section allow us to define at least these top-level views for our data:

1. **carriers view**: the list of all the material supports which carry our texts. Readers interested in codicological aspects or studying the story of a single material object would look into that.
2. **epigrams view**: the list of all epigrams. This is a virtual collection of all the abstractions representing a unitary group of text versions. Most readers will probably start from this list.
3. **snapshots view**: this would be probably best served as an intermediate view in walking the graph. The snapshot concept is an abstraction and is backed up by the [genetic model](architecture.md) defined for Hydra. Yet, the snapshot also has a material part: it is the sum of all signs we group together (base text and its annotations) on a portion of a sheet or on one or more sheets of a carrier, and it implicitly contains all the text versions. So, when flipping the pages of a notebook, we see snapshots; and when drilling down from an epigram node we find snapshots (which in turn contain versions).
4. **versions view**: the list of text versions. These are the only entities with text, so in the end readers will look for them. Anyway, the might want to get at this list in different ways: starting from an upper layer, e.g. when drilling down from an epigram through snapshots; or even by freely browsing all available texts, even when belonging to different epigrams (e.g. when landing to the list from a full-text search).
5. **collections view**: the list of all collections. These are material or immaterial groupings of text versions.

We can thus identify UI blocks which show entities in a compact, table-like layout, while still allowing to walk seamlessly from node to node.

### Carriers View

A paged list of all carriers matching carrier filters:

- a **filters panel** allow to combine various filters to view only the carriers matching them (e.g. name, date range, consistency, specific epigrams included, etc.). The filters start blank, so that the whole list is initially shown.
- each **list item** shows essential metadata about the [carrier](editor#carrier-item) plus **navigation buttons** to walk down from it:
  - epigrams: opens an _epigrams view_ filtered by the epigrams included in the carrier.
  - snapshots: opens a _snapshots view_ filtered by the snapshots included in the carrier.
  - collections: opens a _collections view_ filtered by the versions included in the carrier. This shows all collections which include at least one version from that carrier.

### Epigrams View
