---
title: Types of React APP States
date: "2020-07-16T12:40:32.169Z"
template: "post"
draft: false
slug: "types-of-react-app-states"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

I've been working on several React projects since 2015.

It is such a pleasure to implement the modern web APP or native APP with it.

I can still remember some remarkable timings when using it.

First, contributed a sidebar component to an UI framework. CSS is the most hard part that I tried to avoid organizing them like Bootstrap did in a couple of years ago. I mean, Bootstrap is big. But I wanted to think through the styling independently, just like how I dress everyday.

Second, use React along with an traditional website.

Third, categorize the states based on my experience. And a recommendation from Steve Kinney. Here we go.

- Modal data: The actual data in your application.
  For instance, catalogs, catalog of products.
- UI state: Sort those modal data in ascending or descending order.
  For instance, filters and orders.
- Session state: User relative status.
  For instance, user roles, user access, user login state.
- Communication: The status of CRUD the data from the server.
  For instance, hasLoaded, loading, error, updating.
- Location: The roads of the APP.
  For instance, client routing, url.

Or, it might make sense to think about state relative to time.

- Modal state: This is likely the date in your application. This could be the items in a given list.
- Ephemeral state: Stuff like the value of an input field that will be wiped away when you hit 'enter'. This could the order in which a given list is sorted.
