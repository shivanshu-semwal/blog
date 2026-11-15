---
title: "A Beginner's Guide to Installing Userscripts in Your Web Browser"
layout: post
tags:
  - userscript
  - web-browser
description: How to install and use userscripts safely in Chromium and Firefox.
---

Userscripts are scripts that run JavaScript on top of a website to automate repetitive tasks, hide elements, or modify the page.

## Userscript manager

To run userscripts, you need a browser extension called a userscript manager. Examples include:

- Chrome: Tampermonkey, ViolentMonkey
- Firefox: Greasemonkey, Tampermonkey, ViolentMonkey

I suggest using Violentmonkey because it is available on both browsers and is open source.

## Userscript repositories

You can find userscripts on the internet or create your own.

Recommended repositories:

- GreasyFork
- openuserjs
- Userscript Zone

## Creating your own userscript

Example for hiding YouTube videos with certain words:

```js
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://www.youtube.com/
// @icon         https://www.google.com/s2/favicons?sz=64&domain=youtube.com
// @grant        none
// ==/UserScript==

var g_list = [ "anime" ];

function main() {
    document.querySelectorAll("ytd-video-renderer,ytd-channel-renderer,ytd-grid-video-renderer,ytd-playlist-renderer").forEach(function(elem) {
        var str = elem.innerText;
        for (var i = 0; i < g_list.length; ++i) {
            var ngword = g_list[i];
            if (ngword == "") continue;
            ngword = ngword.replace(/^\s+|\s+$/g, "");
            var obj = new RegExp(ngword, "i");
            var index = str.search(obj);
            if (index != -1) {
                elem.style.display = "none";
            }
        }
    });
}
```

The `@match` directive controls which pages the script runs on.

`MutationObserver` is often used to observe dynamic content and rerun the logic when the page changes.

## References

- Wikipedia Userscript
