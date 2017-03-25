# Marginalia

Randnotizen für (lange) HTML-Seiten

**Marginalien** (dt.) = Randnotizen, Randbemerkungen: auf dem Rand einer Buchseite oder eines Manuskripts platzierte Bemerkungen, die Kommentare, Hinweise oder Korrekturen zu Textstellen bieten (de.wikipedia.org)

## Ziel

Ein Haupttext mit Randnotizen, die nicht stören sollen. Die Randnotizen sind anfangs verborgen und werden einzeln eingeblendet. Dadurch kann der Leser sich auf den Haupttext konzentrieren.

## Funktionsweise

Radio buttons sorgen dafür, dass (keine oder) höchstens eine Randnotiz gleichzeitig eingeblendet wird.

*Show labels* (Beschriftung des Einblenden-Buttons) werden gezeigt, solange die Randnotiz ausgeblendet ist. *Hide labels* (Beschriftung des Verbergen-Buttons) werden gezeigt, während die Randnotiz eingeblendet ist. (Damit kann man nie auf etwas klicken, was die gleiche Seitenansicht noch einmal aufruft.)

*Show labels* und *Hide labels* sind Textelemente, die nacheinander angeordnet sind. Sie können nicht überlappen. (Ich mag keine Chamäleon-Knöpfe, die ihre Funktion wechseln.)

*Side notes* (Randnotizen) werden immer einzeln gezeigt, sie können nicht überlappen. Das sorgt für eine klare Ansicht. Der Haupttext kann mit zahlreichen zusätzlichen Informationen ergänzt werden.

Ausgeblendete Randnotizen liegen hinter der Spalte des Haupttextes. Aktive Randnotizen erscheinen im Randbereich, oben bündig mit dem Absatz oder Block, auf den sie sich beziehen. Ein simpler Übergang (transition) kann verdeutlichen, wozu sie gehört.

## Demo

[Demo, minimales Beispiel auf jsfiddle.net](https://jsfiddle.net/Draussenduscher/odjbLuh8/)

## Nur HTML und CSS

### Hintergrund

CSS-Selektoren sind auf (folgende) Geschwisterelemente und Nachfahrenelemente begrenzt. (Sie »sehen« weder Eltern- noch Vorfahrenelemente.)

`input` ist ein sogenanntes Leeres Element, es kann keine Kind-Elemente enthalten. Es kann jedoch Geschwisterelemente haben. Mit `input` kann man nur Elemente auf der selben Ebene oder *deren* Kindelemente ansprechen.

Unsere Randnotiz soll ein `div` sein, also ein Blockelement. `input` und die Randnotiz werden auf der gleichen Ebene sein, innerhalb des Blocks `.text-with-side-note`. Um mit einem CSS-Selektor angesprochen zu werden,

- muss `side note` ein (folgendes) Geschwisterelement von `input` sein.

`input` und `label` sind einfach Freunde, nicht verwandt im CSS-Sinne; sie können *irgendwo* im Dokument leben. Beide sind durch die Attribute `id` und `for` miteinander verknüpft. Um mit einem CSS-Selektor angesproche zu werden,

- kann `label` ein (folgendes) Geschwisterelement von `input` oder
- ein Kindelement eines (folgenden) Geschwisterelements von `input` sein.

Das führt direkt zu zwei Varianten:

#### Variante 1: Label als Blockelemente

`label` ist ein Blockelement vor, zwischen oder nach Absätzen in der Hauptspalte. `input` steht vor `label` (auf der selben Ebene). `div.side-note` folgt `input` auf der gleichen Ebene.

Mit anderen Worten: `input` schaltet

- sein Geschwisterelement `label` und
- sein Geschwisterelement `div.side-note`.

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

### Variante 2: Label als Inline-Elemente

`label` ist ein Inline-Element, es kann überall innerhalb eines Absatzes in der Hauptspalte leben. Ein `input`-Element steht vor dem `label`-Element (auf einer höheren Ebene). `div.side-note` folgt `label` (auf der gleichen, höheren Ebene). 

Mit anderen Worten: `input` schaltet

- das Kind `label` eines folgenden Geschwisterelements und
- das folgenden Geschwisterelement `div.side-note`.

Es spielt keine Rolle, welches von beiden zuerst steht.

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

Die CSS-Anweisungen beider Varianten kommen sich nicht ins Gehege, sie können glücklich nebeneinander existieren.

Das Behälterelement `div.text-with-side-note` hat zwei Aufgaben:

- Die Randnotiz wird an ihm oben bündig ausgerichtet.
- Es begrenzt die »Sichtweite« des CSS-Selektors, was einfache CSS-Anweisungen ermöglicht. (Man könnte auch input, label und Randnotizen mit einmaligen `id`- und `for`-Attributen versehen, das klingt aber nicht verlockend.)

Ein `div.text-with-side-note` kann mehr als eine Randnotiz beherbergen. Dafür würde ich den betreffenden `input`-, `label`- und `.side-note`-Tags passende Klassen `class="one"`, `class="two"` usw. verpassen.

### Minimales Beispiel

Das minimale Beispiel nutzt
1. Radio buttons (keine oder genau eine Randnotiz wird gezeigt)
2. Label (die Beschriftungen der buttons) sind klickbare Links
3. Absolute Positionierung

Marginalia funktioniert natürlich für mehr als eine Randnotiz. Die radio buttons müssen eindeutige `id`s haben, mit der sie innerhalb der Gruppe der radio buttons-Gruppe stehen. Farben und Transparenzen sind nur zur Verdeutlichung der Größen, Positionen und Übergänge da. Wenn man `input { display: none; }` auskommentiert, kann man sehr schön die Zustände der radio buttons nachvollziehen.

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

HTML-Beispiel: `input`, `label` und die Randnotiz sind auf der selben Ebene.

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

## Einsatz

- Füge dem Behälterelement die Klasse `text-with-side-note` hinzu. Die Randnotiz wird oben bündig mit diesem Block ausgerichtet.
- Plaziere die `input`- und `label`-Elemente in bzw. zwischen den Absätzen, siehe oben
- Das `div`mit der Randnotiz wird unterhalb des `input`-Tags eingefügt, siehe oben

## Anpassungen

Beobachte die Höhen der Hauptspalte und der Randnotizen. Die Randnotizen sollen vollständig hinter der Spalte verschwinden, solange sie inaktiv sind.

## Schöner machen

Marginalia kann einfach verbessert werden, z.B. durch

- CSS-Übergänge (transitions) für z-index und opacity

## Offene Punkte

- Bedienung mit der Tastatur

## Browserunterstützung

All verwendeten CSS-Anweisungen werden gut unterstützt. Wir sind hier auf der Seite des Guten.

## Lizenz

Keine Lizenz. Frei wie frische Luft.