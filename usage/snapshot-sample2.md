---
title: Snapshot Sample 2
layout: default
parent: Usage
nav_order: 6
---

# Snapshot Sample 2

This is a real-world example from VEdition (epigram 4 of notebook H5). The base text is:

```txt
4.
Das iſt Italien das ich verließ! noch ſtäuben die Wege ,
Noch iſt der Fremde geprellt, ſtell er ſich wie er auch will ;
Deutſche Redlichkeit ſuchſt du in allen Winkeln vergebens ,
Leben und Weben iſt hier, aber nicht Ordnung und Zucht ;
Jeder ſorgt nur für ſich, miſtraut dem andern, iſt eitel ,
Und die Meiſter des Staats ſorgen nur wieder für ſich .
Schön iſt das Land , doch ach ! Fauſtinen find’ ich nicht wieder ,
Das iſt Italien nicht mehr , das ich mit Schmerzen verließ.
```

> Note that VEdition encodes also peculiar diplomatic aspects of the manuscript, like spaces before punctuation or `ſ` for `s`, without normalizations. An English translation can be found at <https://www.poetryintranslation.com/PITBR/German/GoetheVenetianEpigrams.php#anchor_Toc171440807>.

---

## Description

This example is very simple because it comes from a 1799 print with a few handwritten annotations, mainly about punctuation. In this notebook we usually define 4 alteration stages:

1. self-corrections by Geist (ink), here absent. The stage is distinct only for alignment with other epigrams in the same source.
2. Riemer 1 (pencil).
3. Goethe 1 (red chalk).
4. Riemer 2 (pencil).

---

## Operations

- 📐 the **first operation** only reflects the print layout, where each even line has been printed with an indent. This alteration is just a way to encode it as base text is plain text only. So, the operation only reflects the typesetter's work and does not belong to Riemer. Yet, to avoid redundant stages, conventionally we never assign a separate stage to indents, which, when present (as typical in print), always belong to the first operation in the snapshot.

As for any operation which does not alter the text, this is an _annotation_ operation. Such an operation type only carries features, i.e. metadata; in this case, the only feature needed is that encoding indents, named `r_char-offsets`. This is a [rendition feature](../model/rendition#rendition-features), i.e. a feature representing visual aspects. All such features have a name prefixed with `r_`.

- ▶️ (1) indent: add various horizontal offsets to specified characters and a constant vertical offset to all lines (to widen the space between lines as we want to reflect the actual print layout): the printed number, which is horizontally centered and thus has a larger offset; and each character starting an even line.

> Note that character offsets are multiple in this feature value. Each offset format is `ID:y=...,x=...` (where we can specify only `x`, only `y`, or both, in any order), where `ID`=target character ID, `y`=vertical offset to add to the computed character baseline (negative=up, positive=down), `x`=X offset to add to the computed character baseline (negative=left, positive=right).
>
> Both `x` and `y` can be followed by a unit specifier. The default unit is just pixels; otherwise, the value is followed by suffixes `tw`=text character average width (according to the current font and size) or `th`=text character average height.

Our value here is:

```txt
1:x=13.5tw 4:y=1th 61:x=3.5tw,y=1th 124:y=1th 184:x=3.5tw,y=1th 241:y=1th 300:x=3.5tw,y=1th 356:y=1th 423:x=3.5tw,y=1th
```

- 👉 **Riemer 1** (light blue):

- ▶️ (2) add comma after `ſich`.
- ▶️ (3) replace `ſ` with `ß`. Note that this change is the source for another document, N7.

- 👉 **Goethe** (red):

- ▶️ (4) Goethe literally rewrites `ß` over the `ß` written by Riemer to confirm the change. So, again this is a replacement operation, where the new value is equal to the old one. This means that in the end the resulting text does not change; but we encode it, because _we are reconstructing a process_, not just a text. The text is a byproduct of the process. In this process, we must encode the action of Goethe who confirmed the change by his secretary. Additionally, that process left traces on paper, as the letter was overwritten, by a different hand and with a different color. In the highly diplomatic context of VEdition we want to preserve visual information, too. This will allow us to reproduce it in text rendition when displaying it.

- 👉 **Riemer 2** (blue):

- ▶️ (5) delete `,` after `vergebens` with a vertical stroke on it. Note that this proposal was rejected in later documents N7 and A1.
- ▶️ (6) replace `;` with `.` after `Zucht`. Again, this proposal was rejected in N7 and A1.
- ▶️ (7) add `˘` (a _breve_ sign) above the `i` of `iſt` to mean that metrically this is a weak position.

---

## DSL

All data we entered via the UI can be encoded as plain text:

- base text:

```txt
4.
Das iſt Italien das ich verließ! noch ſtäuben die Wege ,
Noch iſt der Fremde geprellt, ſtell er ſich wie er auch will ;
Deutſche Redlichkeit ſuchſt du in allen Winkeln vergebens ,
Leben und Weben iſt hier, aber nicht Ordnung und Zucht ;
Jeder ſorgt nur für ſich, miſtraut dem andern, iſt eitel ,
Und die Meiſter des Staats ſorgen nur wieder für ſich .
Schön iſt das Land , doch ach ! Fauſtinen find’ ich nicht wieder ,
Das iſt Italien nicht mehr , das ich mit Schmerzen verließ.
```

- operations:

```txt
1×1: [r_char-offsets="1:x=13.5tw 4:y=1th 61:x=3.5tw,y=1th 124:y=1th 184:x=3.5tw,y=1th 241:y=1th 300:x=3.5tw,y=1th 356:y=1th 423:x=3.5tw,y=1th"]
103×1+]"," [r_fore-color=lightblue *log:="add ',' after ſich" r_t-position=e reason=proposal accepted-source-of=N7]
269×1="ß" [r_fore-color=lightblue *log:="replace 'ſ' with 'ß'" accepted-source-of=N7 *version^:=AS2 reason=proposal]
483×1="ß" [r_fore-color=red r_t-position=inw *log:="replace 'ß' with 'ß'" reason=confirmation accepted-source-of=N7 *version^:=AS3]
181×2- [r_fore-color=blue r_hints=vertical-stroke reason=proposal rejected-source-of="N7 A1" *log:="delete ',' after 'vergebens'" r_h-scale-x=4]
239×1="." [r_fore-color=blue r_hints=vertical-stroke reason=proposal rejected-source-of="N7 A1" *log:="replace ';' with '.' after 'Zucht'" r_t-position=e r_h-scale-x=4]
288×1: [r_hints=note-above r_fore-color=blue note=˘ *log:="add '˘' above the i of 'iſt'" *version^:=AS4 reason=metrical r_h-offset-y=0.5th]
```
