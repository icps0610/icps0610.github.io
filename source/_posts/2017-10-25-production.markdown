---
layout: post
title: "production"
date: 2017-10-25 23:51:28 +0800
comments: true
categories: ror
---

`rake secret` => config/secrets.yml
###### 不能使用tab要用space

`rake db:create db:migrate RAILS_ENV=production`
`rake assets:precompile RAILS_ENV=production`
`rails s -e production -p 80`
