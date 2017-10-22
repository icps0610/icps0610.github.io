---
layout: post
title: "KODExplorer"
date: 2017-08-31 22:02:42 +0800
comments: true
categories: app
---
### raspberry pi 3 B
######apache2
`apt-get install apache2 php5 libapache2-mod-php5 php5-gd php5-curl`  
`/etc/init.d/apache2 restart`  

`git clone https://github.com/kalcaddle/KODExplorer.git`  
`chmod -Rf 777 ./KODExplorer/*`  
`mv KODExplorer /var/www/html`  

######php
#########找到php.ini
{% codeblock lang:php %}
<?php
 phpinfo();
?>
{% endcodeblock %}
#########Configuration File (php.ini) Path
```
#php.ini
upload_max_filesize = 1000M
post_max_size = 1000M
memory_limit  1000M
```