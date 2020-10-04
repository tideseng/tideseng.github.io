---
layout:     post
title:      Vagrant 搭建 Linux
subtitle:   
date:       2020-10-04
author:     tideseng
header-img: 
header-mask: 
catalog: true
tags:
    - linux
---

# VirtualBox

## 下载地址

> https://download.virtualbox.org/virtualbox/6.0.12/VirtualBox-6.0.12-133076-Win.exe

# Vagrant

## 下载地址

> https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.msi

# 搭建CentOS

## 下载镜像

> http://www.vagrantbox.es
>
> https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box

## 添加镜像

vagrant box add centos/7 D:\tool-file\vagrant-centos-7.box

vagrant box list

## Vagrantfile配置

### 生成

vagrant init

### 配置

- 镜像

  config.vm.box = "centos/7"

- ssh端口

  config.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: "true"
  config.vm.network "forwarded_port", guest: 22, host: 3333

- 网络

  config.vm.network "public_network"

- 配置（内存、cpu）

  config.vm.provider "virtualbox" do |vb|
          vb.memory = "3000"
          vb.name= "centos7"
          vb.cpus= 2
      end

## 启动虚拟机

vagrant up（cmd命令行启动，与Vagrantfile同一目录）

```
// 可能出现问题
1. default：Which interface should the network bridge to？
选择网卡，一般选1
```

## ssh配置

### 查看配置

```shell
D:\VirtualMachine\centos7>vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 3333
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile D:/VirtualMachine/centos7/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

### 密码登录

```shell
vagrant ssh
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo vi /etc/ssh/sshd_config
将PasswordAuthentication的值修改为yes
systemctl restart sshd
输入默认密码vagrant验证
# 退出ssh
exit
```

## vagrant登录

通过用户名vagrant进行ssh登录，密码是vagrant

## root登录

通过用户名root进行ssh登录，密码是vagrant

### 修改root密码

```shell
passwd
输入密码（123456）两次
```

## ifconfig命令安装

```shell
# 方式一（升级所有安装包并安装net-tools安装包【不推荐】）
yum upgrade
yum install net-tools
# 方式二（只安装ifconfig安装包【推荐，netstat也需要该安装包】）
yum install ifconfig
一般提示没有ifconfig安装包，需要手动查找
yum search ifconfig
yum install -y net-tools.x86_64
# 方式三（使用其他命令）
ip addr
```

# 搭建Ubuntu

## 下载镜像

> http://www.vagrantbox.es
>
> https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/bionic/current/bionic-server-cloudimg-amd64-vagrant.box

## 添加镜像

vagrant box add ubuntu16 D:\tool-file\vagrant-ubuntu-16.box

vagrant box list

## Vagrantfile配置

### 生成

vagrant init

### 配置

- 镜像

  config.vm.box = "ubuntu16"

- ssh端口（或不填）

  config.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: "true"
  config.vm.network "forwarded_port", guest: 22, host: 2222

- 网络

  config.vm.network "public_network"

- 配置（内存、cpu）

  config.vm.provider "virtualbox" do |vb|
          vb.memory = "3000"
          vb.name= "ubuntu16"
          vb.cpus= 2
      end

## 启动虚拟机

vagrant up

```
// 可能出现问题
1. default：Which interface should the network bridge to？
选择网卡，一般选1
```

## ssh配置

### 查看配置

```shell
D:\VirtualMachine\ubuntu16.4>vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile D:/VirtualMachine/ubuntu16.4/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

### 密码登录

```shell
vagrant ssh
# 设置ubuntu密码
sudo passwd ubuntu
输入密码（vagrant）两次
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo vi /etc/ssh/sshd_config
将PasswordAuthentication的值修改为yes
systemctl restart sshd
输入ubuntu密码vagrant验证
# 退出ssh
exit
```

## ubuntu登录

通过用户名ubuntu进行ssh登录，密码是上面设置的vagrant

## root登录

通过用户名ubuntu进行ssh登录，密码是上面设置的vagrant

### 修改root密码

```shell
# 修改root密码
sudo passwd root
输入密码（123456）两次
# 切换成root用户
su root
# 修改配置
vi /etc/ssh/sshd_config
将PermitRootLogin的值修改为yes
systemctl restart sshd
退出ssh连接
使用root登录
```

# 启动虚拟机

vagrant up（cmd命令行启动，与Vagrantfile同一目录）

# 关闭虚拟机

vagrant halt（cmd命令行启动，与Vagrantfile同一目录）

# 销毁虚拟机

vagrant destroy（cmd命令行启动，与Vagrantfile同一目录）

# 日期同步

```shell
# 查看当前时间
date
# 查看当前系统时间区
timedatectl (UTC时间区)
# 查看系统可选时间区
ls /usr/share/zoneinfo
ls /usr/share/zoneinfo/Asia
# 删除当前系统时间区
rm /etc/localtime
# 新建当前系统时间区
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# 查看当前系统时间区
timedatectl (CST时间区)
# 查看当前时间
date
```

# 主机与虚拟机互ping

## 主机ping通虚拟机

- 虚拟机中：ipconfig/ip addr查看虚拟机IP

- 主机中：ping 虚拟机IP

## 虚拟机ping通主机

- 主机中：ipconfig查看主机IP

- 主机中：关闭Windows防火墙

  控制面板 --> 查看网络状态和任务 --> Windows Defender 防火墙 --> 启用或关闭 Windows Defender 防火墙 --> 关闭专用网络和公用网络的防火墙

- 虚拟机中：ping 主机IP

# JDK安装

>  先创建/data/bin目录，将tar包上传到该目录下

## tar包解压

```shell
tar -zxvf jdk-8u11-linux-x64.tar.gz
```

## 环境配置

```shell
vi /etc/profile
# 在底部加入配置
export JAVA_HOME=/data/bin/jdk1.8.0_11
export PATH=$PATH:$JAVA_HOME/bin
source /etc/profile
java -version
```

# Tomcat安装

## tar包解压

```shell
tar -zxvf apache-tomcat-8.0.18.tar.gz
```

## 启动

```shell
./apache-tomcat-8.0.18/bin/startup.sh
```

## 无防火墙访问

主机浏览器地址中：虚拟机IP:8080（默认虚拟机防火墙处于关闭状态）

## CentOS防火墙访问

```shell
# 查看防火墙状态
firewall-cmd --state
# 防火墙未开启时进行开启
systemctl start firewalld.service
# 开放/关闭指定端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
# 重新载入配置立即生效
firewall-cmd --reload
# 查看是否生效
firewall-cmd --zone=public --query-port=8080/tcp
# 查看防火墙所有开放的端口
firewall-cmd --zone=public --list-ports
# 关闭防火墙
systemctl stop firewalld.service
```

主机浏览器地址中：虚拟机IP:8080

## Ubuntu防火墙访问

> 不建议开启，有可能导致虚拟机启动失败

```shell
# 查看防火墙状态
ufw status
# 防火墙未开启时进行开启
ufw enable
ufw default deny
# 开放/关闭指定端口
ufw allow 8080
ufw deny 8080
# 查看是否生效
ufw status
# 关闭防火墙
ufw disable
```

## 关闭

```shell
./apache-tomcat-8.0.18/bin/shutdown.sh
```

