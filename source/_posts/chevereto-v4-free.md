---
title: 使用1Panel面板安装Chevereto V4开心版教程
date: 2023-11-05 12:36:19
description: 在1Panel面板上安装Chevereto V4
categories:
  - 技术探索
tags:
  - 教程
  - 图床
  - chevereto
cover: https://img.aurorainic.top/i/2023/11/05/p2px1v.webp
---

{% note info flat %}本文涉及内容不一定完全通用于任何情况，仅表述其中一种可行配置。相关资源位于文末{% endnote %}

{% note success flat %}本文中实例所用服务器的系统：CentOS 7.9{% endnote %}

Chevereto作为开源图床程序~~（我知道它有商业授权版）~~中较为知名的一个，为很多人做各种用途（自用存图、做公益图床等等）。不过目前的安装教程大多都是V3版，同时针对新手小白的教程也基本都是宝塔面板。
虽说宝塔面板的“历史”比较久，不过切身可感的缺点也是有的，因此也有越来越多的人选择了今天的主角:飞致云旗下的 **1Panel** 面板。

> Q：1Panel 与 宝塔 的区别？[^1]
> A1：1Panel 为完全容器化管理，环境完全隔离，且简洁开源，技术新。
> A2：宝塔 为直接在宿主机安装程序，环境混合，且闭源收费，问题饱受诟病。

[^1]: 论点引用：https://discuss.flarum.org.cn/d/4354

---

# 0. 1Panel 面板

同宝塔一样，最好在一个新的/刚重装的系统来安装此管理面板。

1Panel 官方提供了一键安装脚本，根据自己服务器的系统，在终端内输入运行对应的脚本即可。
地址：https://1panel.cn/docs/installation/online_installation
比如我的系统是 CentOS ，那么就是

```bash
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && sh quick_start.sh
```

{% note warning flat %}如果您图省事，服务器选的云服务商自己“优化”的linux系统，有可能因为安装脚本无法识别是哪个现有系统而安装失败。（如阿里云的Alibaba Cloud）{% endnote %}


由于 1Panel 采用容器化管理，因此服务器如果没有docker相关程序会自动进行安装。
按终端命令行提示操作并等待安装完成后通过面板安全入口输入账密即可进入 1Panel 面板（记得现在服务器安全组放行面板对应端口）
**请妥善保存好您的面板安全入口与面板账号密码。**

