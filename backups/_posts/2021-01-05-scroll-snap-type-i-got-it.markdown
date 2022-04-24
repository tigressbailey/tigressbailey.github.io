---
layout: post
title: "scroll-snap-type - I Got It"
date: 2021-01-05T19:10:20+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Today I got a new CSS skill according to the latest CSS tricks newsletter.

And it is the `scroll-snap-type`.

I have to admit that there are no new features can stimulate me after CSS Grid.

If you haven't learned it, please visit [CSS Grid Garden](https://cssgridgarden.com/).

It is so much fun to learn it and use it.

Despites the dying IE 11 and some mobile browsers, `scroll-snap-type` is supported in all modern browsers.

It has two major properties: `scroll-snap-type` and `scroll-snap-align`.

In order to enable this feature for swiping the entire page.

1. The parent element needs to define `overflow:auto | scroll` and `width: 100vw` / `height: 100vh`
2. `scroll-snap-type: x mandatory | y mandatory | x proximity | y proximity`
3. The children define `width: 100vw` / `height: 100vh` as well.

`scroll-snap-align: start | center | end` is not required. From my experience, `start` performs more naturally than the others.

If the child element content is longer than the parent content,  `proximity` could allow the user to navigate to the bottom of the content. Then the scrolling effect would work properly.

```HTML
<main class="container">
	<div class="child">
		<h2>Element 1</h2>
	</div>
	<div class="child">
		<h2>Element 2</h2>
	</div>
	<div class="child">
		<h2>Element 3</h2>
	</div>
	<div class="child">
		<h2>Element 4</h2>
	</div>
	<div class="child">
		<h2>Element 5</h2>
	</div>
	<div class="child">
		<h2>Element 6</h2>
	</div>
	<div class="child">
		<h2>Element 7</h2>
	</div>
</main>
```

```CSS
.container {
	width: 40vw;
	height: 70vh;
	margin: 15vh auto;
	overflow-x: auto;
	scroll-snap-type: x proximity;
  color: white;
  background-color: oldlace;
  display: flex;
  align-items: center;
}

.child {
  margin-left: 0.5rem;
	height: 90%;
	scroll-snap-align: start;
	padding: 1rem;
	display: flex;
	align-items: center;
	justify-content: center;
	text-align: center;
}

.child:nth-child(1n) {
	background-color: darkorchid;
	flex: 0 0 130%;
}

.child:nth-child(2n) {
	background-color: indigo;
	flex: 0 0 60%;
}

.child:nth-child(3n) {
	background-color: navy;
	flex: 0 0 40%;
}

.child:nth-child(4n) {
	background-color: palegreen;
	flex: 0 0 50%;
}

.child:nth-child(5n) {
	background-color: yellow;
	flex: 0 0 80%;
}

.child:nth-child(6n) {
	background-color: orange;
  flex: 0 0 60%;
}

.child:nth-child(7n) {
	background-color: tomato;
  flex: 0 0 80%;
}
```

####  Reference:
[Scroll Story](https://css-tricks.com/newsletter/232-scroll-story/)

[Log rocket - how to use css scroll snap](https://blog.logrocket.com/how-to-use-css-scroll-snap/)

[MDN scroll-snap-type](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type)

<!--more-->
