---
title: Accessing Local Dev Server From Anywhere Using Self-Signed Certificate
date: "2022-02-03T12:40:32.169Z"
template: "post"
draft: false
slug: "accessing-local-dev-server-from-anywhere-using-self-signed-certificate"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

## The prerequisites are:

1. If you are working a research task or you need to let BE mate to look into some bugs during API integration.
2. The code cannot be deployed to production or any other environment.
3. You are working remotely and you need to allow colleagues access the local dev server from anywhere.

In this article, I will show you how to do that.

There are several solutions: ngrok / PageKite / localtunnel.

I'm not going to describe the process of comparing all of them. My choice is localtunnel.

### Install localtunnel

```bash
npm install -g localtunnel
```

### start localtunnel

The local dev server is running on port 3000 with protocol `http`.

```bash
lt --port=3000 --subdomain=tigressbailey
```

The localtunnel will create a subdomain for you.
The local dev server can be accessed by `https://baileylocalalphatest.loca.lt/`.
The protocol is `https`. Localtunnel would provide a certificate for you. This is really neat.

### Deal with self-signed certificate

In most cases, our local dev servers' certificates are self-signed if it is using the CRA / NextJS.

It would get a 502 error if you try to run the same command for `https://localhost:3000/`.

This is due to localtunnel would verify certificates for your local HTTPS server.

I've seen tons of Github issues about this, here is the solution:

```bash
lt --port 3000 --subdomain baileylocalalphatest -o --print-requests --local-https --allow-invalid-cert
```

`-o --print-requests` will print the request to the console and opens the localtunnel URL in your browser when the localtunnel is ready.

`--local-https` will use the local HTTPS server to serve the localtunnel.

`--allow-invalid-cert` will allow the localtunnel to accept self-signed certificates.

Then you can share the localtunnel URL with your colleagues.

Happy hacking!

---

I'm enjoying the family time during the Spring Festival.

Happy the year of the tiger.

So thrilled thatI've learned a few Flutter coding skills these days.
