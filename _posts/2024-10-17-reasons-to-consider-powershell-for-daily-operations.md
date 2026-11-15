---
title: "Reasons to Consider PowerShell for Daily Operations"
layout: post
tags:
  - powershell
description: Why PowerShell is a practical tool for automation and repetitive file operations.
---

Here is the situation: you are attempting to copy a large file using File Explorer, but it takes a long time before the copy even starts. This delay happens because Explorer is calculating all the details first.

CLI tools are often more reliable than GUI tools, and commands are easier to automate.

## Some tricks and tips

### Create a new file from the contents of the clipboard

```powershell
Get-Clipboard | Out-File -FilePath "file_path"

# example
Get-Clipboard | Out-File -FilePath "code.py"
```

### Copy files from one location to another

```powershell
# single item
Copy-Item -Path "source_path" -Destination "destination_path"

# multiple items
Copy-Item -Path "C:\source\*.txt" -Destination "C:\destination\"

# copy folder
Copy-Item -Path "C:\source\folder" -Destination "C:\destination\folder" -Recurse

# overwrite
Copy-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt" -Force
```

### Move files and folders

```powershell
# single item
Move-Item -Path "C:\source\file.txt" -Destination "C:\destination\file.txt"

# multiple items
Move-Item -Path "C:\source\*.txt" -Destination "C:\destination\"

# folder
Move-Item -Path "C:\source\folder" -Destination "C:\destination\folder"
```

### Delete files

```powershell
Remove-Item -Path "file_path"
Remove-Item -Path "C:\path\to\folder" -Recurse
Remove-Item -Path "C:\path\to\file.txt" -Force
```

### Find files with a particular extension and move them

```powershell
Get-ChildItem -Path "source_dir" -Recurse -Filter *.zip |
    Move-Item -Destination "destination_dir" -Force
```

## References

* PowerShell docs
* PowerShell blog
