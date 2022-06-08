---
layout: post
title: "auto_hide_taskbar"
date: 2022-01-18 20:03:25 +0800
comments: true
categories: bat
---


``` bat

@echo off
set logPath="z:\taskbar.log"
set /p log=< %logPath%
set cmd1="&{$p='HKCU:SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\StuckRects3';$v=(Get-ItemProperty -Path $p).Settings;$v[8]="
set cmd2=";&Set-ItemProperty -Path $p -Name Settings -Value $v;&Stop-Process -f -ProcessName explorer}"
if %log% == 1 (
    powershell -command %cmd1%3%cmd2%
    echo 0 > %logPath%
) else (
    powershell -command %cmd1%2%cmd2%
    echo 1 > %logPath%
)


```