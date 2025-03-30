# GVE Export

Exporting GVE data is a complex topic, because it includes many different scenarios and for efficiency reasons it also shares logic with different projects like Cadmus.

## Scenarios

To start with, we can envisage at least 3 export scenarios in this context:

1. _exporting Cadmus text_. This is an [independent project](https://vedph.github.io/cadmus-doc/migration/rendering/rendition.html), but GVE borrows a good portion of its logic, because with reference to text rendition they target similar problems.

2. [exporting GVE snapshots into text documents](export-tei). For instance, you might want to generate some kind of TEI document(s) representing a subset of source data.

3. [exporting GVE snapshots into Cadmus items](export-item). This might be useful to ease UX when we start dealing with higher-order entities in our hierarchy, as we will be using Cadmus to edit them.

Let us now discuss each of these scenarios.
