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



### Multiple Classes

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



### Deciding Between Class and ID

Class and ID selectors may be case-sensitive, depending on the document language (in HTML they are).

`.` notation is not guaranteed to work for XML documents. It works in HTML, SVG, MathML.

`#` notation is guaranteed to work in any document language that has an attribute that enforces uniqueness within a document.



## Attribute Selectors

When it comes to both class ID selectors, what you're really doing is selecting values of attributes.



### Simple Attribute Selectors

Useful when you need to select elements that have a certain attribute, regradless of that's attrubute value:

```css
h1[class] { color: silver; }
```

It is useful in XML, where elements and attributes names are usually specific to their purpose.



In html it can be used to style all images that have an alt attribute:

```css
img[alt] { border: 3px solid red; }
```

If you want to boldface any element that includes title imformation, you could write:

```css
*[title] { font-weight: bold; }
```

It is also possible to select based on the presence of more than one attribute:

```css
a[href][title] { font-weight: bold; }
```



### Selection Based on Exact Attribute Value

```css
a[href="http://www.example.com"] { font-weight: bold; }
```

This will boldface the text of any *a* element that has an href attribute with *exactly* the value `http://www.example.com`. Any change at all, even dropping the `www.` part or changing to a secure protocol with `https`, will prevent a match.



Any attribute and value combination can be specified for any element.

As with attribute selection, you can chain together multiple attribute-value selectors:

```css
a[href="http://sergeydarnopykh.github.io"][title="W3C Home"] { font-size: 200%; }
```

Math should be *exact* in any case. Even with list of classes, in order for this to work, we should specify all list of classes in css:

```html
<a class="multiple class list">Hello</a>
```

```css
a[class="multiple"] { font-weight: 18px; }
a[class="multiple class list"] { font-weight: 20px; }
```

Only the second declaration will work.

`Note:` this is not equivalent to `.` class notation.

`Note:` ID selectors and attribute selectors that target the id attribute are not precisely the same:

```css
h1#page-title
h1[id="page-title"]
```



### Selection Based on Partial Attribute Values

| Type          | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| [foo~="bar"]  | Selects any element with an attribute `foo` whose value *contains* the word `bar` in a space-separated list of words |
| [foo*="bar"]  | Selects any element with an attribute `foo` whose value *contains* the substring `bar` |
| [foo^="bar"]  | Selects any element with an attribute `foo` whose value *begins* with `bar` |
| [foo$="bar"]  | Selects any element with an attribute `foo` whose value *ends* with `bar` |
| [foo\|="bar"] | Selects any element with an attribute `foo` whose value *starts* with `bar` *followed by a dash* or whose value is *exactly equal* to `bar` |



#### A Particular Attribute Selection Type

The most common use for this type of attribute selector is to match languages:

```css
*[lang|="en"] { color: white: }
```

Wil match only first two elements:

```html
<h1 lang="en">Hello!</h1>
<h1 lang="en-us">Greetings!</h1>
<h1 lang="fr">Bonjour!</h1>
```

Another example:

Math all images that are figures (`figure-1.gif`, `figure-3.jpg` etc.):

```css
img[src|="figure"] { border: 1px solid gray; }
```



#### Matching one word in a space-separated list

```html
<p class="urgent warning">Don't touch it!</p>
```

```css
p[class~="warning"] { font-weight: bold; }
```

This selector is equivalent to the `.` class notation. But it's still useful, because it can be used with *any* attribute, not just *class*:

```css
img[title~="Figure"] { border: 1px solid gray; }
```



#### Matching a substring within an attribute value

```html
<span class="hey rocky">Mercury</span>
<span class="cloudy planet">Venus</span>
<span class="another planet with clouds">Earth</span>
```

```css
span[class*="cloud"] { font-style: italic; }
```

This will match both Venus and Earth.

Specially style any links to an Oggetto website:

```css
a[href*="oggetto.ru"] { font-weight: bold; }
```

Target any class name that starts with "btn" followed by a dash, and that contains the substring "arrow" preceded by a dash:

```html
<button class="btn-small-arrow-active">Click Me</button>
```

```css
*[class|="btn-small-arrow-active"]:after { content: '↓'}
```



`Note:` in HTML class names, titles, URLs, and iD values are all case-sensitive, but HTML attribute keyterm values, such as input types,are not:

```html
<input type="checkbox" name="rightmargin" value="10px">
```

