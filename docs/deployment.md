# 安装部署

> :bell: 官方建议安装首选使用 [Docker](https://www.docker.io/) 安装部署 Zealot。
> 鉴于 iOS 使用下载服务依赖开启 SSL/TLS 证书，建议使用经过授权的证书服务，比如 [Let's Encrypt](https://letsencrypt.org/)。

## 为什么仅提供 Docker 镜像？

部署基于 Rails 应用是异常复杂，即使你可以在部署服务器安装了 Ruby、ImageMagick、 Node、Postgres 和 Redis，
你仍然需要操心如何运行 Zealot 服务和 Sidekiq 异步任务服务。官方提供的 Docker 镜像把这些烦人的事情都放到了镜像通过一键部署安装脚本完成初始化工作。

## 服务器硬件要求

- 主流单核 CPU，多核更好
- 至少 512 MB 内存
- 64 位 Linux 发行版
- 建议 20 GB 以上硬盘空间（取决于应用和上传频率的多少）

## Docker 部署

本着一键安装的原则，可是现实往往是残酷的，Zealot 配置都是依托于 ENV 环境变量，需要配置好之后再执行一键部署生成脚本。
首先需要克隆[官方 Zealot 部署工具](https://github.com/getzealot/zealot-docker.git)，进入 `zealot-docker`
目录后需要打开 `example.env` 文件配置必要的参数后可直接执行 `./deploy.sh` 脚本：

> 默认会生成管理员账号：`admin@zealot.com` 和密码 `ze@l0t` 和一些演示应用。

```bash
$ git clone https://github.com/getzealot/zealot-docker.git
$ cd zealot-docker
$ ./deploy
```

一键部署生成脚本默认内置了三套模板：

- 使用 Let's Encrypt 证书
- 使用自签名证书
- 纯 Zealot 服务（需反代网关或负载均衡生成 SSL 证书）

对于一键安装部署脚本感兴趣的可以查看 [Docker 手把手部署文档](docker.md)。

### Let's Encrypt SSL 证书

**第一步**：执行部署脚本:

```bash
$ ./deploy
```

**第二步**：检查和配置 `.env` 文件，主要是 `ZEALOT_DOMAIN` 和 `ZEALOT_CERT_EMAIL` 是否填写正确，
其他部分可根据实际情况做对应的配置调整

**第三步**：运行 Zealot 服务:

```bash
$ docker-compose up -d
```

### 自签名 SSL 证书 (不推荐)

这个非必要情况请不要使用，对于 iOS 使用自签名证书**可能需要设备也要安装 SSL 证书后才能访问和安装应用**，
以及 Chrome 浏览器也会因为证书拒绝访问。

> 如果域名是非注册域名则需要绑 host 才可以访问，通常是修改系统的 `/etc/hosts` 文件。

```bash
$ sudo vim /etc/hosts

127.0.0.1 zealot.test
```

### 纯 Zealot 服务

如果已经有 [HAProxy](http://www.haproxy.org/)、
[Nginx](http://nginx.org/) 或 [Caddy](https://caddyserver.com/) 之类网关或负载均衡来做 SSL 证书代理，
在运行之后拿到 `zealot-zealot` 实例的 IP 地址，端口是 3000 反代给上述服务即可。

#### Caddy

```
zealot.test:80, zealot.test:443 {
  # http 301 到 https
  redir 301 {
    if {scheme} is http
    / https://{host}{uri}
  }

  log stdout

  # ssl
  ## 证书文件
  tls /etc/certs/zealot.key /etc/certs/zealot-cert.key
  ##
  tls zealot@exampl.eocm

  # serve assets of zealot
  root /app/public

  proxy / http://172.10.0.172:3000 {
    except /assets /packs /uploads /config /favicon.ico /robots.txt

    transparent
    header_upstream X-Marotagem true
    header_upstream Host {host}
    header_upstream X-Real-IP {remote}
    header_upstream X-Forwarded-For {remote}
  }
}
```

配置只需关系 `tls` 和 `proxy` 后面 ip 的部分即可。

#### Nginx

> 没有配置过，欢迎[补充](https://github.com/getzealot/zealot-docs/issues/new)。
