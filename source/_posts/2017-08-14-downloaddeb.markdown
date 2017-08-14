---
layout: post
title: "download_deb"
date: 2017-08-14 12:55:13 +0800
comments: true
categories: linux
---

>> ref https://askubuntu.com/questions/47865/how-do-i-use-apt-get-to-only-download-packages

```
# dldeb.sh
#! /bin/bash
PACKAGE=$1
URI=`apt-cache show $PACKAGE | grep "Filename:" | cut -f 2 -d " "`
wget http://tw.archive.ubuntu.com/ubuntu/$URI
```