---
title: "GNOME Explained: Its History, GTK Versions, and Components"
layout: post
tags:
  - linux
  - gnome
  - gtk
  - open-source
description: "A guide to GNOME's history and current desktop, the GTK toolkit and its major versions, apps, and source repositories."
---

GNOME is more than the desktop you see after logging in, and GTK is more than a collection of buttons and windows. GNOME is a community and a set of software projects; **GTK** is a user-interface toolkit used by GNOME and many other applications. Understanding how those pieces fit together makes GNOME’s history, its current architecture, and its source code much easier to navigate.

## A short history of GNOME

The GNOME project was founded in 1997 by Miguel de Icaza and Federico Mena. It set out to build a free desktop environment for Unix-like systems using free software. The name originally stood for **GNU Network Object Model Environment**.

GNOME 1 arrived in 1999. **GNOME 2**, released in 2002, became a familiar desktop for many Linux users, with a panel-based interface and a broad collection of applications. This generation also helped establish GNOME’s platform libraries and developer ecosystem.

In 2011, **GNOME 3** introduced a major redesign centered on GNOME Shell. The new overview, activities-based workflow, and modernized interface were a significant change from GNOME 2. The project continued refining the experience through the GNOME 3.x releases.

In 2021, GNOME released **GNOME 40**. This was not “GNOME 4”; the project moved from 3.38 to 40 to make the new version series easier to identify. GNOME 40 also brought a redesigned Activities overview and horizontal workspace navigation. Since then, releases have progressed through 41, 42, and onward.

GNOME is now a global community that maintains the desktop, core libraries, applications, and a development platform. The GNOME Foundation supports the project, but the software is built by contributors from many organizations and independent contributors around the world.

## The GNOME desktop and its parts

GNOME is made up of components that work together, rather than one monolithic program:

- **GNOME Shell** provides the user-facing shell: the top bar, Activities overview, application grid, notifications, and system menus.
- **Mutter** handles window management and compositing. It is also GNOME’s Wayland compositor and coordinates windows, displays, and input.
- **GNOME Settings** and other system tools provide graphical configuration for the desktop and connected devices.
- **GNOME Core apps** provide common tasks such as browsing files, viewing images, using a terminal, and managing calendars.
- **GNOME’s platform libraries** give applications reusable building blocks and APIs.

The system below GNOME also matters. On Linux, GNOME builds on lower-level projects and system services for graphics, audio, networking, and other functionality. GNOME collaborates with the wider Linux ecosystem rather than implementing every system service itself.

### What GTK does

**GTK** is a cross-platform toolkit for building graphical user interfaces. It supplies widgets such as buttons, lists, and text fields, along with APIs for drawing, input, and application windows. GTK is written in C, and language bindings let developers use it from languages including Python, JavaScript, Rust, and Vala.

GTK is not GNOME Shell, and it is not limited to GNOME. Many GNOME applications use GTK, but so do applications such as GIMP, Inkscape, and Transmission. GTK can also be used to build software for Windows and macOS, although availability and integration can differ by platform.

GTK is built on a family of related libraries. **GLib** provides core data structures, utilities, a main loop, and the GObject type system. **GIO** adds file, network, and operating-system integration APIs; **GDK** abstracts the underlying windowing systems; and **GSK** is GTK’s rendering layer. Libraries such as Pango provide text layout and shaping. These projects explain why a GTK application often depends on more than just the `gtk` library.

GNOME applications commonly use **libadwaita** alongside GTK 4. Libadwaita provides GNOME-style widgets and adaptive layout patterns, helping apps maintain a consistent design across desktop and smaller screens. It is an additional library, not a new name for GTK.

## GTK’s major versions

GTK began as the **GIMP Toolkit**, created to build the GIMP image editor. Its first stable release, GTK 1.0, appeared in 1998. The toolkit’s scope grew, and GTK became one of the central technologies of the GNOME desktop.

| Version | Approximate release | What changed |
| --- | --- | --- |
| **GTK 1** | 1998 | The original toolkit, initially developed for GIMP and later adopted by GNOME. |
| **GTK 2** | 2002 | A major generation that improved text handling, accessibility, and theming as GTK matured into a general-purpose application toolkit. |
| **GTK 3** | 2011 | A new API generation that modernized rendering and introduced CSS-based styling. It remains relevant because many applications have not migrated to GTK 4. |
| **GTK 4** | 2020 | A substantial redesign of GTK’s rendering and APIs, intended to make the toolkit more capable and efficient on modern systems. Applications generally need code changes to move from GTK 3 to GTK 4. |

