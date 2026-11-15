---
title: "Step-by-Step Guide to Setting a Folder Thumbnail in Dolphin File Manager"
layout: post
tags:
  - kde
  - linux
  - dolphin
description: Set a folder image as the thumbnail in Dolphin using Keneric.
---

This post is specifically for the Dolphin file manager in KDE. The goal was to have an image inside a folder, named `folder.jpg`, and have it displayed as the folder thumbnail.

For this I used `keneric`.

- Install `keneric`; it creates a shared library file `keneric.so`.
- Create a desktop file named `kenericfolder.desktop` in a KService location such as `~/.local/share/kservices5`.

```ini
[Desktop Entry]
Type=Service
Name=Folder (Keneric)
Name[x-test]=xxFolder (Keneric)xx

X-KDE-ServiceTypes=ThumbCreator
MimeType=inode/directory;

X-KDE-Library=keneric
CacheThumbnail=true
```

- Create a script named `stripPicture` in a directory in `$PATH`.

```sh
#!/bin/sh
# Usage: stripPicture fullname mime exportPicture

fullname="$1"
mime="$2"
exportPicture="$3"

case "$mime" in
    inode/directory)
        cp "$fullname"/folder.jpg "$exportPicture"
        cp "$fullname"/Cover.png "$exportPicture"
        cp "$fullname"/Poster.jpg "$exportPicture"
        cp "$fullname"/Front.jpg "$exportPicture"
    exit
    ;;

    *)
    exit
esac
```

Then open Dolphin, go to `Configure Dolphin`, and under `General > Previews` enable the relevant folder thumbnail setting.

## References

- KDE forum thread on folder thumbnails
