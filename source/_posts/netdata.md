---
title: "Linux系统监控工具netdata"
layout: "post"
categories:
  - Linux
tags:
  - Linux
  - 工具
keywords:
  - Linux系统监控
  - netdata使用配置
  - netdata放置公网
  - netdata
  - 外网配置
date: "2017-12-09 20:57"
updated: "2017-12-09 20:57"
abbrlink:
comments:
---
## Linux系统监控工具
今天推荐一个好看的监控工具netdata,大家可以看看我家的树莓派监控我已经放公网了[自己树莓派](https://rpi.sysblz.com)先上图

![](https://ws1.sinaimg.cn/large/006VAXiDgy1fmatlb4v7bj31fu0pxwk2.jpg)
### 下载安装
下载地址[项目地址](https://github.com/firehol/netdata)
```bash
git clone https://github.com/firehol/netdata.git
cd netdata
sudo ./netdata-install
```
### 配置外网访问
nginx这个安装我就不多说了，不会自己查nmp一件安装脚本一堆，不想脚本安装一个命令就安装了
### nginx配置
```bash
vim /etc/nginx/nginx.conf
```
我的配置文件

```
#均衡负载
upstream brckend {
	server 127.0.0.1:19999;
	keepalive 1024;
}

server {
#https配置我删了需要知道怎么开启https自己查或者留言问我
#这下面是http配置
    listen 80;
    server_name www.xxx.com;
    location / {
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Connection "keep-alive";
        proxy_set_header Host $http_host;
        proxy_pass http://brckend;
        proxy_http_version 1.1;
        proxy_pass_request_headers on;
        proxy_store off;
        gzip on;
        gzip_proxied any;
        gzip_types *;
    }

    #预防攻击
    if ($http_user_agent ~* "WordPress") {
      return 403;
    }

    if ($request_method !~ ^(GET|HEAD|OPTIONS)$ ){
      return 403;
    }
    #禁止爬虫
    if ($http_user_agent ~* "Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Sogou spider|Sogou web spider")
    {
      return 403;
    }
```
### netdata配置
```bash
vim /etc/netdata/netdata.conf

#配置文件中在global下面添加这些
[global]
    bind socket to IP = 127.0.0.1
    access log = none
    disconnect idle web clients after seconds = 3600
    enable web responses gzip compression = no
```

### 增加打开的文件限制
这里系统分是否是systemd类型，想基于Debian系列发布版本的都不是systemd像基于RPM发布版本都是systemd如
#### 不是systemd
给这两个文件添加内容
1. /etc/security/limits.d/nginx.conf
```bash
nginx   soft    nofile  10000
nginx   hard    nofile  30000
```
2. /etc/security/limits.d/netdata.conf
```
netdata   soft    nofile  10000
netdata   hard    nofile  30000
```
刷新系统
```
sysctl -p
```
#### 是systemd系统
创建两个文件
```
mkdir -p /etc/systemd/system/netdata.service.d
mkdir -p /etc/systemd/system/nginx.service.d
```
像这两个文件添加内容
```
[Service]
LimitNOFILE=30000
```
启动服务
```
systemctl daemon-reload
systemctl restart netdata.service
systemctl restart nginx.service
```
检查是否有效
```
cat /proc/$(ps aux | grep "nginx: master process" | grep -v grep | awk '{print $2}')/limits | grep "Max open files"
cat /proc/$(ps aux | grep "netdata" | head -n1 | grep -v grep | awk '{print $2}')/limits | grep "Max open files"
```

#### 配置nginx统计插件
niginx 配置文件下配置

```
location /stub_status
	    {
        	stub_status on;
	        access_log off;
	        #allow 127.0.0.1;
	        #deny all;
	    }
```
如果配置指定域名连接那更改这个文件
1. /etc/netdata/python.d/nginx.conf

url地址改成你的域名如图
![](https://ws1.sinaimg.cn/large/006VAXiDgy1fmb15ylfwuj30g507kt8u.jpg)

### 配置web日志
日志很简单只是没有权限而已
```
chmod 775 /var/log/nginx/access.log
```
如果你自定义类日志文件请去下面文件更改
1. /etc/netdata/python.d/web_log.conf
