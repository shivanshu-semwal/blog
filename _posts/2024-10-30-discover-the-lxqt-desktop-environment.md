---
title: "Discover the LXQT Desktop Environment"
layout: post
tags:
  - linux
  - lxqt
description: A basic overview of LXQt, its components, and how the sessions differ in Debian and Lubuntu.
---


LXQT is a light weight and a highly modular desktop environment. If you want to try it out you can use `lubuntu`, a flavour of ubuntu which comes with `lxqt` installed.

## Components of `lxqt` desktop environment

`lxqt` is modular desktop environment. It has following components:

- `lxqt-panel` a light weight customizable panel 
- `openbox` window manager 
- `PCManFM-Qt` file manager
- `featherpad` text editor
- `sddm` login manager

and mostly other components are also lightweight qt programs.

## Multiple sessions of `lxqt` present in `sddm` login manager.

When you login in using `sddm` login manager you will encounter drop-down to select different `lxqt` sessions, which can be confusing. For each of these options there is a file associated in `/usr/share/xsessions`, which contains information about how to start the session and it's name.

### Lubuntu Session

This is associated with the file `/usr/share/xsessions/lubuntu.desktop`

```ini
[Desktop Entry]
Name=Lubuntu
Comment=Lubuntu session using LXQt
Exec=env LXQT_DEFAULT_OPENBOX_CONFIG="/etc/xdg/xdg-Lubuntu/openbox/lxqt-rc.xml" /usr/bin/startlxqt
Type=Application
```
So for `Exec` option we can see that `lubuntu` session use `openbox` window manager with the config file `~/.config/openbox/lxqt-rc.xml` and it is not the default config for openbox.

### lxqt Session

This is associated with the file `/usr/share/xsessions/lxqt.desktop`

```ini
[Desktop Entry]
Type=Application
Exec=startlxqt
TryExec=lxqt-session
Name=LXQt Desktop
Comment=Lightweight Qt Desktop
```

This also uses the default `openbox` window manger but with the default openbox  config file `~/.config/openbox/rc.xml`.

So each session has a different openbox config file. So use Lubuntu session as it will more customized by default.

## References

- [lubuntu official site](https://lubuntu.me/)
- [lxqt official site](https://lxqt-project.org/)
