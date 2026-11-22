---
title: "Ricing Linux: From X11 Window Managers to Wayland Shells"
layout: post
tags:
  - linux
  - desktop-environments
  - x11
  - wayland
  - customization
description: "How Linux desktop customization evolved from mixing X11 components to building complete Wayland shells."
---

Linux desktops have always offered room to make a computer feel like your own. You can change colors and icons, rearrange panels, replace the window manager, or build a desktop from small programs that each do one job.

This kind of deep customization is often called **ricing**. The word is most associated with carefully themed desktops, but it can describe more than appearance: a customized setup can also change how windows are arranged, how you launch applications, and how you interact with the system.

The tools available for ricing have changed as Linux desktops have moved from X11 to Wayland. The basic goal is the same, but the boundary between the window manager and the rest of the desktop is different.

## What makes up a desktop?

A desktop environment (DE) is not one program. It is a group of components that work together to provide a graphical session. Depending on the desktop, those components may include:

- A **display server** or compositor, which coordinates drawing and input.
- A **window manager**, which places, resizes, and focuses application windows.
- A **panel or bar** for application launchers, workspaces, and system indicators.
- A **desktop shell**, which may provide an overview, application grid, widgets, or system menus.
- Settings tools, a notification service, a lock screen, a file manager, and other utilities.

Some desktop environments keep these pieces closely integrated. Others make it easier to replace or combine them. Knowing which component does what helps make customization less mysterious: changing the panel is a different task from changing the window manager.

## The X11 era: mix and match

For many years, Linux desktops commonly ran on **X11**, also called X or Xorg when referring to the X.Org display server. X11 provided the foundation on which applications and separate desktop components could run. A window manager handled the placement and behavior of windows, while panels, launchers, compositors, and settings tools could be separate programs.

That separation made it natural to assemble a desktop piece by piece. Someone might run a lightweight window manager, add a panel and a launcher, choose a notification daemon, and configure each through its own settings or text files. A tiling window manager such as i3 could replace the window-management part of a more conventional desktop, while other programs supplied features that the user still wanted.

The tradeoff was responsibility: if a small window manager did not include a settings panel, system tray, or notification center, the user had to choose and configure those parts. This is one reason a minimal setup can be both appealing and demanding.

### GNOME, KDE, and their components

Full desktop environments provided a more complete starting point:

- **GNOME** brings together the GNOME Shell, Mutter window manager and compositor, system services, settings applications, and a collection of GNOME apps. Its design favors a consistent, integrated workflow.
- **KDE Plasma** combines the Plasma shell and desktop with KDE applications and system tools. It offers extensive graphical configuration, while still allowing users to customize themes, panels, widgets, and shortcuts.
- **Xfce, LXQt, and other desktops** take different approaches to weight, toolkit, and modularity. LXQt, for example, is built from Qt-based components and can be paired with a window manager such as Openbox.

These are not simply different themes for the same desktop. Each has its own components and conventions. Some parts can be replaced, but mixing services from different environments can require extra configuration and may create duplicate settings tools or background services.

Customization does not require abandoning a complete desktop. Theming GNOME or Plasma, rearranging panels, changing shortcuts, or selecting different applications are all forms of ricing. Replacing the window manager or building a session from standalone components is just a more hands-on route.

## Wayland: a different foundation

**Wayland** changes how graphical applications communicate with the desktop. Unlike the traditional X11 arrangement, a Wayland compositor is the central authority for displaying windows and handling input. In practice, the compositor also provides window-management behavior, so Wayland projects often combine roles that could be separate programs in an X11 setup.

These projects are often casually called Wayland window managers, but they are compositors: each one runs the Wayland session and provides its own window-management behavior. Their approaches vary:

