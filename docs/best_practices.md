# 最佳实践

## 工作流

### iOS 和 Android 应用

#### 集成 Zealot SDK

集成 Zealot SDK 可以让非客户端研发自动触发新版本检查、查看变更日志和安装功能。

#### 安装 fastlane

安装好 fastlane 除了必须 fastlane-plugin-zealot 插件上传应用和调试文件之外强烈建议额外在安装如下插件：

- [ci_changelog](https://github.com/icyleaf/fastlane-plugin-ci_changelog) | 支持多种 CI 系统自动生成变更历史
- [update_jenkins_build](https://github.com/icyleaf/fastlane-plugin-update_jenkins_build) | 自动更新 Jenkins Build 描述
- [humanable_build_number](https://github.com/icyleaf/fastlane-plugin-humanable_build_number) | 生成开发可识别的构建版本号
- [app_info](https://github.com/icyleaf/fastlane-plugin-app_info) | 查看打包成功后应用的信息（metadata）
- [ram_disk](https://github.com/icyleaf/fastlane-plugin-ram_disk) | 创建内存虚拟磁盘，主要用于提升 App 构建速度（如果内存比较大可以考虑）
- [debug_file](https://github.com/icyleaf/fastlane-plugin-debug_file) | 自动化搜索 iOS/macOS dSYM 或 Android Proguard（混淆）并打包 Zip 文件

```ruby
# 构建 iOS 的 ipa 文件并上传 zealot 服务器
lane :upload_app do
  # 打包，比如 iOS 的 gym 或 Android 的 gradle
  gym

  # 上传应用到 Zealot
  zealot(
    endpoint: '...',
    token: '...',
    channel_key: '...'
  )

  # 上传 iOS 的 dSYM
  zealot_debug_file(
    scheme: 'AppName'
  )
end

# 构建 android 的 apk 文件并生成 progguard 包后上传 zealot 服务器
lane :upload_debug_file do
  # 打包，比如 iOS 的 gym 或 Android 的 gradle
  gradle

  # 自动化搜索 Android Proguard（混淆）并打包 Zip 文件
  debug_file

  # 上传应用到 Zealot
  zealot(
    endpoint: '...',
    token: '...',
    channel_key: '...'
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
