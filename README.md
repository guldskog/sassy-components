# BEM 2

###### Introduction

**BEM 2 is inspired by BEM methodology**

Note: To understand how BEM 2 works you must first learn BEM and SASS
- http://getbem.com/introduction/
- http://sass-lang.com/

## Abstract

A nested selector in CSS gives the power of changing one class and effect other
elements (this is not allowed in BEM). Therefor you only need to change one
class and that's very practical when JavaScript is brought to the party. With
nested selectors comes responsibility and that isn't a good thing. To remove
responsibility from the developer we need to add a layer between the compiled
CSS and the written CSS. This layer is key to still have a cascading effect
like BEM. "Listeners" will be new way of thinking when writing the CSS. Very
simple example would be "Element listens if Block has a specific modifier".

## Comparison - CSS Syntax

| Select	                  | BEM	                          | BEM 2
| ------------------------- | ----------------------------- | --------------------------------- |
| Block	                    | .block {…}	                  | .block {…}                        |
| Element	                  | .block__element {…}	          | .block-element {…} .              |
| Modifier -> Block         | .block--modifier {…}	        | .block.m-modifier {…} .           |
| Modifier -> Element       | .block__element--modifier {…} | .block-element.m-modifier {…} .   |
| Block Modifier -> Block	  | None	                        | .block.bm-modifier {…}            |
| Block Modifier -> Element	| None	                        | .block.bm-modifier .block-element |

## Problem with BEM

Let's say we have a block with two elements in it.

```
<div class="block">
  <div class="block__element1"></div>
  <div class="block__element2"></div>
</div>
```

Now we want to change the style of the block to be bigger. With BEM we'd create a new block or use modifiers.
This example shows when modifiers is used.

```
<div class="block block--bigger">
  <div class="block__element1 block__element1--bigger"></div>
  <div class="block__element2 block__element2--bigger"></div>
</div>
```

Now we've added "block" on three elements and "element1", "element2" on two others. This is the way to go if you're going
to fully follow BEM. This gets very complex and tricky when using JavaScript to change the style of a block.

## Solution with BEM 2

Same situation as before but with BEM 2 syntax

```
<div class="block">
  <div class="block-element1"></div>
  <div class="block-element2"></div>
</div>
```

Let's make the block bigger

```
<div class="block bm-bigger">
  <div class="block-element1"></div>
  <div class="block-element2"></div>
</div>
```

Wow! Only one class. Now we may think this is not at all like BEM because it's
now nested and that's the reason we need the layer between compiled CSS and
written CSS, AKA mixins in SASS.

## SASS mixins

| Mixins	             |
| -------------------- |
| +block()	           |
| +element()	         |
| +modifier()          |
| +blockModifier()     |
| +andBlockModifier()	 |

Learning these mixins is all you need.
Let's style this block!

## BEM 2 - Button Example

HTML
```
<div class="button">
  <div class="block-text">Go to Mars</div>
  <div class="block-icon">:)</div>
</div>
```

SASS
```
+block('button')
  width: 100px
  height: 30px
  padding: 5px
  display: flex

  +element('text')
    font-size: 10px

  +element('icon')
    font-size: 8px
    padding-left: 5px
```

Simple as that we now have a button that will send us to mars :D But it's to
small so we need to make it bigger.

HTML
```
<div class="button bm-bigger">
  <div class="block-text">Go to Mars</div>
  <div class="block-icon">:)</div>
</div>
```

SASS
```
+block('button')
  width: 100px
  padding: 5px
  display: flex

  +blockModifier('bigger')
    width: 200px

  +element('text')
    font-size: 10px

     +blockModifier('bigger')
       font-size: 18px

  +element('icon')
    font-size: 8px
    padding-left: 5px

    +blockModifier('bigger')
       font-size: 16px
       padding-left: 10px
```

Here all elements (including the block itself) listens to if the block has the
block modifier **"bigger"** The syntax is very clean. Modifiers still works as usual.

## More "complex" example

```
+block()

  +modifier()

  +modifier()

  +blockModifier()

  +element()

    +modifier()

    +blockModifier()

      +andBlockModifier()

  +element()

    +modifier()

    +blockModifier()

      +andBlockModifier()

        +andBlockModifier()

          +modifier()
```

Try it on Codepen - DEMOS
- No JavaScript -> https://codepen.io/joacimnilsson/pen/wpNopG
- JavaScript Example (Vue) -> https://codepen.io/joacimnilsson/pen/MrLbPr
