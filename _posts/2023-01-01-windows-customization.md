---
title: Some windows customizations to improve workflow
layout: post
tags:
- windows
---

## `debloat_windows()`

Many scripts are available for this purpose. This will remove the
unnecessary software form your windows installation.

## `install_package_manger()`

Install any one of these package manager, and when you
want to install some software first check if it is available
in the package manager.

- chocolatey
    - [https://chocolatey.org/](https://chocolatey.org/)
- scoop
    - [https://scoop.sh/](https://scoop.sh/)

## `install_software()`

- autohotkey
    - For making custom shortcuts
    - [https://www.autohotkey.com/](https://www.autohotkey.com/)
- copyQ
    - Clipboard manager
    - [https://hluk.github.io/CopyQ/](https://hluk.github.io/CopyQ/)
- flameshot
    - better screenshot tool
    - [https://flameshot.org/](https://flameshot.org/)
- audacious
    - light weight music player
    - [https://audacious-media-player.org/](https://audacious-media-player.org/)
- mpv
    - better video player
    - [https://mpv.io/](https://mpv.io/)
- vlc
    - video player, in case mpv fails
    - [https://www.videolan.org/](https://www.videolan.org/)
- sumatra pdf reader
    - light weight pdf reader
    - [https://www.sumatrapdfreader.org/free-pdf-reader](https://www.sumatrapdfreader.org/free-pdf-reader)
- libreoffice
    - for documents, presentations, and editing pdf
    - [https://www.libreoffice.org/](https://www.libreoffice.org/)

## `fix_desktop()`

1. Create four desktops
2. Install this script
   - [https://github.com/pmb6tz/windows-desktop-switcher](https://github.com/pmb6tz/windows-desktop-switcher) or
   - [Download it as zip](https://github.com/pmb6tz/windows-desktop-switcher/archive/refs/heads/master.zip)
     and extract it.
3. Inside it there is a file called `user_config.ahk`, remove everything inside it and add these

```ahk
; for switching desktops
#1::switchDesktopByNumber(1)
#2::switchDesktopByNumber(2)
#3::switchDesktopByNumber(3)
#4::switchDesktopByNumber(4)
; for moving windows between desktops
#+1::MoveCurrentWindowToDesktop(1)
#+2::MoveCurrentWindowToDesktop(2)
#+3::MoveCurrentWindowToDesktop(3)
#+4::MoveCurrentWindowToDesktop(4)
; switch desktop to last opened
#`::switchDesktopToLastOpened()
```

This will add following shortcuts

- `Start` + `1` - Switch to desktop 1
- `Start` + `2` - Switch to desktop 2
- `Start` + `3` - Switch to desktop 3
- `Start` + `4` - Switch to desktop 4
- `Start` + `Shift` + `1` - Switch to desktop 1
- `Start` + `Shift` + `2` - Switch to desktop 2
- `Start` + `Shift` + `3` - Switch to desktop 3
- `Start` + `Shift` + `4` - Switch to desktop 4
- `Start` + ` - Switch to last opened desktop

Put the `desktop_switcher.ahk` on the startup.

> Here is a fun thing you can do now, suppose you want to drag and drop
> certain file in browser to send it to your friend, open browser in
> first desktop and explorer in another, now hold and drag the file
> you want to send, and then press Start + 1, and you can now
> drag and drop files between desktops more easily.

## `fix_shortcuts()`

Make another autohotkey file for this `shortcut.ahk` and add this to startup.

- Here is how you block certain annoying windows shortcuts
    - Like `Start` + `c`, opens cortana,
    which is annoying, you can add this to `shortcut.ahk` file

```ahk
; stop cortana from opening
#c::
sleep 1
return
```

- We don't use `Capslock` that often, so i switched it with `Esc`

```ahk
; switch capslock with esc key
Capslock::Esc
```

- We close windows many times, but the shortcut to close windows is
  not that easy to press, so i changed it to `Start` + `Shift` + `q`

```ahk
; close a window
#<+q::WinClose A
return
```

- Window clipboard history is not that feature rich, so I use copyQ,
  and override the `Start` + `v` shortcut with `copyQ toggle` one.
  Make sure to change the location to where `copyQ` is installed.

```ahk
; open copy q
#v::
 run, C:\Program Files (x86)\CopyQ\copyq.exe toggle
return
```

- I also use this to open a terminal, but I still have some
  problems with how it works.

```ahk
; open terminal ---> too slow didn't work cannot focus the terminal
#Enter::
    run wt
return
```

> Now you can shortcut to programs you open frequently.
