---
layout: post
title: "remove_black"
date: 2018-09-23 16:39:45 +0800
comments: true
categories: ruby
---
``` ruby
# encoding: utf-8
require "watir"
require 'rmagick'

def pixel_not_black(img, x, y)
    px = img.pixel_color(x, y)
    if px.red > 16448 or px.green > 16448 or px.blue > 16448
        return true
    end
end

def locate_left(img)
    (0..img.rows).each do |i|
        if pixel_not_black(img, i, i)
            @x = i
            break
        end
    end
    locate_y(img, 0, @x)
end

def locate_right(img)
    x = img.columns/2 #雙螢幕
    (0..x).each do |i|
        if pixel_not_black(img, x - i, i)
            @x = x - i
            break
        end
    end
    locate_y(img, x, @x)
end

def locate_y(img, st, x)
    (0..img.rows).each do |i|
        y = (i - st).abs
        if pixel_not_black(img, x, y)
            return x, y
        end
    end
end

#screenshot = Magick::Image.capture{ self.filename = "root" }
img = Magick::ImageList.new.read("/tmp/image/base.jpg")
lx, ly = locate_left(img)
rx, ry = locate_right(img)
img.crop!(lx, ly, rx-lx, ry-ly)
img.write("/tmp/image/base1.jpg")


```