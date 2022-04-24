---
layout: post
title: "Logical Assignment Operators"
date: 2020-06-15T14:20:36+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Read a fancy article from Dr. Axel Rauschmayer.

He mentioned about the logical assignment Operators `??=`, `&&=` and `||=`.

From my perspective, `??=` could be very useful than the others.

## Example regarding `??=`

```JavaScript
const books = [
  {
    isbn: '123',
  },
  {
    title: 'ECMAScript Language Specification',
    isbn: '456',
  },
];

// Add property .title where it’s missing
for (const book of books) {
  book.title ??= '(Untitled)';
}

assert.deepEqual(
  books,
  [
    {
      isbn: '123',
      title: '(Untitled)',
    },
    {
      title: 'ECMAScript Language Specification',
      isbn: '456',
    },
  ]);

```

<!--more-->
