---
title: 团队协作空间element-web安装教程
date: 2023-10-31 20:08:32
description: 自部署团队协作应用element-web的简要教程
categories:
  - 技术探索
tags:
  - 教程
  - element
  - 协作
  - docker
  - matrix
cover: https://img.aurorainic.top/i/2023/11/03/sqwdab.webp
---

> Element-web是一个开源的、基于 Matrix 协议的团队协作应用，支持多平台和多端口，包括 Web、桌面端和移动端。Element-web 提供以下功能：
>
> - 即时聊天：支持一对一聊天、群聊、频道聊天等。
> - 文件共享：支持文件上传、分享和下载。
> - 视频会议：支持音频和视频会议。
> - 工作流管理：支持创建和管理任务、项目和流程。
> - 第三方应用集成：支持与其他应用程序集成，例如 GitHub、Google Drive 等。

{% note info flat %}本文内容由本人与“[木创社](https://takagi3.fans)”成员（[一只鬆](https://blog.sotkg.cn/)、[Ice Year](https://blog.iceyear.eu.org/)）共同完成{% endnote %}

# **Matrix介绍**

Matrix是一个开源、可交互、**去中心化**的实时通信服务框架。使用Matrix可以搭建安全的通信服务器，配合支持 Matrix 的客户端可以实现个人、团队间的实时聊天交互。

与常见的QQ、微信、钉钉相比，Matrix的特点就是开源，可私有化部署，保证通信的安全和隐私。与Rocket.chat、MatterMost相比，Matrix的特点还要加上去中心化这个特点。

每个运行Matrix的服务器都是一个节点，用户可以选择在任意节点注册、连接，同一个节点内的用户可以任意通信。同时，节点与节点之间也可以通过联邦（Federation）机制进行通信，实现不同节点的用户之间进行通信。

# 你需要准备的

* 一个运行着Linux 系统的服务器，配置为至少2c2g或以上
* 一个正常运行的域名以及对其 DNS 记录的管理权（除非你想在本地主机上设置它）
* 服务器已经安装 Docker 和 Docker-compose
* 了解基本的 Linux 命令并会使用LInux终端

# 部署后端服务

## 一、配置反向代理服务


1. 新建一个名为`proxy`的文件夹并进入其中

```bash
mkdir proxy && cd proxy
```

{cat_tips_info color=""}我们建议你拥有使用Vim编辑器的能力，但如果你没有，也可以使用服务器面板或其他编辑器管理配置文件。{/cat_tips_info}

2. 创建一个`docker-compose.yml`并且使用Vim打开编辑它

```bash
touch docker-compose.yml && vim docker-compose.yml
```


3. 复制以下内容到`docker-compose.yaml`内

```yaml
version: "3.3"

services:
    proxy:
        image: "jwilder/nginx-proxy"
        container_name: "proxy"
        volumes:
            - "certs:/etc/nginx/certs"
            - "vhost:/etc/nginx/vhost.d"
            - "html:/usr/share/nginx/html"
            - "/run/docker.sock:/tmp/docker.sock:ro"
        networks: ["matrix-network"]
        restart: "always"
        ports:
            - "80:80"
            - "443:443"
            
    letsencrypt:
        image: "jrcs/letsencrypt-nginx-proxy-companion"
        container_name: "letsencrypt"
        volumes:
            - "certs:/etc/nginx/certs"
            - "vhost:/etc/nginx/vhost.d"
            - "html:/usr/share/nginx/html"
            - "/run/docker.sock:/var/run/docker.sock:ro"
        environment:
            NGINX_PROXY_CONTAINER: "proxy"
        networks: ["matrix-network"]
        restart: "always"
        depends_on: ["proxy"]        
  
networks:
    matrix-network:
        driver: bridge
        external: true

volumes:
    certs:
    vhost:
    html:  
```

Tips:

* 如果你已经部署过其他Nginx服务（已占用80＆443端口），并且在服务器上运行，同时你不想影响它的正常工作，你可以手动把Nginx服务下面的`ports`内的参数稍作修改来让容器映射其他端口，但请注意你需要在你已启动的Nginx反代服务自行配置以对其进行正确的反代。
* 它使用默认桥接网络以外的网络，所以你需要在后面的容器中也使用正确的网络配置，使容器间也能够正确通讯。
* 容器端口 80 和 443 已绑定，分别用于 http 和 https。
* 卷证书、vhost 和 html 将在 jwilder/nginx-proxy 和 jrcs/letsencrypt-nginx-proxy-companion 容器之间共享。
* docker 套接字以只读方式安装在 /tmp/docker.sock 处。
* 环境变量 `NGINX_PROXY_CONTAINER` 设置为反向代理容器的容器名称，在我们的例子中是“proxy”
* 网络是外部的。这是为了避免其他容器不共享同一网络的任何问题，因为 docker-compose 在自动创建时如何命名其卷和网络；所以这需要我们创建网络。


4. 创建Docker网络，使用以下命令创建`matrix-network`网络

```bash
docker network create matrix-network
```


5. 启动容器（确认你是在proxy目录下执行此命令）

```bash
docker compose up -d
```

现在，你应该已经完成了反向代理服务器的启动。

## 二、部署Synapse服务器


1. 新建一个名为`synapse`的文件夹

```bash
mkdir synapse && cd synapse
```


2. 在目录下新建`docker-compose.yml`并打开编辑它

```bash
touch docker-compose.yml && vim docker-compose.yml
```


2. 复制以下内容到`synapse`目录下的`docker-compose.yml`内

{cat_tips_info color=""}修改sub.domain.com为你想要Synapse呈现的服务器地址，并且确认您已将服务器的 IP 添加到 域名的DNS 的 A 记录中，并且 CNAME 记录指向确切的子域。{/cat_tips_info}


```yaml
version: "3.3"

services:
    synapse:
        image: "matrixdotorg/synapse:latest"
        container_name: "synapse"
        volumes:
            - "./data:/data"
        environment:
            VIRTUAL_HOST: "sub.domain.com"
            VIRTUAL_PORT: 8008
            LETSENCRYPT_HOST: "sub.domain.com"
            SYNAPSE_SERVER_NAME: "sub.domain.com"
            SYNAPSE_REPORT_STATS: "yes"
        networks: ["matrix-network"]        

    postgresql:
        image: postgres:latest
        restart: always
        environment:
            POSTGRES_PASSWORD: somepassword
            POSTGRES_USER: synapse
            POSTGRES_DB: synapse
            POSTGRES_INITDB_ARGS: "--encoding='UTF8' --lc-collate='C' --lc-ctype='C'"
        volumes:
            - "postgresdata:/var/lib/postgresql/"
        networks: ["matrix-network"]


networks:
    matrix-network:
        driver: bridge
        external: true
volumes:
    postgresdata:
```

Tips:

* 环境变量 VIRTUAL_HOST 和 LETSENCRYPT_HOST 用于 LetsEncrypt 和反向代理容器，它们将生成必要的配置更改以及证书。
* 确保 SYNAPSE_SERVER_NAME 指向 Synapse 服务器的 FQDN（以及子域）。
* 将 VIRUAL_PORT 设置为 8008。synapse 容器公开 HTTP 端口 8008，供客户端与其通信。
* 确保该容器与反向代理容器使用相同的网络，否则容器之间将无法正常通讯。
* 按需修改`POSTGRES_PASSWORD`，`POSTGRES_USER`，`POSTGRES_DB`后方数值为你需要的值以保证数据库安全性。

执行以下命令，创建`data`目录，并生成配置文件

```bash
mkdir data && docker-compose run --rm synapse generate
```

## 三、配置Synapse数据库

我们在前面的文件配置了PostgreSQL数据库，但我们需要对Synapse的配置文件进行修改来让它能够识别并使用这个PostgreSQL数据库。


1. 打开Synapse的data目录，找到`homeserver.yaml`文件并找到以下几行

```yaml
database:
    name: sqlite3
    args:
        database: /path/to/homeserver.db
```

**删除**它们并修改为以下内容：

```yaml
database:
    name: psycopg2
    args:
        user: synapse
        password: somepassword
        host: postgresql
        database: synapse
        cp_min: 5
        cp_max: 10
```

内部关于数据库的配置应该与你之前在`docker-compose.yml`中配置的一致。

{% note info flat %}数据库的配置选择是 psycopg2，它是基于 Python 的 PostgreSQL 适配器。{% endnote %}

## 四、部署并启动服务

```bash
docker compose up -d
```

这样，你应该已经完成了后端服务的部署。

## 五、补充


1. 创建Synapse用户

我们需要打开Synapse容器的终端来使用此命令，逐行粘贴以下命令到你的终端并执行。

```javascript
docker-compose exec synapse bash
register_new_matrix_user -c /data/homeserver.yaml <your matrix server host>
```

{cat_tips_warning color=""}请将“<your matrix server host>”替换为你的Synapse后端地址，注意要带Http协议头，如https://matrix.domain.com{/cat_tips_warning}


接下来Synapse会引导你进行创建用户，如用户名，密码，是否为管理员等。


2. 开启公开注册功能

我们需要打开Synapse的data目录下的`homeserver.yaml`并设置以下参数，然后重启Synapse服务即可。

```javascript
enable_registration: True
```

# 部署前端服务

正如我们说过的，Matrix只是一个协议与框架。Synapse只是一个使用Matrix协议的实现方式。你需要一个 Matrix 客户端才能将其用作消息传递工具。

在我们接下来的示例中，我们将使用Element作为前端客户端，这可能是你可以使用的最流行的 Matrix 客户端之一。

当然，你依然可以选择其他的客户端，例如Schildichat，Fluffychat，Cinny等，他们也是比较流行的Martix客户端。

**下载Element**


1. 前往Element的[Github仓库](https://github.com/vector-im/element-web/releases)以获取最新的Release版本。
2. 下载并解压Element客户端文件并访问其解压目录，然后输入以下命令以创建配置文件

```bash
cp config.sample.json config.json
```


3. 编辑config.json并设置`default_server_config.m.homeserver.base_url`为我们之前部署的Matrix服务端地址。
4. 配置反向代理指向解压Element的目录并为其赋予Https。
5. 访问前端Element客户端测试效果。
