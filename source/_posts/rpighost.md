---
title: 树莓派3安装Ghost博客
tags: 'rpi;ghost'
categories: 树莓派
abbrlink: 4121126904
date: 2017-08-16 00:28:31
updated: 2017-08-16 00:28:31
layout:
comments:
---
现在Ghost博客升级很快，1.0版本以上用Ghost-cli方法安装，本教程是树莓派安装1.5.2版本
## 1. 安装前准备
### 1.1 系统版本检查
最新版本Ghost需要在Ubuntu 16 上安装，如果你使用的是Raspbian系统请替换为Ubutun系统。
树莓派Ubutun系统请[参考该文章安装](https://ubuntu-mate.org/raspberry-pi/)
### 1.3 软件依赖
~~~ bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install nginx
sudo apt-get install mariadb-server
sudo ufw allow 'Nginx Full'
~~~
mariadb 其实就是mysql只是我mysql被收购不开源了，mariadb是mysql创造者把mysql卖给oracle后的继续开源mysql
安装mariadb时要求输入root密码
ufw allow 'Nginx Full' 是允许HTTP和HTTPS访问

### 1.4 安装Node.js
安装最新Nodejs，我安装时候Nodejs版本为(v6.11.1)
~~~
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash
sudo apt-get install -y nodejs
~~~
### 1.5 安装Ghost-Cli工具
安装Ghost命令客户端
~~~
sudo npm i -g ghost-cli
~~~

### 1.6 用户准备
#### 添加用户（如果不准备添加忽略）
~~~
adduser <username>
usermod -aG sudo <username>
su - <username>
~~~
username 请添加你要添加的用户名

#### 准备博客项目存放位置
~~~
sudo mkdir -p /var/www/ghost
sudo chown [user]:[user] /var/www/ghost
cd /var/www/ghost
~~~
## 2. 安装Ghost
~~~
ghost install
~~~

等待下载最新版本Ghost，我安装的时候是1.5.2。安装过程中各种询问
1. 您博客的网址
2. 您的MySQL用户名（root默认情况下）
3. 您的MySQL密码（无论您在安装MySQL服务器时设置为什么）
4. Ghost数据库名称（ghost_production默认情况下）
5. 无论你想要一个ghost用户
6. 是否要配置Nginx
7. 是否要设置SSL
8. 安装系统，所以你可以轻松地启动和停止Ghost
9. 是否要启动Ghost
![](https://ws1.sinaimg.cn/large/006VAXiDgy1filybxi2sgj30n90dbdpj.jpg)
