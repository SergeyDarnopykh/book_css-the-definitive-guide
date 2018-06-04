# Chapter 2. Selectors

## Basic Style Rules

The elements of the document serve as the most basic selectors. In XML a selector can be anything.

```css
quote { color: gray; }
bib { color: red; }
booktitle { color: purple; }
```

If you use an incorrect property or value in a declaration, the whole rule will be ignored.



## Grouping

### Selectors can be grouped:

```css
h2, p { color: gray; }
```

There's a wildcard selector that match *any* element:

```css
* { color: orange; }
```

### Declarations also can be grouped:

```css
h1 {
    font: 18px Helvetica;
    color: purple;
    background: aqua;
}
```



`Note:` semicolons are crucial when you're grouping declarations. If you omit them, the browser will treat multiple declarations as one, and therefore it will be invalid and ignored by the browser:

```css
h1 {
    font: 18px Helvetica color: purple;
}
```

`Note:` the last semicolon can be omitted, but it's not recommended.

### Grouping Everything

```css
h1, h2, h3 {
    font: 18px Helvetica;
    color: red;
}
```

## Class and ID Selectors

### Class Selectors

Class selectors are preceded by `.`

```html
<p class="warning">Don't touch it without gloves on.</p>
```

```css
.warning { font-weight: bold; }
```

`Note:` the `*` selector is implied when an ID, class, attribute selector, pseudo-class or pseudo-element selector is written without being attached to an element selecor.



### Combining selectors

```css
p.warning { font-weight: bold; }
```

The selector now matches *any p elements* that have a *class* attribute containing the word *warning*, but no other elements of any kind.



Another option is to use a combination of a general class selector and an element-specific class selector to make the styles even more useful, as in the following markup:

```css
.warning { font-style: italic; }
span.warning { font-weight: bold; }
```



## Multiple Classes

```css
<p class="urgent warning">Run, Forest, run!</p>
```

The order of the words doesn't matter.

```css
.warning { font-weight: bold; }
.urgent { font-style: italic; }
.warning-urgent { background: silver; }
```

By chaining two class selectors together, you can select only those elements that have both class names, in any order.

If a multiple class selector contains a name that is not in the space-separated list, then the match will fail.

```css
p.warning.help { background: red; }
```

The selector won't match the element above, but will match this element:

```html
<p class="urgent warning help">Help me!</p>
```

### ID Selectors

ID selectors are preceded by `#`

```html
<p id="lead-para">Paragraph</p>
```

```css
#lead-para { font-weight: bold; }
```

The ID selector's name should be *unique* to the document.

Unlike class selectors, ID selectors can't be combined with other IDs, since ID attributes do not permit a space-separated list of words.

Another difference between class and id names is that IDs carry more weight when you're trying to determine which styles should be applied to a given element:

```html
<p class="dangerous" id="safe">Safe?</p>
```

```css
#safe { color: green; }
.dangerous { color: red; }
```

The p element will have green color, because ID `safe` has more weight than class `dangerous` . And it doesn't matter that it was declared earlier in the stylesheet.

## Deciding Between Class and ID

Class and ID selectors may be case-sensitive, depending on the document language (in HTML they are).

`.` notation is not guaranteed to work for XML documents. It works in HTML, SVG, MathML.

`#` notation is guaranteed to work in any document language that has an attribute that enforces uniqueness within a document.



## Attribute Selectors

