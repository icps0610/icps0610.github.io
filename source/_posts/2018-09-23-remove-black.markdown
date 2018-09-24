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
    pc = [px.red, px.green, px.blue].map {|n| n/256 }
    pc.each {|p| return true if p > 87 }
end

def locate_x_left(img)
    if pixel_not_black(img, 0, 0)
        return 0, 0
    end
    (0..img.rows).each do |i|
        if pixel_not_black(img, i, i)
            return locate_y(img, "left", i, i)
        end
    end
end

def locate_x_right(img)
    x = 1920
    if pixel_not_black(img, x, 0)
        return x, 0
    end
    (0..x).each do |i|
        if pixel_not_black(img, x - i, i)
            return locate_y(img, "right", x - i, i)
        end
    end
end

def locate_y(img, direction, x, y)
    direction == "right" ? i = 1 : i = -1
    if pixel_not_black(img, x, y) and pixel_not_black(img, x, y + i)
        locate_y(img, direction, x, y + i)
    else
        return x, y
    end
end

#screenshot = Magick::Image.capture{ self.filename = "root" }
img = Magick::ImageList.new.read("/tmp/image/base0.jpg")
lx, ly = locate_x_left(img)
rx, ry = locate_x_right(img)
img.crop!(lx, ly, rx-lx, ry-ly)
img.write("/tmp/image/base2.jpg")

```