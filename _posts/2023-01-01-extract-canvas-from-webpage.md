---
title: Extract canvas from webpage
layout: post
---

Use this code snippet in developer mode to download canvas as an image.

```js
function downloadCanvasAsImage(name){
    let canvasImage = document.getElementById('pdfCanvas').toDataURL('image/png'); // get the canvas element
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

Suppose a pdf is displayed as a series of canvas then you can download the entire pdf using this method. You need to add delay while using this for which you can use the `setTimeout()` method.

```js
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function downloadIt(){
 // get the end count
    x = parseInt(document.getElementById('page_count').textContent)

 // download image one by one
    for(let i=0;i<x;i++){
        downloadCanvasAsImage('se' + i.toString());
        document.getElementById('next').click();
        await sleep(500);
    }
}
downloadIt()
```

You can use some tool to combine the image to one pdf file. Here is how I do it `convert *.png temp.pdf` using imagemigick.
