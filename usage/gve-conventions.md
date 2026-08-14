# GVE Conventions

## General Principles

Two complementary principles can be used to determine the operations:

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

## Epigram-Specific Conventions

(1) **always encode indentations with an initial annotate** operation via the `char offsets` feature, which contains an expression defining all the horizontal and/or vertical offsets for all characters which start an indented or otherwise offset text.

- set `at`=1 and `run`=1 (conventionally we just target the first character of the text).
- feature value is expressed with syntax `ID:x=N,y=N` for each indentation (you can omit either `x` or `y`). Separate multiple indentations with space.

>This is true also for gaps internal to the text. Remember that all the indents must be ordered by their ID, ascending, in the feature value.

(2) **always add a `log` feature** to each operation summarizing what it does. This helps users focus on the specific area of the text affected by the operation. Be short, and cite the text between quotes e.g. `add "s" after "dog"`.

(3) **always aim at the smallest (or zero) offset**. Try using the proper position feature to position each element, optionally adjusting it with scale and offset, rather than positioning it wrong and then using bigger offsets to move them. For instance position `north-east` rather than positioning north and offsetting the X coordinate to the right; unless the offset would be bigger with this solution.
