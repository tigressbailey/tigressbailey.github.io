---
title: How to test library using private registry
date: "2023-01-11T12:40:32.169Z"
template: "post"
draft: false
slug: "how-to-test-library-using-private-registry"
category: "Frontend"
tags:
  - "Frontend"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

I recently need a private npm registry for testing a standalone library before publishing it to npm.

Verdaccio came to mind - I recall it is introduced via a framework publish showcase.
Verdaccio is a simple, zero-config-required local private npm registry. 

## Install Verdaccio
```bash
yarn global add verdaccio@6-next
```

## Start it

```bash
verdaccio
```

Visit http://localhost:4873/.

This is the private registry

## Create user

```bash
npm adduser --registry http://localhost:4873
```

## Publish
```shell
npm publish --registry http://localhost:4873
```

Visit http://localhost:4873/.

Verify the node module has been published

`npm link` is also very convenient for testing the library before publish.

But it cannot completely simulate the production ENV.

I would suggest using Verdaccio afterwards.

----
Happy new year!!!

I haven't publish anything in last two month because some life updates.

I feel very energized about updating my blog again.

I'm currently sitting in a warm apartment.

Fresh air, unblocked internet and sparkling water perfectly fit in.
