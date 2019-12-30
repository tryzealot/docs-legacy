# 开发指南

**技术栈:**

- Ruby on Rails 驱动 Web 和 API 服务
- Sidekiq 提供异步后台任务管理

**环境依赖:**

- Ruby 2.4+
- Postgres 9.5+
- Redis
- Nodejs 8+
- ImageMagick/GraphicsMagick

如下整理了不同操作系统的本地部署开发教程。

## macOS

### homebrew

首先需要安装 Xcode Command tools:

```bash
$ xcode-select --install
```

如果提示安装失败，需要从 https://developer.apple.com/downloads 下载安装。

之后安装 macOS 的包管理工具 Homebrew

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### git 和 imagemagick

之后开始安装环境依赖

```bash
$ brew install git imagemagick
```

### postgresql 和 redis

```bash
$ brew install redis postgresql
```

运行 postgresql 和 redis 服务

```bash
$ brew services start postgresql
$ brew services start redis
```

### node

```bash
$ brew install node
$ npm install -g yarn
```

### ruby

> 建议使用 2.6+ 版本

#### rvm

```bash
$ curl -sSL https://get.rvm.io | bash -s stable
$ rvm install 2.6 --disable-binary
```

### homebrew

```bash
$ brew install ruby # 通常是最新版本
```

之后需要把 homebrew 安装的 ruby 执行路径加到你当前 shell 的 `PATH` 环境变量之中:

- **zsh** shell 添加到 `~/.zshrc`
- **bash** shell 添加到 `~/.bashrc` 或 `~/.bash_profile`


```bash
export PATH="/usr/local/lib/ruby/gems/2.6.0/bin:/usr/local/opt/ruby/bin:$PATH"
```

添加后记得重载下配置文件

```bash
$ source ~/.zshrc
# or
$ source ~/.bash_profile
```

### bundler

```bash
$ [sudo] gem install bundler
$ bundle install
```

### 初始化数据库

```bash
$ rails db:create
$ rails db:migrate
```

初始化管理员账号和应用样例

```bash
$ rails db:seed
```

### 启动 Zealot 服务

```bash
$ bundle exec guard start
```

打开浏览器 `http://localhost:3000`
