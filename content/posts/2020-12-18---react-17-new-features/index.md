---
title: React 17 New Features
date: "2020-12-18T12:40:32.169Z"
template: "post"
draft: false
slug: "react-17-new-features"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

It has been ages.

Suddenly, December is here.

I enjoyed the lamp soup and finally got a chance to look into React 17.

### Event delegation

In React 16 and earlier, React would do document.addEventListener() for most events. React 17 will call rootNode.addEventListener() under the hood instead.

### Persist event by default

To access the event in an async function, React 17 would keep the synthetic event instead of removing it. The `e.persist()` is no need.

Before
```JavaScript
function handleChange(e) {
  e.persist()
  setData(data => ({
    ...data,
    text: e.target.value
  }));
}

onKeyUp={e => {
    e.persist()
    setTimeout(() => {
        this.filterContent(e.target.value)
    }, 200)
}}
```

After
```JavaScript
function handleChange(e) {
  setData(data => ({
    ...data,
    text: e.target.value
  }));
}

onKeyUp={e => {
  setTimeout(() => {
      this.filterContent(e.target.value)
  }, 200)
}}
```

### New JSX transform

Upgrading to the new transform is completely optional, but it has a few benefits:

- With the new transform, you can use JSX without importing React.
- Depending on your setup, its compiled output may slightly improve the bundle size.
- It will enable future improvements that reduce the number of concepts you need to learn React.

From
```JavaScript
import React from 'react';

function App() {
  return <h1>Hello World</h1>;
}

/*  
Above will be transfer to:
function App() {
  return React.createElement('h1', null, 'Hello world');
}
*/
```

To
```JavaScript
function App() {
  return <h1>Hello World</h1>;
}
```
It will be compiled to 
```JavaScript
// Inserted by a compiler (don't import it yourself!)
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
  return _jsx('h1', { children: 'Hello world' });
}
```