- **Tiling compositors** arrange windows into layouts rather than letting them overlap by default. **Sway** is designed around an i3-compatible workflow, while **Hyprland** combines dynamic tiling with extensive visual customization. **MangoWC** takes inspiration from dwm.
- **Scrollable tiling** is the defining idea behind **Niri**: windows are arranged in columns that can extend horizontally beyond the screen.
- **Stacking and floating** compositors allow windows to overlap, like a traditional desktop. **labwc** is a lightweight, Openbox-inspired option.
- **Plugin-oriented compositing** is the focus of **Wayfire**, which offers a configurable compositor with effects and extensions.
- **Desktop-integrated compositors** such as **Mutter** in GNOME and **KWin** in KDE Plasma are designed to work as part of their respective desktop environments.
- **Miracle-WM** is another hackable tiling compositor, built on Canonical's Mir display-server technology.

These are useful starting points, not strict boxes: many compositors offer more than one layout or let users change their behavior through configuration and extensions. **River** is also worth a look for its non-monolithic approach to a Wayland compositor.

The compositor is only part of the desktop, however. A Wayland session can still use separate programs for a bar, launcher, notifications, wallpaper, clipboard, lock screen, and settings. The exact integration varies by compositor and by the protocols each component supports.

This is why moving to Wayland does not mean customization has gone away. It means the compositor is a more central choice, while many surrounding desktop components remain separate.

## From individual components to complete shells

One way to build a Wayland desktop is to choose each component yourself. Pair a compositor with a bar, launcher, notification daemon, and other tools, then style and configure each one. This keeps the setup flexible, but it can take time to make every piece look and behave consistently.

Many of those components are built with **GTK** or **Qt**. These are general-purpose UI toolkits, not window managers or complete desktop shells. GTK is closely associated with GNOME and is used by projects such as GTK-based panels; Qt is the toolkit behind much of KDE Plasma. Both can create ordinary desktop applications, while their UI and styling systems differ. For custom shell interfaces, **Qt Quick** uses QML to describe dynamic interfaces, whereas GTK applications commonly use GTK widgets and CSS styling.

On Wayland, a toolkit alone does not make an application a desktop panel: panels and desktop widgets need to place their surfaces appropriately, often through the layer-shell protocol or a toolkit library that supports it. GTK and Qt applications can coexist in the same session, so choosing a shell framework does not require using only applications built with that toolkit.

There is no single standard framework or exhaustive catalog: shell projects combine UI toolkits, system-integration libraries, and Wayland protocols in different ways. The following are some of the main options used in current Linux customization projects:

| Framework or toolkit | What it provides | Examples and common uses |
| --- | --- | --- |
| **QuickShell** | A shell-focused toolkit built around Qt Quick and QML | **Dux Modular Shell** and **Caelestia Shell** use QuickShell; it is also used for custom bars, launchers, and widgets. |
| **Astal + Gnim** | Astal supplies Vala/C libraries for desktop integration; Gnim adds JSX to GJS for GTK interfaces | The current **AGS** CLI scaffolds projects using Astal and Gnim. Older AGS v1 configurations used GJS and GTK directly, so both generations appear in dotfiles. |
| **Fabric** | A Python desktop-widget system with reusable widgets and system integrations | Used for custom shells and panels such as **Ax-Shell** for Hyprland and **Zenith** for i3/Sway. |
| **Eww** | A Rust widget system configured with Yuck and styled with CSS; independent of the window manager | Commonly used for custom bars, dashboards, and widgets rather than a complete desktop shell. |
| **GTK with layer-shell support** | A lower-level way to build GTK panels and desktop surfaces positioned by the Wayland layer-shell protocol | Useful for custom GTK bars and panels. Eww also uses GTK layer-shell support on Wayland. This is a library/toolkit approach, not a ready-made shell framework. |

This is a practical overview, not every personal toolkit or one-off project. Also distinguish shell UI frameworks from **compositor frameworks** such as wlroots or Smithay: those help developers implement Wayland compositors and window management, rather than build panels and desktop interfaces.

There are also ready-made shells that combine many of these pieces into a more complete experience:

