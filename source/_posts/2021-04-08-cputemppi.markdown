---
layout: post
title: "cpuTemp_pi"
date: 2021-04-08 19:15:11 +0800
comments: true
categories: linux
---



>> Ref https://github.com/raspberrypi/userland  
>> Ref https://www.raspberrypi.org/forums/viewtopic.php?t=121125  

     

``` 
git clone https://github.com/raspberrypi/userland
cd userland
./buildme


echo $((`cat /sys/class/thermal/thermal_zone0/temp`/1000))°C
```

