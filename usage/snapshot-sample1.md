---
title: Snapshot Sample 1
layout: default
parent: Usage
nav_order: 4
---

# Snapshot Sample 1

![illustration](img/thermopylae-ill.png)

```txt
There was an Old Man of Thermopylae,
Who never did anything properly;
  But they said, "If you choose,
  To boil eggs in your shoes,
You shall never remain in Thermopylae".
```

- adapted from Edward Lear. _A Book of Nonsense._ London: Routledge, Warne & Routledge, 1861. Plate 72.

In this example, we use this mock manuscript text:

![manuscript](img/thermopylae-facs.png)

## Description

Let us pretend this is an autograph manuscript shwowing 3 hands with 3 different colors, in this order:

1. black, which is the base text, following the printed lines in the notebook page.
2. blue.
3. green.

Adopting the convention to describe the various operations on this text from top to bottom and from left to right within each hand, we can provide the following reconstruction.

> Note that the convention does not apply to numberings and other signs added outside the epigram's text to make it an item of some collection. These are usually encoded at the end of each hand.

- **black hand**:

(1) `old` → `Old`. We reconstruct this as an "immediate" correction, i.e. the same hand immediately recognized the lowercase as an error and corrected it into uppercase while writing.

(2) `Termopylae` → `Thermopylae`. The same hand introduces the missing `h`.

(3) `;` → `,` after `Termopylae`. The semicolon here gets changed into a comma by simply crossing out the dot above the comma in the semicolon. This is a compendiary way of representing this replacement, much more effective in handwriting.

(4) delete the comma after `But`.

(5) add a comma after `said`. This is logically connected to the previous operation (we are moving the comma).

(6) delete `fish and`. The hand scribbled on top of these words to mean their deletion.

(7) figs → eggs. The hand here just wrote a smaller `eg` on top of `fi` in `figs`.

(8) `our` → `your`. Here the hand boxed out `our` and added `your` at the right edge of the verse, with the same box, meaning that this new text should replace the original one.

(9) `shoe` → `shoes`. Here the hand just added a smaller `s` at the top right of `shoe`.

(10) reorder `Never you shall` into `You shall never`. This is very compressed in the manuscript, which just added smaller numbers on top of these words to mean their reordering.

- **blue hand**:

(11) add the missing dot above `i` in `did`.

(12) `If` → `Should`. The new word was written above the old one, which was crossed out.

(13) add a circled dot drawing at the beginning of the epigram, to mark it as an item of some collection.

(14) add the number `2` on top of the epigram.

- **green hand**:

(15) annotate `ai` in `remain` as long (a metrical annotation).

(16) add a vertical stroke after `remain` to mark colometry.

(17) `2` → `3` as the epigram number. `3` was written to the right of the original number which was crossed out.

## Base Text

Let us start encoding this snapshot. The first step is defining the base text, representing the starting point for all the transformations:

```txt
There was an old Man of Termopylae;
He never dıd anything properly;
But, they said "If you choose,
To boil fish and figs in our shoe,
Never you shall remain in Thermopylae".
```

👉 Hands-on:

1. create a new item with title `Thermopylae` and the description you like. Ensure its facet is `snapshot`, and save it.
2. edit the newly created item, and add a new snapshot part to it.
3. edit the newly added snapshot part.
4. in the text tab, expand `base text` and click the `base text` button to enter the new text. In the popup window, paste the above text and click `Set`.
5. it's a good idea to save the snapshot part by clicking the bottom `save` button.

![base text](img/ex1-01.png)

## Operations

Let us now define the operations following our description.

### Indentation

Indentation is encoded just like any other aspect of the visual layer, i.e. via features.

To this end, add an **annotate** operation with a `char offsets` feature which contains an expression defining all the horizontal and/or vertical offsets for all characters which start an indented or otherwise offset text.

👉 Hands-on:

1. in the `operations` tab, click the `add operation` button.
2. pick as type `annotate`, set `at`=1 and `run`=1 (conventionally we just target the first character of the text).
3. click the `+feature` button and under name pick the `char offsets` feature; you can also type any characters of this name in the top search box (which opens when you click the name dropdown) to quickly locate it. Then in `value` type `69:x=100 100:x=100`. Let us dissect this value (look at the [rendition documentation](../model/rendition.md) for more):
   - `69:` means that you are targeting character ID 69.
   - `x=100` means that you are setting a horizontal offset equal to 100 pixels (this is just a convenient value, you can change it as you want).
   - again, the next expression after space says to add a horizontal offset equal to 100 to character with ID=100.

4. save the feature (round check button) and the operation (`save` button). The operation appears in the list. Before looking at the rendition, let us add an alteration to the base text with the next operation.

> The separation of indentation from base text here is just a matter of convenience to ensure a uniform model. We set the input text as plain text, and then separate its visual aspect (like indents) on the visual layer via a rendition feature.

