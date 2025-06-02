---
title: GVE to TEI
layout: default
parent: Export
nav_order: 7
---

# Example - Limerick

Let us now consider a more realistic example, by exporting the [limerick](../model/sample-limerick.md) discussed about modeling.

![The Book of Nonsense, page 7](../model/img/nonsense7.png)

The mock autograph created for this text is:

![limerick](../model/img/limerick.png)

with its base text:

```txt
there was an old man with a beard,
who cried: "It is just as I feared!
four larks and a wren,
two swans and a hen,
all built their nests in my beard!"
```

Our snapshot operations have been reconstructed as:

1. (v1) replace "cried" with "said";
2. (v2) replace "swans" with "crows";
3. (v3) insert "have" before "all" (for metrical reasons): version `alpha`;
4. (v4) swap verses 3-4;
5. (v5) replace "crows" with "owls": version `beta`.

So, there are 2 staged versions: an intermediate one (v3=`alpha`) and a final one (v5=`beta`).

## Stage 1

First we use a tree builder to create a "multi" tree from our snapshot. This tree has two branches for the two staged versions v3 and v5. So, its root is just a blank node, branching into two blank nodes for these two versions:

```txt
+ ⯈ [1.1]
  + ⯈ [2.1] →  (sub-id=v3, version=alpha)
  ...
  + ⯈ [2.2] →  (sub-id=v5, version=beta)
  ...
```

## Stage 2

Then we apply the linear merge filter to each branch of the tree. The resulting dump is:

