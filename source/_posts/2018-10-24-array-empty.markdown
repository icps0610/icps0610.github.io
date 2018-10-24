---
layout: post
title: "array_empty"
date: 2018-10-24 11:12:57 +0800
comments: true
categories: ruby
---
>> ref https://stackoverflow.com/questions/5878697/how-do-i-remove-blank-elements-from-an-array
``` ruby
array = [[], "", "2", nil]
array.compact
#[[], "", "2"]
array.reject(&:blank?)
#["2"]
```