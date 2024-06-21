---
title: flarum论坛插件推荐（亲测可用，截至V1.7.2）
date: 2023-04-10 16:35:19
description: 结合实际使用，为各位推荐flarum能用且好用的插件。
categories:
  - 技术探索
tags:
  - 扩展
  - 推荐
  - flarum
  - 论坛
---

{% note info flat %}本文中提及的插件大部分为博主自建论坛所用，会额外推荐其他插件。这些可能与您的需求不一致，您可以前往[flarum中文社区](https://discuss.flarum.org.cn)、[flarum官方英文社区](https://discuss.flarum.org)、[extiverse插件展示站](https://extiverse.com/) 等寻找更多插件。

以下插件均经博主实测可正常运行（博主的运行环境：CentOS 7.9，flarum V1.7.2，PHP 8.1）{% endnote %}

{% note warning flat %}使用安装插件命令前，请务必确保已处于站点文件目录{% endnote %}

---

**0. 清除缓存（插件安装或配置后最好通过以下命令清除缓存以减少意外bug等的发生）**

```php
php flarum cache:clear
```

## 一、帖子编辑、发布等

1. 在帖子中插入 Font Awesome 图标

   ```php
   composer require antoinefr/flarum-ext-bbcode-fa
   ```

2. 发布时预览

   ```php
   composer require zerosonesfun/composer-preview
   ```

3. 显示帖子的修改记录

   ```php
   composer require the-turk/flarum-diff
   ```

4. 帖子内投票

   ```php
   composer require fof/polls
   ```

5. 自动识别并实时显示图片、视频链接

   ```php
   composer require fof/formatting
   ```

## 二、显示、美化

1. 简体中文语言包

   ```php
   composer require flarum-lang/chinese-simplified
   ```

2. 实现网页帖子实时刷新（推荐论坛达到一定用户体量时采用）

   ```php
   composer require ramesh-dada/realtime
   ```

3. 增强对图片的操作优化

   ```php
   composer require darkle/fancybox
   ```

4. 移动端底部菜单栏

   ```php
   composer require acpl/mobile-tab
   ```

5. 帖子摘要

   ```php
   composer require ianm/synopsis
   ```

6. 链接内容预览**（正常显示但大概率点击无反应，慎用）**

   ```php
   composer require datlechin/flarum-link-preview
   ```

7. 移动端搜索组件**（显示可能错位，此时需要后期CSS优化）**

   ```php
   composer require flarumtr/flarum-ext-mobile-search
   ```

8. 以id数字代替原生长地址链接

   ```php
   composer require pipecraft/flarum-ext-id-slug
   ```

9. Font Awesome 6 图标库**（可能会影响功能按钮图标的显示）**

   ```php
   composer require blomstra/fontawesome
   ```

10. 论坛用户名录

    ```php
    composer require fof/user-directory
    ```

11. 自定义静态页面（适用于站点介绍等，可用于独立链接）（HTML页面）

    ```php
    composer require fof/pages
    ```

12. 导航栏显示日/夜间显示模式切换按钮

    ```php
    composer require fof/nightmode
    ```

13. 导航栏链接

    ```php
    composer require fof/links
    ```

14. 将全部帖子显示在论坛首页

    ```php
    composer require fof/frontpage
    ```

15. 显示发帖时用户使用的操作系统**（可能因用户栏多组件挤压造成显示不美观）**

    ```php
    composer require datlechin-posted-on
    ```

16. 被暂停账号的明显显示

    ```php
    composer require matteociaroni-public-suspensions
    ```

17. “相关帖子”

    ```php
    composer require nearata/related-discussions
    ```

18. 正/逆序排序按钮

    ```php
    composer require blomstra-sort-order-toggle
    ```

19. 更改站点/扩展的多语言显示文字

    ```php
    composer require fof-linguist
    ```

20. 在用户头像增加色圈显示用户组

    ```php
    composer require clarkwinkelmann-circle-groups
    ```

## 三、用户互动/使用

1. 关注标签（主题）

   ```php
   composer require fof/follow-tags
   ```

2. 关注帖子

   ```php
   composer require flarum/subscriptions
   ```

3. 【用户视角】我的关注标签（主题）

   ```php
   composer require acpl/my-tags
   ```

4. 关注他人并在Ta发布新帖子时收到通知

   ```php
   composer require ianm/follow-users
   ```

5. 浏览帖子记录

   ```php
   composer require michaelbelgium/flarum-discussion-views
   ```

6. 帖子收藏夹

   ```php
   composer require clarkwinkelmann/flarum-ext-bookmarks
   ```

7. 帖子草稿箱（也有定时发布功能）

   ```php
   composer require fof/drafts
   ```

8. 金钱系统

   ```php
   antoinefr/flarum-ext-money
   ```

9. 每日签到**（需搭配35.前置组件）**

   ```php
   composer require ziiven/flarum-daily-check-in
   ```

10. 用户徽章

    ```php
    composer require v17development/flarum-user-badges
    ```

11. （向授权人员）申请更改个人昵称

    ```php
    composer require fof/username-request
    ```

12. 文件上传组件

    ```php
    composer require fof/upload
    ```

13. 社交网络图标

    ```php
    composer require fof/socialprofile
    ```

14. 用户头像上传时可裁剪

    ```php
    composer require fof/profile-image-crop
    ```

15. 聚合登录（QQ、微信、微博等）**【须获取[彩虹聚合登录](https://u.cccyun.cc)api】**

    ```php
    composer require cccyun/flarum-clogin-oauth
    ```

16. emoji表情选择器（优化型）

    ```php
    composer require clarkwinkelmann/flarum-ext-emojionearea
    ```

17. 举报帖子

    ```php
    composer require flarum/flags
    ```

18. 【用户视角】屏蔽某人的帖子

    ```php
    composer require fof/ignore-users
    ```

19. 以帖子主题名进行搜索

    ```php
    composer require ganuonglachanh-search
    ```

20. 正/逆序排序按钮

   ```php
composer require blomstra-sort-order-toggle
   ```

21. 中文搜索

```php
composer require ganuonglachanh/flarum-ext-search
```

## 四、论坛管理

1. 用户注册后自动分配身份组

   ```php
   composer require fof/default-group
   ```

2. 设置邀请码可入

   ```php
   composer require fof/doorman
   ```

3. 站务警告

   ```php
   composer require askvortsov/flarum-moderator-warnings
   ```

4. 按特定条件过滤注册账户时的电子邮件

   ```php
   composer require nyu8/flarum-email-filter
   ```

5. 过滤关键词，触发自动移入待审核

   ```php
   composer require fof/filter
   ```

6. （刷站点数据）

   ```php
   composer require migratetoflarum/fake-data
   ```

7. 帖子超级置顶

   ```php
   composer require the-turk/flarum-stickiest
   ```

8. 精华帖

   ```php
   composer require fof/frontpage
   ```

9. 修改帖子的发布信息

   ```php
   composer require clarkwinkelmann/flarum-ext-author-change
   ```

10. 禁止一次性邮箱注册

    ```php
    composer require fof/disposable-emails
    ```

11. 封禁ip

    ```php
    composer require fof/ban-ips
    ```

12. 封禁用户

    ```php
    composer require fof/ignore-users
    ```

13. 站点地图生成

    ```php
    composer require fof/sitemap
    ```

14. 合并帖子

    ```php
    composer require fof/merge-discussions
    ```

15. 拆分帖子

    ```php
    composer require fof/split
    ```

16. 最佳回复

    ```php
    composer require fof/best-answer
    ```

17. 黑名单

    ```php
    composer require justoverclock/username-blacklist
    ```

18. 中文搜索

    ```php
    composer require ganuonglachanh/flarum-ext-search
    ```

19. 个人主页背景头图**（对图片比例有要求，慎用）**

    ```php
    composer require sycho/flarum-profile-cover
    ```

20. 站务警告

    ```php
    composer require askvortsov/flarum-moderator-warnings
    ```

21. 锁定帖子（其后无法添加回复）

    ```php
    composer require flarum/lock
    ```

22. 日志记录（免费版）

    ```php
    composer require kilowhat/flarum-ext-audit-free
    ```

23. 针对帖子的复选框

    ```php
    composer require clarkwinkelmann/mass-actions
    ```

24. 移帖

    ```php
    composer require sycho/move-posts
    ```

## 五、首页小组件

{% note info flat %}提醒：首页小组件不宜过多，按需取用{% endnote %}


1. **【前置组件】小组件引擎**

   ```php
   composer require afrux/forum-widgets-core
   ```

2. 论坛数据

   ```php
   composer require afrux/forum-stats-widget
   ```

3. 热门帖

   ```php
   composer require justoverclock/hot-discussions
   ```

4. 最新注册用户显示

   ```php
   composer require justoverclock/last-registered-users
   ```

5. 公告栏

   ```php
   composer require afrux/news-widget
   ```

6. 在线成员

   ```php
   composer require afrux/online-users-widget
   ```

7. 管理组成员栏

   ```php
   composer require justoverclock/staff-members-widget
   ```

8. 发帖量排行榜

   ```php
   composer require afrux/top-posters-widget
   ```



