---
layout: post
title: "Security Coding Guideline"
date: 2020-03-17T11:15:09+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

1. Use simple data binding in JSX.
2. Only use `dangerouslySetInnerHTML` in combination with sanitization: <https://www.npmjs.com/package/dompurify>
3. Do not rely on parsers for security.
   For instance:
   <https://www.npmjs.com/package/url-parse>
   <https://www.npmjs.com/package/react-html-parser>
4. Do not put data in the DOM directly: `domElement.innerHTML = 'Malicious code'`
5. Use `yarn audit/npm audit` to scan the node module dependencies.
6. Keep node modules up to date.

<!--more-->
