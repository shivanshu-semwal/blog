---
title: How to use image a thumbnail of folder
layout: post
tags: 
  - linux
description: Make custom thumbnailer for dolphoin file manager.
---

This post is specifically for dolphin file manager in linux.
So my goal was to achieve following:

- have a image inside some folders with name `folder.jpg`
- and display it as thumbnail of the folder

## Setting up kenric

For this I used [`keneric`](https://github.com/shivanshu-semwal/keneric).

- First install `keneric`.
  This will create a shared library file `keneric.so` in `/usr/lib/x86_64-linux-gnu/qt5/plugins`
- Now create a desktop file `kenericfolder.desktop` in one of the location given by
  command `kf5-config --path services`, which is `~/.local/share/kservices5`
  or `/usr/share/kseervices5` for me

Content of `kenericfolder.desktop`

```
[Desktop Entry]
Type=Service
Name=Folder (Keneric)
Name[x-test]=xxFolder (Keneric)xx

X-KDE-ServiceTypes=ThumbCreator
MimeType=inode/directory;

X-KDE-Library=keneric
CacheThumbnail=true
```

- Now create a script `stripPicture` in one of the locations in your `$PATH` variable.

Contents of `stripPicture`

```sh
#!/bin/sh
# Usage: stripPicture fullname mime exportPicture

fullname="$1"
mime="$2"
exportPicture="$3"

# thumbnail options by mime type
case "$mime" in
    inode/directory)
        cp "$fullname"/folder.jpg "$exportPicture"
        cp "$fullname"/Cover.png "$exportPicture"
        cp "$fullname"/Poster.jpg "$exportPicture"
        cp "$fullname"/Front.jpg "$exportPicture"
    exit
    ;;

    *)
        # case trap
    exit
esac
```

## Configuring dolphin

- Now open dolphin -> open `Configure Dolphin...` window.
- Now in `General` category, in the `Previews` tab disable the `Folder` preview
  and enable `Folders only one image (Keneric)` option.

## References

- [https://forum.kde.org/viewtopic.php?f=224&t=156241](https://forum.kde.org/viewtopic.php?f=224&t=156241)
