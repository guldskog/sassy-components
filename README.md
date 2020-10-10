# Sassy Components

###### Introduction

**Sassy Components is inspired by BEM methodology**

Note: To understand how Sassy Components works you must first learn BEM and SASS
- http://getbem.com/introduction/
- http://sass-lang.com/

## Abstract

A nested selector in CSS gives the power of changing one class and effect other
elements (this is not allowed in BEM). Therefor you only need to change one
class and that's very practical when JavaScript is brought to the party. With
nested selectors comes problem with often very bloated CSS and that isn't a
good thing due to specificity concerns.

To ensure the CSS artist doing it the right way a layer between the compiled
CSS and the written CSS is needed. This layer is key to have a cascading effect
like BEM but still enable nested selectors.

## Comparison - CSS Syntax

| Select	                                        | BEM	                          | Sassy Components
| ----------------------------------------------- | ----------------------------- | ---------------------------------- |
| Block / Component                               | .block {…}	                  | .component {…}                     |
| Element / Part	                                | .block__element {…}	          | .p-part {…} .                      |
| Modifier -> Block / Modification -> Component   | .block--modifier {…}	        | .component.m-modification {…} .    |
| Modifier -> Element / Modification -> Part      | .block__element--modifier {…} | .p-part.m-modification {…} .       |
| Component Modification -> Component	            | None	                        | .component.cm-modification {…}     |
| Component Modification -> Part	                | None	                        | .component.cm-modification .p-part |

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

## Solution with Sassy Components

Same situation as before but with Sassy Components syntax

```
<div class="component">
  <div class="p-part1"></div>
  <div class="p-part2"></div>
</div>
```

Let's make the block / component bigger

```
<div class="component cm-bigger">
  <div class="p-part1"></div>
  <div class="p-part2"></div>
</div>
```

Wow! Only one class. Now we may think this is not at all like BEM because it's
now nested. SASS mixins to the rescue!

## SASS mixins

| Mixins	                  |
| ------------------------- |
| +component()	            |
| +part()	              |
| +modification()           |
| +componentModification()  |
| +and()	                  |

Now when we can use these awesome mixins.
Let's style this block!

## Sassy Components - Button Example

HTML
```
<div class="button">
  <div class="p-text">Go to Mars</div>
  <div class="p-icon">:)</div>
</div>
```

SASS
```
+component('button')
  width: 100px
  height: 30px
  padding: 5px
  display: flex

  +part('text')
    font-size: 10px

  +part('icon')
    font-size: 8px
    padding-left: 5px
```

Simple as that we now have a button that will send us to mars :D But it's too
small so we need to make it bigger.

HTML
```
<div class="button cm-bigger">
  <div class="p-text">Go to Mars</div>
  <div class="p-icon">:)</div>
</div>
```

SASS
```
+component('button')
  width: 100px
  padding: 5px
  display: flex

  +componentModification('bigger')
    width: 200px

  +part('text')
    font-size: 10px

     +componentModification('bigger')
       font-size: 18px

  +part('icon')
    font-size: 8px
    padding-left: 5px

    +componentModification('bigger')
       font-size: 16px
       padding-left: 10px
```

Here all elements (including the component itself) listens to if the component has the
component modification **"bigger"** The syntax is very clean.

## More "complex" example

```
+component()

  +modification()

  +modification()

  +componentModification()

  +part()

    +modification()

    +componentModification()

      +and()

  +part()

    +modification()

    +componentModification()

      +and()

        +and()

          +modification()
```