```txt
+ ⯈ [1.1]
 + ⯈ [2.1] →  (sub-id=v3, version=alpha)
  + ⯈ [3.1] there was an old man with a beard, → #1: there was an old man with a beard,
   + ⯈ [4.1] ↵who  → #35: ↵who 
    + ⯈ [5.1] cried → #40: cried ($del="2fc9efa184 v0:v1 1", $del="2fc9efa184 v0:v1 2", $del="2fc9efa184 v0:v1 3", $del="2fc9efa184 v0:v1 4", $del="2fc9efa184 v0:v1 5")
     + ⯈ [6.1] said: "It is just as I feared! → #151: said: "It is just as I feared!
      + ⯈ [7.1] ↵four larks and a wren, → #71: ↵four larks and a wren, ($seg-in="5625cc16c5 v3:v4 1", $seg-in="5625cc16c5 v3:v4 2", $seg-in="5625cc16c5 v3:v4 3", $seg-in="5625cc16c5 v3:v4 4", $seg-in="5625cc16c5 v3:v4 5", $seg-in="5625cc16c5 v3:v4 6", $seg-in="5625cc16c5 v3:v4 7", $seg-in="5625cc16c5 v3:v4 8", $seg-in="5625cc16c5 v3:v4 9", $seg-in="5625cc16c5 v3:v4 10", $seg-in="5625cc16c5 v3:v4 11", $seg-in="5625cc16c5 v3:v4 12", $seg-in="5625cc16c5 v3:v4 13", $seg-in="5625cc16c5 v3:v4 14", $seg-in="5625cc16c5 v3:v4 15", $seg-in="5625cc16c5 v3:v4 16", $seg-in="5625cc16c5 v3:v4 17", $seg-in="5625cc16c5 v3:v4 18", $seg-in="5625cc16c5 v3:v4 19", $seg-in="5625cc16c5 v3:v4 20", $seg-in="5625cc16c5 v3:v4 21", $seg-in="5625cc16c5 v3:v4 22")
       + ⯈ [8.1] ↵two  → #94: ↵two  ($seg-in="5625cc16c5 v3:v4 23", $seg2-in="5625cc16c5 v3:v4 1", $seg2-in="5625cc16c5 v3:v4 2", $seg2-in="5625cc16c5 v3:v4 3", $seg2-in="5625cc16c5 v3:v4 4")
        + ⯈ [9.1] swans → #99: swans ($del="071f47de7f v1:v2 1", $del="071f47de7f v1:v2 2", $del="071f47de7f v1:v2 3", $del="071f47de7f v1:v2 4", $del="071f47de7f v1:v2 5")
         + ⯈ [10.1] crows and a hen, → #155: crows and a hen, ($seg2-in="5625cc16c5 v3:v4 5", $seg2-in="5625cc16c5 v3:v4 6", $seg2-in="5625cc16c5 v3:v4 7", $seg2-in="5625cc16c5 v3:v4 8", $seg2-in="5625cc16c5 v3:v4 9", $seg2-in="5625cc16c5 v3:v4 10", $seg2-in="5625cc16c5 v3:v4 11", $seg2-in="5625cc16c5 v3:v4 12", $seg2-in="5625cc16c5 v3:v4 13", $seg2-in="5625cc16c5 v3:v4 14", $seg2-in="5625cc16c5 v3:v4 15", $seg2-in="5625cc16c5 v3:v4 16", $seg2-in="5625cc16c5 v3:v4 17", $seg2-in="5625cc16c5 v3:v4 18", $seg2-in="5625cc16c5 v3:v4 19", $seg2-in="5625cc16c5 v3:v4 20")
          + ⯈ [11.1] ↵ → #115: ↵ ($seg2-in="5625cc16c5 v3:v4 21")
           + ⯈ [12.1] have  → #160: have  ($seg-out="c503ad6811 v2:v3 1", $seg-out="c503ad6811 v2:v3 2", $seg-out="c503ad6811 v2:v3 3", $seg-out="c503ad6811 v2:v3 4", $seg-out="c503ad6811 v2:v3 5")
            - ■ [13.1] all built their nests in my beard!" → #116: all built their nests in my beard!"
 
 + ⯈ [2.2] →  (sub-id=v5, version=beta)
  + ⯈ [3.1] there was an old man with a beard, → #1: there was an old man with a beard,
   + ⯈ [4.1] ↵who  → #35: ↵who 
    + ⯈ [5.1] cried → #40: cried ($del="2fc9efa184 v0:v1 1", $del="2fc9efa184 v0:v1 2", $del="2fc9efa184 v0:v1 3", $del="2fc9efa184 v0:v1 4", $del="2fc9efa184 v0:v1 5")
     + ⯈ [6.1] said: "It is just as I feared! → #151: said: "It is just as I feared!
      + ⯈ [7.1] ↵ → #71: ↵
       + ⯈ [8.1] four larks and a wren, → #72: four larks and a wren, ($del="5625cc16c5 v3:v4 1", $del="5625cc16c5 v3:v4 2", $del="5625cc16c5 v3:v4 3", $del="5625cc16c5 v3:v4 4", $del="5625cc16c5 v3:v4 5", $del="5625cc16c5 v3:v4 6", $del="5625cc16c5 v3:v4 7", $del="5625cc16c5 v3:v4 8", $del="5625cc16c5 v3:v4 9", $del="5625cc16c5 v3:v4 10", $del="5625cc16c5 v3:v4 11", $del="5625cc16c5 v3:v4 12", $del="5625cc16c5 v3:v4 13", $del="5625cc16c5 v3:v4 14", $del="5625cc16c5 v3:v4 15", $del="5625cc16c5 v3:v4 16", $del="5625cc16c5 v3:v4 17", $del="5625cc16c5 v3:v4 18", $del="5625cc16c5 v3:v4 19", $del="5625cc16c5 v3:v4 20", $del="5625cc16c5 v3:v4 21", $del="5625cc16c5 v3:v4 22")
        + ⯈ [9.1] ↵ → #94: ↵ ($del="5625cc16c5 v3:v4 23")
         + ⯈ [10.1] two  → #95: two  ($seg2-in="5625cc16c5 v3:v4 1", $seg2-in="5625cc16c5 v3:v4 2", $seg2-in="5625cc16c5 v3:v4 3", $seg2-in="5625cc16c5 v3:v4 4")
          + ⯈ [11.1] swans → #99: swans ($del="071f47de7f v1:v2 1", $del="071f47de7f v1:v2 2", $del="071f47de7f v1:v2 3", $del="071f47de7f v1:v2 4", $del="071f47de7f v1:v2 5")
           + ⯈ [12.1] crows → #155: crows ($del="e5747b06d6 v4:v5 1", $del="e5747b06d6 v4:v5 2", $del="e5747b06d6 v4:v5 3", $del="e5747b06d6 v4:v5 4", $del="e5747b06d6 v4:v5 5")
            + ⯈ [13.1] owls → #165: owls ($seg-out="e5747b06d6 v4:v5 1", $seg-out="e5747b06d6 v4:v5 2", $seg-out="e5747b06d6 v4:v5 3", $seg-out="e5747b06d6 v4:v5 4")
             + ⯈ [14.1]  and a hen, → #104:  and a hen, ($seg2-in="5625cc16c5 v3:v4 10", $seg2-in="5625cc16c5 v3:v4 11", $seg2-in="5625cc16c5 v3:v4 12", $seg2-in="5625cc16c5 v3:v4 13", $seg2-in="5625cc16c5 v3:v4 14", $seg2-in="5625cc16c5 v3:v4 15", $seg2-in="5625cc16c5 v3:v4 16", $seg2-in="5625cc16c5 v3:v4 17", $seg2-in="5625cc16c5 v3:v4 18", $seg2-in="5625cc16c5 v3:v4 19", $seg2-in="5625cc16c5 v3:v4 20")
              + ⯈ [15.1] ↵ → #115: ↵ ($seg2-in="5625cc16c5 v3:v4 21")
               + ⯈ [16.1] two crows and a hen, → #95: two crows and a hen, ($del="5625cc16c5 v3:v4 1", $del="5625cc16c5 v3:v4 2", $del="5625cc16c5 v3:v4 3", $del="5625cc16c5 v3:v4 4", $del="5625cc16c5 v3:v4 5", $del="5625cc16c5 v3:v4 6", $del="5625cc16c5 v3:v4 7", $del="5625cc16c5 v3:v4 8", $del="5625cc16c5 v3:v4 9", $del="5625cc16c5 v3:v4 10", $del="5625cc16c5 v3:v4 11", $del="5625cc16c5 v3:v4 12", $del="5625cc16c5 v3:v4 13", $del="5625cc16c5 v3:v4 14", $del="5625cc16c5 v3:v4 15", $del="5625cc16c5 v3:v4 16", $del="5625cc16c5 v3:v4 17", $del="5625cc16c5 v3:v4 18", $del="5625cc16c5 v3:v4 19", $del="5625cc16c5 v3:v4 20")
                + ⯈ [17.1] ↵ → #115: ↵ ($del="5625cc16c5 v3:v4 21")
                 + ⯈ [18.1] four larks and a wren, → #72: four larks and a wren, ($seg-in="5625cc16c5 v3:v4 1", $seg-in="5625cc16c5 v3:v4 2", $seg-in="5625cc16c5 v3:v4 3", $seg-in="5625cc16c5 v3:v4 4", $seg-in="5625cc16c5 v3:v4 5", $seg-in="5625cc16c5 v3:v4 6", $seg-in="5625cc16c5 v3:v4 7", $seg-in="5625cc16c5 v3:v4 8", $seg-in="5625cc16c5 v3:v4 9", $seg-in="5625cc16c5 v3:v4 10", $seg-in="5625cc16c5 v3:v4 11", $seg-in="5625cc16c5 v3:v4 12", $seg-in="5625cc16c5 v3:v4 13", $seg-in="5625cc16c5 v3:v4 14", $seg-in="5625cc16c5 v3:v4 15", $seg-in="5625cc16c5 v3:v4 16", $seg-in="5625cc16c5 v3:v4 17", $seg-in="5625cc16c5 v3:v4 18", $seg-in="5625cc16c5 v3:v4 19", $seg-in="5625cc16c5 v3:v4 20", $seg-in="5625cc16c5 v3:v4 21", $seg-in="5625cc16c5 v3:v4 22")
                  + ⯈ [19.1] ↵ → #94: ↵ ($seg-in="5625cc16c5 v3:v4 23")
                   + ⯈ [20.1] have  → #160: have  ($seg-out="c503ad6811 v2:v3 1", $seg-out="c503ad6811 v2:v3 2", $seg-out="c503ad6811 v2:v3 3", $seg-out="c503ad6811 v2:v3 4", $seg-out="c503ad6811 v2:v3 5")
                    - ■ [21.1] all built their nests in my beard!" → #116: all built their nests in my beard!"
```

