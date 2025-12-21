---
layout: default
title: "what is box?"
description: "rust box smart pointer — what, why, how"
---

### Understanding the Box Pointer

Here is a diagram showing how the stack points to the heap:

![Rust Box Diagram]({{ site.baseimageurl }}/assets/images/Box.png)

A `Box<T>` allows you to store data on the heap.