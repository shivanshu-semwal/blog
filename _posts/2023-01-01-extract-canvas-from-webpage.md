---
title: Extract canvas from webpage
layout: post
tags:
- webdev
description: How to save canvas element as a image in browser.
---

Use this code snippet in developer mode to download `canvas` element as an image.

```javascript
function downloadCanvasAsImage(name){
    // get the canvas element, you have to change this with the element you want
    let canvasImage = document.getElementById('pdfCanvas').toDataURL('image/png'); 
    
    let xhr = new XMLHttpRequest();
    xhr.responseType = 'blob';
    xhr.onload = function () {
        let a = document.createElement('a');
        a.href = window.URL.createObjectURL(xhr.response);
        a.download =  name + '.png';
        a.style.display = 'none';
        document.body.appendChild(a);
        a.click();
        a.remove();
    };
    xhr.open('GET', canvasImage);
    xhr.send();
}

downloadCanvasAsImage('temp')
```

Here is how it works

- first `canvasImage` should point to the canvas element in DOM, you can use `getElementById` or `getElementByTags` and then on it we use `toDataURL` to convert to a link to download image.
- after that it send `XMLHttpsRequest` to get the images
- the image is then downloaded with name passed as parameter

## Other tricks

Suppose a pdf is displayed as a series of canvas then you can download the entire pdf using this method.
You need to add delay while using this (for the canvas to load completely) for which you can use the `setTimeout()` method.

```js
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function downloadIt(){
    // get the end count, replace this with the one for site you are using
    x = parseInt(document.getElementById('page_count').textContent)

    // download image one by one
    for(let i=0;i<x;i++){
        downloadCanvasAsImage('se' + i.toString());

        // replace this with the next button on your site
        document.getElementById('next').click();

        // wait for the image to load
        await sleep(500);
    }
}
downloadIt()
```

You can use some tool to combine the image to one pdf file.
Here is how I do it `convert *.png temp.pdf` using `imagemagick`.
