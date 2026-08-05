# Snapshot Sample 1

![illustration](img/thermopylae-ill.png)

> There was an Old Man of Thermopylae,
> Who never did anything properly;
>   But they said, "If you choose,
>   To boil eggs in your shoes,
> You shall never remain in Thermopylae".

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

(4) move `,` after `But` so that it appears after `they`. The hand crossed out the first comma and added a second one.

(5) delete `fish and`. The hand scribbled on top of these words to mean their deletion.

(6) figs → eggs. The hand here just wrote a smaller `eg` on top of `fi` in `figs`.

(7) `our` → `your`. Here the hand boxed out `our` and added `your` at the right edge of the verse, with the same box, meaning that this new text should replace the original one.

(8) `shoe` → `shoes`. Here the hand just added a smaller `s` at the top right of `shoe`.

(9) reorder `Never you shall` into `You shall never`. This is very compressed in the manuscript, which just added smaller numbers on top of these words to mean their reordering.

- **blue hand**:

(10) add the missing dot above `i` in `did`.

(11) `If` → `Should`. The new word was written above the old one, which was crossed out.

(12) add a circled dot drawing at the beginning of the epigram, to mark it as an item of some collection.

(13) add the number `2` on top of the epigram.

- **green hand**:

(14) annotate `ai` in `remain` as long (a metrical annotation).

(15) add a vertical stroke after `remain` to mark colometry.

(16) `2` → `3` as the epigram number. `3` was written to the right of the original number which was crossed out.

## Base Text

Let us start encoding this snapshot. The first step is defining the base text, representing the starting point for all the transformations:

```txt
There was an old Man of Termopylae;
He never dıd anything properly;
  But they said, "If you choose,
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

### Black Hand

(1) `old` → `Old`. Here we literally replace `o` with `O`, adding an `immediate` feature with value `1` (which here represents the true value of a boolean feature).

👉 Hands-on:

1. in the base text display, check the `o` character you want to replace. This has ID=14. If you click it, the new operation you are going to add will pick the current selection as its coordinates; or you can just go ahead, add a new operation, and then set the coordinates.
2. in the `operations` tab, click the `add operation` button.
3. pick as type `replace`, set `at`=14 and `run`=1, and enter the new text `O` in `value`.
4. let us now add a feature for "immediate": click the `+feature` button and under name pick the `immediate` feature; you can also type any characters of this name in the top search box (which opens when you click the name dropdown) to quickly locate it. Then in value select `1`, which is the only possible value for this feature, which is a boolean feature. Then, click the round checkmark button to add this feature. You will see it in the features list. Finally, click save to save the operation, and the bottom save button to save the whole part if you want to save it; or just the run button of the newly added operation. This will update the rendition section.
5. in the rendition section, click the play button and look at the `o`: you will see it replaced by an overwritten `O`, just like in our manuscript.  