```css
input[type="CHeckBoX"] { margin-right: 10px; }
```



#### Matching a substring at the beginning of an attribute value

```css
a[href^="https:"] { font-weight: bold; }
a[href^="mailto:"] { font-weight: italic; }
img[alt^="Figure"] { border: 2px solid gray; display: block; margin: 2em auto; }
```



#### Matching a substring at the end of an attribute value

```css
a[href$=".pdf"] { font-weight: bold; }
img[src$=".gif"] { ... }
img[src$=".jpg"] { ... }
```

`Note:` quoting is required if the value includes any special characters, bfeins with a dash or digit, but it's recommended to do it *always*.



### The Case Insensitivity Identifier

```css
a[href$=".pdf" i]
```

This will match all links ending in ".PDF", ".PdF", ".pdf" etc.

This option is available for all attribute selectors, but it applies only to the *values* in the attribute selectors. This isn't an issue in HTML5, because it's case-insensitive.

`Note:` this option may not be supported by all browsers (Opera Mini, Android browser, Edge).



## Using Document Structure

### Understanding the parent-child relationship

If an element is exactly one level above or below another, then they have a *parent-child* relationship.

If the path from one element to another is traced through two or more levels, the elements have an *ancestor-descendant* relationship.

`Note:` because `html` element is ancestor to the entire document, it's called the *root element* in HTML and XHTML.



### Descendant Selectors

```css
h1 em { color: gray; }
```

This rule will make gray any text in an em element that is the descendant of an h1 element. Other em text will not be selected by this rule.

In a descendant selector, the selector side of a rule is composed of two or more space-separated selectors. The space between the selectors is an example of *combinator*.

You aren't limited to two selectors:

```css
ul ol ul em { color: gray; }
```



Practical example: different backgrounds and links' colors in main area and sidebar.

```css
.sidebar { background: blue; }
main { background: white; }
.sidebar a:link { color: white; }
main a:link { color: blue; }
```

Make text wihin b elements that are descended from paragraphs or block quotes gray:

```css
blockquote b, p b { color: gray; }
```

The degree of separation between two selectors can be practically infinite.

The closeness of two elements within the document tree has no bearing on whether a rule applies or not.



### Selecting Children

```css
h1 > strong { color: red }
```

This rule will make red the *strong* element only with a parent of *h1* element.



### Selecting Adjacent Sibling Elements

```css
h1 + p { margin-top: 0; }
```

The selector is reas as, "Selects any `p` element that *immediately follows* an `h1` element that *shares a parent* with the `p` element".

````mermaid
graph TD
div-->ol;
div-->ul;
ol-->li1;
ol-->li2;
ol-->li3;
ul-->li4;
ul-->li5;
ul-->li6;
````

```css
li + li { font-weight: bold; }
```

This will affect only the second and third items in each list.

`Note:` text content between two elements does *not* prevent the adjacent-sibling combinator from working.

The adjacent-sibling combinator can be used in conjunction with other combinators:

```css
html > body table + ul + ol { margin-top: 1.5rem; }
```

The selector translates as: "Selects any `ol `element that *immediately follows* a sibling `ul` element that *immediately follows* a sibling `table` element that is descended from a `body` element that *is itself a child* of an `html` element."



```css
div#content h1 + div ol
```

The selector is read as: "Selects any `ol` element that *is descended* from a `div` when the `div` is the adjacent sibling of an `h1` which *is itself descended* from a `div` whose `id` attribute *has a value of content*".



### Selecting Following Siblings

```css
h2 ~ ol { font-style: italic; }
```

This rule italicizes any `ol` that follows an `h2` and also *shares a parent* with the `h2`. 

The two elements do not have to be adjacent siblings, although they can be adjacent and still match this rule.

`Note:` the whitespace before and after `+`, `>`, `~` are optional.



## Pseudo-Class Selectors

These selectors let you assign styles to what are, in effect, phantom classes that are inferred by the state of certain elements, or markup patterns within the document, or even by the state of the document itself.



### Combining Pseudo-Classes

CSS makes it possible to chain pseudo-classes together. For example, you can make unvisited links red when they're hovered and visited links maroon when *they're* hovered:

```css
a:link:hover { color: red; }
a:visited:hover { color: maroon; }
```

The order order doesn't matter, you could also write `a:link:hover` as `a:hover:link`. 