Summarizing the above data (and using `_` for an initial/final whitespace and `↵` for LF):

- v3:
  - `there was an old man with a beard,`
  - `↵who_`
  - `cried`
  - `said: "It is just as I feared!`
  - `↵four larks and a wren,`
  - `↵two_`
  - `swans`
  - `crows and a hen,`
  - `↵`
  - `have_`
  - `all built their nests in my beard!"`

- v5:
  - `there was an old man with a beard,`
  - `↵who_`
  - `cried`
  - `said: "It is just as I feared!`
  - `↵`
  - `four larks and a wren,`
  - `↵`
  - `two_`
  - `swans`
  - `crows`
  - `owls`
  - `_and a hen,`
  - `↵`
  - `two crows and a hen,`
  - `↵`
  - `four larks and a wren,`
  - `↵`
  - `have_`
  - `all built their nests in my beard!"`

As you can see, here some segments just contain a LF character. As we're going to output XML, such segments are just noise, because we are not going to represent them as newline characters; we will rather end a verse whenever there is one. So, we're going to use another filter to remove these nodes while preserving the newline information.

## Stage 3

To this end, we apply another linear text tree filter, the _block linear text tree filter_. This splits nodes whenever they include a LF character, and removes nodes with one or more LF characters, while adding to the preceding node a feature named `eol-tail`, which marks any segment placed at the end of a line.

