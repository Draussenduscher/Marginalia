# Marginalia

Side notes for (long) HTML pages. Can be shown one by one within the margin. Click on text labels to show side note.

**Marginalia** (engl.) = side notes: marks made in the margins of a book or other document. They may be scribbles, comments, glosses (annotations), critiques, doodles, or illuminations (en.wikipedia.org)

## Purpose

A main story has side notes, which should not clutter the view. Side notes are hidden by default and only one side note can be shown at a time. This keeps the reader focused at the text in the main column. 

## How it works

Radio buttons make sure that none or just one marginal note is active at any time. 

*Show labels* are visible when the *side note* is hidden. *Hide labels* are visible when the *side note* is shown. (I.e. you can never click on anything that will call the same page again.)

*Show labels* and *Hide labels* are text, laid out one after the other. They never overlap. (I don't like »chameleon« buttons.)

*Side notes* are shown one by one, they never overlap. This keeps the viewport clear. A short main story can be complemented with lots of additional information.

Inactive *side notes* are hidden (behind the main column). Active *side notes* are placed in the margin, top aligned with the respective paragraph or div in the main column. A simple transition can make it obvious to the reader where it belongs to.

## Demo

[Demo, minimal example on jsfiddle.net](https://jsfiddle.net/Draussenduscher/odjbLuh8/)

## Just HTML and CSS

### Some background

CSS selectors in general are limited to (following) siblings and children/descendants. (You can neither »see« parents nor preceding siblings.)

`input` is a so called *empty element*, it must not contain anything, can never have children. It can have siblings. This means that with `input` you can only toggle (following) elements on the same level or children of (following) block elements on the same level. 

We want the `side note` to be a div, a block element. `input` and `side note` will live on the same level, within the block `.text-with-side-note`. To be addressed with CSS selectors 

- `side note` must be a (following) sibling of `input`

`input` and `label` are just friends, they can live anywhere within the document. Both are connected through input's `id`and label's `for` attribute. 

To be addressed with a CSS selector

- `label` can be a (following) sibling of `input` or a
-  child of a (following) sibling of `input`.

This directly leads to two variants:

####  Variant 1: Label as block element

`label` is a block element before, between or after the main column paragraphs. `input` precedes `label` (on the same level). `div.side-note` follows the `input` (on the same level).

In other words: `input` toggles 

- its sibling `label` and
- its sibling  `div.side-note`. 

```html
<article>
  <div class="text-with-side-note">
    <p>…</p>
    <input type="radio" name="side-notes" id="show-0"> <label for="show-0">Show Side Note</label>
    <input type="radio" name="side-notes" id="hide-0" checked> <label for="hide-0">Hide Side Note</label>
    <div class="side-note">
      <p>…</p>
    </div>
    <p>…</p>
 </div>
</article>
```

### Variant 2: Label as inline element

`label` is an inline element, it can be text anywhere within the main column paragraphs. `input` precedes the `label` (on a higher level). `div.side-note` follows the `label` (on the same, higher level as the `label`).

In other words: `input` toggles

- its child of a following sibling `label` and
- its following sibling `.div.side-note`.

It makes no difference which comes first.

```html
<article>
  <div class="text-with-side-note">
    <input type="radio" name="side-notes" id="show-0">
    <input type="radio" name="side-notes" id="hide-0">
    <p>…
     <label for="show-0">Show Side Note</label>
     <label for="hide-0">Hide Side Note</label>
    …</p>
    <div class="side-note">
      <p>…</p>
    </div>
    <p>…</p>
 </div>
</article>
```

Both variants can be enabled with non-overlapping CSS rules, they can happily live together.

The containing `div.text-with-side-note` has two functions:

- The side note is top aligned with it.
- It limits the scope CSS selectors, allowing for very simple CSS rules (you could alternatively place inputs, labels and side notes anywhere if you assign unique `id` and `for` values correctly, not very appealing.)

`div.text-with-side-note` can host more than one side note. In this case I would add `class="one"`, `class="two"` etc. to the related `input`, `label` and `.side-note` tags.

### Minimal example

The minimal example uses

1. Radio buttons (none or just one side note can be active)
2. Labels of these buttons as clickable links
3. Absolute positioning

Marginalia works with more than one side note, of course. The radio buttons need to have unique `id`s (within the radio button group). Colours and transparencies are only there to visualise sizes, positions and transitions. If you comment `input { display: none; }` you can monitor the status of the radio buttons in the group.

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
- Place `input`s and `label`s in or between paragraphs (see above)
- The side note div must be below the `input`s , (see above)

## Adjustments

Take care of the heights of main column and side notes. Side notes must disappear behind the column completely when inactive.

## Enhance it

Marginalia can easily be upgraded, e.g. with

- CSS transitions for z-index, opacity

## To be done

- Keyboard operation

## Browser Support

All CSS features used here are well supported. So you should be on the safe side.

## License

No license. Free like fresh air.
