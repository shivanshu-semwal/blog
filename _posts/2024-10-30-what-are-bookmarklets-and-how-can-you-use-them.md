---
title: "What Are Bookmarklets and How Can You Use Them?"
layout: post
tags:
  - browsers
  - bookmarklet
description: A guide to bookmarklets and how to turn JavaScript into browser bookmarks.
---

A bookmarklet is a bookmark stored in a web browser that contains JavaScript commands that add new features to the browser.

The first bookmarklet I found was in a GitHub repository with a script that adds a dropdown menu to change CSS.

```js
function () {
  var body = document.getElementsByTagName("body")[0];
  script = document.createElement("script");
  script.type = "text/javascript";
  script.src = "https://dohliam.github.io/dropin-minimal-css/switcher.js";
  body.appendChild(script);
}
```

Because JavaScript cannot be directly stored as a URL, it is encoded. You can use a site like `mrcoles.com/bookmarklet` to generate the bookmarklet URL.

```bash
python3 -c "import urllib.parse, sys; print('javascript:('+urllib.parse.quote(''.join([line[:-1] for line in open(sys.argv[1]).readlines()]))+')()')" javascript_code.js
```

## References

- Wikipedia on bookmarklets
