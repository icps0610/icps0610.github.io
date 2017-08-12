---
layout: post
title: "youtube-dl"
date: 2016-06-17 20:12:25 +0800
comments: true
categories: app
---

### install  
`apt-get install youtube-dl`  

### mp3  
<pre><code>youtube-dl –extract-audio –audio-format mp3 <font color='red'>url</font>
</code></pre>
### video
<pre><code>youtube-dl -f mp4 <font color='red'>url</font>
</code></pre>

### stream

youtube-dl https://youtu.be/B-MTiuswulY -o - | ffmpeg -threads 4 -i pipe:0 -vcodec mp4 -s qcif -f wav -y - | mplayer -
