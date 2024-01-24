---
title: How to create bookmarklets?
layout: post
tags:
- web
- tools
description: How to create bookmarklets and use them.
---

I was first introduced to bookmaklets by this github repo <https://github.com/dohliam/dropin-minimal-css>.
It contained a bookmarklet which adds a dropdown menu to current webpage from which we can choose different 
minimal css styles.

```js
function () {
  var body = document.getElementsByTagName("body")[0];
  script = document.createElement("script");
  script.type = "text/javascript";
  script.src = "https://dohliam.github.io/dropin-minimal-css/switcher.js";
  body.appendChild(script);
}
```

A bookmarklet is a bookmark stored in a web browser that contains JavaScript commands that add new features to the browser. 
They are stored as the URL of a bookmark in a web browser or as a hyperlink on a web page. 

## Python script to convert a `js` file to bookmarklet

```py
#!/bin/python3

import urllib.parse
import sys

FILE = sys.argv[1]

with open(FILE) as file:
    s = ''
    for line in file.readlines():
        s += line[:-1]
    print(s)
    # print(urllib.parse.unquote(''.join(file.readlines())))
    print('javascript:(' + urllib.parse.quote(s) + ')()')
```

You can also use this site <https://mrcoles.com/bookmarklet/> if you don't have python installed.

## References

- <https://en.wikipedia.org/wiki/Bookmarklet>