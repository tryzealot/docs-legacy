# 最佳实践

## 工作流

### iOS 和 Android 应用

#### 集成 Zealot SDK

集成 Zealot SDK 可以让非客户端研发自动触发新版本检查、查看变更日志和安装功能。

#### 安装 fastlane

安装好 fastlane 除了必须 fastlane-plugin-zealot 插件上传应用和调试文件之外强烈建议额外在安装如下插件：

- ci_changelog - 手机本次打包的用户提交的 Git Commits 的信息做变更日志
- update_jenkins_build - 可手动指定本次打包的描述，比如 ci_changelog 生成的变更日志
- humanable_build_number - 为 iOS 和 Android 自动生成唯一的内部构建版本号
- app_info - 查看打包成功后应用的信息（metadata）
- ram_disk - 创建内存虚拟磁盘缩减构建实践（如果内存比较大可以考虑）

```ruby
lane :adhoc do
  # 打包，比如 iOS 的 gym 或 Android 的 gradle

  # 上传应用到 Zealot
  zealot(
    endpoint: '...',
    token: '...',
    channel_key: '...'
  )
end

lane :distribution do
  # 打包，比如 iOS 的 gym 或 Android 的 gradle

  # 上传 iOS 的 dSYM
  zealot_debug_file(
    scheme: 'AppName'
  )

  # 上传 Android 的 Proguard
  zealot_debug_file(
    build_type: 'release',
    flavor: 'googleplay'
  )
end
```

### 打包服务器

比如 Jenkins，配置好项目，设置好 git hook 的轮训触发打包，没什么好说的

### Zealot 服务

除了管理每次上传的应用和调试文件，在每次接收到上传的应用会触发已配置的网络钩子用来给第三方服务发送通知有新版本收到，同时对于集成 Zealot SDK 的手机客户端每次第一次打开 App 都会收到新版本的安装提醒。
