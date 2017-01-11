---
layout: post
title: "current_page"
date: 2016-11-28 13:37:45 +0800
comments: true
categories: ror
---
> ref : https://kmywblog.wordpress.com/2015/10/13/

``` ruby
# app/helpers/application_helper
module ApplicationHelper
 
	def render_active(title,path)
		c = current_page?(path) ? "active" : ""
		content_tag(:li, link_to( title, path ), :class => c )
	end
 

end
``` 

``` ruby
# views
<%= render_active( 'Home', root_path ) %>
``` 
