---
title: 简洁的静态站点导航模板：vitepress-fe-nav
date: 2023-07-25 15:05:55
description: 简洁明快的静态站点导航模板
categories:
  - 技术探索
tags:
  - 推荐
  - 导航
  - 静态
  - vitepress
  - vue
cover: https://img.aurorainic.top/i/2023/07/25/o4ctfi.webp
---

# 一、前言

关于站点导航我们见得很多，成熟的项目也有，比如 [WebStack](https://github.com/WebStackPage/WebStackPage.github.io)。

不过这些多数都是基于 WordPress 等动态博客框架，并且作为站点导航，搜索框也是常伴的。但很多人只是想要一个纯粹的、只需要展示站点链接信息等等。

而作为依托动态博客运行的独立站点，是需要一台用以运行的主机/服务器的。

> 我没有自己的服务器怎么办？

静态博客/站点，其中一个优势就是可以实现 "Serverless"（无服务器）运行，只需将静态资源文件打包发到任何可以 severless 运行的平台都可以实现站点的运行和公开可用。如 Github Pages, 国内云服务商的 oss, cos 等。

同时，部分支持静态站点运行的平台也可以免费给你一个可用（不一定好用）的域名，这也省去了部分朋友买域名的流程（如 Github Pages）。

这里要介绍的项目，就是以 Vitepress（基于 Vue）框架定制的[**vitepress-fe-nav**
](https://github.com/maomao1996/vitepress-fe-nav)

---

# 二、功能/优点

- 作为静态站点，加载速度快
- 模板默认中文
- 自带前端导航模块
- 支持日夜颜色模式自适应切换
- 支持 Github Pages 直接部署上线

---

# 三、使用

在自己的账号下新建仓库，在创建页面导入该仓库内容（不推荐直接 fork 项目仓库的方式，后续运行和配置修改中可能会出现未知 bug）。

![image](https://img.aurorainic.top/i/2023/07/25/nup8iy.webp)

![image](https://img.aurorainic.top/i/2023/07/25/nv4so9.webp)

创建个人项目后，根据需要对根目录的内容信息进行更改增减。

完成后依据实际情况，将整个项目打包下载本地后上传至静态托管平台；或外部平台直接导入 Github 仓库进行托管。

---

# 四、基础编辑教程

教程目的：面向对 Vitepress 了解不多、仅想套用模板做站点的定制化指引。  
（注：本文内容目标：达成基本的样式套用，深入修改请参照 Vue 文档等）  
（请在贵站中标注本项目仓库地址等信息）

## 首页配置

这里指前端导航页访问的初始页面。

### 1.主体部分

修改位置：/docs/index.md

范例：

```ts
hero:
  name: 茂茂的 //左侧第一行
  text: 个人前端导航  //左侧第二行
  tagline: 使用 VitePress 打造个人前端导航  //第三行小注内容
  image:
    src: /logo.png //页面大图地址（图像最好切圆后使用）
    alt: 茂茂物语
  actions:  //跳转按钮，可按需增减
    - text: 茂茂物语
      link: https://notes.fe-mm.com
    - text: 前端导航
      link: /nav/
      theme: alt  //此行代表跳转至新标签页显示
    - text: mmPlayer
      link: https://netease-music.fe-mm.com
      theme: alt
features:
  - icon: 📖  //图标（输入法的表情icon即可）
    title: 前端物语  //小标题
    details: 整理前端常用知识点<br />如有异议按你的理解为主，不接受反驳  //注释
```

### 2.导航栏与页脚

**2.1 导航栏**

修改位置：/docs/.vitepress/configs/nav.ts

范例（按需增减）：

```ts
export const nav: DefaultTheme.Config['nav'] = [
  { text: '个人主页', link: 'https://fe-mm.com' }, //切行无影响
  {
    text: '茂茂物语', //显示文本
    link: 'https://notes.fe-mm.com', //链接
  },
]
```

**2.2 社交链接&页脚**

修改位置：/docs/nav/index.md

```ts
export default defineConfig({
    ---
    socialLinks: [{ icon: 'github', link: 'https://github.com/maomao1996/vitepress-fe-nav' }], //社交链接

    footer: {
      message: '如有转载或 CV 的请标注本站原文地址',
      copyright: 'Copyright © 2019-present maomao'
    },  //页脚，可按Vue支持格式修改
})
```

## 站点列表页

一般对应 `https://域名(ip)/nav`



### 1.站点列表数据

修改文件: /docs/nav/data.js

此处的站点信息涉及四个属性：  

| 属性值 | 作用                          |
| :----- | ----------------------------- |
| icon   | 图标地址（可填绝对/相对路径） |
| title  | 站点标题                      |
| desc   | 站点描述                      |
| link   | 链接地址（必填）              |

除 link 外，其余属性可按需填入。

基本结构如下:

```ts
export const NAV_ATA: NAVATA[] = [
  {
    title: '类别1' //分类标题
    items: [
      {
        icon: '',
        title: '',
        desc: '',
        link: ''
      }
    ]
  },
  {
    title: ''  //分类标题
    items: [
      {
        icon: '',
        title: '',
        desc: '',
        link: ''
      },
      {
        icon: '',
        title: '',
        desc: '',
        link: ''
      }
    ]
  }
]
```

### 2.页面自定义

**2.1 添加其他元素**

修改位置：/docs/nav/index.md

Nav 页本身属于 MD 文件渲染，因此除引用的 data 文件用于数据列表显示，还可以添加其他内容。

例如以下范例：

```ts
# 前端导航  //标题

::: tip
该导航由 [maomao](https://github.com/maomao1996) 开发，如有引用、借鉴的请保留版权声明：<https://github.com/maomao1996/vitepress-fe-nav>
:::  //引用Notes提示块

<MNavLinks v-for="{title, items} in NAV_DATA" :title="title" :items="items"/>  //引用data.ts文件显示站点列表

<br />

::: tip
该导航由 [maomao](https://github.com/maomao1996) 开发，如有引用、借鉴的请保留版权声明：<https://github.com/maomao1996/vitepress-fe-nav>
:::  //引用Notes提示块
```

**2.2 其他部分**

修改位置：/docs/.vitepress/config.ts

## 站点属性配置

**3.1 站点图标（favicon）**

修改位置：/docs/.vitepress/configs/head.ts  
在对应位置更改即可。

**3.2 站点标题与图标**

修改位置：/docs/.vitepress/config.ts

站点标题：

```ts
export default defineConfig({
  ---
  lang: 'zh-CN',  //语言，建议中文（zh-CN）
  title: '',  //站点标题
  description: '',  //简介
  head,
})
```

站点图标：

```ts
export default defineConfig({
  ---
  /* 主题配置 */
  themeConfig: {
    i18nRouting: false,

    logo: '/logo.png',  //更改此处
```

---

# 版权注明：

本模板由 [茂茂 | maomao](https://github.com/maomao1996) 开发

项目源仓库地址：https://github.com/maomao1996/vitepress-fe-nav
