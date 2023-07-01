---
title: Reader View - a solution to modern web
layout: post
tags:
- web
description: All about reader view extensions and apps.
---

## Introduction

Reader view is inbuilt in firefox and it is open source.
Here is the library used for that - <https://github.com/mozilla/readability>.
This extension <https://add0n.com/chrome-reader-view.html> uses the same library but is also available for other browsers.

## How it works?

Reader view parses a web page and removes the custom styles used by website and shows them in a generic way.
This will also remove ads, and other useless things that are distracting. But it don't work on all sites,
you can report those sites to <https://github.com/mozilla/readability>.

## Customizations

In this extension you can provide addition css and make the reader view much much better.
You can create custom styles and modify them.

Here are some tips from my side:

- Pick some better font.
  Decide on which serif, sans-serif and monospace font you like.
- You can change `sans-serif`, `sans`, font to your choice using `data-font` attribute choice:

```css
/* sans-serif font */
body[data-font="sans-serif"] {
  font-family: "CMU Sans Serif";
}

/* serif font */
body[data-font="serif"] {
  font-family: "CMU Serif";
}
```

- You can change the colors for different mode, using the attribute `data-mode`
    - `sepia`
    - `light`
    - `dark`
    - `nord`

```css
/* dark mode */
body[data-mode="dark"] h1 {
  color: #ebcb8b;
  text-align: center;
}

body[data-mode="dark"] h2 {
  color: #a3be8c;
}

body[data-mode="dark"] {
  background-color: #1a1a1a;
  color: #f2f2f2;
}

body[data-mode="dark"] a:visited {
  color: #bf616a;
}

body[data-mode="dark"] a:link,
body[data-mode="dark"] a:link:hover,
body[data-mode="dark"] a:link:active,
body[data-mode="dark"] a:link * {
  color: #d08770;
}
```

- You can also mix these attributes,

```css
body[data-mode="dark" data-font="serif"]{
  font-family: "Computer Modern";
}

body[data-mode="dark" data-font="sans-serif"]{
  font-family: "Computer Modern";
}
```

- Finally you can use the developer tools to inspect elements if you can't figure out `class` of some elements to target them.