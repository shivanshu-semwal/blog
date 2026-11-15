---
title: "How to Configure Fonts in Linux"
layout: post
tags:
  - fonts
  - linux
description: A guide to font configuration, font directories, and fontconfig examples on Linux.
---

Fonts on Linux are usually managed by `fontconfig`, which handles fallback fonts and default font selection.

## Location of fonts

Fonts are stored in:

- `/usr/share/fonts`
- `/usr/local/share/fonts`
- `~/.local/share/fonts`

## Configuration of `fontconfig`

Important files include:

- `/etc/fonts/fonts.conf`
- `/etc/fonts/conf.d`
- `~/.config/fontconfig/conf.d`
- `~/.config/fontconfig/fonts.conf`

## Utilities

- `fc-list`
- `fc-match`
- `fc-cache`
- `fc-cat`
- `fc-query`
- `fc-pattern`
- `fc-validate`

## Sample `fontconfig` configuration

### Enable antialiasing

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <match target="font">
        <edit mode="assign" name="antialias"><bool>true</bool></edit>
    </match>
</fontconfig>
```

### Set default sans and monospace fonts

```xml
<?xml version='1.0'?>
<!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer><family>CMU Sans Serif</family></prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer><family>Iosevka Term</family></prefer>
  </alias>
</fontconfig>
```

## References

- Arch Wiki on font configuration
- Wikipedia article on Fontconfig
