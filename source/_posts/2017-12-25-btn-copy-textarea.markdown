---
layout: post
title: "btn_copy_textarea"
date: 2017-12-25 22:19:24 +0800
comments: true
categories: ror
---

>>ref https://stackoverflow.com/questions/3739330/copy-textarea-text-to-clipboard-html-javascript

``` html

<TEXTAREA COLS="40" ROWS="8" id="dic" name="dic"  class="form-control">
test</TEXTAREA>
<button class="btn btn-default" onclick="copy_to_clipboard('eic')">Copy</button>
<button class="btn btn-default" onclick="copy_to_clipboard('dic')">Copy</button>
<script>
function copy_to_clipboard(id)
{
    document.getElementById(id).select();
    document.execCommand('copy');
}
</script>



```