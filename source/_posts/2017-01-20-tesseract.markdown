---
layout: post
title: "tesseract"
date: 2017-01-20 11:10:53 +0800
comments: true
categories: app
---

`apt-get install tesseract-ocr tesseract-ocr-chi-tra`

`tesseract image.jpg out -l chi_tra`


## golang
`go get github.com/otiai10/gosseract/v2`

``` bash
tessbridge.cpp:5:10: fatal error: leptonica/allheaders.h: No such file or directory
 #include <leptonica/allheaders.h>
          ^~~~~~~~~~~~~~~~~~~~~~~~
```

###
`sudo apt install libtesseract-dev`