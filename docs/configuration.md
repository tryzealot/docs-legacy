# 项目配置

Zealot 项目仅支持使用 ENV 环境变量来配置，具体可参考项目的 [example.env](https://github.com/tryzealot/zealot/blob/develop/example.env)。

## 配置域名

```bash
# 域名配置
ZEALOT_DOMAIN=zealot.com
```

## HTTPS 证书

如果部署是单台机器的独立服务，建议开启 Let's Encrypt 免费 SSL 证书，只需要设置

```bash
ZEALOT_CERT_EMAIL=zealot@example.com
```

> 和下面自签名证书文件名二选一，不能同时设置

如果部署的机器只能使用自签名证书，则需要配置：

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

```bash
# 开启游客模式
ZEALOT_GUEST_MODE=true

# 关闭游客模式
ZEALOT_GUEST_MODE=false
```

### 是否开启注册

```bash
# 开启注册
ZEALOT_REGISTER_ENABLED=true

# 关闭注册
ZEALOT_REGISTER_ENABLED=false
```

### 第三方登录

目前已接入的第三方登录：

- [x] Google
- [x] LDAP

#### Google

```bash
## 从这里获取 Client ID 和 Secret: https://code.google.com/apis/console/
GOOGLE_OAUTH_ENABLED=true
GOOGLE_CLIENT_ID=
GOOGLE_SECRET=
```

#### LDAP

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

## 定时任务

### 清理老版本

按照官方开发者长期的使用观察一个可靠的清理老版本的逻辑是时刻关注当前主版本的所有上传版本，
之前上传的历史版本只需要保留最后一个上传构建版本基本上就满足绝大数情况，举个例子：

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
