---
title: Sticky Position - What Should We Know
date: "2021-05-31T12:40:32.169Z"
template: "post"
draft: false
slug: "sticky-position-what-should-we-know"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Sticky is awesome.

I have nailed it by optimizing the react-big-calendar in a previous project and creating our own timeline component.

Some gotchas come along with it as well.

Just remember below points, it is never going to make any troubles in future.

1. It won't work if it is the only element inside the container.

   In another word, it should be sticky comparing to the siblings in the same level.

2. It won't work if any parent level container is set `overflow: auto/hidden/scroll`.

   Well, set a fixed height would solve this problem.

   ```CSS
   .wrap {
      height: 80vh;
      position: relative;
      overflow: scroll;
      margin: 10em auto 20em;
      max-width: 960px;
      scroll-snap-type: x mandatory;
      -webkit-overflow-scrolling: touch;
      /*overscroll-behavior: contain;*/
   }
   ```

3. It won't work if none of top/left/bottom/right are provided.

   Assign a top px value would make it work,

   ```CSS
   .headers {
      position: sticky;
      top: 0;
      z-index: 1;
   }
   ```

4. In safari, it has a specific bug that if a sticky element is the direct child of the container:

   ```HTML
   <ScrollContainer>
      <StickyItem />
   </ScrollContainer>
   ```

   The `StickyItem` can only be sticky in one page width/height.

   To resolve this issue, downgrade the `StickyItem` as the grandchild of the container:

   ```HTML
   <ScrollContainer>
      <ScrollContent>
        <StickyItem />
      </ScrollContent>
   </ScrollContainer>
   ```
