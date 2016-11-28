---
layout: post
title: "currentpage"
date: 2016-11-28 13:37:45 +0800
comments: true
categories: ror
---
> ref : https://kmywblog.wordpress.com/2015/10/13/

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
