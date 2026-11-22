---
title: "KDE Explained: Its History, Components, Applications, and Qt"
layout: post
tags:
  - linux
  - kde
  - qt
  - open-source
description: "A guide to KDE's history, today's Plasma, Frameworks, and applications, where their code lives, and how Qt shaped the project."
---

KDE is often used as another name for a Linux desktop, but the project has grown far beyond one desktop environment. Today, **KDE is a community** that develops a desktop, application-development libraries, and a large collection of end-user applications. Qt is the cross-platform toolkit that helped make the project possible and remains central to much of its software.

This distinction is useful whether you are choosing a desktop, looking for KDE apps, or trying to find the source code for a component you want to build or contribute to.

## A short history of KDE

In 1996, Matthias Ettrich announced the **Kool Desktop Environment**, a graphical environment for Unix systems. Its name was a playful reference to the proprietary Common Desktop Environment (CDE). KDE was built with C++ and Qt, which let developers create a consistent interface using a cross-platform toolkit.

The first stable release, KDE 1, arrived in 1998. KDE 2 followed in 2000 with projects such as Konqueror, which combined file-management and web-browsing features. KDE 3 became a widely used desktop generation, with a large set of integrated applications and libraries.

KDE 4, released in 2008, was a major redesign. It introduced **Plasma** as the new desktop shell and brought in technologies such as the desktop search and metadata system Nepomuk. The changes were ambitious, and the transition was difficult for some users, but the name Plasma signaled a shift toward a more flexible desktop interface.

In 2009, the project clarified its naming: **KDE became the name of the community**, while **Plasma** became the name of its desktop environment. That distinction still matters. KDE is not a single application or a single desktop; it is the community and ecosystem behind many projects.

## What KDE is made of today

KDE’s software is commonly described in three broad groups. They are related, but they are not three parts of one program or one source repository.

### Plasma: the desktop

**KDE Plasma** is the desktop environment: the panels, widgets, application launcher, settings, desktop shell, and other features that make up a graphical session. **KWin** is its window manager and compositor. Together, they provide the familiar Plasma desktop on Linux, including sessions based on X11 and Wayland.

Plasma is the part most people mean when they say “the KDE desktop.” It is highly configurable, but it is designed to work as a complete desktop out of the box.

### KDE Frameworks: libraries for developers

**KDE Frameworks** is a collection of libraries that extends Qt and helps developers build applications. Rather than each app implementing common tasks independently, it can use libraries for things such as file access, configuration, archives, internationalization, and responsive interfaces.

Examples include **KIO** for file and network access, **KConfig** for application settings, **KArchive** for archive handling, and **Kirigami** for adaptive Qt Quick interfaces. Frameworks are also used by software outside KDE; they are not limited to Plasma applications.

### KDE Gear and the applications

**KDE Gear** is the name used for coordinated releases of many KDE applications. The applications themselves are separate projects: they have their own source code, development teams, and, in some cases, release schedules.

