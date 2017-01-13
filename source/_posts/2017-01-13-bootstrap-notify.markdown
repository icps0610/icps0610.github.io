---
layout: post
title: "bootstrap_notify"
date: 2017-01-13 21:25:05 +0800
comments: true
categories: ror
---

### https://github.com/jclay/bootstrap-notify-gem
`gem 'bootstrap_notify'`

<pre><code>#application.js
//= require jquery
//= require jquery_ujs
//= require bootstrap_notify
</code></pre>

<pre><code>#application.scss
@import "bootstrap";
@import "bootstrap_notify";
</code></pre>

``` erb
#app/views/partials/_notify.erb
<% if !notice.blank? %>
	<script> 
		$(document).ready(function () {
			$('.top-left').notify({
				<% flash.each do |name, msg| %>
					message: { text: "<%= msg %>" }
				<% end %>  
			}).show();
		});
	</script>
<% end %>
```
``` erb
#app/views/layouts/application.erb in head tag
<%= render(:partial => "partials/notify") %>
```

``` erb
#app/views/layouts/application.erb in body tag
#<div class='notifications top-left'></div>
```

``` ruby
#app/controllers/uploads_controller.rb
flash[:success] = "uploaded"
```