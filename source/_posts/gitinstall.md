---
layout: 
title: git 常用配置
date: 2017-08-24 08:22:28
updated: 2017-08-24 08:22:28
comments: 
tags: 
    -git
    -Linux
categories: 工具
---
## arch Linux中Git命令自动补全
刚从基于Debian系统转到基于arch Linux系统，结果用Git命令的时候发现Git命令自动没办法使用。查看wiki文件两种解决办法
1. 环境变量法
``` bash
vim ~/bashrc
#添加一句
[ -f /usr/share/git/completion/git-completion.bash ] && source /usr/share/git/completion/git-completion.bash
#刷新环境变量
source ~/bashrc
```
2. 安装bash-completlon
```bash
yaourt -S bash-completion
```
参考：[archlinux wiki](https://wiki.archlinux.org/index.php/git)

顺便提一句防止自己忘记，git配置分级别：
1. 系统级别--system 写入/etc/gitconfig
2. 全局级别--global 写入~/.gitconfig或~/.config/git/config
3. 项目级别--local  写入仓库.git/config (平时写的时候直接省略直接写git config)

配置分级优先级:项目级别-->全局级别-->系统级别

## 初始Git配置
```bash
#设置全局用户信息
git config  --global user.name wind
git config  --global user.email @gmail.com
#也可以单独给项目设置用户去掉--global全局参数，参照
git config user.name

#编辑工具
git config  --global core.editor=vim
git config  --global merge.tool=vimdiff
```
## Git认证，经常使用github经常输入密码太麻烦
Git认证有两种模式一种HTTPS和SSH

#### https认证设置

```bash
git config --global credential.helper cache #设置认证储存方式cache(15分钟缓存)，store(明文储存)
```
#### SSH认证
1. 生成ssh公私密匙
```bash
ssh-keygen -t rsa -b 4096 -c “@gmail.com”
```
当前目录下会有刚刚要求你输入的id_rsa(私)和id_rsa.pub(公)
2. 上传公匙
公匙上传到github 【Settings】-->【SSH and GPG keys】-->【New SSH Key】

![](https://ws1.sinaimg.cn/large/006VAXiDgy1fiuoz5sl3oj30wx0j4dhv.jpg)

3. PC储存私匙到系统
```bash
ssh-add id_rsa
```
4. 验证公私密匙
```bash
ssh -T git@github.com
#看到下面这一句说明验证成功
#Hi leolee! You've successfully authenticated, but GitHub does not provide shell access.
```
参考:
    [深入浅出Git权限校验](http://debugtalk.com/post/head-first-git-authority-verification/)
    [云服务器之密钥认证和git hook配置](http://www.jianshu.com/p/9fcd5522e86b)