The range goes well beyond utilities that ship with a desktop. Examples include **Dolphin** (file manager), **Konsole** (terminal), **Kate** (text editor), **Okular** (document viewer), **KDE Connect** (phone and computer integration), **Kdenlive** (video editor), and **Krita** (digital painting). KDE’s [applications catalog](https://apps.kde.org/) lists more than 200 apps and lets you browse by category and platform.

KDE develops and publishes these applications as free and open-source software, but it does not mean every app is preinstalled with every Linux distribution. Distributions decide which apps to package and include. Depending on the application and platform, you can also find releases through app stores such as Flathub or get build and installation instructions from the project’s own page. KDE apps can run on Linux desktops other than Plasma, and some are available for Windows, macOS, or mobile devices too.

## Where is the code?

KDE’s source code is hosted primarily on [KDE Invent](https://invent.kde.org/), its GitLab-based development platform. Projects are organized into groups, and each application or library generally has its own repository. There is no single “KDE source tree” that contains every KDE product.

| What you want to work on | Where to start |
| --- | --- |
| Plasma desktop components | The [Plasma projects on KDE Invent](https://invent.kde.org/plasma), including repositories such as `plasma-desktop` and `kwin` |
| KDE Frameworks libraries | The [Frameworks projects on KDE Invent](https://invent.kde.org/frameworks), such as `kirigami` |
| An application such as Dolphin or Kate | Find the app in the [KDE applications catalog](https://apps.kde.org/), then follow its source-code or development links |
| Building KDE software | KDE’s [developer guide](https://develop.kde.org/docs/getting-started/building/) and its recommended `kde-builder` setup |

If you want to add a feature, first identify the project that owns the behavior. A Plasma panel feature belongs in a Plasma repository; a reusable Qt helper may belong in Frameworks; a file-manager feature belongs in Dolphin. Then read that repository’s contribution instructions, build the project, and check its issue tracker for related work. `kde-builder` can set up and build KDE projects and their dependencies without requiring you to manually assemble the whole desktop.

## How Qt shaped KDE

Qt’s history and KDE’s history are closely connected. Qt was first publicly released in 1995, and KDE was announced the following year with Qt as its foundation. Qt provided C++ APIs for user interfaces and common application features across multiple operating systems. That saved KDE developers from writing separate interface code for each platform and helped KDE apps share a consistent look and behavior.

The relationship also raised an important question: could a community-built desktop depend on a toolkit controlled by a company? KDE and Qt’s original owner, Trolltech, addressed this with the **KDE Free Qt Foundation**, established in 1998 to help ensure Qt would remain available as free software. Qt’s licensing and ownership changed over the years; Trolltech was acquired by Nokia, and Qt later became part of the independent Qt Group. Today Qt is developed with both open-source and commercial licensing options.

Qt’s major generations brought opportunities as well as work for KDE:

- **Qt 3 and KDE 3** formed a mature generation of the desktop and its applications.
- **Qt 4** brought a substantial redesign and became the basis for KDE 4’s technology.
- **Qt 5**, released in 2012, introduced a more modular structure and expanded Qt Quick, which uses QML for fluid, declarative interfaces.
- **Qt 6**, first released in 2020, modernized Qt around newer C++ and graphics technologies. It moved Qt’s own build system to CMake, advanced Qt Quick, and introduced a rendering abstraction that can use different graphics APIs.

Each major Qt transition requires projects to port their code and revisit dependencies. KDE’s corresponding generations—KDE 3, KDE 4, and today’s Plasma 6 and Frameworks 6—reflect that ongoing relationship. Qt is not the KDE desktop itself: it is a toolkit and application platform that KDE uses, extends, and builds upon. KDE Frameworks add community-developed capabilities on top of it.

## Where KDE is now

KDE is a distributed community rather than a company selling one operating system. KDE e.V., a nonprofit organization, represents the community in legal and financial matters, while contributors around the world develop the software. Distributions package KDE software and decide how to integrate it into their operating systems.

Today the KDE ecosystem includes the Plasma desktop, libraries for Qt developers, and hundreds of applications. It also reaches beyond traditional desktop PCs through efforts such as Plasma Mobile and apps designed for different screen sizes. Plasma 6 and Frameworks 6 are the current major generations of KDE’s core technologies; individual applications continue to evolve on their own schedules.

For users, this means you can install Plasma as a complete desktop or use individual KDE apps in another desktop environment. For developers, it means you can contribute to one focused project without needing to take on all of KDE. And for anyone learning about Qt, KDE is one of the largest examples of how a toolkit can support a broad, independent open-source software ecosystem.

## Further reading

- [What is KDE?](https://kde.org/community/whatiskde/)
- [KDE history timeline](https://timeline.kde.org/)
- [KDE products](https://kde.org/products/)
- [KDE applications catalog](https://apps.kde.org/)
- [KDE Frameworks](https://develop.kde.org/products/frameworks/)
- [KDE source code on Invent](https://invent.kde.org/)
- [Building KDE software](https://develop.kde.org/docs/getting-started/building/)
- [The Qt 6.0 release announcement](https://www.qt.io/blog/qt-6.0-released)
- [Qt history](https://wiki.qt.io/Qt_History)
