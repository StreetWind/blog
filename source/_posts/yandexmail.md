---
title: "自定义域名企业邮箱配置"
layout: "post"
tags:
  - 其他
categories: 其他
keywords:
  - 自定义域名邮箱
  - YandexMail
date: "2017-12-08 08:56"
updated: "2017-12-08 08:56"
abbrlink: 231249583
comments:
---
## 自定义域名企业邮箱配置
>   因为，我有自己的域名就想用自己的域名做邮箱。结果国内腾讯，阿里之类的免费企业邮箱自定义域名现在都要备案。<br/>我域名没有备案搜索了一圈国外企业邮箱[zoho](https://www.zoho.com/mail/),和[YandexMail](https://mail.yandex.com/)
### YandexMail邮箱
 YandexMail邮箱支持自定义域名，是俄罗斯最大搜索引擎公司提供的。
 优点：
 1. 支持1000个自定义用户
 2. 每个用户10G空间
 3. 支持POP3,IMAP,SMTP协议客服端

 缺点：
 1.Foxmail发送失败s收件无问题
 用来几天感觉没有太大问题，我用win10自带邮件系统。
 ![](https://ws1.sinaimg.cn/large/006VAXiDgy1fm94qr1ia5j30vq0acq38.jpg)

 #### 验证域名
 地址：[验证地址](https://domain.yandex.com/domains_add/)
 ![](https://ws1.sinaimg.cn/large/006VAXiDgy1fm97imgiatj30y50dhmyt.jpg)

 如果未注册，会弹出注册,按照步骤严重域名所有权添加MX记录
 #### 添加邮件用户
 ![](https://ws1.sinaimg.cn/large/006VAXiDgy1fm97vb79jwj311r0afgnb.jpg)

 注意DNS配置，如果你不把域名提交给他管理要自己去更改MX,TXT记录
 ### 修改DNS记录
 ![](https://ws1.sinaimg.cn/large/006VAXiDgy1fm97z8kh45j30u80mpq4z.jpg)

 注意TXT记录一定要添加不然你的邮件可能被其他邮件公司认为是垃圾邮件
 ### IMAP，POP3，SMTP 地址
 |协议|地址|SSL|端口|
 |-|-|-|-|-|
|IMAP   | imap.yandex.com  |  YES | 993  |
|POP3   | pop.yandex.com  |  YES |  995 |
|SMTP   | smtp.yandex.com  | YES  | 465  |
### zoho
[注册地址](https://workplace.zoho.com/orgsignup.do?plan=free)
优点：
1. 支持25个自定义用户+25推荐后获得的用户
2. 每个用户20G的空间
3. 支持最多20M附件
4. 支持smtp发送信件

缺点：
1. 免费用户不支持IMAP,POP3

![](https://ws1.sinaimg.cn/large/006VAXiDgy1fm94ocpkq9j31590do40i.jpg)
