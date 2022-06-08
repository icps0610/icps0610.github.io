---
layout: post
title: "scheduledTask"
date: 2022-03-05 03:08:03 +0800
comments: true
categories: Powershell
---

``` go

`Get-ScheduledTask -TaskPath \ | Get-ScheduledTaskInfo | Where { $_.TaskName -match "jdc"} | Format-Table -Property TaskName, LastRunTime, NextRunTime`  


`schtasks /query  | findstr "jdc"`  
```

>> ref https://docs.microsoft.com/zh-tw/windows-server/administration/windows-commands/schtasks-query