---
title: Critical Path of Progressive Web App
date: "2018-11-08T12:40:32.169Z"
template: "post"
draft: false
slug: "critical-path-of-progressive-web-app"
category: "Frontend"
tags:
  - "Frontend"
  - "pwa"
  - "Web Development"
description: "Everyone has keen interests in Progressive Web App.I was wondering how can I explain it in a simple way."
socialImage: ""
---

Everyone has keen interests in Progressive Web App.
I was wondering how can I explain it in a simple way.

***

## Progressive Metadata

- Viewport

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

- Fullscreen

```
<meta name="apple-mobile-web-app-capable" content="yes">
```

- StatusBar

```
<meta name="apple-mobile-web-app-status-bar-style" content="black">
```

- Home Screen Title

```
<meta name="apple-mobile-web-app-title" content="APP title">
```

## Manifest.json

```
<link rel="manifest" href="/manifest.json">
```

```
{
  "name": "APP title",
  "short_name": "AT",
  "icons": [
    {
      "src": "/img/launcher-icon-2x.png",
      "size": "192x192",
      "type": "image/png"
    },
    {
      "src": "/img/launcher-icon-4x.png",
      "size": "512x512",
      "type": "image/png"
    }
  ],
  "theme_color": "#c3c3c3",
  "background_color": "#c3c3c3",
  "display": "standalone"
}
```

> You can use `fullscreen`, `standalone`, `minimal-ui`, `browser` for `display`. If the mobile browser doesn't support the `fullscreen`. It will degrade to `standalone`. This is a progressively feature.


## Home screen Icon

One handy choice - [Add to Homescreen](https://github.com/cubiq/add-to-homescreen)
