---
layout: post
title: "硬碟速度測試"
date: 2016-06-18 04:08:53 +0800
comments: true
categories: linux
---
<pre><code>dd if=/dev/zero of=test_disk bs=1k count=1000000
dd if=test_disk of=/dev/null bs=2k
rm test_disk
</code></pre>
