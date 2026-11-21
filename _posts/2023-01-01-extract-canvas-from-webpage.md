---
title: Extract canvas from webpage
layout: post
tags:
- webdev
description: How to save canvas element as a image in browser.
---


Use developer tools to locate the canvas you want to download. Get it's `id` attribute, in my case it was `pdfCanvas`. Now enter this function in developer tools console:

```javascript
function downloadCanvasAsImage(name){
    // id of canvas element
    let canvasId= 'pdfCanvas'

    // get the canvas element, you have to change this with the element you want
    let canvasImage = document.getElementById(canvasId).toDataURL('image/png'); 
    
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
```

Now use this function to download the image,

```javascript
let image_name = 'image.png'
downloadCanvasAsImage(image_name)
```

Here is how it works:

- first `canvasImage` should point to the canvas element in DOM, you can use `getElementById` or `getElementByTags` and then use `toDataURL` to get the URL to download image.
- after that it sends a `XMLHttpsRequest` to get the images
- the image is then downloaded with name passed as function parameter.
