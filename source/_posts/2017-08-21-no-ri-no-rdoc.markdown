---
layout: post
title: "no_ri_no_rdoc"
date: 2017-08-21 20:27:38 +0800
comments: true
categories: ruby
---
>>ref  https://stackoverflow.com/questions/1381725/how-to-make-no-ri-no-rdoc-the-default-for-gem-install

```
ou just add following line to your local ~/.gemrc file (it is in your home folder)

gem: --no-document or you can add this line to the global gemrc config file. Here is how to find it (in Linux)

strace gem source 2>&1 | grep gemrc
```