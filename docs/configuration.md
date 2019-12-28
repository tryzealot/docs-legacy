# 项目配置

Zealot 项目仅支持使用 ENV 环境变量来配置，具体可参考项目的 [example.env](https://github.com/getzealot/zealot/blob/develop/example.env)。

## 配置域名

```bash
# 域名配置
ZEALOT_DOMAIN=zealot.com
```

## HTTPS 证书

如果部署只是一台机器，建议开启 Let's Encrypt 免费 SSL 证书，只需要设置

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

## 第三方登录

目前已接入的第三方登录：

- [x] Google
- [x] LDAP

### Google

```bash
## 从这里获取 Client ID 和 Secret: https://code.google.com/apis/console/
GOOGLE_OAUTH_ENABLED=true
GOOGLE_CLIENT_ID=
GOOGLE_SECRET=
```

### LDAP

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
