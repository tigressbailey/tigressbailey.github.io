---
layout: post
title: "Container Queries Feature Is So Cool"
date: 2021-05-18T19:08:54+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Let's say the HTML structure is below:

```HTML
<div class="card-container">
  <div class="card">
    <figure> ... </figure>
    <div>
      <div class="meta">
        <h2>...</h2>
        <span class="time">...</span>
      </div>
      <div class="notes">
        <p class="desc">...</p>
        <div class="links">...</div>
      </div>
      <button>...</button>
    </div>
  </div>
</div>
```

In order to use `@container`, you first need to create a parent element that has `containment`. 

In order to do so, you’ll need to set `contain: layout inline-size` on the parent. You can use `inline-size` since we currently can only apply container queries to the inline axis. This prevents your layout from breaking in the block direction.

```CSS
.card-container {
  contain: layout inline-size;
  width: 100%;
}
```

Setting `contain: layout inline-size` creates a new containing block and new block formatting context, letting the browser separate it from the rest of the layout. Now, we can query!

```CSS
/* when the parent container is smaller than 850px, 
remove the .links div and decrease the font size on 
the episode time marker */

@container (max-width: 850px) {
  .links {
    display: none;
  }

  .time {
    font-size: 1.25rem;
  }

  /* ... */
}

/* when the parent container is smaller than 650px, 
decrease the .card element's grid gap to 1rem */

@container (max-width: 650px) {
  .card {
    gap: 1rem;
  }

  /* ... */
}
```

<!--more-->
