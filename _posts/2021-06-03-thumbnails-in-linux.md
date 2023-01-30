---
title: Thumbnails mystery in linux
layout: post
tags:
  - linux
---

Thumbnails are essential part of a file manager, especially if you have huge amount of images, and ebooks.

How thumbnails are generated and where should they be placed is defined in [freedesktop Specification](https://www.freedesktop.org/wiki/Specifications/thumbnails/).

So, the usual location of thumbnail generated is in - `$HOME\.cache\thumbnails`. This directory contain other sub directories such as `large`, `normal`, `small`.

The thumbnail is stored in `.png` format, and the name of the thumbnail file is the md5 hash for the file uri given as `file:///location_of_file`, for e.g. a file `/home/shiv/Documents/mypdf.pdf` will have a thumbnail named `d9682995d06cca8e3b838a294aff4e91.png`.

Here is a command I used to generate the thumbnail name for the file:

```bash
echo -n "file:///home/shiv/Documents/mypdf.pdf" | md5sum | awk '{print $1}'
```

## How to set a custom thumbnailer for `pcmanfm-qt` file manager

The thumbnailers used by `pcmanfm-qt` are placed in `/usr/share/thumbnailers`.

Here is how I set custom pdf thumbnailer for `.pdf` files.

- install `pdftoppm` (Portable Document Format (PDF) to Portable Pixmap (PPM) converter)
- create file `/usr/bin/pdftoppm-thumbnailer`

```bash
#!/bin/bash

out="$3"
pdftoppm -png -singlefile -scale-to $1 "$2" "${out%".png"}"
```

- give the file execute permission `chmod +x /usr/bin/pdftoppm-thumbnailer`
- create file `/usr/share/thumbnailers/pdftoppm.thumbnailer`

```
[Thumbnailer Entry]
Exec=pdftoppm-thumbnailer %s %i %o
MimeType=application/pdf;application/x-pdf;image/pdf;application/x-gzpostscript;application/postscript;
```

- restart `pcmanfm-qt`

## Other resource

- [Arch Wiki on File manager functionality](https://wiki.archlinux.org/title/File_manager_functionality)
