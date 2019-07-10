---
layout: post
title: "error_page"
date: 2019-05-10 15:02:36 +0800
comments: true
categories: ror
---

>> ref https://rubyplus.com/articles/4061-How-to-handle-exceptions-in-Rails-5
``` ruby
# route.rb

match "*path", to: "page#catch_404", via: :all

# application_controller.rb
rescue_from ActionController::RoutingError do |exception|
 logger.error 'Routing error occurred'
 render plain: '404 Not found', status: 404 
end

# page_controller.rb

def catch_404
  raise ActionController::RoutingError.new(params[:path])
end

```