---
title: Editor: Snapshot Part
layout: default
parent: Usage
nav_order: 3
---

# Editor - Snapshot Part

The snapshot part is the core of the GVE edition and implements the [snapshot model](../model/snapshot.md).

Although the model is simple, the UI provides a lot of editing functions on top of it, grouped in 3 main sections:

- text tab: base text.
- operations tab: operations applied to the base text.
- snapshot rendition: visual rendition of the snapshot.

## Text Tab

### Text Tab - Base Text

The text tab contains the collapsible **base text section**, where you enter the base text, usually just once. So, this is usually collapsed to give more room to the UI.

When you create a new snapshot, the first thing you must do is setting the base text by clicking the _base text_ button in this section and then typing or pasting it in the popup.

>⚠️ WARNING: setting the base text has the effect of clearing all the operations! This is required, because all operations depend on it, and become meaningless once you change the base text. If you happen to find out that your base text needs some changes after you have entered operations, you can anyway copy them (with the Copy DSL operations button) and paste them back later (with the Add operations batch button); you will then have to adjust their coordinates according to your changes.

Once you set the base text, this gets displayed here character by character:

- each character gets a **numeric ID** (starting from 1) which uniquely identifies it within this snapshot. Numeric IDs are never reused: once an ID is assigned, is stays assigned to that specific character forever (in the context of this snapshot), even if an operation deletes them. The ID is displayed below each character.
- the end of each line is explicitly marked by a **line-feed** character (LF, represented with a down arrow). After this, the next line is displayed below the previous one. Each line is numbered for your convenience, starting from 1.
- you can type text in the **search box** to highlight all the matches in the displayed text.
- you can click a character to **select** it. Once this is done, you can Ctrl+click another character to select the whole span of characters from the first to the last clicked. In both cases, the current selection is displayed next to the search box, like `7x3`, where `7` is the ID of the first selected character and `3` is the length of the selected text span.
- the **copy button** allows you to copy the selected text.
- the **copy coords** button allows you to copy the coordinates of the selected text.

### Text Tab - Result
