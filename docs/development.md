# 开发指南

**技术栈:**

- Ruby on Rails 驱动 Web 和 API 服务
- Sidekiq 提供异步后台任务管理

**环境依赖:**

- Ruby 2.6+ (推荐 3.0)
- Postgres 9.5+
- Redis
- Nodejs 8+
- ImageMagick/GraphicsMagick
- webp ffi

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

### git、imagemagick 和 webp

之后开始安装环境依赖

```bash
$ brew install git imagemagick webp
```

M1 用户需要设置依赖编译路径到 SHELL 的配置文件中：

```
export CPATH=/opt/homebrew/include/
export LIBRARY_PATH=$LIBRARY_PATH:/opt/homebrew/lib/
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

Postgresql 还需要创建默认用户名：

```bash
$ createuser --superuser zealot

# 如果担心权限过高可以只开启创建数据库权限
$ createuser --createdb zealot
```

### node

```bash
$ brew install node
$ npm install -g yarn
```

### ruby

可以通过 asdf、rvm 或 homebrew 任意一种方式安装

#### asdf

一个支持主流开发语言版本切换的工具，请按照[官方安装教程](http://asdf-vm.com/guide/getting-started.html)好之后安装 ruby

```bash
asdf plugin add ruby
asdf install ruby 2.7.0
asdf global ruby 2.7.0
```

#### rvm

```bash
$ curl -sSL https://get.rvm.io | bash -s stable
$ rvm install 2.7 --disable-binary
```

### homebrew

```bash
$ brew install ruby # 通常是最新版本
```

之后需要把 homebrew 安装的 ruby 执行路径加到你当前 shell 的 `PATH` 环境变量之中:

- **zsh** shell 添加到 `~/.zshrc`
- **bash** shell 添加到 `~/.bashrc` 或 `~/.bash_profile`

```bash
export PATH="/usr/local/lib/ruby/gems/2.7.0/bin:/usr/local/opt/ruby/bin:$PATH"
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

## 疑难杂症

### M1 芯片 MacOS 问题

```
aarch64-darwin/libwebp_ffi.bundle => aarch64-darwin/jpegdec.o
```

使用 `bundle install` 会遇到如上问题这个是因为 homebrew 安装 webp 依赖之后编译路径无法被找到，上面有解决办法。

```
An error occurred while installing debase (0.2.4.1), and Bundler cannot continue.
```

使用 `bundle install` 会遇到如上问题这个是因为 debase  安装 webp 依赖之后编译路径无法被找到，上面有解决办法。
