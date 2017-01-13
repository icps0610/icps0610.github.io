---
layout: post
title: "link_entire_table"
date: 2017-01-14 01:44:58 +0800
comments: true
categories: ror
---

http://stackoverflow.com/questions/4904938/link-entire-table-row

``` erb
<script>
  $('tr').click( function() {
    window.location = $(this).find('a').attr('href');
}).hover( function() {
    $(this).toggleClass('hover');
});
</script>
<style>
  tr.hover {
   cursor: pointer;
   /* whatever other hover styles you want */
}
</style>
```