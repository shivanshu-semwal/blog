---
title: "How to Switch from Gnome to i3wm on Debian (Bookworm)"
layout: post
tags:
  - debian
  - gnome
  - i3wm
  - bookworm
description: Moving from GNOME to i3wm on Debian Bookworm and handling the missing desktop features.
---

`i3wm` is a tiling window manager and `gnome` is a desktop environment. If you want to switch, you have to recreate a lot of the functionality that a desktop environment normally provides.

You will need to handle:

* notification center
* multimedia key control
* brightness key control
* status bar
* autostarting programs
* keyring
* touchpad and keyboard settings

## Touchpad settings

I use `xorg`, not `wayland`. To configure the touchpad, create `/etc/X11/xorg.conf.d/30-touchpad.conf` with:

```ini
Section "InputClass"
    Identifier "touchpad overrides"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "TappingButtonMap" "lrm"
    Option "TappingDrag" "on"
    Option "DisableWhileTyping" "off"
EndSection
```

## Keyring issue

To run Chrome with `gnome-keyring` support, add the following to `~/.profile`:

```bash
case "$DESKTOP_SESSION" in
    i3)
        export $(gnome-keyring-daemon --start --components=secrets,ssh,pkcs1)
        ;;
esac
```

Or launch the daemon from i3 config:

```bash
exec --no-startup-id gnome-keyring-daemon --start --components=secrets,ssh,pkcs1
```

Then run Chrome with:

```bash
google-chrome --password-store=gnome-libsecret
```

## Other Xorg settings

- turn off the beep sound: `xset b off`
- set DPI: `xrandr --dpi 96`

## References

- Jaytaala Confluence article on i3 setup
