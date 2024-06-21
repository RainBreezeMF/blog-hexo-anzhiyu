---
title: 通过免费腾讯企业邮使用flarum邮件服务
date: 2023-05-03 20:49:25
categories:
  - 技术探索
tags:
  - 论坛
  - 教程
  - 邮箱
  - flarum
  - smtp
---

# 前言

Flarum论坛的邮件服务主要用于新用户注册后激活链接邮件的发送。从这点来说，哪怕自己用的QQ邮箱其实也可以（[现成教程](https://discuss.flarum.org.cn/d/2401)）

不过这种方式有个好处，相比于朴实无华的 @qq.com 后缀，如果是以你自己的域名（比如我的“takagi3.cn”）作为邮箱后缀的话，用户怎么说也能感觉到一点...“正式”。并且，如果用自己邮箱的话也会导致私人事务和网站中与人沟通两类邮件混杂的问题，“分离”处理也是提高自己的效率嘛。

“我懂我懂，不就域名邮箱吗？”

但腾讯企业邮是可以免费开的啊（需求量不算特别大的话）![腾讯企业邮开通界面](https://img.aurorainic.top/i/2023/07/25/fbd9nx.png)

在腾讯自家的dnspod解析的话，也能一键直达（当然我这是已经开通过的）![Image description](https://img.aurorainic.top/i/2023/07/25/fc6nyw.png)


# 正题 - 教程

## 1. 企业邮开通

前往腾讯企业邮开通页面：https://buy.cloud.tencent.com/exmail?specId=gs-1ctj0

一般选基础版本即可。这里时长可选最高三年，你暂且也别担心，大不了三年后再说。~~毕竟自己的站点很多也不保证能稳稳运行三年。~~

![Image description](https://img.aurorainic.top/i/2023/07/25/fcjmop.png)

如果你的域名是用dnspod解析，在下方可以直接选择你要用的域名，解析控制台会自动添加该域名的MX邮件服务器解析记录。
![Image description](https://img.aurorainic.top/i/2023/07/25/fcozls.png)


如果不在dnspod解析，需要在域名解析商手动添加解析记录：

- 主机记录，一般情况下填入 "@"即可
- 记录类型：MX
- 记录值：mxbiz1.qq.com，mxbiz2.qq.com（分别添加两条记录）  

----------

- 主机记录：一般情况下填入 "@"即可
- 记录类型：TXT
- 记录值：v=spf1 include:spf.mail.qq.com ~all  

----------

- 主机记录：mail
- 记录类型：CNAME
- 记录值：exmail.qq.com  

---

开通成功后在该邮箱详情中选择邮箱激活
![Image description](https://img.aurorainic.top/i/2023/07/25/fdrwt7.png)

---

（账号授权，一路同意跟着走就行）
![Image description](https://img.aurorainic.top/i/2023/07/25/fdwvcq.png)

---

这里是会新开通一个企业微信，按照要求进行管理员信息验证、微信扫码绑定账号即可
![Image description](https://img.aurorainic.top/i/2023/07/25/fdzyw3.png)

---

## 2. 邮箱配置

上述步骤以后，邮箱就开好了。但这只是解决了 @example.com 的问题，那 "@" 前面的部分怎么改呢？比如 feedback@example.com？

### 2.1 编辑邮箱地址

进入企业微信后台：https://work.weixin.qq.com/

然后进入以下界面：
![Image description](https://img.aurorainic.top/i/2023/07/25/fec173.png)

![Image description](https://img.aurorainic.top/i/2023/07/25/fefgjp.png)

![Image description](https://img.aurorainic.top/i/2023/07/25/fekmcu.png)

---

在此处修改好STMP服务的范围，可以全选以免麻烦。
![Image description](https://img.aurorainic.top/i/2023/07/25/fewc46.png)

---

### 2.2 邮箱授权码获取

进入腾讯企业邮后台页面：https://exmail.qq.com/login （不管是登录还是里面的界面像网页版的普通QQ邮箱就对了），账密或扫码登录。

这里更推荐使用你需要使用的邮箱地址加先前设置的密码登录企业邮，因为扫码登录会涉及到管理员+你个人两个“企业内”账户的邮箱选择问题，可能就会跳到企业微信的后台了。
![Image description](https://img.aurorainic.top/i/2023/07/25/fez6hl.png)

登录后扫码验证即可。

---

进入邮箱页面后，进入“邮箱绑定”，需要绑定你的微信号，按要求操作。
![Image description](https://img.aurorainic.top/i/2023/07/25/ff1yb0.png)

完成后安全设置内点击“开启安全登录“（这里已经开通过了所以不一样）

之后点击“生成新密码”，妥善保存。

---

## 3. Flarum邮件配置
![Image description](https://img.aurorainic.top/i/2023/07/25/ff4mrk.png)


进入论坛后台管理，左侧选择“邮箱”。

- 地址：填入你设置好的邮箱地址

- 驱动：SMTP

- 主机：smtp.exmail.qq.com

- 端口：465

- 加密协议：ssl（一定小写）

- 用户名：同“地址”

- 密码：即在上一步中获取的授权码

---

保存，完成！

可以在下方点击“发送测试邮件”，测试设置生效结果。
![Image description](https://img.aurorainic.top/i/2023/07/25/ff7zdj.png)

---

**可选配置：DKIM验证**
可能各位碰到过自己论坛发出的邮件被收件人误判为垃圾邮件从而“收不到”的情况。
“为域名配置DKIM签名，可以有效防范仿冒和网络钓鱼邮件，或被收信方标记为垃圾邮件”（企业微信后台管理对此的描述）

![Image description](https://img.aurorainic.top/i/2023/07/25/ffm875.png)

在**企业微信**管理后台的邮箱页-设置内下方：DKIM处选择配置，并在解析服务内添加对应的TXT验证解析即可。
