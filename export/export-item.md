# Exporting to Cadmus Items

Until now, we have focused on the snapshot, which is our model designed to represent autographs in a highly structured, rich, and compact manner.

As you know, above the snapshot we have epigrams, and above epigrams we have collections. The snapshot is a faithful representation of what you find on the original sheet(s), and describes multiple versions of what we regard as the same text.

Once we have the snapshot, in an abstract scenario one might either create a single epigram entity representing what we define as the "final" version; or, like in this case, create multiple epigram entities, each representing a specific version of the text.

Now, the GVE snapshot editor (which is totally independent from Cadmus) will be embedded in Cadmus to provide a unified database and editing environment; and the same environment will be used to edit epigrams and collections (once we define their models). So, for the end user there will be a single, uniform editing experience, whatever the entity we are dealing with.

So, let us imagine our user workflow: it starts with a snapshot. The user just enters data in the snapshot editor, embedded in the Cadmus system. Once done, we have a model which defines multiple versions of a text at once.

Some of these versions will be selected by the user to represent what he flags as a "staged" version, i.e. a waypoint in the path towards the "final" version of the text, even though in this case there never was one. This staged version will correspond to a specific version of an epigram: an epigram entity.

Thus, the user would create a new epigram entity for each staged version, and then enter additional data about it, making it part of a "constellation" of epigram versions under the umbrella of a single abstract "epigram" which was never materialized (and thus has no entity in the database; yet, all the epigram entities stemming from the same source are grouped in this constellation).

For each of these epigram entities, he would start by entering its text, some additional metadata for classification and identification, and then create a reasoned critical apparatus with all the annotations he wants to add. Many of these annotations will be drawn from metadata linked to each snapshot versions, while others will possibly added by his critical reflection.

So, it's important to notice that the step leading from a snapshot to an epigram is not an automatic one: it is the product of a critical work by the editor. Yet, of course it's based on the snapshot data, which in turn were the combined result of a detailed objective description of the autograph and its subjective critical interpretation.

In this context, the user can just go on and start from scratch, entering all these data in the epigram. Or, we could think of a shortcut for easing his work at its beginning.

On a totally manual approach, the user could simply generate the text from the snapshot, copy it, create a new item for the epigram, paste the text in it, and then create annotations, while constantly keeping eye on the snapshot metadata.

On a shortcut approach, which is just a quicker alternative to the manual one, one might imagine an automated process which:

1. takes a snapshot as its input.
2. generates all the versions from it, picking the ones flagged as staged.
3. for each staged version, creates a new epigram entity, and stores in it the version's text and a bunch of essential notes automatically extracted from the snapshot output.

Of course, this is far from being a finite product: it's just a draft to start with, which avoids doing all these operations from scratch. Anyway, it would not only save time, but also make the whole editing process easier and more streamlined for users.

So, what we are defining here is just another form of data export: we start from the GVE snapshot, and export it into multiple Cadmus items, each representing an epigram entity, with its text and "apparatus". This will thus automatically define the starting point for the scholarly work to be done at the epigram level.

Of course, defining this export process implies a lot of preliminary work:

- we must determine what kind of output we would like for epigram entities, i.e. their model. Here we are not dealing with a traditional critical text with an apparatus of variants; rather, we have a version of a text, with notes about how each portion of it was transformed from a previous version. In a sense, this would be a more "narrative" apparatus, rather than following the highly standardized formalism of a critical apparatus of variants. As such, it will probably make sense more for human readers than for machines; but that's right what we want in this kind of output, which targets them. That's just a human-friendly, text-based presentation of a subset of our source data.
- we must define an algorithmic process for generating the draft of such epigram's critical text from a snapshot's version. This is a complex task, because of the high level of structure and complexity represented by the snapshot model in comparison with the much more constrained formalism we need to apply to its output.

This is right what called for a first revision of the current snapshot implementation. I am not speaking about the model here, which remains exactly the same; I am just referring to implementation details. To ease later processing like this type of export, I needed more specific metadata to be automatically injected in the generated versions. Sure, data are already there; but adding some specific, well-designed metadata to the output would make it easier to deal with for further processing.

To be more concrete, this means having more features linked to text nodes. As you know, the snapshot model is based on a graph of linked nodes. Each node here represents a single character of the text, which is variously connected by links to build different sequences, each corresponding to a version. Operations (like replace, insert, delete, etc.) just add new sets of links, tagged for a specific version, to connect the same nodes in different ways (and possibly newly added nodes, for insertions or replacements).

Operations also have metadata, and inject them into their output: either into the output version as a whole, or into specific nodes of it. So, a replace operation might inject metadata about the supposed reason for the replacement, or the ink used to annotate it, or the source of it, etc. Such metadata are known as _features_, attached to the version or to specific nodes; and these features are an open set. According to their lifetime, they can be overwritten by later operations, or removed, or just inherited.

So, this is our extension point, and that's right what I leveraged to add more metadata. I carefully designed a set of special features named trace features.
