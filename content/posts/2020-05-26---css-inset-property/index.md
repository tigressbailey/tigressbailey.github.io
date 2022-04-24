---
title: CSS Inset Property
date: "2020-05-26T12:40:32.169Z"
template: "post"
draft: false
slug: "css-inset-property"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Firefox 66 now supports CSS inset property.

It is defined as a shorthand of top, right, bottom and left.

The values can be used per the margin property order: top, right, bottom and left.

For instance .example-a equals .example-b.

```CSS
.example-a {
  writing-mode: sideways-rl;
  position: absolute;
  inset: 20px 40px 30px 10px;
  background-color: #c8c800;
}
```

```CSS
.example-b {
  writing-mode: sideways-rl;
  position: absolute;
  top: 20px;
  right: 40px;
  bottom: 30px;
  left: 10px;
  background-color: #c8c800;
}
```

As I tested, inset has the same effect as top/right/bottom/left when position is absolute, fixed, relative and sticky.

It doesn't affect the element when the position is static as well.

### Reference

<https://developer.mozilla.org/en-US/docs/Web/CSS/inset>

<https://drafts.csswg.org/css-logical/#propdef-inset>

<https://caniuse.com/#search=inset>

