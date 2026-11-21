---
title: "Reasons to Consider PowerShell for Daily Operations"
layout: post
tags:
  - powershell
description: Why PowerShell is a practical tool for automation and repetitive file operations.
---


Here’s the situation: you’re attempting to copy a large file using File Explorer, but it’s taking a long time even before the copying begins. This delay occurs because Explorer calculates certain information about the files to provide an accurate progress bar, which can lead to wasting over a minute when dealing with large files.

Additionally, if you want to remove all zip files from a folder, doing it through Explorer requires more manual effort compared to using a single command. While you could achieve this in another programming language, the time spent switching contexts would be greater since you'd need to write, test, and execute a script.

Moreover CLI’s are more reliable than GUI’s and if you do something in a CLI then there is a better chance that you can automate that easily.

## Some tricks and tips!

### Create a new file from contents of clipboard

```powershell
Get-Clipboard | Out-File -FilePath "file_path"

# example
Get-Clipboard | Out-File -FilePath "code.py"
```

### Copy files form one location to another

```powershell
# for single item
Copy-Item -Path "source_path" -Destination "destination_path"

# for multiple items
Copy-Item -Path "C:\source\*.txt" -Destination "C:\destination\"

# copy folder
Copy-Item -Path "C:\source\folder" -Destination "C:\destination\folder" -Recurse

# overwrite
Copy-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt" -Force

# overwrite but ask first
Copy-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt" -Confirm
```

### Move files and folders

```powershell
# single item
Move-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt"

# multiple items
Move-Item -Path "C:\source\*.txt" -Destination "C:\destination\"

# folder
Move-Item -Path "C:\source\folder" -Destination "C:\destination\folder"

# overwrite
Move-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt" -Force

# overwrite but ask first
Move-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt" -Confirm
```

### Delete files (will permanently delete files)

```powershell
# single item
Remove-Item -Path "file_path"

# folder
Remove-Item -Path "C:\path\to\folder" -Recurse

# force
Remove-Item -Path "C:\path\to\file.txt" -Force

# ask before deleting
Remove-Item -Path "C:\path\to\file.txt" -Confirm
```

### Find all files with particular extension in a folder and move them to one place

```powershell
Get-ChildItem -Path "source_dir" -Recurse -Filter *.zip | 
    Move-Item -Destination "destination_dir" -Force
```

### Delete Items (move to recycle bin)

```powershell
# TODO
```

### Create new image from image copied to clipboard like when you take screenshot

```powershell
# todo
```

## References

* [https://learn.microsoft.com/en-us/powershell/](https://learn.microsoft.com/en-us/powershell/)
    
* [https://devblogs.microsoft.com/powershell-community/](https://devblogs.microsoft.com/powershell-community/)
    
* [https://devblogs.microsoft.com/powershell-community/cheat-sheet-console-experience/](https://devblogs.microsoft.com/powershell-community/cheat-sheet-console-experience/)
