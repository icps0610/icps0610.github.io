---
layout: post
title: "rtl8192eu-linux-drives"
date: 2018-03-20 23:38:25 +0800
comments: true
categories: linux
---
`https://github.com/lord2y/rtl8192eu-arm-linux-driver`

```
apt-get install git raspberrypi-kernel-headers build-essential dkms
dkms add .
dkms install rtl8192eu/1.0

dkms status

dkms uninstall rtl8192eu/1.0  
dkms remove rtl8192eu/1.0 --all  

`https://icps0610.github.io/blog/2018/03/21/wifi-ap/`
```

```
#語言
apt-get install language-pack-gnome-zh-hant language-pack-gnome-zh-hans language-pack-zh-hant language-pack-zh-hans
rm /var/lib/locales/supported.d/*
echo "zh_TW.UTF-8 UTF-8" > /var/lib/locales/supported.d/local
echo "en_US.UTF-8 UTF-8" >> /var/lib/locales/supported.d/local
echo "zh_TW BIG5" >> /var/lib/locales/supported.d/local
locale-gen

echo "export LANG=\"zh_TW.UTF-8\"">> /etc/environment
echo "export LANGUAGE=\"zh_TW:zh\"">> /etc/environment
echo "export LC_ALL=\"zh_TW.UTF-8\"">> /etc/environment
dpkg-reconfigure locales
update-locale
#設定時間
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime

#sourlist
sed -i 's/tw.archive.ubuntu.com/free.nchc.org.tw/g' /etc/apt/sources.list
#deb http://ftp.tku.edu.tw/kali kali main contrib non-free
#deb http://ftp.tku.edu.tw/kali-security kali main contrib non-free
#google
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - 
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
add-apt-repository ppa:fcitx-team/nightly

#桌布
/usr/share/backgrounds/warty-final-ubuntu.png 

#安裝
apt-get install -y google-chrome-stable qrencode zbar-tools medusa hydra hydra-gtk john ettercap-graphical ophcrack ruby ruby-dev bundler netcat-traditional bkhive samdump2 reaver build-essential aircrack-ng wifite nmap netdiscover chntpw git rdesktop wireshark pv deborphan  bleachbit  crunch gparted 
apt install curl

#nc -e option
rm /etc/alternatives/nc && ln -s /bin/nc.traditional /etc/alternatives/nc

#移除
apt purge -y firefox thunderbird  gnome-keyring empathy aisleriot  landscape-client-ui-install brasero

#clean
#!/usr/bin/env ruby
# encoding: UTF-8
#apt-get purge -y `deborphan --nice-mode --guess-all`
print "upgrade? "
system "apt-get update && apt-get dist-upgrade" if gets.chomp.downcase == "y"
system "apt-get autoclean"
system "apt-get clean"
system "apt-get autoremove"
system "apt-get --purge autoremove -y"
system "rm /var/cache/apt/archives/* 2>/dev/nul"
system "rm /var/cache/apt/archives/partial/* 2>/dev/nul"
system "rm -R /var/log/* 2>/dev/nul"
system "rm -R /tmp/* 2>/dev/nul"
system "rm /root/.local/share/Trash/files/* 2>/dev/nul"
system "updatedb"
system "rm /root/.bash_history 2>/dev/nul"
exit

#~/.bashrc
#if [ -f /etc/bash_completion ] && ! shopt -oq posix; then
#    . /etc/bash_completion
#fi

sudo apt-get remove ibus
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install fcitx fcitx-config-gtk fcitx-chewing
im-switch -s fcitx -z default

echo "enabled=0" > /etc/default/apport
gem install headless nokogiri colorize watir-webdriver  sinatra  net-ssh net-scp --no-ri --no-rdoc shotgun

sudo apt-get -y update
sudo apt-get -y install build-essential zlib1g-dev libssl-dev libreadline6-dev libyaml-dev
cd /tmp
wget http://cache.ruby-lang.org/pub/ruby/2.0/ruby-2.0.0-p481.tar.gz
tar -xvzf ruby-2.0.0-p481.tar.gz
cd ruby-2.0.0-p481/
./configure --prefix=/usr/local
make
sudo make install

wget http://c758482.r82.cf2.rackcdn.com/sublime-text_build-3083_amd64.deb

Open your Software Center
In the search, type: "midori-granite"
Just click in "Remove" button of this item.

/usr/share/applications/pantheon-files.desktop

```