---
title: 美观现代的在线 Hexo 博客编辑器：Hexon
date: 2023-07-25 20:34:14
description: 让编辑Hexo文章如Typora或Notion一样，一体化、美观，同时兼具便捷。
categories:
  - 技术探索
tags:
  - 推荐
  - Hexo
  - 编辑器
cover: https://img.aurorainic.top/i/2023/07/25/yrm6wu.webp
---

# 一、前言

不知你在为自己的 Hexo 博客写文章时是什么流程呢？

本地用 Markdown 软件写好以后传上去？在 Vscode Codespace 里写完直接上传仓库同步部署？（采用此类方式的请退出本文）



虽然没人能具体规定我们要如何使用 Hexo、上传信息到 Hexo 根目录等，公认的流程是：

本地 Hexo 项目 -> Git 上传 -> 静态托管平台



好吧，有些时候我们其实可以尝试下一体化管理你在博客发布的文章、页面等的面板。

你可能会问，这不就是 [**hexo-admin**](https://github.com/jaredly/hexo-admin) 吗？

![hexo-admin 示例页面](https://img.aurorainic.top/i/2023/07/25/xnlae1.webp)

不过这稍显不太美观，所以请出今天的主角：[**Hexon**](https://github.com/gethexon/hexon)

---

# 二、简介

本地编辑后同步至托管平台，相比于直接通过修改仓库文件内容而言，可以上线前自主排查bug，减少因重复修改内容造成无意义的部署资源的浪费等。

而 Hexon 的主页，相比于上文提到的 hexo-admin 有几个特点：

- 功能较为全面
- 较为美观（支持自适应深色模式）
- 分类明确，结构清晰

## 主页

![实机实例](https://img.aurorainic.top/i/2023/07/25/xxbja9.webp)

1：侧边栏：主功能区

	1.1 操作
	
		“部署”：等同于 ``hexo deploy ``
	
		"生成"：``hexo generate ``
	
		“清理”：等同于  ``hexo clean ``
	
		“同步到 GIT”：一键完成 ``git commit & git push ``
	
		“从 GIT 同步”：将 Git 仓库内容下载与本地文件同步
	
	1.2 筛选
	
		通过文章分类进行显示。
	
	1.3 分类（等同于 categories）

2：二级显示区，带有搜索框，可在限定范围进行搜索。

3：功能栏

	从前至后为“编辑”、“删除”、“同步到 Git ” （后两项可能因bug等原因无法操作故忽略）

4：标题

5：更新时间

6：发布时间

7：front-mattter 信息区域

## 编辑页

![image](https://img.aurorainic.top/i/2023/07/25/yrm6wu.webp)

上方功能栏除“保存”按钮（保存至本地）外与主页面的一致。

右侧信息栏可便捷编辑页面/文章信息，其中“标签”、“分类”可选择已有项（Hexon自动解析），额外的 front-matter 选项可在 “Frontmatters” 填写。

---

# 三、使用

1. 确保本地（Win/Mac/Linux）已安装以下程序并可用：``git`` ``hexo`` ``Node.js``

2. 安装（先 cd 至要保存的根目录）

   2.1 从 Hexon 源仓库克隆运行目录

   ```bash
   git clone https://github.com/gethexon/hexon
   ```

   2.2 安装运行依赖

   ```bash
   pnpm install
   ```

   （若 pnpm 命令显示无效，而 Node 运行正常，输入 

   ```
   npm install pnpm
   ```

   以启用 pnpm）

   2.3 初始化

   ```bash
   pnpm run setup
   ```

   ![image](https://img.aurorainic.top/i/2023/07/25/yz8ioc.webp)

   此处会弹出几个需要输入的会话，分别如下：

   ``? Which port do you like Hexon running at? (5777)``：设置 Hexon 运行端口（默认为5777）

   ``? Your hexo blog path? Absolute or relative path to hexon.``：输入 Hexo 博客运行目录

   ``? Username ?``：设置 Hexon 用户名

   ``? Password ? [input is hidden]``：设置 Hexon 用户密码（此处输入不可见，完成后回车）

   出现以下信息，表示安装完成，可以准备开启 Hexon使用了。

   ```bash
   ⚙ Install
   
   Hexon has been installed to `/root/hexon`
   Run `pnpm start` to start
   Run `pnpm prd` to start with pm2
   To uninstall, remove the following foler: /root/hexon
   ```

   **注：**"/root/hexon" 为本人安装时的情况，请根据实际情况处理。

3. 启动

   ```bash
   pnpm start
   ```

   完成后，访问 ``localhost/ip（视实际情况而定）:设置的端口（默认5777）` 即可进入 Hexon

   若想要持久化运行（进程守护），可使用 pm2 

   ```bash
   pnpm prd
   ```

4. 移除 Hexon

   删除 Hexon 根目录文件夹即可。

5. 其他使用命令

   ``pnpm resetpwd``：重置用户密码

   ``pnpm script`：管理自定义脚本


---

更多详情，请至 Hexon 项目了解：https://github.com/gethexon/hexon
