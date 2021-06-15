---
layout: post
title: "工作排程器"
date: 2021-06-15 16:52:08 +0800
comments: true
categories: win
---

### taskschd.msc  
### schtask  
##### https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-change  

``` bat
//列出
schtasks /query  

//查詢LineSend
schtasks /query /fo LIST /v /tn LineSend

變更
schtasks /TN LineSend /CHANGE /ST 14:00  

不需密碼
/RU SYSTEM
```
