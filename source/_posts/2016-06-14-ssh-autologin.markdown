---
layout: post
title: "ssh自動登入"
date: 2016-06-14 12:50:31 +0800
comments: true
categories: ruby
---
``` ruby
# encoding: utf-8
require "net/ssh"
require "net/scp"
class SSH

    def initialize
        local_ip, local_user = set_local
        set_remote.each do |remote_ip, remote_user, remote_passwd|
            one_n(remote_ip, remote_user, remote_passwd, local_user, local_ip)
            n_one(remote_ip, remote_user, remote_passwd, local_user, local_ip)
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
            one_n_script(remote_ip, remote_user, remote_passwd, local_user, hostname)
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

    def one_n_script(remote_ip, remote_user, remote_passwd, local_user, hostname)
        Net::SSH.start(remote_ip, remote_user, :password => remote_passwd ) do |ssh|
            ssh.exec!("mkdir ~/.ssh")
            ssh.exec!("touch ~/.ssh/authorized_keys")
            ssh.exec!("sed -i /#{local_user}@#{hostname}/d ~/.ssh/authorized_keys")
            ssh.exec!("cat id_rsa.pub >> ~/.ssh/authorized_keys")
            ssh.exec!("chmod 700 ~/.ssh")
            ssh.exec!("chmod 400 ~/.ssh/authorized_keys")
        end
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

    def set_remote
        if ARGV.count == 3
            return [ARGV]
        elsif ARGV[0] == "group"
            [["192.168.0.123", "root", "4567"]]
        else
            puts "Remote_ip, Remote_user, Remote_passwd"
            exit
        end
    end

    def set_local
        ENV['SSH_AUTH_SOCK'] = `ssh-agent`.split("\n")[0].scan(/SSH_AUTH_SOCK=(.*); e/).join
        ips = `ifconfig`.scan(/inet (.*)\s+net/).join.split(" ")
        if ips.size == 1
            local_ip = ips[0]
        else
            ips.each_with_index do |l, i|
                puts "#{i+1}. #{l}"
            end
            print ">> "
            choice = STDIN.gets.chomp.to_i-1
            local_ip = ips[choice]
        end
        local_user = ENV['USER']
        return local_ip, local_user
    end


    def ssh_keygen(local_ip)
        path = "#{ENV['HOME']}/.ssh/#{local_ip}"
        if not File.exist?(path)
            system("ssh-keygen -t rsa -f #{path} -N '' -q")
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