---
title: "Discover the LXQT Desktop Environment"
layout: post
tags:
  - linux
  - lxqt
description: A basic overview of LXQt, its components, and how the sessions differ in Debian and Lubuntu.
---

LXQt is a lightweight and modular desktop environment. If you want to try it, you can use Lubuntu, a Ubuntu flavor that ships with LXQt.

## Components of LXQt

LXQt is modular. It has the following pieces:

- `lxqt-panel`
- `openbox`
- `PCManFM-Qt`
- `featherpad`
- `sddm`

## Multiple sessions in sddm

When you log in with sddm, you can see multiple LXQt sessions. Each session is linked to a desktop entry file such as:

- `/usr/share/xsessions/lubuntu.desktop`
- `/usr/share/xsessions/lxqt.desktop`

These differ by the `Exec` value and which Openbox config they use.

## References

- Lubuntu official site
- LXQt official site
