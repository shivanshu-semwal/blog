---
title: LXQT Desktop Environment
layout: post
tags:
    - linux
---

LXQT is a light weight desktop environment. It is highly modular DE.

It comes with a `lxqt-panel` which you can replace with other one.

`lubuntu` comes with `lxqt` installed.

There are two session you can start in it:

Location - `/usr/share/xsessions`

File - `lubuntu.desktop`

```
[Desktop Entry]
Name=Lubuntu
Comment=Lubuntu session using LXQt
Exec=env LXQT_DEFAULT_OPENBOX_CONFIG="/etc/xdg/xdg-Lubuntu/openbox/lxqt-rc.xml" /usr/bin/startlxqt
Type=Application
```

So the `lubuntu` session use `openbox` wm with the config file `~/.config/openbox/lxqt-rc.xml`.

File - `lxqt.desktop`

```
[Desktop Entry]
Type=Application
Exec=startlxqt
TryExec=lxqt-session
Name=LXQt Desktop
Comment=Lightweight Qt Desktop
```

This uses the default `openbox` config file `~/.config/openbox/rc.xml`
