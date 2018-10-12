---
layout: post
title: "fuzzystringmatch"
date: 2018-10-05 14:07:20 +0800
comments: true
categories: ruby
---

### https://github.com/kiyoka/fuzzy-string-match

require 'fuzzystringmatch'
jarow = FuzzyStringMatch::JaroWinkler.create( :native )
p jarow.getDistance(  "jones",      "johnson" )