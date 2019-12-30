# 组件库

Zealot 提供丰富的全套组件，涉及 iOS、Android 以及打包流程方方面面。

## iOS 库

iOS 组件提供为 Zealot 检查新版本和安装的服务。

> TODO

## Android 库

Android 组件提供为 Zealot 检查新版本和安装的服务。

> TODO

## Fastlane 插件

> 第一次听说 fastlane，那你得多 out 了？！快看看[深入浅出 Fastlane 一看你就懂](https://icyleaf.com/2016/07/fastlane-in-action/) 系列教程不补课吧。

[fastlane-plugin-zealot](https://github.com/getzealot/fastlane-plugin-zealot) 是专门为 Zealot 提供
的上传 iOS、Andorid 应用的 fastlane 插件。如果要使用 `fastlane-plugin-zealot` 可通过下面方法添加到 fastlane 体系中：

```bash
$ fastlane add_plugin zealot
```

传入如下参数插件会在成功打包后自动获取 app 的路径并进行上传：

```ruby
zealot(
  endpoint: 'https://example.com',
  token: '...',
  channel_key: '...',
)
```

#### 参数

```
+-----------------+-------------------------------------------------+------------------------+----------+
|                                            zealot Options                                             |
+-----------------+-------------------------------------------------+------------------------+----------+
| Key             | Description                                     | Env Var                | Default  |
+-----------------+-------------------------------------------------+------------------------+----------+
| endpoint        | The endpoint of zealot                          | ZEALOT_ENDPOINT        |          |
| token           | The token of user                               | ZEALOT_TOKEN           |          |
| channel_key     | The key of app's channel                        | ZEALOT_CHANNEL_KEY     |          |
| file            | The path of app file. Optional if you use the   | ZEALOT_FILE            |          |
|                 | `gym`, `ipa`, `xcodebuild` or `gradle` action.  |                        |          |
| name            | The name of app to display on zealot            | ZEALOT_NAME            |          |
| changelog       | The changelog of app                            | ZEALOT_CHANGELOG       |          |
| slug            | The slug of app                                 | ZEALOT_SLUG            |          |
| release_type    | The release type of app                         | ZEALOT_RELEASE_TYPE    |          |
| branch          | The name of git branch                          | ZEALOT_BRANCH          |          |
| git_commit      | The hash of git commit                          | ZEALOT_GIT_COMMIT      |          |
| password        | The password of app to download                 | ZEALOT_PASSWORD        |          |
| source          | The name of upload source                       | ZEALOT_SOURCE          | fastlane |
| timeout         | Request timeout in seconds                      | ZEALOT_TIMEOUT         |          |
| hide_user_token | replase user token to *** to keep secret        | ZEALOT_HIDE_USER_TOKEN | true     |
| fail_on_error   | Should an error uploading app cause a failure?  | ZEALOT_FAIL_ON_ERROR   | false    |
|                 | (true/false)                                    |                        |          |
+-----------------+-------------------------------------------------+------------------------+----------+
```

#### 其他有用插件

除此之外，作为项目的作者还开源了不少其他的 fastlane 插件总有一个你会用到：

插件 | 说明 |
---|---
[fastlane-plugin-ci_changelog](https://github.com/icyleaf/fastlane-plugin-ci_changelog)  | 支持多种 CI 系统自动生成变更历史
[fastlane-plugin-update_jenkins_build](https://github.com/icyleaf/fastlane-plugin-update_jenkins_build)  | 自动更新 Jenkins Build 描述
[fastlane-plugin-humanable_build_number](https://github.com/icyleaf/fastlane-plugin-humanable_build_number)  | 生成开发可识别的构建版本号
[fastlane-plugin-app_info](https://github.com/icyleaf/fastlane-plugin-app_info) | 解析 apk/ipa 包的 metadata 并打印
[fastlane-plugin-android_channels](https://github.com/icyleaf/fastlane-plugin-android_channels)  | 通用性 Android 多渠道打包
[fastlane-plugin-ram_disk](https://github.com/icyleaf/fastlane-plugin-ram_disk)  | 利用内存做虚拟磁盘加速打包