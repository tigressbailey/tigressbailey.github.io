---
title: Update package.json Using Yarn
date: "2019-09-02T12:40:32.169Z"
template: "post"
draft: false
slug: "update-package-json-using-yarn"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
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
