---
layout: post
title: "current_page"
date: 2016-11-28 13:27:58 +0800
comments: true
categories: ror
---
> ref : https://kmywblog.wordpress.com/2015/10/13/%E5%A6%82%E4%BD%95%E5%9C%A8-rails-%E5%8B%95%E6%85%8B%E8%AA%BF%E6%95%B4-html-%E4%B8%AD-class-%E7%9A%84-value/

``` ruby
# app/helpers/application_helper
module ApplicationHelper
 
 def active_class(link_path)
  current_page?(link_path) ? "active" : ""
 end
 
end
```
``` ruby
# views
 <li class="<%= active_class(root_path) %>"><%= link_to '全部清單',items_path %>
```