The resulting nodes are now:

- v3:
  - `there was an old man with a beard,` with `eol-tail`
  - `who_`
  - `cried` with `$del` (v0:v1)
  - `said`: with `$seg-out` (v0:v1)
  - `: "It is just as I feared!` with `eol-tail`
  - `four larks and a wren,` with `$seg-in` (v3:v4) and `eol-tail`
  - `two_` with `$seg2-in` (v3:v4)
  - `swans` with `$del` (v1:v2)
  - `crows` with `$seg-out` (v1:v2), `$seg2-in` (v3:v4)
  - `_and a hen,` with `$seg2-in` (v3:v4) and `eol-tail`
  - `have_` with `$seg-out` (v2:v3)
  - `all built their nests in my beard!"` with `$anchor` (v2:v3)

```txt
there was an old man with a beard
who {cried}1[said]1: "It is just as I feared!`
(four larks and a wren,)4
(two )4{swans}2([crows]2)4 and a hen,
[have]3 all built their nests in my beard!"
```

- 4: the input of the operation past v3.
- 3: insert `have_` before `all`
- 2: replace `swans` with `crows`
- 1: replace `cried` with `said`

- v5:
  - `there was an old man with a beard,` with `eol-tail`
  - `who_`
  - `cried` with `$del` (v0:v1)
  - `said`: with `$seg-out` (v0:v1)
  - `: "It is just as I feared!` with `eol-tail`
  - `four larks and a wren,` with `$del` (v3:v4) and `eol-tail`
  - `two_` with `$seg2-in` (v3:v4)
  - `swans` with `$del` (v1:v2)
  - `crows` with `$del` (v4:v5)
  - `owls` with `$seg-out` (v4:v5)
  - `_and a hen,` with `$seg2-in` and `$seg-out` (v3:v4), and `eol-tail`
  - `two crows and a hen,` with `$del` (v3:v4) and `eol-tail`
  - `four larks and a wren,` with `$seg-in` and `$seg2-out` (v3:v4), and `eol-tail`
  - `have_` with `$seg-out` (v2:v3)
  - `all built their nests in my beard!"` with `$anchor` (v2:v3)

```txt
there was an old man with a beard,
who {cried}1[said]1: "It is just as I feared!
{four larks and a wren,}4
(two )4{swans}2{crows}5[owls]5
([ and a hen, ])4
{two crows and a hen,}4
([four larks and a wren,])4
[have ]3all built their nests in my beard!"
```

- 5: replace `crows` with `owls`
- 4: swap verses `four larks and a wren,` / `two crows and a hen`
- 3: insert `have_` before `all`
- 2: replace `swans` with `crows`
- 1: replace `cried` with `said`

TODO
