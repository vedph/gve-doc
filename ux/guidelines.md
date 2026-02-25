---
title: VEdition Guidelines
layout: default
parent: User Experience
nav_order: 3
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
  - [Using Features](#using-features)
    - [Using Features Metadata](#using-features-metadata)
    - [Using Renditional Features](#using-renditional-features)
    - [Using Non-Renditional Features](#using-non-renditional-features)

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

## Using Features

Here are some general hints about using operation features.

### Using Features Metadata

The following are the main metadata of a feature:

- **set policy**:
  - _multiple_ (operator `=` in DSL), which can occur 0-N times in its set. When a feature with the same name already exists in the target set, a multiple feature is added, too. For instance, there might be two distinct reasons for an operation, like "confirmation" combined with "metrical"; in this case, a `reason` feature would be added multiple times to the same set.
  - _single_ (operator `:=` in DSL), which can occur 0-1 times in its set. When a feature with the same name already exists in the target set, the single feature replaces the one with the same name.
  - _first-single_ (operator `==` in DSL), which is treated as _single_ the first time it is added to the set, and then as _multiple_. This can be used to replace a _set_ of features with another one. For instance, one might want to add a new set of reasons in the next step, removing the previous ones.
- **global** (prefix `*` in DSL): a global feature is a feature attached to the chain data context as a whole, rather than to specific nodes in it. When a feature isn't global, it will be assigned to the nodes targeted by its operation, rather than to the whole text features set. As operations are executed, they accumulate on nodes and/or the whole text, unless their set policy dictates a different behavior. In the end, this will produce a rich set of highly granular metadata, mapped either to specific portions of the text or to the text as a whole. For non-global features, target nodes are determined by the operation type: for instance, a deletion targets the node being deleted, while an insertion targets the nodes being inserted.
- **short-lived** (suffix `^` in DSL): once added, the feature will be removed the next time features get updated (i.e. at the next operation). This is useful for instance when adding the `*version^` feature, which should tag a single output as representative of a named text version.
- **negated** (prefix `!` in DSL): a negated feature means that any existing feature with the same name should be removed. In other terms, this is used to delete an existing feature from the target set if present.

### Using Renditional Features

- **(almost) everything is an operation**: in the model, everything but the base text (which is the first input) is an operation. So, whatever annotation or change you want to represent, it must be an operation, with its features. This is true also for basic layout aspects like indentations: to add indents, just add a single initial annotation operation which adds horizontal offsets to the first character of each indented line.

- **variable value syntax**: the value of features may change its syntax according to the feature selected. For instance, the `r_char-offsets` feature used to set indents has a syntax where the ID of each character is followed by `:` and the horizontal (`x`) or vertical (`y`) indent, e.g. `65:x=100 179:x=100 295:x=100 406:x=100`.

- **adding text**: when you have to add text, consider its role in the snapshot:
  - if it is part of the _text_, like an insertion or new text instead of an existing one (=a replacement), the text is just the value of the corresponding add or replace operation. The relative position of the text is defined by the `r_t-position` property, optionally shifted by the corresponding offset properties.
  - else, the text is just part of a _hint_. Some hints are designed in such a way to display text, drawing it from other features. For instance, the `note` hint displays a textual note, assuming that its text value is found in an additional `note` property. The position within the hint is defined in the hint itself as a drawing; the position of the whole hint is defined by `r_h-position`, optionally shifted by the corresponding offset properties.

For instance, the `note` hint is defined as follows:

```js
note: {
  svg: '<g><text x="0" y="0" font-family="Arial" font-size="14" fill="{{r_fore-color}}" id="placeholder">{{note}}</text></g>',
  position: "n",
  offsetX: 0,
  offsetY: 0,
  scaleX: 1,
  scaleY: 1,
  rotation: 0,
  animation: "#scale-in",
},
```

Its `svg` property shows that there is a group (`g`) including a `text` element, where some values are constant, and others are drawn from features with a specified name.

For instance, attributes like `font-family` and `font-size` here have constant values, even if nothing prevents us from making them variables, and let these variables be set by a corresponding feature.

Instead, `fill` has a variable named `r_fore-color` as its name (all variables by convention are included between `{{...}}`); the same holds for `note`, which is the content of the note. This means that the effective values for these variables will be extracted from additional features named after the variables themselves. In practice, this means that when you add the `note` hint, you _must_ add also:

- a `r_fore-color` feature for the color of the note.
- a `note` feature with the text of the note.

- **customizing rendition**: rendition can be variously customized by adding more features which override the default settings of each hint or text. For instance, when you set `r_fore-color` equals to `red` you are overriding (=replacing) changing the default foreground color (black) with red; often this also affects hints, where a custom color is applicable, like for the note example shown above.

- **hints have variable count**: for each operation there can be zero or more hints. In most cases, each operation has one hint (e.g. a delete operation with a visual hint represented by a diagonal stroke on the text being deleted), but this is not a requirement. For instance, if a hand adds 3 numbers above 3 words to represent their reordering, we will typically represent this first with 3 distinct annotation operations each with a note hint (whose value is the number), followed by (usually) a move operation which effectively reorders the text. This move operation will have no hints, because its effect is already visually represented by all the previous annotations on the sheet, which added the numbers above words.

### Using Non-Renditional Features

Most of the features are renditional, while others are pure metadata with no implied visuals. Currently, these are:

- **comment** (`comment`): an editorial comment of any sort. Note that this is totally distinct from notes: `r_note` and other similarly named rendition features represent textual annotations found on the snapshot, while this is a pure comment from the editor. The `note` feature exists as a shortcut to provide text for a note hint (see above about adding text).
- **epigram number** (`epigram-nr`): the number assigned to the epigram. There can be multiple numbers, represented by multiple properties of this type.
- **immediate** (`immediate`): added when the editor thinks that the operation was executed immediately after a previous action. For instance, the author might write a base text with an incorrect or incomplete word, and immediately after fix it, before continuing to write the rest of the text.
- **log** (`log`): this logs the purpose of the operation. It is recommended to always include the log, with a global scope and as a single feature, so that each operation gets a sort of short description about its meaning.
- **page beginning** (`page-beg`): a feature attached to the first character of the text starting a new page.
- **page number** (`page-nr`): the number of the current page. This typically is coupled with `page-beg`, whose value is just boolean, to specify the number of the new page. This distinction is made so that the model can also be used without explicit page numbers.
- **parallel** (`parallel`): added when the editor regards the change represented by the operation as thought by its author a potential alternative to the previous text, rather than a correction.
- **reason** (`reason`): the reason for the operation. This is an editorial annotation; for instance, one might want to state that the reason for a word replacement is metrical.
- **stage name** (`version`): this assigns an alteration stage name to the version output by the operation which receives this feature. This means that its output is the final state of the alteration stage with that name; the following operation (if any) will belong to the next stage.
