---
layout: post
title: "ssh自動登入"
date: 2016-06-14 12:50:31 +0800
comments: true
categories: ruby
---
``` ruby
# encoding: utf-8
begin
    require "net/ssh"
    require "net/scp"
rescue LoadError
    puts "缺少套件net-ssh net-scp 準備安裝.."
    `gem install net-ssh net-scp --no-ri --no-rdoc`
    retry
end

class SSH

    def initialize
        remote_ip, remote_user, remote_passwd = set_remote
        local_ip, local_user = set_local
        one_n(remote_ip, remote_user, remote_passwd, local_user, local_ip)
        n_one(remote_ip, remote_user, remote_passwd, local_user, local_ip)
    end

    def set_remote
        if ARGV.count == 3
            return ARGV[0], ARGV[1], ARGV[2]
        else
            puts "Remote_ip, Remote_user, Remote_passwd"
            exit
        end
    end

    def set_local
        ENV['SSH_AUTH_SOCK'] = `ssh-agent`.split("\n")[0].scan(/SSH_AUTH_SOCK=(.*); e/).join
        if not Gem::Platform.local.os == 'linux'
            local_ip   = "192.168.0.100"
            local_user = "icps"
        else
            local_ip = `ifconfig | grep Bcast | cut -d :  -f 2 | cut -d " " -f 1`.split("\n").first
            local_user = ENV['USER']
        end
        return local_ip, local_user
    end

    def n_one(remote_ip, remote_user, remote_passwd, local_user, local_ip)
        puts "本地端為SSH主機,#{local_user}@#{local_ip}\n"
        if check_remote(remote_ip)
            puts ">> #{remote_ip}連線正常"
            puts "   #{remote_ip}產生ssh key"
            if remote_user == "root"
                home = "/root"
            else
                home = "/home/#{remote_user}"
            end
            n_one_script(remote_ip, remote_user, remote_passwd, home, local_ip)
            puts "   #{remote_ip}下載ssh key"
            download_key(remote_ip, remote_user, remote_passwd, home, local_ip)
            `mkdir ~/.ssh 2> /dev/null`
            `chmod go-w ~`
            `chmod 700 ~/.ssh`
            `chmod 600 ~/.ssh/authorized_keys`
            `sed -i /#{remote_user}@#{@hostname}/d ~/.ssh/authorized_keys`
            `cat #{local_ip}.pub >> ~/.ssh/authorized_keys`
            `rm #{local_ip}.pub`
            puts "   USER: #{remote_user} 已完成"
        else
            puts ">> #{remote_ip} 沒有回應"
        end
        puts
    end

    def download_key(remote_ip, remote_user, remote_passwd, home, local_ip)
        Net::SCP.download!(remote_ip, remote_user, "#{home}/.ssh/#{local_ip}.pub", "#{local_ip}.pub",  :ssh => { :password => remote_passwd })
    end

    def n_one_script(remote_ip, remote_user, remote_passwd, home, local_ip)
        Net::SSH.start(remote_ip, remote_user, :password => remote_passwd ) do |ssh|
            @hostname = ssh.exec!("hostname").chomp
            ssh.exec!("mkdir ~/.ssh")
            ssh.exec!("chmod 700 ~/.ssh")
            ssh.exec!("rm #{home}/.ssh/#{local_ip}")
            ssh.exec!("rm #{home}/.ssh/#{local_ip}.pub")
            ssh.exec!("ssh-keygen -t rsa -f #{home}/.ssh/#{local_ip} -N '' -q")
            ssh.exec!("sed -i '/HOST #{local_ip}/,+1d' #{home}/.ssh/config")
            ssh.exec!("echo HOST #{local_ip} >> #{home}/.ssh/config")
            ssh.exec!("echo IdentityFile ~/.ssh/#{local_ip} >> #{home}/.ssh/config")
        end
    end

    def one_n(remote_ip, remote_user, remote_passwd, local_user, local_ip)
        puts "IP列表為SSH主機, 使用者: #{local_user}"
        hostname = `hostname`.chomp
        puts "產生ssh key..."
        ssh_keygen(local_ip)
        if check_remote(remote_ip)
            puts ">> #{remote_ip}連線正常"
            puts "   上傳key到 #{remote_ip}"
            upload_key(remote_ip, remote_user, remote_passwd, local_ip)
            puts "   #{remote_ip} 執行指令"
            one_n_script(remote_ip, remote_user, remote_passwd, local_user)
            `sed -i '/HOST #{remote_ip}/,+1d' ~/.ssh/config 2>/dev/null`
            `echo HOST #{remote_ip} >> #{ENV['HOME']}/.ssh/config`
            `echo IdentityFile #{ENV['HOME']}/.ssh/#{local_ip} >> #{ENV['HOME']}/.ssh/config`
            puts "   >> #{remote_user}@#{remote_ip}"
        else
            puts ">> #{remote_ip} 沒有回應"
        end
    end

    def upload_key(remote_ip, remote_user, remote_passwd, local_ip)
        Net::SCP.upload!(remote_ip, remote_user, "#{ENV['HOME']}/.ssh/#{local_ip}.pub", "id_rsa.pub",  :ssh => { :password => remote_passwd })
    end

    def one_n_script(remote_ip, remote_user, remote_passwd, local_user)
        Net::SSH.start(remote_ip, remote_user, :password => remote_passwd ) do |ssh|
            ssh.exec!("sed -i /#{local_user}@#{@hostname}/d ~/.ssh/authorized_keys")
            ssh.exec!("mkdir ~/.ssh")
            ssh.exec!("cat id_rsa.pub >> ~/.ssh/authorized_keys")
            ssh.exec!("chmod go-w ~")
            ssh.exec!("chmod 700 ~/.ssh")
            ssh.exec!("chmod 600 ~/.ssh/authorized_keys")
        end
    end

    def ssh_keygen(local_ip)
        if not File.exist?("#{ENV['HOME']}/.ssh/#{local_ip}")
            system("ssh-keygen -t rsa -f #{ENV['HOME']}/.ssh/#{local_ip} -N '' -q")
        end

    end

    def check_remote_os_method(remote_ip)
        if not Gem::Platform.local.os == 'linux'
            `ping -n 1 -w 1 #{remote_ip} | grep 64`
        else
            `ping -c 1 -w 1 #{remote_ip} | grep 64`
        end
    end

    def check_remote(remote_ip)
        #system("nc -nz -w 1 #{remote_ip} 22") == true
        if check_remote_os_method(remote_ip) == ""
            return false
        else
            return true
        end
    end
end
SSH.new

```