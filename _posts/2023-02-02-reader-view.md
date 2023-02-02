---
title: Reader View in modern web
layout: post
tags:
- web
---

## Introduction

Reader view is inbuilt in firefox and it is open source.
This extension <https://add0n.com/chrome-reader-view.html> use the same implementation of firefox.

## Custom CSS

In this extension you can provide addition css and make the reader view much much better.
You can create custom styles and modify them.

Here are some tips from my side:

- pick some better, serif and sans-serif and monospace font which you like. You can use fonts which  are available in firefox pocket.

- Now you can change `sans-serif`, `sans`, font to your choice.

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

- You can change the colors for different mode.
    - sepia
    - light
    - dark
    - nord

```css
/* sepia mode */
body[data-mode="sepia"] {
}
```

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

- for all mode

```css
/* for all mode */
body {
  bottom: 64px;
}

a:link {
  text-decoration: none;
  font-weight: normal;
}

pre {
  white-space: pre-wrap;
}
```

> Finally you can use the developer tools to inspect elements if you can't figure out class of some elements.

Here is how the body part,

```html
<!-- mode: dark font:serif -->
<body tabindex="1" data-images="false" data-mode="dark" data-font="serif" data-loaded="true">
```

Here is what you can do,

```css
body[data-mode="dark" data-font="serif"]{
 font-family: "Computer Modern";
}

body[data-mode="dark" data-font="sans-serif"]{
 font-family: "Computer Modern";
}
```

Similarly for other styles and there corresponding fonts.

## What I use

```css
/* latin-ext */
@font-face {
  font-family: 'Zilla Slab';
  font-style: italic;
  font-weight: 400;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa4ZfeM_74wlPZtksIFaj8K8VSMZlE.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: 'Zilla Slab';
  font-style: italic;
  font-weight: 400;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa4ZfeM_74wlPZtksIFaj8K_1SM.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
/* latin-ext */
@font-face {
  font-family: 'Zilla Slab';
  font-style: italic;
  font-weight: 600;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFanZfeM_74wlPZtksIFaj8CIHCZV3B3Taw.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: 'Zilla Slab';
  font-style: italic;
  font-weight: 600;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFanZfeM_74wlPZtksIFaj8CIHCZWXB3.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
/* latin-ext */
@font-face {
  font-family: 'Zilla Slab';
  font-style: normal;
  font-weight: 400;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa6ZfeM_74wlPZtksIFajQ6_UyI.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: 'Zilla Slab';
  font-style: normal;
  font-weight: 400;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa6ZfeM_74wlPZtksIFajo6_Q.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
/* latin-ext */
@font-face {
  font-family: 'Zilla Slab';
  font-style: normal;
  font-weight: 600;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa5ZfeM_74wlPZtksIFYuUe6H2pW2hz.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: 'Zilla Slab';
  font-style: normal;
  font-weight: 600;
  src: url(https://fonts.gstatic.com/s/zillaslab/v11/dFa5ZfeM_74wlPZtksIFYuUe6HOpWw.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: "IdealSans";
  src: url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-Book_Web.7d16617ee3afbae04aad0840190a4fa2.woff),
    url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-Book_Web.8ceb4de0789288847af513bd51c5818a.woff2);
}
@font-face {
  font-family: "IdealSans";
  font-weight: bold;
  src: url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-Semibold_Web.ab443439365d14e4d3f33b96ace4cf3d.woff),
    url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-Semibold_Web.b23b3824fab9ab4dc8006a6f24423b0f.woff2);
}
@font-face {
  font-family: "IdealSans";
  font-style: italic;
  src: url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-BookItalic_Web.086c41cbfa1e131056192654defa325c.woff),
    url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-BookItalic_Web.e3cf9a006e02acd2311610854754c42b.woff2);
}
@font-face {
  font-family: "IdealSans";
  font-style: italic;
  font-weight: bold;
  src: url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-SemiboldItalic_Web.3e33dacd17e8b971382f9075f200fdf5.woff),
    url(https://assets.getpocket.com/web-ui/assets/IdealSansSSm-SemiboldItalic_Web.3e33dacd17e8b971382f9075f200fdf5.woff);
}

body[data-font="serif"]{font-family: "Zilla Slab";}
body[data-mode="dark" data-font="sans-serif"]{font-family: "IdealSans";}

body[data-mode="dark" data-font="serif"]{ font-family: "Zilla Slab";}
body[data-mode="dark" data-font="sans-serif"]{font-family: "IdealSans";}

body[data-mode="dark"] { background-color: #1a1a1a; color: #f2f2f2;}
body[data-mode="dark"] h1 { color: #ebcb8b; text-align: center;}
body[data-mode="dark"] h2 { color: #a3be8c; }
body[data-mode="dark"] a:visited { color: #bf616a; }
body[data-mode="dark"] a:link,
body[data-mode="dark"] a:link:hover,
body[data-mode="dark"] a:link:active,
body[data-mode="dark"] a:link * { color: #d08770; }
body[data-mode="dark"] pre { background-color: #080808; padding: 10px;}
body { bottom: 64px; }
a:link { text-decoration: none; font-weight: normal; }
pre { white-space: pre-wrap;}
```
