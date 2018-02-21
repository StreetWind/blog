---
title: manjaro 安装配置
tags: 学习
categories: '学习'
date: 2018-02-21 21:27:10
updated: 2018-02-21 21:57:17
layout:
comments:
---


## manjaro系统更新
#### 选择中国最快官方更新源
```
sudo pacman-mirrors -c China
sudo pacman -Syyu
```
#### 安装yaourt
```
sudo pacman -S base-devel yaourt
sudo pacman -S archlinuxcn-keyring
```
#### 安装常用软件
安装：vim，git，wget
```
sudo pacman -S vim git wget
```
默认的vim不要用，我长用的通用配置
```
bash <(curl -fsSL https://git.io/vFUhE)
```
#### 添加aur社区源
修改pacman配置文件/etc/pacman.conf
```
sudo vim /etc/pacman.conf
```
在最后加上下面配置
```
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```
##### 开启aria2c多线程加速下载
1. 安装aria2c
```
sudo pacman -S aria2c
```
2. 修改pacman配置文件/etc/pacman.conf
找到Xfercommand修改成如下
```
XferCommand  = /usr/bin/aria2c -x 8 -s 8 --dir $(dirname %o) -o $(basename %o) %u
```
3. 保存更新系统
```
yaourt -Syyua
```
#### 安装输入法

安装输入sogou输入法
```
yaourt -S fcitx-im fcitx-configtool fcitx-sogoupinyin
```
输入环境添加
```
vim ~/.xprofile
```
在文件中添加如下配置
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx
```
#### 安装QQ，微信，网易云音乐
```
yaourt -S deepin.com.qq.office deepin.com.wechat netease-cloud-music
```
我安装后 微信 QQ 输入法无法用，修改微信后QQ的启动脚步。
微信启动脚本：/opt/deepinwine/apps/Deepin-WeChat/run.sh
QQ启动脚本：/opt/deepinwine/apps/Deepin-TIM/run.sh
在脚本中添加如下配置：
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
