---
title: How to set up xampp on linux
layout: post
tags: 
  - programming
  - linux
---

- First download the latest version of `xampp` for GNU/Linux form here [https://www.apachefriends.org/index.html](https://www.apachefriends.org/index.html).
- Open terminal, cd into the directory where the installer is, give execute permission for the installer using `chmod +x [filename]`, and then run the installer giving root permissions (`sudo`).
- The installer will install the binaries in `/opt/lampp` directory.
- Now cd into the `/opt/lampp` directory and do `ls -all` i.e. `cd /opt/lampp; ls -all`
- Some important files and directories in `/opt/lampp` are
    - `xampp` - this is the main binary file for starting and stopping the server
    - `uninstall` - this is the binary for uninstalling `xampp`
    - `manager-linux-x64.run` - this is used for running the graphical interface, just run this in root privilege.
    - `bin` - this directory contains the binaries for `mysql` `perl` `apache` ...
    - `configuration.ini` - this file contains the setting for running the servers, if you want to change the configuration of the server, like port no, root directory, edit this file.

- Once, you start the servers, either using `xampp` or `manager-linux-x64.run` (GUI), then go and open the `localhost:80` on browser.
- Don't include this directory in the `PATH` variable, just make aliases for some specific program you want, or softlink them in the `/usr/bin` directory.

Before searching on internet, read the FAQ's at least one time.
[https://www.apachefriends.org/faq_linux.html](https://www.apachefriends.org/faq_linux.html)