### Black Hand

(1) **replace** `old` → `Old`. Here we literally replace `o` with `O`, adding an `immediate` feature with value `1` (which here represents the true value of a boolean feature).

👉 Hands-on:

1. in the base text display, check the `o` character you want to replace. This has ID=14. If you click it, the new operation you are going to add will pick the current selection as its coordinates; or you can just go ahead, add a new operation, and then set the coordinates.
2. in the `operations` tab, click the `add operation` button.
3. pick as type `replace`, set `at`=14 and `run`=1, and enter the new text `O` in `value`.
4. let us now add a couple of features:
   - _immediate_: click the `+feature` button and under name pick the `immediate` feature. Then in `value` select `1`, which is the only possible value for this feature, which is a boolean feature. Then, click the round checkmark button to add this feature.
   - _log_: conventionally we always add a log feature to each operation to quickly describe it. So repeat the above procedure, pick `log` as the feature name, and in `value` type something like `uppercase 'old'`.
   - finally, click `save` to save the operation, and the bottom `save` button to save the whole part if you want to save it; or just the run button of the newly added operation. This will update the rendition section.

5. in the rendition section, click the play button and look at the `o`: you will see it replaced by an overwritten `O` after a few instants, just like it happened in our manuscript.

![operation editor](img/ex1-02.png)

As for the rendition, there is no need for rendition-oriented features here, because the defaults are just fine. The `O` appears overwritten and has the same black color of the base text, because the default position for added text is origin and the default text color is black.

![rendition](img/ex1-03.png)

Note the indents, and on the left the 3 colored rectangles representing `v0` (=base text), `v1` (indentations), `v2` (replacement).

(2) **replace** `Termopylae` → `Thermopylae`: here we add `h` after `T`. So, we can encode this as an add-after operation. Note that we might as well encode it as a replacement considering the whole word (`Termopylae` → `Thermopylae`), but usually at least in GVE we aim to a representation very close to the visual (diplomatic) layer.

We thus repeat the above procedure: add a new operation, set type=add-after and `at`=25 (the `T` of `Termopylae`). As for features, add:

- `position`=`north-east` because the `h` is positioned at the top-right of `T`.
- `font size`=`18` to make it smaller. The default font size in this project is 24.
- `log`=`add 'h' after 'T' in 'Termopylae'`.

If you play the rendition again, it ends with this image:

![rendition](img/ex1-04.png)

(3) **replace** `;` → `,` after `Termopylae`. Visually, in our manuscript we just have a descending diagonal stroke on the dot above the comma of the semicolon. We now want to encode exactly this on the visual side, while still preserving the effective text alteration, which is a replacement (comma instead of semicolon). So, first we start with the text layer, adding a replace operation at 35 with run 1 (=`;`) with value `,`. Then, we add these features:

- `hints`=`diagonal stroke down`: this is the sign used to mean the deletion of the dot. As for all signs, it comes from our catalog of hints.
- `log`=`cross out dot of ';' after Termopylae`
- `foreground color`=`black`: this is the color for the hint. You must always explicitly define a color for it (unless you encode it in the hints catalog).
- `hint Y offset`=`-0.25th`: this moves the hint up for about one quarter the average character height; otherwise, given its default origin position, the line would extend up to the comma below the dot.
- `overridden text value`=(empty): this tells the renderer to not display the added text, here comma. This would be pointless, because the comma is already there. So, textually we replace `;` with `,`, but visually we just draw a line on the dot of `;`, without rewriting the comma. This is exactly what we encode with these features.

![rendition](img/ex1-05.png)

(4) **delete** the comma after `But`. We could also represent this with a move operation, but in this example we aim at maximum granularity. At any rate, we consider this delete logically connected to the next addition, so we add a group ID to this operation (with value `move-comma`): we will add the same group ID to the next operation too, thus virtually grouping them.

So here the operation is of type delete, at 72 with run 1. Its features are:

- `hints`=`diagonal stroke up`
- `foreground color`=`black`
- `hint Y offset`=`0.25th`: this moves the stroke one-quarter the average character height down.
- `log`=`delete comma after 'But'`

![rendition](img/ex1-06.png)

(5) **add comma after** `said`: we add an add-after operation at 82, with group ID=`move-comma` and these features:

- `position`=`east`: place the comma at the right of `said`.
- `log`=`add comma after 'said'`

![rendition](img/ex1-07.png)

(6) **delete** `fish and` plus its following space. Note that we include the space because otherwise the resulting text would have 2 spaces, the one before `fish` and the other after `and`. So we add a delete operation at 108 with run 9 and these features:

- `hints`=`scribble (wavy)`
- `foreground color`=`black`
- `hint X scale`=`0.8`: this scales down the scribble drawing width to 80% so that it does not cover the end part of `and`, like in the manuscript. Of course this is just a diplomatic detail and we could omit it as well.
- `log`=`delete 'fish and_'`

