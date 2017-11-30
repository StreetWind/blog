---
title: nmap 常用命令速查
layout: post
tags:
    - Linux
categories: 工具
abbrlink: 7914343816
date: 2017-11-30 14:47:00
update: 2017-11-30 15:12:00
layout:
comments:
---
## nmap 常用参数
#### 主机发现
```bash
-sL                     仅仅是显示,扫描的IP,不会进行任何扫描(域名解析IP之类)
-sn                     ping扫描,即主机发现
-Pn                     不检测主机存活
-PS/PA/PU/PY[portlist]  TCP SYN Ping/TCP ACK Ping/UDP Ping发现
-PE/PP/PM               使用ICMP echo, timestamp and netmask 请求包发现主机
-PO[prococol list]      使用IP协议包探测对方主机是否开启
-n/-R                   不对IP进行域名反向解析/为所有的IP都进行域名的反响解析
```
### 扫描技术
```bash
-sS/sT/sA/sW/sM                 TCP SYN/TCP connect()/ACK/TCP窗口扫描/TCP Maimon扫描
-sU                             UDP扫描
-sN/sF/sX                       TCP Null，FIN，and Xmas扫描
--scanflags                     自定义TCP包中的flags
-sI zombie host[:probeport]     高级僵尸扫描([参照](https://nmap.org/book/idlescan.html))
-sY/sZ                          SCTP INIT/COOKIE-ECHO 扫描
-sO                             使用IP protocol 扫描确定目标机支持的协议类型
-b “FTP relay host”             使用FTP bounce scan
```
### 指定端口和端口扫描顺序
```bash
-p                      特定的端口 -p 80,443 或者 -p 1-65535 全端口扫描 或者 -p T:80 U:8081 扫描TCP 80端口和UDP 8081端口
-F                      快速扫描模式,比默认的扫描端口还少
-r                      不随机扫描端口,默认是随机扫描的
--top-ports "number"    扫描开放概率最高的number个端口,出现的概率需要参考nmap-services文件,ubuntu中该文件位于/usr/share/nmap.nmap默认扫前1000个
--port-ratio "ratio"    扫描指定频率以上的端口
```
### 服务和版本探测
```bash
-sV                             开放版本探测,可以直接使用-A同时打开操作系统探测和版本探测
--version-intensity "level"     设置版本扫描强度,强度水平说明了应该使用哪些探测报文。数值越高，服务越有可能被正确识别。默认是7
--version-light                 打开轻量级模式,为--version-intensity 2的别名
--version-all                   尝试所有探测,为--version-intensity 9的别名
--version-trace                 显示出详细的版本侦测过程信息
-sR                             RPC扫描-sR很少被需要
```
### 系统识别
```bash
-O              启用操作系统检测,-A来同时启用操作系统检测和版本检测
--osscan-limit  针对指定的目标进行操作系统检测(至少需确知该主机分别有一个open和closed的端口)
--osscan-guess  推测操作系统检测结果,当Nmap无法确定所检测的操作系统时，会尽可能地提供最相近的匹配，Nmap默认进行这种匹配
```
### 时间和性能
```bash
-T                                                                  设置时间模板-T0-5 paranoid (0)、sneaky (1)、polite (2)、normal(3)默认模式、 aggressive (4)延时10ms和insane (5)延时5ms
--min-hostgroup <milliseconds>; --max-hostgroup <milliseconds>      调整并行扫描组大小 min-hostgroup默认5 
--min-parallelism <milliseconds>; --max-parallelism <milliseconds>  调整探测报文的并行度
--min-rtt-timeout ， --max-rtt-timeout ， --initial-rtt-timeout     调整探测报文超时已毫秒为单位
--host-timeout <milliseconds>                                       放弃低速目标主机
--scan-delay <milliseconds>; --max-scan-delay <milliseconds>        调整探测报文的时间间隔
```
### 防火墙/IDS躲避和哄骗
```bash
-f; --mtu value                 指定使用分片、指定数据包的MTU.
-D decoy1,decoy2,ME             使用诱饵隐蔽扫描
-S IP-ADDRESS                   源地址欺骗
-e interface                    使用指定的接口
-g/ --source-port PROTNUM       使用指定源端口
--proxies url1,[url2],...       使用HTTP或者SOCKS4的代理

--data-length NUM               填充随机数据让数据包长度达到NUM
--ip-options OPTIONS            使用指定的IP选项来发送数据包
--ttl VALUE                     设置IP time-to-live域
--spoof-mac ADDR/PREFIX/VEBDOR  MAC地址伪装
--badsum                        使用错误的checksum来发送数据包
```
### 输出
```bash
-oN                     将标准输出直接写入指定的文件
-oX                     输出xml文件
-oS                     将所有的输出都改为大写
-oG                     输出便于通过bash或者perl处理的格式,非xml
-oA BASENAME            可将扫描结果以标准格式、XML格式和Grep格式一次性输出
-v                      提高输出信息的详细度
-d level                设置debug级别,最高是9
--reason                显示端口处于带确认状态的原因
--open                  只输出端口状态为open的端口
--packet-trace          显示所有发送或者接收到的数据包
--iflist                显示路由信息和接口,便于调试
--log-errors            把日志等级为errors/warings的日志输出
--append-output         追加到指定的文件
--resume FILENAME       恢复已停止的扫描
--stylesheet PATH/URL   设置XSL样式表，转换XML输出
--webxml                从namp.org得到XML的样式
--no-sytlesheet         忽略XML声明的XSL样式表
```
### 其他选项
```bash
-6                      开启IPv6
-A                      OS识别,版本探测,脚本扫描和traceroute
--datedir DIRNAME       说明用户Nmap数据文件位置
--send-eth / --send-ip  使用原以太网帧发送/在原IP层发送
--privileged            假定用户具有全部权限
--unprovoleged          假定用户不具有全部权限,创建原始套接字需要root权限
-V                      打印版本信息
-h                      输出帮助
```
### 脚本
应为脚本很多没办法说明详细查看翻译的[nmap脚本引擎](https://t0data.gitbooks.io/nmap-nse/content/chapter1.html)
