# Export by Replay

As GVE model focuses on the author editing process describing it in operational terms, we can leverage its power even when exporting data.

As an example, say we want to get a TEI output where, for a given text (an epigram, a page, etc.):

1. all the versions are inside an `app` element;
2. in `app`, each version is inside a `lem` or `rdg` element;
3. in `lem`/`rdg`, a `mod` contains more metadata about the version (in attributes);
4. in `mod`, we use `del`, `add`, or `subst` as customary in TEI critical.

This is the structure used by the Saba edition, and thus can be displayed in EVT 3.

## Procedure

### 1. Operations

We just start with the GVE model recipe, i.e. the base text and its operations. This time, we use them to directly generate a tree structure for each staged version:

- build a linear tree from base text (v0) having a blank root node.
- for each operation:
  - if it's INS: add new nodes with features `$ins`=`OPID N` (`OPID`=operation ID, `N`=operation number), and operation-injected features;
  - if it's DEL: mark deleted nodes with features `$del` (as for `$ins`) and operation-injected features;
  - if it's REP/MOV: DEL and INS;
  - if it's SWP: DEL and INS for 1st segment, DEL and INS for 2nd segment;
  - if it's ANN: add operation-injected features.

All the globally-injected features are stored in the root node.

This results into a linear tree structure where each node is a single character and has all the injected features, plus these trace-like features starting with `$`. Of course, we will need a separate tree for each staged version, rather than a single graph like in the chain; but the model behind these structures is the same, and works in the same way.

As an example, let us consider this mock autograph:

![mock autograph](img/mock-autograph.png)

The base text, written on the regular lines of the notebook, is:

```txt
         10        20        30        40        50
123456789-123456789-123456789-123456789-123456789-123456789-1
he had a red hat for sure,|and long moustache and grey beard.
```

Say that our interpretation defines the following operations starting from the base text, which is the one first written down on the regular notebook lines:

1. insert "very " before "long": `32+["very " [*log="insert 'very ' before 'long'"]` (`v1`);
2. replace "red" with "green": `10x3="green" [*log="replace 'red' with 'green'"]` (`v2`);
3. delete "grey": `51x5- [*log="delete grey" *version^="A"]` (`v3`, staged as `A`);
4. move "for sure," before "he": `18x9>[1@fs [*log="move 'for sure,' before 'he'"]` (`v4`);
5. delete tail space: `17-@fs [*log="delete tail ' '"]` (`v5`);
6. insert space before "he": `1+[" "@fs [*log="insert space before 'he'"]` (`v6`);
7. swap moustache/beard: `37x9<>56x5 [*log="swap 'moustache' / 'beard'" *version^="B"]` (`v7`, staged as `B`).

From base text we get this tree:

1. root node;
2. nodes for `he had a red hat for sure,|and long moustache and grey beard.`.

We then run the operations:

1. insert "very" before "long": `he had a red hat for sure,|and [very ]long moustache and grey beard.`.
2. replace "red" with "green": `he had a [×red][green] hat for sure,|and [very ]long moustache and grey beard.`
3. delete "grey": `he had a [×red][green] hat for sure,|and [very ]long moustache and [×grey] beard.`
4. move "for sure," before "he": `[for sure,]he had a [×red][green] hat [×for sure,]|and [very ]long moustache and [×grey] beard.`
5. delete tail space: `[for sure,]he had a [×red][green] hat[× ][×for sure,]|and [very ]long moustache and [×grey] beard.`
6. insert space before "he": `[for sure,][ ]he had a [×red][green] hat[× ][×for sure,]|and [very ]long moustache and [×grey] beard.`
7. swap moustache/beard: `[for sure,][ ]he had a [×red][green] hat[× ][×for sure,]|and [very ]long [×moustache][beard] and [×grey] [×beard][moustache].`

To keep the example short, say that we are interested in exporting the final version only; anyway, whatever the number of versions the procedure is the same.

The next step is segmenting the text, i.e. merging nodes to create the maximum-length spans of texts sharing the same set of annotations.

## 2. Segmentation

For segmentation we must define which injected features are relevant. This is up to the user, and depends on the markup we want to generate. Trace features instead are always relevant because they define the text which works as the basis for markup.

First, we filter the tree so that we remove annotations on any whitespace nodes. This avoids redundant annotations, whence this tree (each character is a node, and we wrap annotated characters in `[...]` for `$ins` or `[×...]` for `$del`):

```txt
[for sure,] he had a [×red][green] hat [×for sure,]|and [very] long [×moustache][beard] and [×grey] [×beard][moustache].
```

The procedure is:

- let node = root's child node;
- do:
  - let start = node;
  - collect node's relevant features in a set;
  - while node's child (if any) is whitespace, or has the same set, move to it by setting node=node's child (after moving, stop if the node was LF, to preserve line breaks at segment end);
  - if node is not start, set start's text to the string resulting from appending the text of all the nodes from start to the last collected node, and remove all the collected nodes except start. This has the effect of merging all the collected nodes into one, with a longer text, which is the maximum span of text having the same relevant features;
  - node = child (if any) of last collected node: if no more children, stop; else continue the loop.

>The whitespace condition is due to the filter we applied, which removed features from whitespace characters. Such characters are considered as neutral for features, so they always match, or we would risk to break a segment too early.

So, for our example, we get these nodes:

1. root;
2. `for sure,`: `$ins` OPID 4;
3. ` he had a `;
4. `red`: `$del` OPID 2;
5. `green`: `$ins` OPID 2;
6. ` hat `
7. `for sure,`: `$del` OPID 4;
8. `and `
9. `very`: `$ins` OPID 1;
10. ` long `
11. `moustache`: `$del` OPID 7;
12. `beard`: `$ins` OPID 7;
13. ` and `
14. `grey`: `$del` OPID 3;
15. `beard`: `$del` OPID 7;
16. `moustache`: `$ins` OPID 7.
17. `.`

Note that this procedure, as usual in our approach, leverages a dynamic segmentation process where we do not set any specific segmentation unit in advance. We just calculate the best unit for the most efficient and compact output during export.

At the end of this segmentation process, our linear tree structure has not changed, but in most cases it now contains less nodes, because of merging. We can now proceed to materialize this structure into markup.

### 3. Materialization

- at the root node add `app` with child `lem` (in our case the final version is the "preferred" one) with child `mod`, with all the metadata you might want to encode;
- let node = root's child node;
- pop all the stack elements corresponding to OPID which are no longer present in node's features;
- if required, push a block element (e.g. `l`, `p`...) as child of `app`;
- do:
  - examine node's features:
    - `$ins`: push an `ins` element with its attributes;
    - `$del`: if next segment has a `$ins` with the same OPID, push a `subst`; then, push a `del` element with its attributes;
  - if node's text ends with LF and we are using a block, pop the whole stack and the block element if required; then, push the popped stack back recreating all the elements which were in it before closing the block;
  - node = node's child, stop if no child.

So for our example (I am just writing the essential markup here, without detailed metadata or custom renditions; also, I assume `l` as block element):

```xml
<app>
  <lem>
    <mod>
      <ins>for sure,</ins> he had a <subst><del>red</del><ins>green</ins></subst> hat <del>for sure,</del>and <ins>very</ins> long <subst><del>moustache</del><ins>beard</ins></subst> and <del>grey</del><subst><del>beard</del><ins>moustache</ins></subst>.
    </mod>
  </lem>
  <!-- other versions would go here... -->
</app>
```
