---
title: Logical Assignment Operators
date: "2020-06-15T12:40:32.169Z"
template: "post"
draft: false
slug: "logical-assignment-operators"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
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
