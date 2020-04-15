---
layout: post
title: "Flexbox - the Ultimate Summary"
date: 2020-04-15T09:42:42+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

## Concept

Flexbox is 1 dimension layout, in which case, we can consider it as one continuous row.

## Key items

A Flexbox layout could not live without both flex container and flex items.
We can consider them as the row and cells when layouting using the float layout.
And the flex items must be the direct children of the flex container.

## The axis

When flex container set to column, main axis matches the direction of the flex container, which is vertical.
The relevant cross axis is horizontal.

When flex container set to row, main axis matches the direction of the flex container, which is horizontal.
The relevant cross axis is vertical.

## Flexbox Properties

### Parent (Flex Container)

```CSS
display: flex | inline-flex;

flex-direction: row | row-reverse | column | column-reverse;

flex-wrap: wrap | nowrap | wrap-reverse;

flex-flow (shorthand for flex-direction and flex-wrap)

justify-content (main axis): flex-start | flex-end | center | space-between | space-around | space-evenly;

align-items (cross axis - adjust to individual sizes): flex-start | flex-end | center | baseline | stretch;

align-content (cross axis - adjust to largest item): flex-start | flex-end | center | stretch | space-between | space-around;
```

### Children (Flex Items)

```CSS
order: <integer>;

flex-grow: <number>;

flex-shrink: <number>;

flex-basis: <length> | auto;

flex: shorthand for grow, shrink, and basis (default:  0 1 auto)

align-self: overrides alignment set on parent
```

## Resources

- Chris Coyer's Complete Guide to Flexbox

  <https://css-tricks.com/snippets/css/a-guide-to-flexbox/>

- Smashing Magazine: Harnessing Flexbox for Today's Web Apps

  <http://www.smashingmagazine.com/2015/03/02/harnessing-flexbox-for-todays-web-apps/>

- Solved by Flexbox

  <https://philipwalton.github.io/solved-by-flexbox/>

- Flexy Boxes playground and code generator

  <http://the-echoplex.net/flexyboxes/>

- Getting started with Flexbox grid systems

  <http://www.webdesignerdepot.com/2016/02/getting-started-with-flexbox-grid-systems/>

- Flexbox Froggy

  <http://flexboxfroggy.com/>

- Flexbox Defense

  <http://www.flexboxdefense.com/>

- Flexbox Grid

  <http://flexboxgrid.com/>

- Bootstrap 4's Flexbox-based grid

  <https://getbootstrap.com/docs/4.0/layout/grid/>

- Zurb Foundation's XY Grid

  <http://foundation.zurb.com/sites/docs/xy-grid.html>

<!--more-->
