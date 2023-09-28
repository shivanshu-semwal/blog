---
title: Downloading files using curl
layout: post
tags:
- web
- linux
description: How to download files using curl.
---

Here is a trick to use curl to download items which fails due to restriction put in servers.
(Like they will check `User-Agent` or some other cookie value.)

First open that resource using browser. Open network tab in developer tools and reload the page.
After that select the resource which you want to download, and right click on it and select
`Copy Value`. From the dropdown you can then select `Copy as curl`.
