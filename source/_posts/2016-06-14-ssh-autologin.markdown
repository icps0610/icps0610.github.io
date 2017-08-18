---
layout: post
title: "ssh自動登入"
date: 2016-06-14 12:50:31 +0800
comments: true
categories: ruby
---
``` ruby
# encoding: utf-8
if `gem list | grep net-ssh` == ""
  puts "缺少套件net-ssh net-scp 準備安裝.."
  `gem install net-ssh net-scp --no-ri --no-rdoc`
end
require "net/ssh"
require "net/scp"
$ip=[]
if ARGV.count == 3
  $ip << [ ARGV[0], ARGV[1], ARGV[2] ]
else
  puts "target user passwd"
  exit
end
$localip = `ifconfig | grep cast`.split("\n").map{|i|i.scan(/\d+\.\d+\.\d+\.\d+/)[0]}

if $localip.count > 1
  puts $localip
  print "----------\n> "
  ch = STDIN.gets.to_i
  $localip = $localip[ch-1]
end
$localip =  $localip
$localuser=ENV['USER']

class SSH_n_1

  def initialize
    puts "本地端為SSH主機,#{$localuser}@#{$localip}"
    puts
    $ip.each do |host,user,passwd|
      if system("nc -nz -w 1 #{host} 22") == true
        puts ">> #{host}連線正常"
        puts "   #{host}產生ssh key"
        if user == "root"
          @home="/#{user}"
        else
          @home="/home/#{user}"
        end
        script(host,user,passwd)
        puts "   #{host}下載ssh key"
        download_key(host,user,passwd)
        `mkdir ~/.ssh 2> /dev/null`
        `chmod go-w ~`
        `chmod 700 ~/.ssh`
        `chmod 600 ~/.ssh/authorized_keys`
        `sed -i /#{user}@#{@hostname}/d ~/.ssh/authorized_keys`
        `cat #{$localip}.pub >> ~/.ssh/authorized_keys`
        `rm #{$localip}.pub`
        puts "   USER: #{user} 已完成"
      else
        puts ">> #{host} 沒有回應"
      end
      puts
    end

  end

  def download_key(host,user,passwd)
    Net::SCP.download!(host,user, "#{@home}/.ssh/#{$localip}.pub", "#{$localip}.pub",  :ssh => { :password => passwd })
  end

  def script(host,user,passwd)
    Net::SSH.start(host,user, :password => passwd ) do |ssh|
      @hostname=ssh.exec!("hostname").chomp
      ssh.exec!("mkdir ~/.ssh")
      ssh.exec!("chmod 700 ~/.ssh")
      ssh.exec!("rm #{@home}/.ssh/#{$localip}")
      ssh.exec!("rm #{@home}/.ssh/#{$localip}.pub")
      ssh.exec!("ssh-keygen -t rsa -f #{@home}/.ssh/#{$localip} -N '' -q")
      ssh.exec!("sed -i '/HOST #{$localip}/,+1d' #{@home}/.ssh/config")
      ssh.exec!("echo HOST #{$localip} >> #{@home}/.ssh/config")
      ssh.exec!("echo IdentityFile ~/.ssh/#{$localip} >> #{@home}/.ssh/config")
    end
  end

end

class SSH_1_n

  def initialize
    puts "IP列表為SSH主機, 使用者: #{$localuser}"
    @hostname=`hostname`.chomp
    puts "產生ssh key..."
    ssh_keygen

    $ip.each do |host,user,passwd|
      if system("nc -nz -w 1 #{host} 22") == true
        puts ">> #{host}連線正常"
        puts "   上傳key到 #{host}"
        upload_key(host,user,passwd)
        puts "   #{host} 執行指令"
        script(host,user,passwd)
        `sed -i '/HOST #{host}/,+1d' ~/.ssh/config 2>/dev/null`
        `echo HOST #{host} >> #{ENV['HOME']}/.ssh/config`
        `echo IdentityFile #{ENV['HOME']}/.ssh/#{$localip} >> #{ENV['HOME']}/.ssh/config`
        puts "   >> #{user}@#{host}"
      else
        puts ">> #{host} 沒有回應"
      end
      puts
    end

  end

  def upload_key(host,user,passwd)
    Net::SCP.upload!(host,user, "#{ENV['HOME']}/.ssh/#{$localip}.pub", "id_rsa.pub",  :ssh => { :password => passwd })
  end

  def script(host,user,passwd)
    Net::SSH.start(host,user, :password => passwd ) do |ssh|
      ssh.exec!("sed -i /#{ENV['USER']}@#{@hostname}/d ~/.ssh/authorized_keys")
      ssh.exec!("mkdir ~/.ssh")
      ssh.exec!("cat id_rsa.pub >> ~/.ssh/authorized_keys")
      ssh.exec!("chmod go-w ~")
      ssh.exec!("chmod 700 ~/.ssh")
      ssh.exec!("chmod 600 ~/.ssh/authorized_keys")
    end
  end

  def ssh_keygen
    if not File.exist?("#{ENV['HOME']}/.ssh/#{$localip}")
      system("ssh-keygen -t rsa -f #{ENV['HOME']}/.ssh/#{$localip} -N '' -q")
    end

  end

end
#IP列表為SSH主機
SSH_1_n.new
#本地端為SSH主機
SSH_n_1.new


```