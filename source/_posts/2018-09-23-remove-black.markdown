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

def pixel_black(img, x, y)
    pixel = img.pixel_color(x, y)
    if not [ pixel.red, pixel.green, pixel.blue ] == [ 0, 0, 0 ]
        return true
    end
end

def locate_x(img)
    x = img.columns/2
    y = img.rows
    i = 0
    loop do
        if pixel_black(img, i, i) 
            @fxy ||= locate_y(img, "left", i, i)
        end
        if pixel_black(img, x, y) 
            @bxy ||= locate_y(img, "right", x, y)
        end
        if defined?(@fxy) and defined?(@bxy)
            return [@fxy, @bxy].flatten
        end
        i += 1
        x -= 1
        y -= 1
    end
end

def locate_y(img, direction, x, y)
    if direction == "left"
        i = -1
    else
        i =  1
    end
    if pixel_black(img, x, y) and pixel_black(img, x, y + i)
        locate_y(img, direction, x, y + i)
    else
        return x, y
    end
end

screenshot = Magick::Image.capture{ self.filename = "root" }
fx, fy, bx, by = locate_x(screenshot)
screenshot.crop!(fx, fy, bx-fx, by-fy)
screenshot.write("/tmp/screenshot.jpg")
```