---
layout: post
title: "octopress"
date: 2016-06-16 20:45:44 +0800
comments: true
categories: app
---

## install
https://github.com/imathis/octopress  

`git clone https://github.com/imathis/octopress`  
`bundle install`  
`rake install`  
###### 開新的repo，名稱打上 username.github.io  
`rake setup_github_pages` 
###### 輸入: git@github.com:username/username.github.io.git  
`rake generate`  
`rake deploy`　　　=>　　　`rake gen_deploy`  

`git add .`  
`git commit -am "ok"`  
`git push origin source`  
  

> ! [rejected]        master -> master (non-fast-forward)  
> `cd _deploy`  
> `git pull origin master`  
> `cd ..`  
> `rake deploy`  

###### Custom Domains  
`echo 'your-domain.com' >> source/CNAME`  

###### rake preview
``` ruby
#Rakefile
#28 
server_port     = "80"  
#86 
rackupPid = Process.spawn("rackup -o 0.0.0.0 --port #{server_port}")
```
## debug
`jekyll serve --trace`

> jekyll 2.5.3 | Error:  user bg.UAS doesn't exist
> rm source/images/~bg.UAS


## command

##### 產生文章  
`rake new_page["about"]`  
`rake new_post["title"]`  
`rake generate preview`
`rake gen_deply`

###### 上傳  
`git add .`  
``git commit -am "`date +%Y-%m-%d:%H:%M:%S`"``  
`git push origin source`  
`bundle exec rake gen_deploy`  
  
###### 下載
`git clone -b source git@github.com:username/username.github.io.git octopress`  
`cd octopress`  
`git clone git@github.com:username/username.github.io.git _deploy`  
`git pull origin source`  
`cd ./_deploy`  
`git pull origin master`  

## homepage

``` ruby
#source/_includes/custom/navigation.html
<ul class="main-navigation">
  <li><a href="{% raw %}{{ root_url }}{% endraw %}/">Home</a></li>
  <li><a href="{% raw %}{{ root_url }}{% endraw %}/recent">Recent</a></li>
  <li><a href="{% raw %}{{ root_url }}{% endraw %}/blog/archives">Archives</a></li>
  <li><a href="{% raw %}{{ root_url }}{% endraw %}/blog/categories">Categories</a></li>
</ul>

```

``` ruby
#Rakefile
#21
blog_index_dir  = 'source/blog'
```

`mkdir source/recent`  
`mv source/index.html source/recent/`   

<pre><code>#source/recent/index.html
#7 
{% raw %}{% for post in site.posts limit: site.recent_posts %}{% endraw %}
</code></pre>
`rake new_page["index.html"]`  

## Category

> ref  http://codemacro.com/2012/07/18/add-category-list-to-octopress/   
> ref  https://github.com/shunnien/octopress-category-list 

###### code  
``` ruby
#plugins/category_list_tag.rb
module Jekyll
  class CategoryList < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
  class CategoryListTop < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<h2 class='entry-title'><a href='/#{category_url}/'>#{category}</a></h2>"
      end
      html

    end
  end
end

Liquid::Template.register_tag('category_list_top', Jekyll::CategoryListTop)
Liquid::Template.register_tag('category_list', Jekyll::CategoryList)

```



###### asides

``` html
#source/_includes/asides/category_list.html  
<section>  
  <h1>Categories</h1>  
    <ul id="categories">
       {% raw %}{% category_list %}{% endraw %}
    </ul>
</section>
```
###### 在側邊加入連結
``` ruby
#_config.yml
#56 
default_asides: [asides/category_list.html]
```
###### top

``` html
#source/blog/categories/index.html
---
layout: page
title: Categories
footer: false
---
<style>
a {
    text-decoration:none;
}
</style>
<article>
  {% raw %}{% category_list_top %}{% endraw %}
</article>

```
###### 在上方加入連結
``` 
#source/_includes/custom/navigation.html
<ul class="main-navigation">
  <li><a href="{% raw %}{{ root_url }}{% endraw %}/blog/categories">Categories</a></li>
</ul>
```
## Background image in header  
> ref https://github.com/imathis/octopress/issues/356

``` ruby
#sass/custom/_styles.scss
header[role="banner"] {
     background-image: url(/images/bg.png);
     background-repeat: no-repeat;
}
```


## 新產生的文章給予 uncategorized  
``` ruby
#Rackfile
#117
post.puts "categories: uncategorized"
``` 

## 貼語法語法

> ref  http://blog.slaks.net/2013-06-10/jekyll-endraw-in-code/  

###### 若要顯示 {% raw %}{% category_list_top %}{% endraw %}

<pre><code>
  {% assign oTag = '{%' %}{{ oTag }}{% raw %} raw %}{% endraw %}<font color='red'>{% raw %}{% category_list_top %}{% endraw %}</font>{{ oTag }}{% raw %} endraw %}{% endraw %}

</code></pre>

###### 若要顯示 {{ oTag }}{% raw %} raw %}{% endraw %}{{ oTag }}{% raw %} endraw %}{% endraw %}
<pre><code>{% raw %}{% assign <font color='green'>oTag</font> = '{%' %}{% endraw %}
# 然後在貼下半部
<font color='green'>{% raw %}{{ oTag }}{% endraw %}</font><font color='red'>raw %}{{ oTag }}{% raw %} endraw %}{% endraw %}</font>
<font color='green'>{% raw %}{{ oTag }}{% endraw %}</font><font color='red'>endraw %}{{ oTag }}{% raw %} endraw %}{% endraw %}</font>
</code></pre>  
###### 若要顯示  \` 前後用兩個 \` 包起來

<pre><code>``git commit -am "`date +%Y-%m-%d:%H:%M:%S`"``  
</code></pre>

## 圖片
<pre>
  ![ping.png](/images/ping.png)
</pre>

## 連結
<pre>
  [Google](https://www.google.com.tw/)
</pre>
