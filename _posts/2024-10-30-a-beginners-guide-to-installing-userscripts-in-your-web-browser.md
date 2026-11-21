---
title: "A Beginner's Guide to Installing Userscripts in Your Web Browser"
layout: post
tags:
  - userscript
  - web-browser
description: How to install and use userscripts safely in Chromium and Firefox.
image:
  path: /assets/img/covers/a-beginners-guide-to-installing-userscripts-in-your-web-browser-cover.png
  alt: "A Beginner's Guide to Installing Userscripts in Your Web Browser"
---


Userscripts are uses to run javascript on top of a website to automate some repetitive task or add or remove elements, and many other tasks like web scraping.

## Userscript manager

To run userscripts, you need a browser extension that runs userscript called **userscript manager**. Here is list of some of these extensions for Google Chrome and Firefox:

- Chrome
    - [Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) Properitary, exposes more features
    - [Violentmonkey](https://chrome.google.com/webstore/detail/violent-monkey/jinjaccalgkegednnccohejagnlnfdag) Open source
- Firefox
    - [Greasemonkey](https://addons.mozilla.org/firefox/addon/greasemonkey/) Open source
    - [Tampermonkey](https://addons.mozilla.org/firefox/addon/tampermonkey/) Properitary, exposes more features
    - [Violentmonkey](https://addons.mozilla.org/firefox/addon/violentmonkey/) Open source

I suggest using **Violentmonkey** <https://violentmonkey.github.io/> as it is available for both Mozilla Firefox and Google Chrome and is also open source.

## Userscript repositories

Now you can find and use userscripts form internet or create your own. Before installing any userscript check it's source code also as it can be malicious.

Here are some userscript repositories:

- Greasyfork - <https://greasyfork.org/>
- openuserjs - <https://openuserjs.org/> - 
- Userscript Zone - <https://www.userscript.zone/>

## Creating your own userscript

Creating a userscript according to you usecase is much better and more secure. Here are some sample examples.

### Hiding YouTube videos with certain words

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

//================================
// Configurations
//   - specify texts you don't want to see.
var g_list = [ "anime" ];
//================================

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

var observer = new MutationObserver(function(mutations) {
    observer.disconnect();
    main();
    observer.observe(document, config);
});

var config = {
    childList: true,
    characterData: true,
    subtree: true
}

observer.observe(document, config);
```

Here is how it works:

- The top part is related configuration of userscript. Here the main part to focus is the `@match` option which defines for which URL you want to run the script.

```
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
```

- `MutationObserver` observes the page for changes, by recursively calling the function.
- Inside the `main()` function you have to get the elements and based on a criteria set their `display` style is set to `none`.

### Hiding certain subreddits posts from Reddit

This is useful when you are not logged in, also here we will use the old interface of reddit [old.reddit.com](https://old.reddit.com).

```js
// ==UserScript==
// @name     blacklist-subreddits
// @version  1
// @match    https://old.reddit.com/*
// @grant    none
// ==/UserScript==


// configuration
sub_list = [ "r/memes" ]

function main(){
 document.getElementById("siteTable").childNodes.forEach(function(item){
    subreddit = item.getElementsByClassName("subreddit hover may-blank");
    if (subreddit.length === 0) {}
    else{
      sub = subreddit[0].innerHTML
      if(sub_list.includes(sub)){
        item.style.display = "none";
      }
    }
  })
}

var observer = new MutationObserver(function(mutations) {
    observer.disconnect();
    main();
    observer.observe(document, config);
});

var config = {
    childList: true,
    characterData: true,
    subtree: true
}

observer.observe(document, config);
```

## References

- [Wikipedia Userscript](https://en.wikipedia.org/wiki/Userscript)