The “+” in the old name **GTK+** was dropped; the project now calls itself GTK. A new major GTK version is not simply a visual theme update. APIs and implementation details can change, so application developers port their code deliberately. Different GTK generations can be installed side by side, allowing older GTK 3 apps to run while newer apps use GTK 4.

As of **November 22, 2026**, GTK’s site lists **GTK 4.22.5** as the latest stable release, **GTK 4.23.4** as the latest unstable release, and **GTK 3.24.52** as the latest old stable release. GTK 2 is an older generation and is not the version to choose for new applications. These numbers change over time, so check the [GTK website](https://www.gtk.org/) for the current release status.

## GNOME applications

GNOME develops a collection of apps that work well with its desktop, including **Files** (file manager), **Web** (web browser), **Calendar**, **Maps**, **Loupe** (image viewer), **Papers** (document viewer), **Console**, and **Software** (app manager). The [GNOME Apps catalog](https://apps.gnome.org/) is a good place to browse the current selection.

GNOME’s app ecosystem also includes **GNOME Circle**, a collection of independent applications that follow GNOME’s design and community principles. Not every app in the wider GNOME ecosystem is installed by default, and individual projects can have their own release schedule and installation methods. Linux distributions choose which applications to package, while many apps are also distributed as Flatpaks or through other platform-specific channels.

GTK applications can run outside GNOME. You can use them in desktops such as KDE Plasma or Xfce, just as a GNOME desktop can run applications built with toolkits other than GTK. A toolkit choice affects an app’s interface and dependencies; it does not determine which desktop environment you must use.

## Where is the code?

GNOME hosts most of its development on [GNOME GitLab](https://gitlab.gnome.org/). Projects have separate repositories, so you work on the component or application that owns the feature rather than cloning one enormous “GNOME” repository.

| What you want to work on | Where to start |
| --- | --- |
| GNOME Shell | [`GNOME/gnome-shell`](https://gitlab.gnome.org/GNOME/gnome-shell) |
| Window management and Wayland compositor | [`GNOME/mutter`](https://gitlab.gnome.org/GNOME/mutter) |
| GTK toolkit | [`GNOME/gtk`](https://gitlab.gnome.org/GNOME/gtk) |
| GLib, GObject, and GIO | [`GNOME/glib`](https://gitlab.gnome.org/GNOME/glib) |
| GNOME-style GTK widgets | [`GNOME/libadwaita`](https://gitlab.gnome.org/GNOME/libadwaita) |
| A GNOME application | Search [GNOME GitLab](https://gitlab.gnome.org/GNOME) for the application’s project, or follow its source link from the [apps catalog](https://apps.gnome.org/) |

Before changing code, read the project’s contribution guide and issue tracker. For example, a feature in the Activities overview belongs in GNOME Shell, while a new GTK widget belongs in GTK itself. GNOME’s [developer documentation](https://developer.gnome.org/documentation/) explains the platform and tools. **GNOME Builder** is an integrated development environment for GNOME apps, and **Meson** is commonly used to build GNOME projects.

## Where GNOME is now

As of November 2026, the latest GNOME desktop release is **GNOME 51**, released in September 2026. GNOME continues to develop its shell, compositor, platform, and applications together while coordinating with the larger open-source desktop ecosystem. Its current desktop is designed around the GNOME Shell and Mutter, with ongoing development for modern display technologies and a consistent app experience.

GTK is also an active project in its own right. GTK 4 is the current stable generation for new development, while GTK 3 remains available for applications that have not migrated. GNOME applications often add libadwaita for GNOME-specific design, and the underlying GTK libraries are useful to developers well beyond GNOME.

The key distinction is simple: **GNOME is the desktop and software community; GTK is a UI toolkit used by GNOME and many other projects.** GNOME Shell, Mutter, GTK, GLib, libadwaita, and individual apps are separate projects with separate codebases. Once you know which component owns a feature, you can find its repository, documentation, and contribution process.

## Further reading

- [The GNOME story](https://www.gnome.org/about/)
- [GNOME release history](https://release.gnome.org/)
- [GNOME 51 release notes](https://release.gnome.org/51/)
- [GTK](https://www.gtk.org/)
- [GTK documentation](https://www.gtk.org/docs/)
- [GTK architecture](https://www.gtk.org/docs/architecture/)
- [GNOME Developer Documentation](https://developer.gnome.org/documentation/)
- [GNOME Core components](https://developer.gnome.org/components/)
- [GNOME Apps catalog](https://apps.gnome.org/)
- [GNOME GitLab](https://gitlab.gnome.org/)
