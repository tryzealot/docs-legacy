# 项目配置

Zealot 项目仅支持使用 ENV 环境变量来配置，具体可参考项目的 [example.env](https://github.com/tryzealot/zealot/blob/develop/example.env)。

## 配置域名

必须配置否则下载应用和一些静态资源会无法正确访问到

```bash
# 域名配置，无需配置 http:// 或 https://
ZEALOT_DOMAIN=zealot.com
```

## 配置管理员账户和密码

默认不配置如下也会生成下面的账户和密码作为管理员权限的账户，建议配置后重新修改密码。

```bash
ZEALOT_ADMIN_EMAIL=admin@zealot.com
ZEALOT_ADMIN_PASSWORD=ze@l0t
```

## HTTPS 证书

**因苹果的硬性要求线上部署和下载服务强制 HTTPS**，因此再部署的时候下载应用会强制走 HTTPS 协议。

如果部署是单台机器的独立服务，建议开启 Let's Encrypt 免费 SSL 证书，只需要设置

```bash
ZEALOT_CERT_EMAIL=zealot@example.com
```

> 和下面自签名证书文件名二选一，不能同时设置

如果部署的机器只能使用自签名证书，则需要配置，但需要注意的是 iOS 的下载需要在 iOS 设备授权自签名的证书后才允许下载安装。（不推荐）

```bash
ZEALOT_CERT=zealot.test.pem
ZEALOT_CERT_KEY=zealot.test-key.pem
```

> 和 Let's Encrypt 注册电子邮箱名二选一，不能同时设置

## 邮件服务

```bash
# 发送邮件配置
SMTP_ADDRESS=smtp.gmail.com
SMTP_PORT=587
SMTP_DOMAIN=gmail.com
SMTP_USERNAME=you@gmail.com
SMTP_PASSWORD=yourpassword
SMTP_AUTH_METHOD=plain
SMTP_ENABLE_STARTTLS_AUTO=true

# 邮件默认收发人配置
ACTION_MAILER_DEFAULT_FROM=you@gmail.com
ACTION_MAILER_DEFAULT_TO=you@gmail.com
```

## 登录

### 开启游客模式

开启游客模式后允许应用的查看、下载和安装，建议公司内部对权限依赖不高的状况使用，具体权限对比可以[看这里查看详情](permissions.md)

```bash
# 开启游客模式
ZEALOT_GUEST_MODE=true

# 关闭游客模式
ZEALOT_GUEST_MODE=false
```

### 是否开启注册

关闭注册之后，管理员可以通过管理面板的用户管理手动添加用户。

```bash
# 开启注册
ZEALOT_REGISTER_ENABLED=true

# 关闭注册
ZEALOT_REGISTER_ENABLED=false
```

### 第三方登录

目前已接入的第三方登录：

服务 | 标识符
---|---
飞书 | `feishu`
Gitlab | `gitlab`
Google | `google_oauth2`
LDAP | `ldap`

如果以上服务需要设置回调地址的话请统一设置为：

```
http://zealot.com/users/auth/:provider/callback
```

其中 `:provider` 是上面支持第三方服务的标识符，例如开启了飞书那就把 `:provider` 替换成 `feishu`。

#### 飞书

1. 去注册一个飞书账号并下载手机 App
1. 注册[飞书开发平台](https://open.feishu.cn/app/)并创建企业自建应用获得 app_id, app_secret
1. 填写 callback url 为 `http://zealot.com/users/auth/feishu/callback` (域名根据实际情况修改）
1. 添加用户字段信息授权：`email 地址` （可选）
1. 至少勾选一个应用方式并创建新版本后发布

```bash
FEISHU_ENABLED=true
FEISHU_APP_ID=
FEISHU_APP_SECRET=
```

#### Gitlab

1. 注册[官方](http://gitlab.com)或自部署 Gitlab 服务
1. 在用户设置（Preferences） -> 应用（Applications） 创建一个新应用
1. 回调地址（Redirect URI） 配置为  `http://zealot.com/users/auth/gitlab/callback` (域名根据实际情况修改）
1. 添加用户字段信息授权（scope）：`read_user`，默认 zealot 仅用此授权，如果你的授权范围是 `api` 也没问题

```bash
GITLAB_ENABLED=true
GITLAB_SITE=https://gitlab.com/api/v4
GITLAB_SCOPE=read_user
GITLAB_APP_ID=
GITLAB_SECRET=
```

#### Google

1. 注册 Google 账号
1. 开通 [Google Cloud Platform](https://console.cloud.google.com/apis/dashboard) 服务
1. 选择或创建 Project 并前往 Credentials
1. 创建 OAuth Client ID 选择 Web Application 后添加回调地址（Authorized redirect URI） 配置为 `http://zealot.com/users/auth/google_oauth2/callback` (域名根据实际情况修改）
1. 创建成功后获得 Client ID 和 Client Secret

```bash
GOOGLE_OAUTH_ENABLED=true
GOOGLE_CLIENT_ID=
GOOGLE_SECRET=
```

#### LDAP

1. 自部署或使用现有的 LDAP 服务
1. 自配置或查找如下参数的值

```bash
LDAP_ENABLED=true
LDAP_HOST=10.0.0.1
LDAP_PORT=389
LDAP_METHOD=plain
LDAP_BIND_DN="cn=Manager,dc=example,dc=com"
LDAP_PASSWORD=password
LDAP_BASE="ou=People,dc=example,dc=com"
LDAP_UID=uid
```

## 外部存储

假如你有可以复用的 postgresql 和 redis 服务也可以自定义配置使用，而无需使用 docker-compose 默认配置的服务。

### Postgresql

```bash
ZEALOT_POSTGRES_HOST=127.0.0.1
ZEALOT_POSTGRES_PORT=5432
ZEALOT_POSTGRES_USERNAME=postgres
ZEALOT_POSTGRES_PASSWORD=ze@l0t
ZEALOT_POSTGRES_DB_NAME=zealot
```

### Redis

主要是为了兼容 heroku 才采用的 URL 连接方式

```bash
REDIS_URL=redis://user:pass@127.0.0.1:6379/0
```

## 定时任务

### 清理老版本

按照官方开发者长期的使用观察一个可靠的清理老版本的逻辑是时刻关注当前主版本的所有上传版本，
之前上传的历史版本只需要保留最后一个上传构建版本基本上就满足绝大数情况，举个例子：

> 额外补充：当前逻辑相对省事但缺乏灵活度，其实有想过支持多种处理逻辑，具体参见 https://github.com/tryzealot/zealot/issues/376

```
- 2.0
  - 3
  - 2
  - 1
- 1.0.1
  - 10
  - 9
  - 8
  ...
- 1.0
  - 5
  - 4
  - 3
  ...
```

任务执行时会清理掉 1.0.1 版本包含 9 以下和 1.0 版本包含 4 以下所有版本，最终保留的版本文件是：

```
- 2.0
  - 3
  - 2
  - 1
- 1.0.1
  - 10
- 1.0
  - 5
```

磁盘空间无所谓的话，可以通过设置环境变量 `ZEALOT_KEEP_UPLOADS=true` 来忽略定时任务的清理。