![](https://img.aurorainic.top/i/2023/11/05/lqiy3z.webp)

# 1. 安装基础应用

进入面板后，我们需要安装运行网站必需的几部分。

**左侧功能栏第二个就是应用商店**：

![image](https://img.aurorainic.top/i/2023/11/05/lsxq25.webp)



## 1.0 端口开放

这里有两种方式：外部、内部

外部即云服务商管理面板中的“服务器安全组”，进入相关页面添加需要放行的端口即可。

内部则要进入 1Panel 面板的 **主机-防火墙**

![image](https://img.aurorainic.top/i/2023/11/05/ly8ahf.webp)

部分服务器或系统新装/重装时已经安装了firewalld，若尚未安装可通过终端安装。对于 CentOS：

```bash
yum install firewalld
```

安装好防火墙组件后，回到这个页面点击启用。

![image-20231105132810020](C:\Users\Mu Feng\AppData\Roaming\Typora\typora-user-images\image-20231105132810020.png)

这样就可以在管理页面添加需要放行的端口了，同时对于应用商店内安装的程序，当端口被占用时会显示占用该端口的程序是什么。

![image-20231105133119200](C:\Users\Mu Feng\AppData\Roaming\Typora\typora-user-images\image-20231105133119200.png)

## 1.1 OpenResty

有人可能会奇怪：我在宝塔面板见到的都是 Nginx/Apache2 ，OpenResty是什么东西？

实际上 OpenResty 是兼容 Nginx 的，在 1Panel 面板下的程序实际上都是 docker 容器化独立运行的，同时只有同在容器环境内的程序可以相互联通。1Panel 官方主推的便是容器化下的 OpenResty 来作为 Web 和反代服务器。

**找到OpenResty并点击“安装**

![OpenResty安装](https://img.aurorainic.top/i/2023/11/05/luoack.webp)



**一般情况下保持默认即可，点击下方“确认”**

![image](https://img.aurorainic.top/i/2023/11/05/lvud6s.webp)

## 1.2 数据库

数据库方面，Chevereto 支持 MySQL 5.x 及 8.0。而 MariaDB（同源但效率更高） 与 MySQL 5.7 高度互相兼容，因此也可以使用 MariaDB。本文以 MySQL 5.7 示例。

**进入安装页面**

![image](https://img.aurorainic.top/i/2023/11/05/np80v2.webp)

版本选择有5.6, 5.7, 8.0。按需选择即可，这里我们选择 5.7

root用户密码这里随机生成，也可自主填写。

**应用端口默认是没有对外开放的，可在右侧列表下滑至底部，勾选“端口外部访问”，安装时将自动将对应端口添加至面板的防火墙中**

**[ ! ] 如果服务器安全组是在云服务商面板来设置，那么这里开端口就没什么用了。**

信息确认好后点击“确认”即可。

## 1.3 PHP

Chevereto 的 PHP 版本要求 8.0 及以上。

由于 1Panel 采用容器化管理机制，因此这里安装 PHP 与 一般情况不同。

**进入 网站-运行环境**

![image](https://img.aurorainic.top/i/2023/11/05/m8x3ze.webp)



**点击“创建运行环境”**

![image](https://img.aurorainic.top/i/2023/11/05/m9o6wl.webp)



- **名称**：``支持英文、数字、-和_,长度2-30,并且不能以-_开头和结尾``

- **来源**：一般通过应用商店的镜像源部署安装即可

- **应用**：这里实例我们选择 PHP 8 ，8.0.30

- **PHP拓展源**：决定下载源，请考虑你的服务器所在地区选择，有国内也有国外的。

**扩展：**

必选：``fileinfo``（1Panel预装）、``pdo_mysql``、``exif``、``mysqli``、``imagick``（与``gd``任装一个但最好都装以免报错）

选装：``Redis``、``opcache``、``curl``



**全部完成后点击“确认”，等待状态变为“正常”**

![image](https://img.aurorainic.top/i/2023/11/05/mezeiz.webp)

# 2. 准备

## 2.1 数据库

![image](https://img.aurorainic.top/i/2023/11/05/nphm4s.webp)

- （数据库）**名称**：最好用英文+数字，同时确保字符编码为 ``utf8mb4``

- **用户名**：（自定）

- **密码**：可使用随机生成的也可自定义

  其余不变

**完成后点击确认**



## 2.2 站点配置

### 2.2.1 创建站点

**路径：**网站-网站，**点击“创建网站”**

由于我们是用容器化 PHP 环境来运行站点，所以这里选择**“运行环境”**

![image](https://img.aurorainic.top/i/2023/11/05/n5mrmg.webp)

- **分组**：若您有多个站点可以调整至不同分组方便管理

- **类型**：有 PHP 与 Nodejs 两种，这里我们选择 PHP

- **运行环境**：选择我们刚刚的 PHP 8.0 的那个PHP
- **PHP_FPM 端口：**（一般默认即可）
- **主域名：**（根据实际情况填写）
- **代号：**也就是这个站点在“网站”列表中的备注

**完成后点击“确认”**



### 2.2.2 文件

**点击所示按钮进入网站目录，并进入``index``文件夹**

{cat_tips_info color=""}1Panel创建的站点目录下有四个文件夹：``index`` ``log`` ``ssl`` ``waf``，只有``index``是一般意义上的“网站根目录”{/cat_tips_info}

![image-20231105141016581](C:\Users\Mu Feng\AppData\Roaming\Typora\typora-user-images\image-20231105141016581.png)



**删除``index.php``，点击“上传”按钮，将程序包上传至该目录**

![image](https://img.aurorainic.top/i/2023/11/05/nekbml.webp)



**解压压缩包**（解压后，压缩包可删可不删）

![image](https://img.aurorainic.top/i/2023/11/05/nkq42d.webp)



**设置文件访问权限**

![image-20231105142907533](C:\Users\Mu Feng\AppData\Roaming\Typora\typora-user-images\image-20231105142907533.png)

1. 确保文件夹与其中的所有文件权限均为 ``755``



2. 在 **网站配置-网站目录**中

![image](https://img.aurorainic.top/i/2023/11/05/ouxddx.webp)

### 2.2.3 解析&其他

**将您的域名解析至服务器**

这里示例地址为：``test.mufeng086.com``，因此在``mufeng086.com``的解析中添加主机记录为``test``的A记录。

如使用主域名，那么主机记录填入``@``即可。

![image](https://img.aurorainic.top/i/2023/11/05/nghmoq.webp)



**配置SSL证书**

证书可在freessl等地免费申请，此处不赘述。

![image](https://img.aurorainic.top/i/2023/11/05/ntct72.webp)
![image](https://img.aurorainic.top/i/2023/11/05/ntgr4x.webp)

SSL证书有两种方式：DNS账户申请，或手动填写（其他地方申请下来的证书）信息

- DNS账户：

  在 **网站-证书** 中添加 Acme 账户和你域名托管的DNS平台账号（支持阿里云DNS、Dnspod、Cloudflare），然后点击“**创建证书**”，根据要求填写。创建成功后回到本页，在“**SSL选项**“中选择“**选择已有证书**"，选择你的 Acme 账号，选中你所申请的证书，点击“**保存**”。

- 手动填写：

  在“**SSL选项**“中选择“**手动导入证书**"，填入对应信息，完成后点击“**保存**”。

  ![image](https://img.aurorainic.top/i/2023/11/05/nx2orw.webp)

**站点伪静态**

在 **网站配置-伪静态**中输入以下内容，并点击“**保存与重载**”

![image](https://img.aurorainic.top/i/2023/11/05/ovlamv.webp)

~~~
location ~* /(importing|app|content|lib)/.*\.(po|php|lock|sql)$ {
    deny all;
}
location ~ \.(jpe?g|png|gif|webp)$ {
    log_not_found off;
    error_page 404 /content/images/system/default/404.gif;
}
location ~* /.*\.(ttf|ttc|otf|eot|woff|woff2|font.css|css|js)$ {
    add_header Access-Control-Allow-Origin "*";
}
location / {
    index index.php;
    try_files $uri $uri/ /index.php$is_args$query_string;
}
~~~



# 3. 安装

{% note warning flat %}若您的服务器在中国大陆内，需要确保您的域名在所在云服务商已办理备案接入/过白，否则可能无法访问。{% endnote %}

上述流程完成后，访问您的站点域名。

若一切顺利，您应该会看到如下画面：

![image](https://img.aurorainic.top/i/2023/11/05/owc41e.webp)

- **数据库主机&端口**：

  1Panel 的程序皆以 docker 容器化运行，而这个站点本质上也是在容器化运行，因此数据库连接也有点不同。![image](https://img.aurorainic.top/i/2023/11/05/oxhm09.webp)

  在面板数据库管理页面，点击“**连接信息**”

  ![image-20231105150858595](C:\Users\Mu Feng\AppData\Roaming\Typora\typora-user-images\image-20231105150858595.png)

  MySQL在本机的连接地址一般是``localhost:3306``，这里是``mysql:3306``，因此需要将安装页面上的"localhost"改成"mysql"，端口如果数据库安装时没有改那么这里就不用改。

- 其他填写项均为之前创建数据库时的信息，对应填入即可。

- **数据库前缀**：（一般不用动）

完成后点击“继续”

---

填写管理员信息

![image](https://img.aurorainic.top/i/2023/11/05/p1hfcf.webp)

---

**enjoy~**

![image](https://img.aurorainic.top/i/2023/11/05/p24e2m.webp)

---

Chevereto V4 开心版（V4.0.7）
蓝奏云：https://itxe.lanzout.com/itD5M0ozrzif
