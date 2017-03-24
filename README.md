# Marginalia
Side notes for (long) html pages. Can be shown one by one within the margin. Click text labels *below the paragraph* to show side note.

**Marginalia** (engl.) = side notes: marks made in the margins of a book or other document. They may be scribbles, comments, glosses (annotations), critiques, doodles, or illuminations (en.wikipedia.org)

**Marginalien** (dt.) = Randnotizen, Randbemerkungen: auf dem Rand einer Buchseite oder eines Manuskripts platzierte Bemerkungen, die Kommentare, Hinweise oder Korrekturen zu Textstellen bieten (de.wikipedia.org)

## Purpose

A main story has side notes, which should not clutter the view. Side notes are hidden by default and only one marginal note can be shown at a time.

## How it works

Radio buttons make sure that none or just one marginal note is active at any time. This keeps the reader focused at the main column. Neither Show and Hide labels nor the side notes can ever overlap. This way a short main story can be complemented with lots of additional information.

Inactive side notes are hidden behind the main column. They are placed in the margin, top aligned with the relevant paragraph or div in the main column when active. A simple transition can make it obvious to the reader where it came from.

## Demo

[Demo, minimal example on jsfiddle.net](https://jsfiddle.net/Draussenduscher/odjbLuh8/)

## Just HTML and CSS

### Minimal example

The minimal example uses

1. Radio buttons (none or just one side note can be active)
2. Labels of these buttons as clickable links
3. Absolute positioning

Marginalia works with more than one side note, of course. The radio buttons need to have unique `id`s. Colours and transparencies are only there to visualise sizes, positions and transitions. If you comment `input { display: none; }` you can monitor the status of the radio buttons in the group.

```html
<article>
  <div class="text-with-side-note">
    <h2>Text with Side Note 1</h2>
    <p>...</p>
    <input type="radio" name="side-notes" id="show-0"> <label for="show-0">Show Side Note</label>
    <input type="radio" name="side-notes" id="hide-0" checked> <label for="hide-0">Hide Side Note</label>
    <div class="side-note">
      <h3>Side Note A</h3>
      <p></p>
    </div>
    <h3>Next Header</h3>
 </div>
</article>
```

HTML example, `input` and `label` are placed on the same level as the side note itself.

```css
article { width: 60%; }

.text-with-side-note { position: relative; background-color: hsla(0, 0%, 90%, .8); }

.side-note { position: absolute; right: 0; top: 0; z-index: -1; 
	width: calc((100% - 60%) / (60 / 100)); /* Width related to containing main block */
	background-color: hsl(120, 20%, 90%); }

input { display: none; } /* Buttons hidden */

label { color: green; cursor: pointer; }

label[for^=show] { visibility: visible; } /* Initial states */
label[for^=hide] { visibility: hidden; }

input[id^=show]:checked ~ label[for^=show] { visibility: hidden; } /* Toggle checked labels visibility */
input[id^=show]:checked ~ label[for^=hide] { visibility: visible; }

input[id^=show]:checked ~ .side-note { right: calc(-1 * ((100% - 60%) / .6)); }
```

CSS

## How to use

- Add the class `text-with-side-note` to the containing block. The side note will be top aligned with this block.
- Place `input`s and `label`s between paragraphs and on the same level with them (not within paragraphs)
- The side note div must be below the `input`s and `label`s, on the same level as well

## Adjustments

Take care of the heights of column and side notes. Side notes must disappear behind the column completely when inactive.

## Enhanced it

Marginalia can easily be upgraded, e.g. with

- CSS transitions for z-index, opacity

## Limits

This simple version works with the clickable labels placed as separate block *on the same level with the containing paragraph*.

I have made up another solution SideNotes where labels can be placed inline *inside the paragraph*.

## To be done

- Keyboard operation

## Browser Support

All CSS features used here are well supported. So you should be on the safe side.

## License

No license. Free like fresh air.
