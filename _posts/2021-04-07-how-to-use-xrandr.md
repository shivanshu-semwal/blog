---
title: How to use xrandr?
layout: post
tags: 
   - linux
---

`xrandr` is the tool to set the resolution on linux.

- `xrandr -q` - this will list the available output devices.

## How to set up a custom resolution

- **Step 1** First find the name of the device you want to set the custom resolution for using `xrandr -q`

```
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

- **Step 2** Check if the custom resolution you want to set is already present for the device.

In my previous example we have following resolutions available

```
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

If the custom resolution is already present goto **Step 6**

- **Step 3** If the custom resolution is not available we can make one. Use this command to generate a "modeline" `cvt [x] [y] [refresh-rate]`

For my case I used `cvt 1600 900 60` and the output is

```
# 1600x900 59.95 Hz (CVT 1.44M9) hsync: 55.99 kHz; pclk: 118.25 MHz
Modeline "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsyncFor
```

- **Step 4** Now add this new mode using `xrandr`, (copy the part of output form previous command)

```
xrandr --newmode "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsyncFor
```

- **Step 5** Now add it to the table of possible resolutions of an output of your choice

In my case

```
xrandr --addmode HDMI-0 1600x900_60.00
```

- **Step 6** Now select the custom resolution you want

In my case

```
xrandr --output HDMI-0 --mode 1600x900_60.00
```