![rendition](img/ex1-08.png)

(7) **replace** `fi` in `figs` with `eg`, whence `eggs`. This is a replacement operation at 117 with run 2 with value=`eg` and these features:

- `position`=`north`: the text is added above the old one.
- `font size`=`18`: the added text is smaller.
- `hints`=`diagonal stroke up`: the stroke on `fi`.
- `foreground color`=`black`
- `log`=`replace 'fi' of 'figs' with 'eg'`

![rendition](img/ex1-09.png)

(8) **replace** `our` with `your`. Note that we could just add `y` before `our` as well; but this representation is closer to the visual layer, where the hand boxed the original word and fully wrote the new one, boxed too.

So, to be consistent with a granular diplomatic representation, here we can define 3 logically grouped operations (all sharing an `our-2-your` group ID):

- **annotate** `our` (i.e. at 125x3) with a box. Features:
  - `hints`=`box`
  - `foreground color`=`black`
  - `log`=`box 'our'`
- **replace** `our` with `your` (same coordinates, value=`your`). Features:
  - `position`=`east`: position the new text to the right of the verse.
  - `X offset`=`1tw`: offset the new text to the right to leave some gap, like in the manuscript, approximately equal to the average width of 1 character.
  - `displaced text span`=`129x5`: place the added text with reference to `shoe,` rather than with reference to the targeted text. This allows positioning it at the right of the whole verse. Otherwise, it would be positioned at the right of the replaced text in that verse, overwriting what follows.
  - `log`=`replace 'our' with 'your'`
- **annotate** `your` with a box.

![rendition](img/ex1-10.png)

(9) **add** `s` after `shoe` (add-after at 132, value=`s`). Features:

- `position`=`north-east`: position at top-right of `shoe`.
- `font size`=`18`: make added text smaller.
- `Y offset`=`0.25th`: slightly offset down.
- `log`=`add 's' after 'shoe'`

![rendition](img/ex1-11.png)

(10) reorder `Never you shall` into `You shall never` by adding numbers on top of these words (3, 1, 2). This is encoded with 6 grouped operations (group ID=`reorder`):

- **annotate** `you` with `1` (at 141, run 3). This annotation uses a hint including a placeholder: the hint represents some text written on top of the reference text as an annotation which is not going to become part of the text, like here the number `1`. The hint is `note (interlinear above)` meaning a note placed above its reference text, typically between two lines. This means the text will be smaller to fit into the interlinear space, and it will be placed above the reference text. The text itself is provided to the hint by another feature, named `note`. So, features are:
  - `hints`=`note (interlinear above)`
  - `note`=`1`: the value to fill the text placeholder in the hint.
  - `log`=`add '1' on 'you'`
- **annotate** `shall` with `2` (at 145, run 5). Features:
  - `hints`=`note (interlinear above)`
  - `note`=`2`: the value to fill the text placeholder in the hint.
  - `log`=`add '1' on 'you'`
- **annotate** `Never` with `3` (at 135, run 3). Features:.
  - `hints`=`note (interlinear above)`
  - `note`=`3`: the value to fill the text placeholder in the hint.
  - `log`=`add '1' on 'you'`
- **move** `Never_` after `shall_`. Until now we have just added numbers on top of words, to encode the visual layer. We now need to apply the change meant by them by reordering the text as indicated by those numbers. This implies moving `Never` and its following space after `shall` and its following space, whence `you shall Never`. Features:
  - `log`=`move 'Never_' after 'shall_'`
- **replace** `y` with `Y` in `you`. This is implicitly required by the fact that now `you` has become the first word of the sentence and must be capitalized to get a uniform text. Features:
- `overridden text value`=(empty): we need to tell the renderer to avoid displaying the new text `Y`, so we can reproduce the facsimile. This is just an operation implicit in the reordering, and is not visible in the manuscript.
- `log`=`uppercase 'y' of 'you'`
- **replace** `N` with `n` in `Never`. For the same reason, we need to lowercase `Never` which now is no longer the first word of the sentence. Features:
  - `overridden text value`=(empty) as above.
  - `stage name`=`AS1`: this is an important feature: it says that we have completed all the operations belonging to the black hand, which ended with a specific stage of the text we consider as self-contained and meaningful. All the operations we have encoded so far were just steps towards this waypoint. We thus flag the stage reached at this point as the first alteration stage (`AS1`=alteration stage 1 is just a convention, you can name the stage as you want).
  - `log`=`lowercase 'N' of 'Never'`

> 💡 Note that the operations adding numbers are all equal except for their coordinates and the note's text. So you can use the clone button of the first such operation to quickly generate the others and then adjust their properties accordingly.

![rendition](img/ex1-12.png)

If you look at the rendition while it plays, you can note that the visual result of the operations affecting text is no different from that after the operations which add number.
