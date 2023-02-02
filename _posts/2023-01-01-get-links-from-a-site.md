---
title: Extract all links from a site
layout: post
tags:
- webdev
---

To extract all links form a website, you can run this code snippet in developer console of your browser.

```js
function download(content, mimeType, filename){
  const a = document.createElement('a') // Create "a" element
  const blob = new Blob([content], {type: mimeType}) // Create a blob (file-like object)
  const url = URL.createObjectURL(blob) // Create an object URL from blob
  a.setAttribute('href', url) // Set "a" element link
  a.setAttribute('download', filename) // Set download filename
  a.click() // Start downloading
}

// get all links
s = document.getElementsByTagName('a')
a="";
for(i=0;i<s.length;i++){
    a = a + '\n' + s[i].href
}

// copy links to file and download it
download(a, 'text/plain', 'links.txt')
```

Here is how it works:

- first we extract all `a` elements for the html of webpage
- now we create a string by concatenation of all links
- then we download a file with the string placed inside it using `download` function