---
title: 树莓派3下arch Linux arm 一些优化
layout: post
tags:
  - rpi
  - arch
categories: 树莓派
abbrlink: 13694117
date: 2017-08-31 03:09:05
updated: 2017-08-31 03:09:05
---

# 树莓派安装arch Linux 系统优化
没有显示器和键盘的自己找路由器里面的树莓派IP ssh登录
默认ssh用户：alarm
默认ssh密码：alarm
默认root用户密码也是root
## 0. 修改中科大源
修改为中科大的源
```bash
nano /etc/pacman.d/mirrorlist
Server = http://mirrors.ustc.edu.cn/archlinuxarm/$arch/$repo
```

## 1. 更新系统
```bash
pacman -Syu
```
## 2. 添加普通用户root权限sudo
```bash
su #提示输入root密码
pacman -S sudo
nano /etc/sudoers
```
取消这两行注释，或者在最后添加这两行
```bash
%wheel ALL=(ALL) ALL

%wheel ALL=(ALL) NOPASSWD: ALL
```

## 3. 修改默认用户和root默认密码
```bash
sudo passwd alarm
sudo passwd root
```
## 4. 添加自己好记的普通用户
```bash
sudo useradd -d /home/mofeng -m mofeng
sudo passwd mofeng
```
## 5. 开启ssh登录的root权限
```bash
nano /etc/ssh/sshd_config
PermitRootLogin yes
```
## 6. 修改树莓派名称
```bash
nano /etc/hostname
```
## 7. 安装常用工具

wget git curl vim 这几个我经常用到所以先安装了
```bash
pacman -S wget git curl vim
```
## 8. 安装aru支持（yaourt）工具

yaourt 编译的时候的环境可能缺少所以先安装
```bash
pacman -S base-devel
```
编译yaourt相关依赖[参照文件](http://doku.ben00it.fr/dokuwiki/doku.php?id=linux:raspberry:yaourt)

```bash
pacman -S gcc cmake fakeroot pkg-config
```
安装yajl，有能力的自己编译安装，参照文件有写为什么要这样做我简单化不去多做解释

```bash
cd /tmp/
mkdir yajl
cd yajl
vim PKGBUILD
makepkg -Acs
sudo pacman -U yajl-2.1.0-1-armv7h.pkg.tar.xz
```
PKGBUILD文件内容
```
# Maintainer: Dave Reisner <d@falconindy.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: Andrej Gelenberg <andrej.gelenberg@udo.edu>

pkgname=yajl
pkgver=2.1.0
pkgrel=1
pkgdesc='Yet Another JSON Library'
arch=('i686' 'x86_64' 'armv7' 'armv7h' 'armv7l')
url='http://lloyd.github.com/yajl/'
license=('ISC')
makedepends=('gcc' 'make' 'cmake')
source=("$pkgname-$pkgver.tar.gz::https://github.com/lloyd/$pkgname/archive/$pkgver.tar.gz")
md5sums=('6887e0ed7479d2549761a4d284d3ecb0')
build() {
  cd "$pkgname-$pkgver"
  cmake -DCMAKE_INSTALL_PREFIX=/usr .
  make
}
package() {
  cd "$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
}
```
package-query安装

```bash
cd /tmp
git clone https://aur.archlinux.org/package-query-git.git
cd package-query-git
makepkg -Acs
pacman -U package-query-*.xz
```
yaourt 安装
```bash
cd /tmp/
git clone https://aur.archlinux.org/yaourt.git
makepkg -Acs
sudo pacman -U yaourt*.tar.xz
```
## 9. 网络工具包

ifconfig,route在net-tools中，nslookup,dig在dnsutils中，ftp,telnet等在inetutils中,ip命令在iproute2中

```bash
pacman -S net-tools dnsutils inetutils iproute2
```



## 10. /boot/config.txt优化

#### 10.1 音频

```bash
pacman -S alsa-utils

# 配置文件中添加
dtparam=audio=on
hdmi_drive=2 #HDMI输出音频
audio_pwm_mode=2 #3.5mm口输出音频
```

#### 10.2 开启GPIO
如果你你们安装yaourt要不你自己手工安装，或者在去aur查找
```bash
yaourt -S python-raspberry-gpio
```

#### 10.3 开启I2C

安装i2c-tools和lm_sensors包

```bash
yaourt -S i2c-tools lm_sensors
```
/boot/config.txt中开启i2c的支持

```
dtparam=i2c_arm=on
```

[无桌面参考](https://rocka.me/archive/my-first-archlinux-rpi3)
[配置桌面环境参考](https://zhuanlan.zhihu.com/p/25988799)
[arch中添加rc.locl启动](https://www.yumao.name/read/archlinux-systemd-rc-local/)
