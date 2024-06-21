---
title: 美观的服务器监控面板：Ward
description: 有一个简洁美观的服务器监控面板~
date: 2023-12-27 21:42:46
categories:
  - 技术探索
tags:
  - 监控
  - 服务器
cover: https://img.aurorainic.top/i/2023/12/27/zciauo.webp
---

{% note info flat %} 友情提醒：好看能用≠实用 {% endnote %}

G站有很多服务器监控面板，比如uptimerobot，uptimeKuma等等。

今天推荐一个高颜值的面板：Ward

![预览](https://img.aurorainic.top/i/2023/12/27/zciauo.webp)

---

#  一、特点

- 支持 ``windows`` ``linux``
- 支持明/暗主题（但不能自动切换）
- 美观，进入时有动效
- 页面支持自适应

# 二、使用

**有两种使用方式，按需选择。**

## 1. docker

{% note warning flat %}使用此方式前，请确保服务器已安装docker并正常运行中。{% endnote %}

在终端命令行输入以下命令推送ward镜像并运行容器：

```bash
docker run --restart unless-stopped -it -d --name ward  -p 4000:4000 -e WARD_PORT=4000 -e WARD_THEME=dark --privileged antonyleons/ward
```

完成后，在浏览器输入``你的服务器ip:4000``

## 2. jar本地运行

### 2.1 安装java环境

以CentOS为例，在终端命令行输入以下命令安装OpenJDK：

```bash
sudo yum update
sudo yum install java-11-openjdk
```

完成后可以输入以下命令验证安装：

```java
java -version
```

![](https://img.aurorainic.top/i/2023/12/27/10u4tzq.webp)

---

### 2.2 下载并运行

{% note info flat %} 以下文件名仅为示例，请按照实际情况修改 {% endnote %}

前往仓库下载页下载程序包（.jar）文件：https://github.com/Rudolf-Barbu/Ward/releases
或在终端命令行使用以下命令（默认存放在``/root``文件夹）

```bash
wget https://github.com/AntonyLeons/Ward/releases/download/2.4.0/ward-2.4.0.jar
```



CD进入程序包所在的文件夹，输入命令启动：

```java
java -jar ward-2.4.0.jar
```

完成后，在浏览器输入``你的服务器ip:4000``



此方式为直接运行，Ctrl+C 和关闭命令行窗口都会中断程序的运行。

要想后台运行，则启动命令改为：

```java
java -jar ward-2.4.0.jar &
```



要想长期保持运行且不受命令行关闭影响：

```java
nohup java -jar ward-2.4.0.jar &
```

## 3. 自定义

ward有三项配置可以自行修改：

| 设置项     | 描述                              | 默认值 |
| ---------- | --------------------------------- | ------ |
| serverName | 要显示的页面名称。                | Ward   |
| theme      | 明暗主题，输入`light` 或 `dark`。 | light  |
| port       | 要侦听的外部端口。                | 4000   |

位置：

- **jar本地运行**：``setup.ini``文件
- **docker运行**：设置环境变量，重新启动容器后生效。

## 4. 自行编译

若担心安全性、想自行编辑代码等，可自行将项目下载到服务器本地。

```bash
git clone https://github.com/AntonyLeons/Ward
```

将项目文件夹作为Maven项目导入代码编辑器中。

```
mvn clean package
```

