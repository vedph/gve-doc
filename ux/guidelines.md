---
title: VEdition Guidelines
layout: default
parent: User Experience
nav_order: 2
---

# Guidelines

- [Guidelines](#guidelines)
  - [Principles](#principles)
  - [Rendition](#rendition)
    - [Rendition Entities](#rendition-entities)
    - [Added Elements Rendition](#added-elements-rendition)
    - [Displacement](#displacement)
    - [Text Rendition Features](#text-rendition-features)
      - [Hint Rendition Features](#hint-rendition-features)

Here we collect some practical guidelines agreed for the VEdition project.

The team choices are:

- adopt a **symbolic** (feature-based) rather than graphic (SVG-based) representation for diplomatic visuals. While the two types of representation are not mutually exclusive, purely economical considerations forced the team to opt for the more economical solution. So, rather than literally drawing visuals on top of a facsimile, we are assigning a number of typological features to each operation, like shape (e.g. horizontal stroke, vertical stroke, diagonal stroke, etc.), color (e.g. black ink, orange ink, etc.), position (e.g. interlinear above, interlinear below, baseline above, etc.), and so forth. These will be defined in taxonomies used by the Cadmus-based editor for snapshots.

- adopt a **diplomatic**-first definition of operations, meaning that wherever there is a choice between a purely logical, text-focused representation of an edit and a diplomatic-based one aimed at representing all the visual counterparts of the edit, the latter is to be preferred.

## Principles

The second choice can be formalized with these two complementary principles:

(P1) **respect diplomatic features**. When what could be envisaged as a single operation has different visual parts, rather split this into multiple operations, assigning diplomatic features accordingly.

(P2) **do as much as possible with the minimum number of operations**. This principle comes second, so if it conflicts with P1, P1 wins.

Let us make an example: say we have an epigram where the word `Wägen` is changed to its singular form `Wägt`. From a text-focused point of view, we could just represent it as a word replacement: replace `Wägen` with `Wägt`.

Note that one could also replace `en` with `t` to get the same result; but replacing the whole word would probably be functionally better in terms of the potential usages of this information. For instance, say that for searching or filtering purposes we are going to extract in a list all the operations from the texts:

- if we replaced `Wägen` with `Wägt`, this list entry would include these two words.
- if instead we replaced `en` with `t`, this list entry would include `en` and `t`, which are clueless when extracted from their context. This is why in a text-focused perspective we would prefer the whole-word scope for the replace, even if both replacements will of course output the same text.

Anyway, let us say that on the sheet we see these signs:

- a `t` overwritten on `e` with orange ink.
- a vertical stroke on the final `n` with orange ink.

Now, here we have _two_ different visuals (a letter and a vertical stroke) for different aspects of what according to P2 we would consider as a _single_ replace operation. So, here P2 is in conflict with P1; which means that P1 wins over P2. So, rather than a single replace operation, we would have two operations:

1. replace `e` with `t`. Diplomatic features would tell us that this is overwritten with orange ink.
2. delete `n`. Diplomatic features would tell us that this is represented by a vertical stroke with orange ink.

This of course reflects in greater detail what we have on our sheet: the writing process was right a "patch" of the existing word to transform it from its plural to its singular form, and this happened with two operations: overwrite `e`, and delete `n`.

So, P1 here wins over P2, and we prefer 2 operations rather than a one. Given that both operations are logically connected, we can assign both to the same group by giving them the same group ID. Group IDs are arbitrarily chosen strings one can assign to any number of operations within the same snapshot to say that they are logically connected. Such IDs by convention should include only lowercase letters without diacritics, dashes, and digits, and typically provide some human-friendly clue about their meaning.

Thus, we could assign a group ID like `waegt` to both the operations (replace and delete). This would connect them together, while still preserving the preference for P1 over P2.

## Rendition

The rendition of a feature-based symbolic visualization is a very complex topic. Here we just summarize the main terms to serve as a reference for editors.

>The whole rendition happens using [SVG](https://developer.mozilla.org/en-US/docs/Web/SVG), a standard XML dialect used for drawing geometries and text.

### Rendition Entities

The visualization component renders these entities:

- **base text**: the `v0` text, the input text to start with to transform it via editing operations. In most cases, in real world it is visually identified by the text written on regular rows, with the same ink, by the same hand.
- **additional text**: the text successively added by insert/replace operations. This is drawn character by character, just like the base text, but its size, position, and rendering features are calculated according to the operation's features and the default rendering settings.
- **hints** which represent the visual counterparts of each editing operation. Each operation has 0-N hints, defined in the order they are meant to be displayed. Each hint can be any shape, e.g. a stroke on a word corresponding to a delete operation for that word, or also some static text like an annotation. Hints too are defined via SVG.

So there is a fundamental difference between the SVG elements added to represent the base text and all the others added later, i.e. additional text and hints:

- the base text elements are added all at once at the very start, following a predictable layout (even with some optional overrides defined by init features).
- the added elements are added later, when moving from one version to the next one; their position and size are computed according to their rendition properties and features, and refer to some specific portion of the base text. When they are added, typically they enter with some specific animation. Animation will be everywhere in the visualization, because here it bears a markedly semantic value, representing the changes affecting the text in its transformation over time.
- all elements added stay there forever, thus accumulating when moving from one version to the next one. This implies that if moving backwards they are removed.

While additional text is just text drawn according to its settings, hints can be any sort of graphical signs (strokes, boxes, circles, callouts, etc.). It is not possible to predict all the possible shapes, which will vary in dramatic ways among texts of the same or different projects. So, the system adopts a catalog of graphical resources for hints and their animations, too. This catalog is part of the settings received by the software component in charge of the visualization, and is designed for reuse. Each hint has an SVG code fragment with all sorts of drawings required to represent its appearance, and a corresponding animation for its entrance. Animations are fragments of JavaScript code, dynamically incorporated by the component at runtime. So, in the end we define a gallery of SVG and JS code fragments, each with its identifier, and features will just pick them as desired. At any time we can add new items to this gallery.

### Added Elements Rendition

We thus call both _additional text_ and _hints_ as **added elements**, in contrast with **base elements** which exist since the beginning and are added only at once and never removed.

The whole rendition is functional to represent annotations on top of the base text. So, all the added elements define their position (and for hints also their size) with **reference** to the portion of text affected by their operation. For instance, a delete operation refers to the characters it deletes; a replace operation to the characters it replaces; a move or swap operation to the character it moves; an add operation to the anchor character before or after it new characters are added; and an annotate operation to the characters it annotates.

If we consider how these reference text characters are rendered (as base text), i.e. character by character where each character is an SVG text element, we can define the reference position in terms of what we call **reference text bounding rectangle** (RBR for short):  this is _the rectangle including all the bounding rectangles of all the subsequent characters displayed for the text referenced by an operation_. So if a segment to be deleted is "def", its bounding rectangle is the rectangle including all the rectangles for "d", "e", "f". So, placing added text or hints is a matter of placing the imaginary rectangle encompassing all their content (let us call it EBR, the added element bounding rectangle) relative to this imaginary RBR.

⚠️ Note that if the segment happens to be _split_ between lines, because it includes a LF, then there will be **multiple RBR's**, one for each portion of the segment text. In this case:

- the **additional text** just refers to the first RBR.
- the **hint** is _repeated_ for each RBR, except the hint having a placeholder element. So, more formally, for hints:

- for each reference bounding rectangle:
  - for each hint:
    - unless the hint is a placeholder hint and has already been drawn once, draw it and animate it.

>The logic of this repetition for hints (and not for additional text or for placeholder hints) is that in most cases hints represent decorations on top of their reference text, e.g. a strikethrough on top of the text being deleted; so, if this text happens to be split between two lines, we still need to apply strikethrough to all the reference text, and not only to a portion of it. The exception for the placeholder element is due to the fact that the placeholder element is an element including dynamically defined text (e.g. the text being added by a replace/insert operation), and this text should not be repeated.

The values for the **position** relative to the RBR are named after the cardinal points of a compass position, and refer to a position of the EBR "outside" or "inside" the RBR:

- **outside**:
  - `n`: EBR-bottom edge aligned with RBR-top edge; horizontally centered.
  - `s`: EBR-top edge aligned with RBR-bottom edge; horizontally centered.
  - `w`: EBR-right edge aligned with RBR-left edge; vertically centered.
  - `e`: EBR-left edge aligned with RBR-right edge; vertically centered.
  - `nw`: EBR-bottom-right corner aligned with RBR-top-left corner.
  - `ne`: EBR-bottom-left corner aligned with RBR-top-right corner.
  - `sw`: EBR-top-right corner aligned with RBR-bottom-left corner.
  - `se`: EBR-top-left corner aligned with RBR-bottom-right corner.

This ASCII-picture diagram shows a smaller EBR positioned with reference to a bigger RBR (the different sizes are just used to make the diagrams smaller and have no implication on the elements sizes):

```txt
+---+       +---+       +---+
|nw |       | n |       |ne |
+---+-------+---+-------+---+
    |                   |
+---+                   +---+
| w |                   | e |
+---+                   +---+
    |                   |
+---+-------+---+-------+---+
|sw |       | s |       |se |
+---+       +---+       +---+
```

- **inside**:
  - `inw`=northwest corner: EBR-top-left corner aligned with RBR top-left-corner.
  - `ine`=northeast corner: EBR-top-right corner aligned with RBR top-right-corner.
  - `isw`=southwest corner: EBR-bottom-left corner aligned with RBR bottom-left-corner.
  - `ise`=southeast corner: EBR-bottom-right corner aligned with RBR bottom-right-corner.
  - `o` for origin: the EBR origin (=center) aligned with the RBR origin (=center). This is the _default_ when position is not explicitly specified.

The corresponding ASCII-picture diagram is:

```txt
+---+-----------+---+
|inw|           |ine|
+---+   +---+   +---+
|       | o |       |
+---+   +---+   +---+
|isw|           |ise|
+---+-----------+---+
```

⚠️ Note that hints have a crucial difference with reference to the above description: when they include a **handle** element, their position is not defined by their bounding rectangle, but rather by the bounding rectangle of this handle element. Other than that, in the end it always is a bounding rectangle to use for positioning with reference to the RBR, whether it is computed like for other elements, or overridden by the handle.

### Displacement

Occasionally, added text or hints may be displaced with reference to the RBR. For instance, in an epigram it happens that a word in the internal lines of an epigram is boxed, and the word replacing it is not written near it, but rather in the free space above the whole epigram, with a similar box which makes it clear their connection.

In this case, in our model we still use the same logic: the only difference is that we pick another set of characters as the foundation for the RBR. In the above example, these characters will be the characters of the first line of the epigram base text which happen to be aligned with the new boxed word. This is accomplished with _displacement-span_ features, assigned to the nodes which should be used for calculating the RBR for that operation in place of the default RBR (based on the text referenced by that operation).

### Text Rendition Features

>In all the features lists that follow, we always use the feature ID, rather than the human-friendly label used in the editor to represent it. In most cases anyway the label just reflects this ID, e.g. `r_hints` has label `hints`, etc. so editor users should have no issues in finding them.

- `r_char-offsets` (this is applicable only as an init-feature): the character offsets for rendering some specific text nodes (and thus indirectly also all the character nodes following it, up to line end). In most cases, this is used to change line layout by adding indent, or increasing or decreasing space between lines. Usually, text is rendered in a regular, predictable way, just like text is normally rendered in frame: one character after another on the same baseline, until a newline character is found. When this happens, a new line starts below the previous one, left-aligned with it. This is the default layout which assigns the position to each baseline, according to general settings defined in the component's settings (default font size, default spacing between lines, etc.). This default position can be changed for specific characters by the value of this feature, whose format repeats the pattern `ID:y=...,x=...` (where we can specify only `x`, only `y`, or both, in any order; values represent `px` units). In this pattern:
  - `ID`=character ID. Each character has its unique numeric ID directly got from snapshot data. So here we target the character to be offset. As per the general text layout rules, once we offset this target characters, all the other characters will follow on the same baseline, unless any of them has another offset.
  - `y`=Y offset to add to the computed character baseline, thus shifting its default position down (when the offset is positive) or up (when the offset is negative).
  - `x`=X offset to add to the computed character baseline, thus shifting its default position to the right (when the offset is positive) or to the left (when the offset is negative).

>Init features are those features injected by a first operation of type annotation. When a set of operations starts with an annotation operation, this is done to initialize specific features for the whole text at once, like defining line indentation.

- `r_char-offset`: this works as `r_char-offsets`, but it is provided in the features of a single character node, instead of being an initial setup for multiple characters. For instance, `3:y=10,x=20` means that before rendering the character with ID=3, the baseline will be shifted down by 10 and the X position of the next character will be offset by 20, thus effectively leaving a wider gap between the previous character and the next ones. All the following characters up will follow on this newly defined baseline, unless any of them happens to have another offset.

>The logic for these two features, `r_char-offsets` and `r_char-offset`, is equal; the only difference is that the former works on multiple characters at once. This is preferable to having just `r_char-offset` and using it multiple times, because when a character gets an offset, this affects not only it, but also all the following characters. So, order of application matters, and this could not be ensured when using multiple features, unless introducing ad-hoc logic which would be against generalization.

- `r_font-family`: the font family.
- `r_font-size`: the font size.
- `r_fore-color`: the foreground color.
- `r_back-color`: the background color.
- `r_italic`: the italic value (boolean).
- `r_bold`: the bold value (boolean).
- `r_underline`: the underline value (0-N being the thickness in `px`).
- `r_overline`: the overline value (as for underline).
- `r_strike`: the strikethrough value (as for underline).
- `r_text-line-style`: the style of overline, underline, or strikethrough: `solid`, `dotted`, `dashed`, `wavy` (as in SVG and CSS; corresponds to SVG `text-decoration-line`).
- `r_text-line-color`: the default color of the overline, underline or strikethrough.
- `r_rotate`: the rotation amount to be applied to each single character.
- `r_hints`: this feature is used to link the operation to any number of hints. The value is a space-delimited list of hint IDs, in the order they should be rendered for that operation.
- `r_hint-vars`: this has no effect on text; it is just a rendition feature used only to define a set of variables to be used by hints placeholders. Each variable has form name=value, and is separated with space. For instance, `color=red bold=1` to pass variables `color`=`red` and `bold`=`1` to the hints being rendered for the operation being processed.

The following text rendition features are applicable only to **added text** (=text nodes added by insert/replace operations):

- `r_t-position`: the position of added text relative to the RBR.
- `r_t-offset-x`: the X offset for the added text, relative to the computed `r_t-position`.
- `r_t-offset-y`: the Y offset for the added text, relative to the computed `r_t-position`.
- `r_t-solid`: a boolean value (`1` or `0`), telling whether the added text behaves as "solid" (thus triggering elements spreading) or not (which is the default).
- `r_t-displaced-span`: a span of base text with format `IDxN` where `ID`=node ID and `N`=count of chars to include, to be used as the RBR instead of the default RBR. This works exactly like a displaced hint, but is applied to added text.

#### Hint Rendition Features

Hints have their design-time properties set for position, offset, scale, rotation and other behavior. Yet, most of these properties can be overridden by specific features, the hint rendition features (similarly to what happens with text rendition features). By definition, they are applicable only to hints, and are used to override their corresponding property in the hint's design.

By default, the hint rendition features apply to all the hints in the operation (those listed by `r_hints`), unless the property value starts with `@` followed by a space delimited list of hint IDs, ended by `:`; in this case, it applies ONLY to those hints matching the list. For instance, `r_h-position`=`e` applies to all hints overriding their `position` property to `e`; while `@alpha beta:e` applies only to hints with IDs `alpha` and `beta`, overriding their `position` property to `e`. These hint property override features are:

- `r_h-position`: override hint's relative `position` property.
- `r_h-offset-x`: override hint's `offsetX` property.
- `r_h-offset-y`: override hint's `offsetY` property.
- `r_h-scale-x`: override hint's `scaleX` property.
- `r_h-scale-y`: override hint's `scaleY` property.
- `r_h-rotation`: override hint's `rotation` property.
- `r_h-solid`: override hint's `solid` property.
- `r_h-displaced-span`: override hint's `desplacedRefSpan` property for displaced hints.
