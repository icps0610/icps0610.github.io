---
layout: post
title: "waitr-scoll"
date: 2018-09-24 20:38:17 +0800
comments: true
categories: ruby
---

### https://github.com/p0deje/watir-scroll

``` ruby
browser = Watir::Browser.new :firefox
browser.button(text: 'Click').scroll.by(0, 800)
```