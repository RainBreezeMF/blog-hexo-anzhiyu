---
title: 早期捣鼓监控单页的尴尬史
date: 2023-04-06 16:21:14
description: 部署站点监控，结果自闹乌龙（笑）
categories:
  - 技术探索
tags:
  - 站点监控
cover: false
---

从别人那里看到一个站点监控的项目：https://github.com/KJZH001/uptimeStatusRevise ，正好我也有几个网站，放个监控页也不错

结果装完莫名出错了。不清楚它到底是受限在哪了。
信息站是vercel托管的hexo页面，能检测到，然后共享图仓是星辰云港区的服务器带着，倒检测不到了。

没懂其实，假如真是因为国内机器被q了倒也说得通，可那台机子是港区位置加回陆线。按通常理解似乎并不该是这种结果来着

---

2023.3.30 更新：
后来又看到了Uptime Kuma，整体不管后段还是状态显示都还不错。
不过这次总算是看见了些表面现象。

对于部署在vercel的站点（比如我自己的博客），用默认的HTPP(S)并没有报错，不过问题还是出现在我那台星辰云港区的机器上

在那台机子上带了三个站点，不过只有一个站点用http监控会发生这种事。

---

2023.3.31更新：
我说怎么回事，我把原本的shareimg.mufeng086.top的SSL证书用在现在的shareimg.takagi3.cn上了

纯粹是场乌龙🤣