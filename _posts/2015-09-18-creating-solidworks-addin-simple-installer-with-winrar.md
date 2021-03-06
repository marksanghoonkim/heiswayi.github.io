---
layout: post
title: Creating SolidWorks Add-in Simple Installer With WinRAR
description: Using WinRAR SFX and batch scripting for creating simple package installer.
keywords: solidworks addin, simple installer, winrar sfx
---

When I was working as Application Engineer for one of the top SolidWorks Reseller local companies, I was assigned to develop a company's first SolidWorks Add-in DLL. To make the installation process of the add-in as simple as possible, I opted to use WinRAR SFX and batch scripting to automate the assembly (DLL) registration with Microsoft .NET Framework.

> **PREREQUISITE:** This process required a [WinRAR](http://www.rarlab.com/download.htm) software to be installed in my PC and prepared my add-in files or folders before I can use WinRAR SFX to create a package installer for my add-in.

These are the steps I used to create my add-in package installer using the WinRAR SFX:-

### Step 01

![Step 01](http://heiswayi.github.io/images/20150918/Image-001.png)

I added all of my add-in files and folders to the WinRAR archive.

### Step 02

![Step 02](http://heiswayi.github.io/images/20150918/Image-002.png)

Under General tab, I selected **Create SFX archive** in Archiving options and enter my Archive name. Other options are optional or I just leave it as **defaults**.

### Step 03

![Step 03](http://heiswayi.github.io/images/20150918/Image-003.png)

Under Advanced tab, I clicked on **SFX options** button.

### Step 04

![Step 04](http://heiswayi.github.io/images/20150918/Image-004.png)

In the **Advanced SFX options**, under General tab, I entered the Path to extract. In my case, I used `C:\IME` as the Absolute path for my add-in installation directory.

### Step 05

![Step 05](http://heiswayi.github.io/images/20150918/Image-005.png)

Under Setup tab, I entered the filename that I want to run/execute after extraction is finished. In my case, `INSTALL64.BAT` is a batch file that I want it to run/execute after the package extraction.

### Step 06

![Step 06](http://heiswayi.github.io/images/20150918/Image-006.png)

Under Advanced tab, I marked **Request administrative access** as checked since I need to register my add-in assembly into Microsoft .NET Framework. So, the user will be prompted to run the package installer with administrative access.

### Step 07

![Step 07](http://heiswayi.github.io/images/20150918/Image-007.png)

Under Update tab, I checked **Extract and replace files** for Update mode and **Overwrite all files** for Overwrite mode. This will be useful when in the future time I deliver a new update or release of the current program, the new installation will replace and overwrite all of the old files.

### Step 08

![Step 08](http://heiswayi.github.io/images/20150918/Image-008.png)

Under Text and icon tab, here I entered the Title of SFX window, Text to display in SFX window and assign the SFX icon/logo.

### Step 09

![Step 09](http://heiswayi.github.io/images/20150918/Image-009.png)

Finally, under Module tab, I selected the SFX module that I want to use. In my case, I chose **Default.SFX** - Windows GUI RAR SFX.

### Step 10 (Final)

![Step 10](http://heiswayi.github.io/images/20150918/Image-010.png)

Then, I pressed OK and OK again to finish it.

### Viola..

At the end, I got my add-in package installer (setup.exe) as shown in the picture below:

![Step 11](http://heiswayi.github.io/images/20150918/Image-011.png)

To test if the file is OK, I tried to run it (double-clicked), it showed up something like this (refer the picture below):

![Step 12](http://heiswayi.github.io/images/20150918/Image-012.png)

That means my add-in installer file is working!

### The Batch files

I used the batch scripting to automate some commands. If you noticed, there were two batch files in my add-in installation package; `INSTALL64.BAT` and `UNINSTALL64.BAT`. Both were used for the installation and uninstallation of my SolidWorks Add-in. By using the batch scripting, I can register my add-in assembly into Microsoft .NET Framework so that it will work with the main SolidWorks software.

Here's the batch script inside the `INSTALL64.BAT` file:

```
@echo off

set TargetLocation=c:\IME\
set FolderToCopy=IMEInterX64
set FileToCopy1=UNINSTALL64.BAT

echo.
echo 1. Setup configuration data

start "" /d "%~dp0%FolderToCopy%" "addconfigs.exe"

echo #1 DONE
echo.
echo 2. Register assemblies

SET AssemblyPath="%TargetLocation%%FolderToCopy%\IMEInterX2_SwAddin.dll"
set FMWK=Framework64
IF EXIST "%Windir%\Microsoft.NET\%FMWK%\v4.0.30319" (set FMWKVersion=v4.0.30319) ELSE (set FMWKVersion=v2.0.50727)
echo Detected %FMWK% %FMWKVersion%
echo Running command...
@ECHO ON
"%Windir%\Microsoft.NET\%FMWK%\%FMWKVersion%\regasm" /codebase %AssemblyPath%
@ECHO OFF

echo #2 DONE

echo.
echo INSTALLATION COMPLETE
pause
```

> `addconfigs.exe` is a mini program to create and load configuration data into Windows Registry.

> `IMEInterX2_SwAddin.dll` is my SolidWorks Add-in program (assembly) that need to be registered (regasm) with Microsoft .NET Framework in order to work in the main SolidWorks software.

### The Uninstallation process

Basically, `UNINSTALL64.BAT` file is for removing all the add-in files and folders, deleting configuration data in the Windows Registry and finally the file will terminate itself. If I want to create the Uninstaller shortcut for my program, I can do that inside `INSTALL64.BAT` file, then link the shortcut to `UNINSTALL64.BAT` file when executed.