- **Dux Modular Shell (DMS)** is a complete shell built with QuickShell and backend services. It aims to replace the collection of separate programs often used for panels, locking, idle handling, notifications, and launching, and supports several Wayland compositors.
- **Caelestia Shell** is built with QuickShell and is designed to work with the Hyprland compositor.
- **Noctalia** is another configurable Wayland shell, bringing features such as bars, widgets, a dock, launcher, notifications, wallpaper, and lock screen into one project.
- For a component-based setup, **Waybar** is a standalone status bar, not a complete shell; **nwg-shell** bundles separate GTK-based desktop tools. Both suit users who prefer assembling a session from components.

A prebuilt shell can be a faster starting point than configuring every component, though it may be designed around a particular compositor or set of tools. A complete shell is not itself a window manager: it runs on top of a compositor and provides the user-facing desktop features around it.

The choice is not simply between a finished desktop and doing everything from scratch. A shell can provide the common interface while leaving individual components replaceable; a self-assembled setup can also reuse existing tools instead of recreating them.

## Choosing a way to rice

The right starting point depends on how much of the desktop you want to maintain:

| Starting point | What you customize | Main tradeoff |
| --- | --- | --- |
| A full desktop such as GNOME or Plasma | Themes, panels, shortcuts, widgets, and apps | Most convenient; some pieces are designed to work together |
| A modular desktop such as Xfce or LXQt | Desktop components and often the window manager | More choice, with some integration work |
| A standalone window manager or compositor | The window behavior plus separately chosen desktop tools | Very flexible, but you assemble and maintain more pieces |
| A shell framework or prebuilt shell | A unified interface, either built yourself or configured | Frameworks need development; prebuilt shells may have project-specific assumptions |

It is also worth considering the practical details: whether your hardware works well with the compositor, whether the applications you rely on run correctly, and whether you want to troubleshoot several independently configured programs. A beautiful setup is more enjoyable when it remains comfortable to use and easy to recover.

## The same idea, new building blocks

Ricing has never been limited to one display server or one kind of desktop. X11 made it straightforward to combine independent programs around a window manager. Wayland places more responsibility in the compositor, while leaving plenty of room for separate tools and new shell frameworks.

You can keep a familiar desktop and adjust its appearance, replace just a few components, start from a prebuilt shell, or assemble a session one piece at a time. The best setup is the one whose appearance and workflow you enjoy—and whose moving parts you are happy to understand and maintain.

## Further reading

- [X.Org Foundation](https://www.x.org/wiki/)
- [Wayland project](https://wayland.freedesktop.org/)
- [GNOME Shell](https://gitlab.gnome.org/GNOME/gnome-shell)
- [KDE Plasma](https://kde.org/plasma-desktop/)
- [LXQt](https://lxqt-project.org/)
- [Sway](https://swaywm.org/)
- [Hyprland](https://hypr.land/)
- [Niri](https://github.com/niri-wm/niri)
- [labwc](https://labwc.github.io/)
- [Wayfire](https://wayfire.org/)
- [River](https://codeberg.org/river/river)
- [MangoWC](https://github.com/mangowm/mango)
- [Miracle-WM](https://miracle-wm.org/)
- [QuickShell](https://quickshell.org/)
- [Astal](https://github.com/Aylur/astal)
- [AGS](https://github.com/Aylur/ags)
- [Gnim](https://github.com/Aylur/gnim)
- [Fabric](https://github.com/Fabric-Development/fabric)
- [Eww](https://github.com/elkowar/eww)
- [Dux Modular Shell](https://github.com/duxlabs-dev/dux-shell)
- [Caelestia Shell](https://github.com/caelestia-dots/shell)
- [Noctalia](https://github.com/noctalia-dev/noctalia)
- [Waybar](https://github.com/Alexays/Waybar)
- [nwg-shell](https://github.com/nwg-piotr/nwg-shell)
