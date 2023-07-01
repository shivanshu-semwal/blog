---
title: LXQT Desktop Environment
layout: post
tags:
    - linux
description: About LXQT desktop environment and lubuntu which uses lxqt.
---

LXQT is a light weight desktop environment. It is highly modular DE.
`lubuntu` is a flavour of ubuntu which comes with `lxqt` installed.

It has following components:

- `lxqt-panel` which you can replace with other one.
- `openbox` which is window manager
- `PCManFM-Qt` file manages
- `featherpad` text editor
- mostly other components are also lightweight qt programs
- `sddm` login manager

There are many session you can start in it, present in the `sddm` login manager which is confusing
if you don't know what they means. The files related to them are present in `/usr/share/xsessions`

## Lubuntu Session

- File - `lubuntu.desktop`

```ini
[Desktop Entry]
Name=Lubuntu
Comment=Lubuntu session using LXQt
Exec=env LXQT_DEFAULT_OPENBOX_CONFIG="/etc/xdg/xdg-Lubuntu/openbox/lxqt-rc.xml" /usr/bin/startlxqt
Type=Application
```

- So for `Exec` option we can see that `lubuntu` session use `openbox` wm with the config file `~/.config/openbox/lxqt-rc.xml`.

## lxqt Session

- File - `lxqt.desktop`

```ini
[Desktop Entry]
Type=Application
Exec=startlxqt
TryExec=lxqt-session
Name=LXQt Desktop
Comment=Lightweight Qt Desktop
```

- This uses the default `openbox` config file `~/.config/openbox/rc.xml`

So each session has a different config file, and of course the lubuntu session will be more customized.
