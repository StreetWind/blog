---
title: title
layout: post
date: 2017-08-29 17:48:52
updated: 2017-08-30 22:21:00
tags:
  - Linux
  - rpi
categories: 树莓派
---
> 很早以前的树莓派B板，前两天升级内核果断的停止工作了，可是我一直拿他做数据库服务器。我的数据全没有了。
> 所以逼得没办法只能qemu模拟的方式启动树莓派系统。现在终于把数据库备份出来了。数据无价请做好备份！！哎！
> 这次qemu方式加载中一个学习记录记住万一那天我又用上了不用在到处找了

#Arch Linux Install QEMU
安装QEMU
```
yaourt -S qemu
#因为我们要运行arm架构所以安装qemu-arch-extra 包
yaourt -S qemu-arch-extra 
```
#环境准备
#### 1. raspbian 镜像准备
[官网下载](https://www.raspberrypi.org/downloads/)
[下载库](https://downloads.raspberrypi.org/)
我下载的是最小树莓派系统：[2017-08-16-raspbian-stretch-lite.zip](https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-08-17/2017-08-16-raspbian-stretch-lite.zip)
#### 2. 内核准备
###### 2.1. 自己编译内核(QEMU启动失败，不过保留方法以后替换内核懒得查找)
>os rasbian 使用的内核版本uanem -r查看内部版本号
内核下载:
1. [raspberrypi linux](https://github.com/raspberrypi/linux)(git cloen https://github.com/raspberrypi/linux.git)
2. [linux-kernel 归档](https://www.kernel.org/)能找到最新稳定版本内核
3. [Linux-kernel-4.4.84](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.4.84.tar.xz)

###### 2.1.1 下载内核文件
>我SD的OS是raspberrypi所以我使用第一个内核文件
```
#下载4.12.9版本内核
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.4.84.tar.xz
xz -d linux-4.4.84.tar.xz && tar -xvf linux-4.4.84.tar
rm linux-4.4.84.tar
cd linux-4.4.84/
#raspberrypi内核
git cloen https://github.com/raspberrypi/linux.git)
cd linux
```

###### 2.1.2 编译arm内核
>编译内核
```
#设置编译环境变量
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
####如果你缺少arm-linux-gnueabi-gcc编译工具请安装
yaourt -S arm-linux-gnueabihf-gcc
####如果你嫌弃安装麻烦
git clone https://github.com/raspberrypi/tools.git
export PATH=$PATH:(你glone下来的路径)arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin
###如我是64位的系统
export PATH=/home/mofeng/work/linux-kernel/raspberrypi-tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin
### 检查编译工具链是否完整
arm-linux-gnueabihf-gcc -v
###我选用默认配置
make -j4  defconfig
###改变配置
make -j4 menuconfig
##构建内核
make -j4  #我PC的CPU核心是4核所以我-j4来加速编译
##复制内核到上一级
cp arch/arm/boot/Image ../kernel-rpi.img
```

###### 2.2 别人编译好的qemu-kernel
这个是别人编译好的在[github](https://github.com/dhruvvyas90/qemu-rpi-kernel)
```
git clone https://github.com/dhruvvyas90/qemu-rpi-kernel.git
cd qemu-rpi-kernel
```
文件里面有kernel-qemu-4.4.34-jessie编译好的内核，如果你还是想自己生成tool目录下有./build-kernel-qemu生成脚步

下面我们使用4.4.34版本内核启动

```
qemu-system-arm -kernel kernel-qemu-4.4.34-jessie -cpu arm1176 -m 256 -M versatilepb -no-reboot -serial stdio -append "root=/dev/sda2 panic=1 rootfstype=ext4 rw" -redir tcp:5022::22 -hda /dev/sdb
```
> 参数解释
> -kernel 内核文件
> -cpu arm运行cpu可以qemu-arm -cpu help
> -m 是内存大小
> -M 支持的机器使用命令qemu-system-arm -M help查看 [wiki](https://wiki.qemu.org/index.php/Documentation/Platforms/ARM)
> -append 告诉内核参数
> -hda 指定硬盘，也可以img文件

