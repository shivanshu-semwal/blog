---
title: "Noobs guide to CLI"
layout: post
tags:
  - cli
description: A beginner-friendly introduction to command-line interfaces and shells.
---

Command Line Interface: https://en.wikipedia.org/wiki/Command-line_interface

## How it all works

```text
                 +--------+       +---------------------+
                 |        |       |                     |
                 |        |       |                     |
                 |        |       |    Graphical        |
 +----------+    |        |       |      User           |
 |          |    |        |       |       Interface     |
 | keyboard |--->| shell  |======>|                     |
 |  mouse   |    |        |       |       OR            |
 |          |    |        |       |    Monitor          |
 +----------+    |        |       |                     |
                 |        |       |                     |
                 +--------+       +---------------------+
```

Shell is the program which interacts with you and provides the output displayed on a monitor.

Examples:

- press `s` on keyboard; the shell shows the letter `s` on the monitor
- press `Tab` for autocompletion
- `Ctrl+L` clears the screen

In early days of computing, only one program used to run that displayed the output of the shell, but nowadays you can run multiple GUI terminals or console applications.

**Note:** Comparing Unix/Linux terms with Microsoft Windows won't make much sense if you go into detail.

**Additional notes:**

- Most console applications on Windows open in `conhost.exe`; Windows Terminal is the newer replacement.
- Other terminal emulators for Windows include ConEmu, Hyper, Alacritty, and MinTTY.
- Linux has many terminal emulators such as Alacritty, GNOME Terminal, Konsole, and more.

**Useful tips**

- If you are on Windows, try using `Windows Terminal`.

## Scripting

When you have a bunch of commands you want to run, you can paste them into a file and make a script.
Every shell supports some type of scripting.

| Shell | Script | Extension |
| --- | --- | --- |
| cmd | Batch Script | `.bat` |
| powershell | PowerShell script | `.ps1` |
| bash | bash shell script | `.sh` |

## Environment of a shell

The value of the variables in a shell determines its environment.
One well-known variable is `PATH`.

## Internal and External Commands

- Internal commands are built into the shell.
- External commands are searched in the directories listed in `PATH`.

## Command structure

A command consists of a command name and one or more arguments.

Examples:

- `ls`
- `echo "Hello world"`
- `echo -e "hello world"`
- `ls --color`
- `del /s *.txt`

## CMD.exe

- Default shell on Windows.
- Limited history and completion features.

### Basic commands

| Command | Description |
| --- | --- |
| `cd` | change directory |
| `md` | make directory |
| `rd` | remove directory |
| `time` | display time |
| `cls` | clear screen |
| `dir` | list directory files |
| `exit` | quit |
| `ren` | rename files |
| `copy` | copy files |
| `move` | move files and folders |
| `del` | delete files |
| `erase` | delete files |
| `echo` | display on screen |
| `type` | display contents of file |
| `path` | display path variable |

## PowerShell

- Cross-platform shell by Microsoft.
- It does have configuration files.
- It introduces objects instead of plain text.

### Help

- `Get-Help -Name command-name`
- `command-name --help`

## Bash

- Unix-like shell.
- Has good tab completion and many features.

### Help

- `man command-name`
- `command-name --help`
- `command-name -h`

### Basic commands

| Command | Description |
| --- | --- |
| `cd` | change directory |
| `pwd` | print working directory |
| `mkdir` | make directory |
| `rmdir` | remove directory |
| `exit` | quit |
| `clear` | clear screen |
| `ls` | list directory files |
| `mv` | move files |
| `cp` | copy files |
| `rm` | remove files |
| `date` | display current date |
| `time` | display time |
| `cal` | display current month calendar |
