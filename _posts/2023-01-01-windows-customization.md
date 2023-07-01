---
title: Some windows customizations to improve workflow
layout: post
tags:
- windows
description: Modify windows for a better developer experience.
---

Windows have many shortcomings for a developer, so here are some ways I deal with them.

{:toc}

## Debloat Windows

Windows have many programs that you never use and some cause distractions and annoying popup messages.
For this many scripts are available which will remove the unnecessary software form your windows installation.

## Install Package Manger

Install any one of these package manager, and when you want to install some software first check if it is available
in the package manager.

- Chocolatey
    - <https://chocolatey.org/>
- Scoop
    - <https://scoop.sh/>

## Install Software for missing functionalities

- `autohotkey`
    - For making custom shortcuts
    - <https://www.autohotkey.com/>
- `copyQ`
    - Clipboard manager, better than windows clipboard manager
    - <https://hluk.github.io/CopyQ/>
- `flameshot`
    - better screenshot tool, helps in annotation
    - <https://flameshot.org/>
- `audacious`
    - light weight music player, if you want one
    - <https://audacious-media-player.org/>
- `mpv`
    - better video player than windows one
    - <https://mpv.io/>
- `vlc`
    - video player, in case mpv fails
    - <https://www.videolan.org/>
- `sumatra pdf reader`
    - light weight pdf reader
    - <https://www.sumatrapdfreader.org/free-pdf-reader>
- `libreoffice`
    - for documents, presentations, and editing pdf
    - <https://www.libreoffice.org/>

If you think some windows app is lacking something, there will always be some better alternative
due to huge user base of windows.

## Some virtual desktop switching capabilities (optional)

1. Create four virtual desktops
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

Here is a simple use of this script, suppose you want to **drag and drop certain file in web browser**,

- open browser in first desktop and
- explorer in other desktop
- now hold and drag the file you want to send, and then press Start + 1 to go to first desktop while holding file
- now you can drag and drop files between desktops, which is not possible by default.

## Add custom shortcuts

Make another autohotkey file for this `shortcut.ahk` and add this to startup.
In this script you can add you custom keyboard setting or you can block some windows keyboard settings.

- Here is how you block certain annoying windows shortcuts
    - Like `Start` + `c`, opens cortana, which is annoying, you can add this to `shortcut.ahk` file

```ahk
; stop cortana from opening
#c::
sleep 1
return
```

- We don't use `Capslock` that often, so I switched it with `Esc`

```ahk
; switch capslock with esc key
Capslock::Esc
```

- We close windows many times, but the shortcut to close windows is `Alt` + `F4`, which no sane person will
  use repeatedly. So I changed it to `Start` + `Shift` + `q`

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

Similarly you can use shortcuts to open other programs.

These are just a few suggestions to get you started; there are many more things that can be done to improve workflow.
