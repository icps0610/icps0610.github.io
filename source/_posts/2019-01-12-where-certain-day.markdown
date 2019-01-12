---
layout: post
title: "where_certain_day"
date: 2019-01-12 01:39:03 +0800
comments: true
categories: ror
---
date = A.last.created_at
A.where(created_at: date.midnight..date.end_of_day)