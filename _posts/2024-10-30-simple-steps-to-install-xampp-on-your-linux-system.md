---
title: "Simple Steps to Install XAMPP on Your Linux System"
layout: post
tags:
  - linux
  - xampp
description: Installing XAMPP for local PHP development on Linux.
---

XAMPP is a popular PHP development environment. It stands for Apache + MariaDB + PHP + Perl.

Follow these steps:

- Download the latest XAMPP binary for GNU/Linux.
- Open a terminal, `cd` into the download directory, make the installer executable with `chmod +x`, and run it with `sudo`.
- The installer installs the binaries under `/opt/lampp`.
- Run `cd /opt/lampp; ls -all` to inspect the installation.
- Important files include:
  - `xampp`: main control binary
  - `uninstall`: uninstall script
  - `manager-linux-x64.run`: graphical manager
  - `bin`: contains binaries for the services
  - `configuration.ini`: service configuration
- Start the servers and then open `localhost:80` in a browser.

Do not include `/opt/lampp` in `PATH`; use aliases or symlinks for specific binaries instead.
