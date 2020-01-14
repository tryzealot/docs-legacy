# Docker 安装部署

这是一篇手把手来指导使用 Docker 部署文档，同时也是解释[一键部署脚本]deployment.md)的分解。

## 安装 Docker

> TODO

## 安装 Docker-Compose

> TODO

## 生成 .env 配置文件

从预先 `config.env` 配置文件复制一份部署使用的配置文件

## 配置域名

二次校验如果没有配置会再提醒

## 配置证书

部署脚本提供三种方式，就算使用最后一种生成的也是 https 的协议头

## 生成 docker-compose.yml

## 创建持久化存储的 docker volumes

保存 zealot 和依赖服务的数据安全，其中 `zealot-app` 是 `zealot-zealot` 和 `zealot-worker` 强依赖的，
`web` 会依赖其中的 public 做静态文件托管

## 拉取（更新）镜像

第一次使用会自动从 Docker hub 下载镜像，后续是更新操作，通常只会更新 zealot 镜像。其他几个依赖服务镜像都是固定版本号

## 设置数据库数据

第一次使用会初始化数据库、加载范例应用数据和设置管理员账号，后续这是因 zealot 更新需要的操作

## 运行 docker-compose

为了安全期间和用户的自定义操作需要手动执行 `docker-compose up -d` 来运行服务。
