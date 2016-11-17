---
layout: post
title: "datepicker"
date: 2016-11-17 21:03:30 +0800
comments: true
categories: ror
---
### [bootstrap-datepicker](https://github.com/Nerian/bootstrap-datepicker-rails)


```	ruby
gem 'bootstrap-datepicker-rails'
```
```	ruby
<%= form_tag items_path, method: :get do %>
  <%= text_field_tag :q, nil, placeholder:Time.now.localtime.strftime("%m/%d/%Y"), 'data-provide':'datepicker' %>
  <%= submit_tag %>
<% end %>
```