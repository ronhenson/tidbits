---
title: "Windows 10 Move Library Folder"
created: 2021-09-26 08:11:55
tags: devops
keywords: windows, folder, directory, drive, hard link, mlink
---

# Windows 10 Move Library Folder

## Move Your User Library Folder to Another Drive

In order to free up space on your primary drive, you may want to move your library folders, or even your entire Users folder, to a separate drive. In order to do this, you'll want to first move the folder to the drive of your choice, then set up a symbolic link (symlink for short) to the new location in the default location. Below are the specific steps I recommend following for this procedure.

First, I recommend that you close all programs but the file explorer, as most programs utilize the folder(s) you are about to move. Then, navigate to your primary Users folder (or just the primary drive, if you're moving all user folders), select the folder(s) you would like to move, cut them, navigate to the location you would like them moved to (I recommend moving them to the exact same location, except with the drive letter changed), and paste. If prompted, give administrator privileges in order to move these vital folders.

Next, you'll want to set up a symlink. To do this, we'll need to open up an administrator command prompt. Open Start and either find Command Prompt on the All Apps menu or search for it, then right-click and select "Run as administrator". The command you'll use is mklink. If you're just moving the entire Users folder, the command would look something like, `mklink /J C:\Users D:\Users`. If you're moving a specific user's folder, the command would look something like, `mklink /J C:\Users\<User> D:\Users\<User>`. If either of the locations in your command contains spaces, surround the location with quotation marks, e.g. "C:\Users\This User\". The switch /J sets up something called a hard link, which basically is something that looks exactly like a real folder to the computer but really directs the computer to a different location. This prevents any issues that would appear if we had used a soft link (which is basically what the computer puts on the desktop when you place a shortcut to a folder) if a program wants to save something to a folder in your libraries. The first location you see in the command is the location and name of the symlink you're placing, and the second location is the location the symlink directs the computer towards.

And there we go. To test, navigate to the previous location of the folder(s) you did this with, and you should see a folder icon with an arrow in the corner of it, indicating that the computer recognizes it as a hard link. Good luck, and have fun out there!
