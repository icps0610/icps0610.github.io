---
layout: post
title: "rename去掉中文括弧"
date: 2017-08-21 02:55:06 +0800
comments: true
categories: linux
---
rename -n 's/（(.*)）/($1)/' *
rename -v 's/\s+(.*)/$1/' *