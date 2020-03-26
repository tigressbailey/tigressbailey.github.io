---
layout: post
title: "Advanced Search Operators"
date: 2020-03-26T09:26:34+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

As an efficient developer, it is quite important to get the precise knowledge and learn.

Search operators can be used in this case.

I will list my collection and hope it helps!

- allintitle: and intitle:
  ```
  allintitle: ES2020
  intitle: ES2020
  ```

- -exclusion
  ```
  allintitle: ES2020 -Private
  ```

- "Terms"
  ```
  "JavaScript is the best language"
  ```

- site:
  ```
  site:eloquentpoker.com
  ```

- -allinurl: and -inurl:
  ```
  site:mozilla.com -inurl:www
  ```

- -site:
  ```
  site:github.com.\* -site:github.com
  ```

- filetype:
  ```
  site:github.com filetype:md
  ```


<!--more-->
