---
title: "Guide to Setting Custom Resolutions using xrandr"
layout: post
tags:
  - linux
  - xrandr
  - xorg
description: Learn how to create and apply a custom display resolution using xrandr on Xorg.
---


`xrandr` is the tool to set the resolution on linux. `xrandr` only works for X Window System, if you are using Wayland this won't work.

## How to set up a custom resolution

### Step 1: Finding name of device for which you want to set custom resolution 

Use `xrandr -q` to list the available output devices.

```bash
Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
DP-0 disconnected primary (normal left inverted right x axis y axis)
DP-1 disconnected (normal left inverted right x axis y axis)
DP-2 disconnected (normal left inverted right x axis y axis)
DP-3 disconnected (normal left inverted right x axis y axis)
HDMI-0 connected 1920x1080+0+0 (normal left inverted right x axis y axis) 477mm x 268mm
   1024x768      60.00 +
   1920x1080     60.00*   59.94
   1440x900      59.89
   1360x768      60.02
   1280x1024     60.02
   1280x960      60.00
   1280x800      59.81
   1280x720      59.94
   720x480       59.94    59.94
```

Here I can see that the display which is connected to my device is `HDMI-0`.

### Step 2: Check for available custom resolutions

In my previous example we have following resolutions available

```bash
   1024x768      60.00 +
   1920x1080     60.00*   59.94
   1440x900      59.89
   1360x768      60.02
   1280x1024     60.02
   1280x960      60.00
   1280x800      59.81
   1280x720      59.94
   720x480       59.94    59.94
```

If the custom resolution is already present skip to **Step 6**

### Step 3: Create a custom resolution

If custom you want is not available we can make one. Use this command to generate a "modeline", `cvt [x] [y] [refresh-rate]`

For my case I used `cvt 1600 900 60` and the output is

```bash
# 1600x900 59.95 Hz (CVT 1.44M9) hsync: 55.99 kHz; pclk: 118.25 MHz
Modeline "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsyncFor
```

### Step 4: Adding new mode to you `xrandr`

Now add this new mode using `xrandr`, (copy the part of output form previous command)

```bash
xrandr --newmode "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsyncFor
```

### Step 5: Adding the new mode to the device

Now add it to the table of possible resolutions of an output of your choice. In my case

```bash
xrandr --addmode HDMI-0 1600x900_60.00
```

### Step 6: Selecting the custom resolution

Now select the custom resolution you want. In my case

```bash
xrandr --output HDMI-0 --mode 1600x900_60.00
```
This will change the custom resolution for the selected device.

## References

- [xrandr Official Wiki](https://x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html)
- [Arch Wiki xrandr](https://wiki.archlinux.org/title/Xrandr)
