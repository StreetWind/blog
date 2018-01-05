---
title: "VM安装Mac系统"
layout: "post"
categories:
  - 学习
tags:
  - 学习
keywords:
  - VM安装Mac系统
date: "2018-01-02 23:49"
updated: "2018-01-05 23:49"
abbrlink:
comments:
---
## VM安装MAC系统
各类环境测试需求，所以VM成了我电脑常客。最近测试需要苹果系统，可是手上也没有苹果电脑，谁叫我是穷逼啦！
VM没办法直接安装Mac系统，国外的大神做了一个VMware Unlock工具解锁VM对Mac系统的支持。
>不要问我VM怎么安装

我的系统环境：
系统：Windows 10专业版
VM版本:VMware Pro 14.1
Mac OS:10.13
### 下载解锁工具
> 保持最新解锁工具[下载](https://bit.ly/DownloadUnlocker)
> 上传百度网盘2.1.1版本[下载](https://pan.baidu.com/s/1pLj3knD) 密码：iapp
#### 解锁工具安装
解压后找到win-install文件右键以管理员身份运行
### VM安装Mac系统前设置
##### VM设置选项
>图片比较多，每步我都截图了，如果你VM使用熟练直接跳到最后两张图对CPU设置相关

![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2q27nadej30e90c8dgg.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2q5geqs2j30e90c8t8y.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qa00c6pj30e90c8q39.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qadabz3j30e90c8jrk.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qami9hnj30e90c874e.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qazgxmtj30e90c8mxh.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qbc1di0j30e90c8aac.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qbldol8j30e90c80sy.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qbv0p9jj30e90c8q33.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qc3ss7sj30e90c8q37.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qccgxyuj30e90c8mxh.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qcltif0j30e90c874f.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qcw1m1xj30e90c8aad.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qd5zukej30e90c874m.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qde7trqj30kp0h6q3k.jpg)
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2qdr0212j30kp0h6wf2.jpg)
最后一步是选择你下载的Mac OS系统文件
你要下载我安装的版本[Mac OS 10.13版本下载](https://drive.google.com/file/d/0B9Eqsl9MKX8peTFhM2RSVnJfNFU/view)(这文件在google云盘上不了google下载不了很正常)
网上找的10.13.2网盘 [网盘地址](https://pan.baidu.com/s/1qXPhme4) 密码:bav4
#### 修改VM配置
> 找到你设置的位置，在图四的那个位置。找到已.vmx后缀的文件
> 如：我VM命名为macOS 10.13所以文件就是macOS 10.13.vmx
> 右键以记事本本方式打开在最后加入一行smc.version = "0"保存
```
smc.version = "0"
```
### MacOS安装
1. 如图镜像引导成功
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2sloo4ngj30yk0oxwfc.jpg)
2. 格式化磁盘
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2tozndcxj30yk0oxk22.jpg)
2.1 格式步骤
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2tow1n9gj30yk0oxqa0.jpg)
3. 关闭磁盘工具后选择安装MacOS
4. 选择，安装MacOS后一直下一步，选择安装磁盘继续如果没有磁盘关闭后重复第二步格式磁盘。如图
![](https://ws1.sinaimg.cn/large/006Vguv0gy1fn2tue0xqrj30yk0oxjzh.jpg)
5. 等待安装完成。
