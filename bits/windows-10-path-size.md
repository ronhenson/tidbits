---
title: "Windows 10 Path Size"
created: 2021-09-26 08:56:12
tags: tags
keywords: template
---

# Windows 10 Path Size

Path length default is 260 character limit

- Tap on the Windows-key, type gpedit.msc, and hit enter.
- Confirm the UAC prompt if it appears.
- Use the hierarchy on the left to navigate to the following policy: Local Computer Policy > Computer Configuration > Administrative Templates > System > Filesystem > NTFS.
- Locate the Enable NTFS long paths policy and double-click on it.
- Switch its state to enabled.
- Click ok.
