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


`i3wm` is a tiling window manager and `gnome` is a desktop environment, so you have to keep some things in mind if you want to switch to `i3wm`.

You will need to add the functionality that a desktop environment provides on your own. Here is a list of those features

* notification center
    
* multimedia keys control
    
* brightness key control
    
* status bar
    
* autostarting programs
    
* keyring, `gnome-keyring`
    
* touchpad and keyboard settings
    

## Touchpad settings

I use `xorg` display server not `wayland`. So to configure touchpad with the settings I need, I followed following steps:

* Create file - `/etc/X11/xorg.conf.d/30-touchpad.conf`
    
* Paste following contents into the file
    
    ```plaintext
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
    

Here I suppose you are using `libinput` driver. Also for other option on this check [xorg.conf(5) § INPUTCLASS\_SECTION](https://man.archlinux.org/man/xorg.conf.5#INPUTCLASS_SECTION)

Also check this [libinput archo wiki](https://wiki.archlinux.org/title/Libinput) article.

## Keyring Issue

After lot of debugging, I was able to run chrome with `gnome-keyring` support.

First you have to add this to `$HOME\.profile`

```bash
case "$DESKTOP_SESSION" in
    i3)
        export $(gnome-keyring-daemon --start --components=secrets,ssh,pkcs1)
        ;;
esac
```

or you can add following to i3 config `exec --no-startup-id gnome-keyring-daemon --start --components=secrets,ssh,pkcs1`. I prefer the `.profile` method.

Now to run google chrome you have to use `google-chrome --password-store=gnome-libsecret`. Also you have to use `gnome-libsecret` for it to work. In `google-chrome --help` it shows `gnome` but that won't work.

```plaintext
--password-store=<basic|gnome|kwallet>
# this is wrong
```

## Setting other `xorg` setting

* To turn of the beep sound when some error is made - `xset b off`
    
* For dip settings - `xrandr --dpi 96`
    

## References

* [https://confluence.jaytaala.com/display/TKB/My+Manjaro+i3+setup](https://confluence.jaytaala.com/display/TKB/My+Manjaro+i3+setup) more details about customization.
