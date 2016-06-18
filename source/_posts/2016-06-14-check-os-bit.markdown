---
layout: post
title: "check os bit"
date: 2016-06-14 01:34:10 +0800
comments: true
categories: bat
---
<pre><code>@echo off
if %PROCESSOR_ARCHITECTURE% == AMD64 echo 64
if %PROCESSOR_ARCHITECTURE% == x86 echo 32
pause
</code></pre>