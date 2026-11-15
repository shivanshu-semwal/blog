---
title: "Guide to Setting Custom Resolutions using xrandr"
layout: post
tags:
  - linux
  - xrandr
  - xorg
description: Learn how to create and apply a custom display resolution using xrandr on Xorg.
---

`xrandr` is the tool to set the display resolution on Linux. It works on the X Window System.

## How to set up a custom resolution

### Step 1: Find the output device

Use `xrandr -q` to list the outputs.

```bash
Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
DP-0 disconnected primary
HDMI-0 connected 1920x1080+0+0
    1024x768      60.00 +
    1920x1080     60.00*
    1440x900      59.89
```

### Step 2: Check if your resolution already exists

If it is already available, skip to Step 6.

### Step 3: Create a custom resolution

Use `cvt` to generate a modeline:

```bash
cvt 1600 900 60
```

This yields a modeline like:

```bash
Modeline "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsync
```

### Step 4: Add the mode

```bash
xrandr --newmode "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsync
```

### Step 5: Add it to the device

```bash
xrandr --addmode HDMI-0 1600x900_60.00
```

### Step 6: Select the mode

```bash
xrandr --output HDMI-0 --mode 1600x900_60.00
```

## References

- xrandr official wiki
- Arch Wiki xrandr
