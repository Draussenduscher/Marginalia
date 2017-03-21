# Marginalia
Side notes for long html pages. Can be shown one by one within the margin. Click text labels *below the paragraph* to show side note.

**Marginalia** = side notes: marks made in the margins of a book or other document. They may be scribbles, comments, glosses (annotations), critiques, doodles, or illuminations (en.wikipedia.org)

**Marginalien** = Randnotizen, Randbemerkungen: auf dem Rand einer Buchseite oder eines Manuskripts platzierte Bemerkungen, die Kommentare, Hinweise oder Korrekturen zu Textstellen bieten (de.wikipedia.org)

## Purpose

A (long) text has side notes, which should not clutter the view. Side notes are hidden by default and can be shown one by one. Only one side can be shown at a time.

## Just HTML and CSS

### Minimum example

The minimal example uses

1. Radio buttons (none or just one side note can be active)
2. Labels of these buttons as clickable links
3. Absolute positioning

Marginalia works with more than one side note, of course. The radio buttons need to have unique ids.

### Enhanced example

Marginalia can easily be upgraded, e.g. with

- CSS transitions for z-index, opacity

### Adjustments

Take care of the heights of column and side notes. Sides must disappear completely when inactive.

##Limits

This simple version works with the clickable labels placed as separate block *below the paragraph*.

I have made up another solution Marginalia+ where the labels can be placed inline *inside the paragraph*.

## To be done

- Keyboard operation

## How it works

Radio buttons make sure that none or just one side is active at any time. This keeps the reader focused at the main column. Neither Show and Hide labels nor the side notes can ever overlap.

Inactive side notes are hidden behind the column. They are placed top aligned with the relevant paragraph or div in the main column when active. A simple transition can make it obvious to the reader where it came from.
