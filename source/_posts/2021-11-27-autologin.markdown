---
layout: post
title: "autologin"
date: 2021-11-27 06:44:09 +0800
comments: true
categories: win
---

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device]
"DevicePasswordLessBuildVersion"=dword:00000000


Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"AutoAdminLogon"="1"
"DefaultUserName"="abc"
"DefaultPassword"="123"

netplwiz
```