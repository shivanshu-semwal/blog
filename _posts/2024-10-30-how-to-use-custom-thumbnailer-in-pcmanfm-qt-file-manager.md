---
title: "How to use custom thumbnailer in pcmanfm-qt file manager"
layout: post
tags:
  - linux
  - pcmanfm-qt
description: Generate custom thumbnails for PDFs and other files in pcmanfm-qt using thumbnailers.
---

Thumbnails are essential for a file manager, especially for images and ebooks.

The thumbnailers used by `pcmanfm-qt` are in `/usr/share/thumbnailers`.

To create a custom PDF thumbnailer:

- install `pdftoppm`
- create `/usr/bin/pdftoppm-thumbnailer`

```bash
#!/bin/bash

out="$3"
pdftoppm -png -singlefile -scale-to $1 "$2" "${out%".png"}"
```

- make the script executable: `chmod +x /usr/bin/pdftoppm-thumbnailer`
- create `/usr/share/thumbnailers/pdftoppm.thumbnailer`

```ini
[Thumbnailer Entry]
Exec=pdftoppm-thumbnailer %s %i %o
MimeType=application/pdf;application/x-pdf;image/pdf;application/x-gzpostscript;application/postscript;
```

Then restart `pcmanfm-qt`.

## Other resources

- Arch Wiki on file manager functionality
