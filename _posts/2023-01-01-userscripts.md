---
title: How to use userscripts
layout: post
tags:
    - webdev
    - web
description: How to use userscripts to do repetitive tasks.
---

Userscripts, are uses to run javascript on top of a website to automate some repetitive task or add or remove elements, and many other tasks like scraping.

## User script manager

So to run userscripts, you first need a extension that run userscript called **user script manager**.
Here are some for chrome and firefox.

- Chrome
    - [Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) Properitary, exposes more features
    - [Violentmonkey](https://chrome.google.com/webstore/detail/violent-monkey/jinjaccalgkegednnccohejagnlnfdag) Open source
- Firefox
    - [Greasemonkey](https://addons.mozilla.org/firefox/addon/greasemonkey/) Open source
    - [Tampermonkey](https://addons.mozilla.org/firefox/addon/tampermonkey/) Properitary, exposes more features
    - [Violentmonkey](https://addons.mozilla.org/firefox/addon/violentmonkey/) Open source

**Violentmonkey** <https://violentmonkey.github.io/> availiable for both firefox and chrome, also open source. I suggest you use this one.

## User script repositories

Now you can find and use userscripts form internet or create your own. Before installing some userscript check it's code too, it maybe malicious.

- <https://greasyfork.org/>
- <https://openuserjs.org/> - openuserjs
- <https://www.userscript.zone/>

## Create you own user script

Creating your own userscript according to you usecase is much better, here are some examples.

### Remove youtube videos with certain words

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

So here is how it works

- first the top part with configuration

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

Here the main part to focus is the `@match` option which defines for which url you want to run the script.

- now you create a `MutationObserver`  and then observe the page for changes, by recursively calling the function.
- inside the `main()` function you have to get the elements and based on a criteria set their `dispay` to `none`

### Remove post from reddit with certain subreddits

This is useful when you are not logged in, also here we will use the old interface of reddit.

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
