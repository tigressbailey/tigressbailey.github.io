---
title: What is the differences between Docker and Virtual Machines
date: "2022-08-25T12:40:32.169Z"
template: "post"
draft: false
slug: "what-is-the-differences-between-docker-and-virtual-machines"
category: "Devops"
tags:
  - "Devops"
  - "Docker"
  - "Virtual Machines"  
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Before Clarify the differences, let's list the OS layers for the reference purposes.
1. Application
2. OS Kernel

The OS Kernel layer is responsible for communicating with the hardwares.
Windows and Linux have different OS Kernels for sure.

Applications are different among Linux-based systems.
Centos, Debian and Ubuntu have their own application layer. But the OS Kernel layer is the same.

The vital difference between Docker and Virtual Machine is Docker virtualized the application layer. Virtual Machine virtualized both the application layer and OS kernel layer.
According the different virtualized setup, we can list some key points:
1. Docker image size is much smaller
2. Docker container is faster

Well, it looks like Docker just win it all. Please don't forget below.

1. Virtual Machine images support crossing-platforms.
2. Docker images cannot support crossing-platforms without Docker Toolbox. It cannot support different system versions sometimes.

Now we could easily chose Docker and Virtual Machine based on the requirements.