It's also possible to assign separate hover styles to unvisited and visited links that are in another language:

```css
a:link:hover:lang(de) { color: gray; }
a:visited:hover:lang(de) { color: silver; }
```

`Note:` you cannot combine mutually exclusive pseudo-classes (for example, `a:link:visited` doesn't make any sence and will never match anything).



### Structural Pseudo-Classes

The majority of pseudo-classes refer to the markup structure of the document. Most of them depend on patterns within the markup, such as choosing every third paragraph, but others allow you to address specific types of elements.

All pseudo-classes, without exception, are a word preceded by `:`, and they can appear anywhere in a selector.

`Note:` pseudo-classes always refer to the element to which they're attached, and no other. For a few of the structural pseudo-classes it's a common error to think they are descriptors that refer to descendant elements.

In other words, the effect of pseudo-classes is to apply a sort of a "phantom class" to the element to which they're attached.

```css
#ericmeyer:first-child
#ericmeyer > :first-child
```

The first selector selects Eric Meyer himself and only if he's the first child of his parents.

The second selector selectrs the first child of Eric Meyer.



#### Selecting the root element

```css
:root { border: 10px dotted gray; }
```

In HTML it's *always* the `html` element. In XML however, it's not. Because in XML the root element may differ in every language (or even in different documents in one language).

`Note:` there's a difference between writing `:root` and `html` .



#### Selecting empty elements

With pseudo-classs `:empty`, you can select any element that has no children of any kind, *including* text nodes, which covers both text and whitespace. It can be useful in suppressing elements that a CMS has generated without filling in any actual content.

`Note:` an element should be empty from a parsing perspective, in order to be matched (no whitespace, visible content or descendant elements).

Here, only first and last elements will be matched:

```html

```

```css
p:empty { display: none; }
```



`:empty` matches HTML's empty elements, like `img`, `input` and even `textarea` without default text.

`:empty` is unique in that it's the only CSS selector that takes text nodes into consideration when determining matches.



#### Selecting unique children

```css
img:only-child { border: 1px solid black; }
```

This rule selects elements when they are the *only child* element of another element.

Things to remember about this `:only-child`:

1. You always apply it to the element you want to be an only child, not to the parent element.
2. When you use `:only-child` in a descendant selector, you aren't restricting the elements to a parent-child relationship.

```css
a[href] img:only-child
```

It will match  any image that is only child and *is descended* from an `a` element, not necessarily *is a child* of an `a` element. To restrict the rule to parent-child relationship, you should write:

```css
a[href] > img:only-child
```



If you want to match images that are the only images inside hyperlinks, you should use `:only-of-type`:

```css
a[href] > img:only-of-type
```

The differenct is that `:only-of-type` will match any element that is the only of its type among all its siblings, whereas `:only-child` will only match if an element has no siblings at all.

This can be very useful in cases such as selecting images within paragraphs without having to worry about the presence of hyperlinks or other inline elements:

```css
p > img:only-of-type { float: right; margin: 20px; }
```

You could also use this pseudo-class to apply extra styles to an h2 when it's the only one in a section of a document:

```css
section > h2 { 
    margin: 1rem 0 0.33rem;
    font-size: 1.8rem;
    border-bottom: 1px solid gray;
}

section > h2:only-of-type { font-size: 2.4rem; }
```

`Note:` `:only-of-type` refers to elements and nothing else (it *doesn't* work on classes, *type* means the *element type*).



#### Selecting first and last children

The pseudo-class `:first-child` is used to select elements that are the first children of other elements:

```css
li:first-child { font-weight: bold; }
```

The mirror image of `:first-child` is `:last-child`. It selects elements that are the last children of other elements:

```css
li:last-child { font-style: italic; }
p:last-child { font-size: 15px; }
```

`Note:` this two rules will select the same elements:

```css
p:only-child { color: red; }
p:first-child:last-child { color: red; }
```



#### Selecting first and last of a type

You can select the first or last of a type of element within another element:

```css
table:first-of-type { color: blue; }
p:last-of-type { color: red; }
```

These rules select first `table` and last `p` inside of a given element.

`Note:` this does not apply to the entire document, it will not select the first `element of type` in the document and skipp all others. 

`Note:` this two rules will select the same elements:

```css
table:only-of-type { color: red; }
table:first-of-type:last-of-type { color: red; }
```



#### Selecting every nth child

