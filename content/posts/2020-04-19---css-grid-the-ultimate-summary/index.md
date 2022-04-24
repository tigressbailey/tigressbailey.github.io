---
title: CSS Grid - the Ultimate Summary
date: "2020-04-19T12:40:32.169Z"
template: "post"
draft: false
slug: "css-grid-the-ultimate-summary"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

## Concept

It is working as 2 dimensions.

No `Row` markup.

It has Grid container which is the wrapper.
And it has Grid items which are the cells.

If cells has the same dimension, they overlap with each other.

## Grid Properties

### Parent (Grid Container)

```CSS
display: grid | inline-grid;

grid-template-columns
grid-template-rows: [optional: line name, in square brackets] <track-size> | <repeat>;
    track-size: length, %, fr, auto
    line name: an arbitrary name for this item. If no name assigned, a number is used

    examples:
    .myClass {
        grid-template-columns: [col1] 40px [col2] 3fr;
        grid-template-rows: 50% 25vh auto;
    }

    .anotherClass {
        grid-template-rows: repeat(2, 350px [name]) 10%;
    }
    translates to
    .anotherClass {
        grid-template-rows: 350px [name] 350px [name] 10%;
    }

grid-template-areas:
    List of names of areas. First, name areas via selector. Then specify layout via this property. Area name must be specified for each column/row. A . indicates no content in this row/column.

    Note: in this example, the lines are named automatically: header-start, header-end, article-start, article-end, etc.

    example:
    .class1 {
        grid-area: header;
    }
    .class2 {
        grid-area: article;
    }
    .class3 {
        grid-area: aside;
    }
    .wrapper {
        grid-template-columns: 1fr 3fr;
        grid-template-rows: auto;
        grid-template-areas:
        "header header header header"
        "aside . article article";
    }

grid-template:
    Shorthand for grid-template-rows, grid-template-columns, and grid-template-areas in 1 declaration. Not covered in class.

grid-column-gap: <number>;
grid-row-gap: <number>;
    Distance between rows and/or columns. At one point, only pixels were accepted for this - browser bug?

grid-gap:
    Shorthand for grid-column-gap and grid-row-gap.
    1 number = same in all directions
    2 numbers = row column

justify-items: start | end | center | stretch;
    align grid items on row axis
    stretch is default

align-items: start | end | center | stretch;
    align grid items on column axis
    stretch is default

justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
    If size of grid container is bigger than total of grid items, you can align grid items within the container (like flexbox). This works on row axis.

align-content:  start | end | center | stretch | space-around | space-between | space-evenly;
    If size of grid container is bigger than total of grid items, you can align grid items within the container (like flexbox). This works on column axis.

grid-auto-columns
grid-auto-rows: <track-size>;
    If you create grid cells beyond those specified in grid-template-columns and grid-template-rows, this specifies how big these extra rows/columns should be.


grid: shorthand for all of the above properties. Not covered in class.
```

### Children (Grid Items)

```CSS
grid-column-start
grid-column-end
grid-row-start
grid-row-end: <number> \| <name> | span <number> | span <name> | auto;
    This is the longhand for declaring individual values for start and end points for rows and columns.

    example:
    .class1 {
        grid-column-start: 1;
        grid-column-end: span 4;
        grid-row-start: 3;
        grid-row-end: span footer-end;
    }

grid-column
grid-row: <start-line> / <end-line> | <start-line> / span <value>;
    Combines start and end values, as used extensively in class.

    example:
    .class1 {
        grid-column: 1 / span 4;
        grid-row: 3 / span footer-end;
    }

grid-area:  <name> | <row-start> / <column-start> / <row-end> / <column-end>;
OR
<name>;
    If you're confused, no wonder. grid-area can be used in 2 different ways:
        a. Assign a name for the grid-template-areas property (see above example under grid container/grid-template-areas)

        b. Assign a name AND the dimensions for a grid-template-areas property. If you use this methodology, you would not necessarily need a grid-template-rows and grid-template-columns declaration, depending on other factors.

        example:
        .class1 {
            grid-area: 1 / name3 / namedline / 4;
        }

justify-self: start | end | center | stretch;
    Aligns content in a grid item on the row axis. Overrides justify-items.

align-self: start | end | center | stretch;
    Aligns content in a grid item on the column axis. Overrides align-items.
```

## POLYFILLS & FALLBACKS

- Old spec:
  [Grid-layout-polyfill](https://github.com/codler/Grid-Layout-Polyfill)

- New spec:
  [CSS-grid-polyfill](https://github.com/FremyCompany/css-grid-polyfill)

- @supports may help with all but IE browsers:
  [@Supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)

- Rachel Andrew: Grid "fallbacks" and overrides
  [Cheat sheets](https://rachelandrew.co.uk/css/cheatsheets/grid-fallbacks)

## Resources

- CSS Tricks: A Complete Guide to Grid

  <https://css-tricks.com/snippets/css/complete-guide-grid/>

- Grid by Example

  <http://gridbyexample.com/>

- Practical CSS Grid: Adding Grid to an Existing Design

  <https://alistapart.com/article/practical-grid>

- Basic concepts of grid layout

  <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout>

- CSS Grid Inspector (in Firefox):

  <https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts>

- Grid "fallbacks" and overrides

  <https://rachelandrew.co.uk/css/cheatsheets/grid-fallbacks>

- Things I’ve Learned About CSS Grid Layout

  <https://css-tricks.com/things-ive-learned-css-grid-layout/>

- GRID PILE: Stacking CSS Grids for Impossible Layouts

  <https://www.linkedin.com/pulse/grid-pile-stacking-css-grids-impossible-layouts-rand-hendriksen/>

- Breaking Down a CSS Grid Layout

  <http://csskarma.com/blog/css-grid-layout/>

- A Collection of Interesting Facts about CSS Grid Layout

  <https://css-tricks.com/collection-interesting-facts-css-grid-layout/>

- Is it really safe to start using CSS Grid Layout?

  <https://rachelandrew.co.uk/archives/2017/07/04/is-it-really-safe-to-start-using-css-grid-layout/>

- Bootstrap to CSS Grid

  <https://medium.com/@tallys/bootstrap-to-css-grid-87b3f5f830e4>

- Firefox Developer version -- Grid tools

  <https://mozilladevelopers.github.io/playground/03-firefox-devtools>
