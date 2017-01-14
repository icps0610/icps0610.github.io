---
layout: post
title: "dupclicate_elements"
date: 2017-01-14 17:11:55 +0800
comments: true
categories: ruby
---

``` ruby  
array = ["aa","bb","cc","bb","bb","cc"]
array.each_with_object(Hash.new(0)){|w,c| c[w]+=1}.each do |w,c|
	puts "#{w} #{c} times"
end
```  