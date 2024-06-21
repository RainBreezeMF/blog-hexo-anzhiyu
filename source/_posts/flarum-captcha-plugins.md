---
title: flarum论坛中的人机验证方案对比 & Turnstile 安装教程
date: 2023-06-24 14:54:51
description: 我们都不太喜欢人机验证工具，要么过验证烦，要么用户或者网站/服务器起码有一个卡顿。但总归不能完全抛弃，毕竟网站做大以后一些不想来的东西都会来了。
categories:
  - 技术探索
tags:
  - 论坛
  - flarum
  - 人机验证
---

{% note info flat %}本文内容的实际情况会因设备自身、网络连接状况等不同，仅供参考。{% endnote %}

# 前言

说起网站使用的人机验证（尤其是国外网站），大多数人第一反应是Google的 reCaptcha，或者 hCaptcha 之类的。除此之外其实还有个来自 Cloudflare 的 Turnstile

## 1. Google reCaptcha 

[![pCt0zCQ.png](https://s1.ax1x.com/2023/06/24/pCt0zCQ.png)](https://imgse.com/i/pCt0zCQ)

reCaptcha 发展多年，而一直困扰用户的便是它的图形验证码。虽然reCaptcha在中国大陆网络下的调用没什么问题，但关于其图形验证的“阴间”也在民间被拉出来“批判”了不知道多少次。

[![pCt0jUS.png](https://s1.ax1x.com/2023/06/24/pCt0jUS.png)](https://imgse.com/i/pCt0jUS)
[![pCt0XE8.png](https://s1.ax1x.com/2023/06/24/pCt0XE8.png)](https://imgse.com/i/pCt0XE8)

虽说它的难度是可以进行调节的（上为我自己站点的数据），但毕竟在多数人看来，验证肯定是便捷和验证目的双得最好，即使用者能尽量无感通过人机验证，同时尽可能减少机器人的影响。

注意，上述针对的实际上是 reCaptcha V2 的"复选框"验证模式，这也是相对更常用的模式。你也可以选择隐形徽章。（徽章模式虽然没选择框但徽章还挺大的说）
（放在插件里就是"Checkbox"和"Invisible"的选择了，但是在 reCaptcha 设置时就得选好是复选框还是徽章（创建后验证方式不可更改），对应功能的密钥对应在插件中的选项否则会报错）
[![pCt0v4g.png](https://s1.ax1x.com/2023/06/24/pCt0v4g.png)](https://imgse.com/i/pCt0v4g)
[![pCt0LHf.png](https://s1.ax1x.com/2023/06/24/pCt0LHf.png)](https://imgse.com/i/pCt0LHf)

实际上，如果在V3的 reCaptcha 验证机制下，这东西并不会打扰用户。因为其机制是评分，只需要管理员在后台查看分析状况即可。

那你可能会说，我直接用 V3 的不就完了？
答：插件不支持。
使用 fof/recaptcha 插件，这是往论坛里加入 reCaptcha 时多数站长的流程，但它只支持 V2 的验证方式，也就是复选框/徽章的方式。如果想用 V3 的话得脱开这个插件，按照 Google 给的文档自己去配置了，尤其对于小白并不算多友好。

## 2. hCaptcha

[![pCtBEUU.png](https://s1.ax1x.com/2023/06/24/pCtBEUU.png)](https://imgse.com/i/pCtBEUU)

hCaptcha 与 reCaptcha一样是有图形验证码和徽章两种模式，虽然对于 reCaptcha 来说难度配置和弹出验证的判断阈值可调节性比起 reCaptcha V2 要更细，但问题也有：卡顿 。 **（这个可能很少见）** 

不知道是不是我这网络问题，从我自己的使用来看，有些时候要么点击验证框按钮后一直转圈很长时间出不来，要么反复2-4轮才给通过。

## 3. Turnstile

TurnStile 是 Cloudflare 在2022年9月宣布的项目，其宣传的就是去掉人工验证来完成人机交互验证。

由于Cloudflare服务在国内网络环境下（多数时候，没被污染的情况下）畅通，因此turnstile一样不需要担心网络环境造成的卡顿问题。
从使用而言，除非被疑似为问题流量，不然点一次一般3秒内便显示通过。

turnstile在 flarum 插件下的可配置场景有注册、登录、和忘记密码。

---

怎么说呢，我个人还是倾向于 turnstile 的场景方案。前两者在账号方面仅在注册时加以验证，而配置Turnstile下已经做到基本无感验证的登录~~起码心理作用上~~比起前两者可能更好些。

Turnstile是一个新的人机验证“品牌”，比起已经发展了十余年的 reCaptcha、hCaptcha 来说比较短，自然也有对此能力不信任的空间。

这里我放个意见：倘若不是体量足够大，或者容易招来机器人的内容的论坛站，这几家其实都不错。至于选择谁，取决于你的偏好和长期运行中的稳定以及用户交互友好程度等等了。

# Turnstile 配置教程

## 获取 Turnstile 站点密钥

1. 前往 Cloudflare 相应页面：[跳转](https://dash.cloudflare.com/?to=/:account/turnstile)
如果没有 Cloudflare 账号，跟随注册流程注册即可

2. 来到 Turnstile 界面，选择“添加站点”
[![pCtB9vn.png](https://s1.ax1x.com/2023/06/24/pCtB9vn.png)](https://imgse.com/i/pCtB9vn)

3. Turnstile 信息配置

**“站点名称”**：这里仅相当于“备注”，随便填。
**“域”**：这里填写你要添加 Turnstile 的网站的域名（不用加http(s)），如果域名已在 CloudFlare 托管，可直接点击输入框右侧下拉箭头选择你的域名；若不是，则手动填入域名，并在完成后点击弹出的带有“（自定义域）“选项”。
**“小组件模式”**：这里提供三种验证模式，其中：
“托管”模式下会根据判断选择是否要求用户进行点击交互操作；“非交互式”模式下则自动进行验证；“不可见”模式下用户则完全不可见验证部件。

[![pCtBPuq.png](https://s1.ax1x.com/2023/06/24/pCtBPuq.png)](https://imgse.com/i/pCtBPuq)

设置好以后点击“创建”。随后弹出以下页面，上面的两串密钥在论坛插件设置内会用到，请妥善保存。
[![pCtBiD0.png](https://s1.ax1x.com/2023/06/24/pCtBiD0.png)](https://imgse.com/i/pCtBiD0)

## 插件配置

1. 在服务器的站点目录内，通过Composer输入以下命令安装 Turnstile 插件：

   ```php
   composer require blomstra/turnstile:"*"
   ```

   安装完成后，在论坛后台启用该插件：[![pCtBS3j.png](https://s1.ax1x.com/2023/06/24/pCtBS3j.png)](https://imgse.com/i/pCtBS3j)

2. 在插件设置页填写两组密钥（上面步骤中获得）
[![pCtBpgs.png](https://s1.ax1x.com/2023/06/24/pCtBpgs.png)](https://imgse.com/i/pCtBpgs)
“站点公钥”：对应上述密钥中的“站点密钥”。
“私钥”：对应上述密钥中的“密钥”。

配置完成后点击保存即可。

[![pCtBFbV.png](https://s1.ax1x.com/2023/06/24/pCtBFbV.png)](https://imgse.com/i/pCtBFbV)
[![pCtBAET.png](https://s1.ax1x.com/2023/06/24/pCtBAET.png)](https://imgse.com/i/pCtBAET)
