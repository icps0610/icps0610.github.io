---
layout: post
title: "sublime text"
date: 2016-06-14 01:02:30 +0800
comments: true
categories: app
---

sublime 3 
- ChineseLocalizations 中文化
- BracketHighlighter   括弧
- Insert Nums          插入行號

sublime 2

- 中文化  %APPDATA%\Sublime Text 2\Packages\Default
- 安裝套件 https://sublime.wbond.net/installation
- ctrl +shit+p，然後輸入install Package <enter>
- 多行編輯 ctrl + shift + L  
`beautifyruby`

`"default_line_ending": "unix"`

<pre><code>{
#user
"bracket_styles": {
 
    "default": {
     
        "color": "entity.name.class",
        "style": "highlight",
         
        "quote_scope" : "bracket.quote",
        "curly_scope" : "bracket.curly",
        "round_scope" : "bracket.round",
        "square_scope": "bracket.square",
        "angle_scope" : "bracket.angle",
        "tag_scope"   : "bracket.tag",
         
        // 高亮樣式 (solid|outline|underline|none)
        "quote_style" : "solid",
        "curly_style" : "solid",
        "round_style" : "solid",
        "square_style": "solid",
        "angle_style" : "solid",
        "tag_style"   : "outline"
        },
    }
}
</code></pre>

<pre><code>
Emmt   #settings

{
	"snippets":{
		"html":{
			"snippets":{
			"ff" : "<%= form_for ${1:} %>\n<% end %>",
			"sf" : "<%= simple_form_for ${1:} %>\n<% end %>",
			"fl" : "<%= link_to ${1:} %>",
			"fb" : "<%= button_to ${1:} %>",
			"fe" : "<%= ${1:} %>",
			"ee" : "<% ${1:} %>",
			"eif" : "<% if ${1:} %>",
			"ed" : "<% end %>",
			"else" : "<% else %>"
			}
		}
	}
}
</code></pre>

pythonpep8autoformat 

SublimeLinter-ruby

```
#Preferences.sublime-settings

{
    "tab_size": 4,
    "translate_tabs_to_spaces": false,
    "default_line_ending": "unix"
}
```