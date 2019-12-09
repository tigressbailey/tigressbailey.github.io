---
layout: post
title: "How to Resolve Slowly Brew Update"
date: 2019-12-09T14:22:05+08:00
author: tigressbailey
sitemap: false
keywords: "brew update is slow"
description: "How to Resolve Slowly Brew Update"
---

Recently, I've found `brew update` is extremely slow no matter I was in the office or at home.
To be honest, the network is not so good in China these days.
So, here comes the solution.

1. Set up a Socks5 proxy.
2. Open the bask profile or .zshrc. I'm using ZSH, so:
```
vi $HOME/.zshrd
```
3. Add the proxy and unproxy command into the file. For example, the socks5 URL is 127.0.0.1:1081.
```
alias proxy='export all_proxy=socks5://127.0.0.1:1081'
alias unproxy='unset all_proxy'
```
4. Save and `source $HOME/.zshrc`

Now, use `proxy && brew update && unproxy` is resolved my problem.

<!--more-->
