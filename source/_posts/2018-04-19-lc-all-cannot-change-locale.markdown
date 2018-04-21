---
layout: post
title: "LC_ALL_cannot_change_locale"
date: 2018-04-19 20:23:27 +0800
comments: true
categories: linux
---
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
locale-gen en_US.UTF-8
dpkg-reconfigure locales