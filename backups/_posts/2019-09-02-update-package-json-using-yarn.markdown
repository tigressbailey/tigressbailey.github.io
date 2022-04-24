---
layout: post
title: "Update package.json Using Yarn"
date: 2019-09-02T22:15:33+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Do you know `yarn upgrade`, `yarn upgrade --latest` and `yarn upgrade -interactive-upgrade` only update the yarn.lock file?

I've tried tons of solutions using `yarn blablabla` with no luck.

The ultimate spell is... `npm-check-updates` plus `yarn`.

Here is the code:
```
yarn global add npm-check-updates
ncu -u
yarn
```

<!--more-->
