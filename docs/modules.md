# 组件库

Zealot 提供丰富的全套组件，涉及 iOS、Android 以及打包流程方方面面。

## iOS 库

iOS 组件提供为 Zealot 检查新版本和安装的服务，支持 Swift 和 Objective-C。

### 安装

#### Cocoapods

使用 [Cocoapods](https://cocoapods.org) 安装 Zealot 需要把它加到 `PodFile`:

```ruby
pod 'Zealot', :git => 'https://github.com/tryzealot/zealot-ios.git', :branch => 'master'
```

保存后开始安装：

```bash
pod install
```

### 使用

1. 在 AppDelegate 文件t引入 Zealot 框架头：

```swift
// Swift
import Zealot
```

```objectivec
// Objective-C
#import <Zealot/Zealot-Swift.h>
```

2. 接着在上面文件的 `application:didFinishLaunchingWithOptions:` 方法追加启动代码：

```swift
// Swift
// 单个渠道
let zealot = Zealot(endpoint: "http://zealot.com", channelKey: "...")
zealot.checkVersion()

// 多个渠道，比如测试版本，内测版本
let zealot = Zealot(endpoint: "http://zealot.com",
                 channelKeys: [
                   "beta": "xxxxxxx",
                   "test": "yyyyyyy"],
          default_enviroment: "beta")

// 最后触发监测方法
zealot.checkVersion()
```

```objectivec
// Objective-C
// 单个渠道
Zealot *zealot = [[Zealot alloc] initWithEndpoint:@"http://zealot.com"
                                       channelKey:@"..."];

// 多个渠道，比如测试版本，内测版本
Zealot *zealot = [[Zealot alloc] initWithEndpoint:@"http://zealot.com"
                                      channelKeys:@{
                                              @"beta": @"xxxxxxx",
                                              @"gray": @"yyyyyyy"
                                          }
                               default_enviroment:@"beta"];

// 最后触发监测方法
[zealot checkVersion];
```

## Android 库

Android 组件提供为 Zealot 检查新版本和安装的服务，支持 Kotlin 和 Java。

### 安装

#### JitPack

使用 [jitpack](https://jitpack.io) 安装，先需要添加 maven 仓库：

```groovy
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```

之后在主 app 项目的 `build.gradle` 添加 zealot：

```groovy
dependencies {
  implementation 'com.github.tryzealot:zealot-android:master-SNAPSHOT'
}
```

#### JCenter

> 还未发布暂时不可用

```groovy
implementation 'im.ews.zealot:zealot:0.1.0'
```

### 使用

在你的 `Application` 文件的 `onCreate` 方法添加启动代码：

```kotlin
// Kotlin

// 单个渠道
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("...")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch()

// 多个渠道，比如测试版本，内测版本
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("xxxxxxx", "beta")
      .setCHannelKey("yyyyyyy", "test")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch()
```

```java
// Java

// 单个渠道
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("...")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch();

// 多个渠道，比如测试版本，内测版本
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("xxxxxxx", "beta")
      .setCHannelKey("yyyyyyy", "test")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch();
```

### 用户权限

使用 Zealot SDK 需要开启网络权限

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## Fastlane 插件

> 第一次听说 fastlane，那你得多 out 了？！快看看[深入浅出 Fastlane 一看你就懂](https://icyleaf.com/2016/07/fastlane-in-action/) 系列教程补补课吧。

[fastlane-plugin-zealot](https://github.com/tryzealot/fastlane-plugin-zealot) 是专门为 Zealot 提供
的上传 iOS、Andorid 应用和调试文件的 fastlane 插件。如果要使用 `fastlane-plugin-zealot` 可通过下面方法添加到 fastlane 体系中：

```bash
$ fastlane add_plugin zealot
```

插件包含多个 action：

- `zealot`: 上传 iOS 或 Android 应用
- `zealot_debug_file`: 上传调试文件（iOS 是 dSYM，Android 是 Proguard）
- `zealot_version_check`: 检查当前版本是否已存在

### zealot

上传 iOS 或 Android 应用，传入如下参数插件会在成功打包后自动获取 app 的路径并进行上传：

```ruby
zealot(
  endpoint: 'https://zealot.com',
  token: '...',
  channel_key: '...',
)
```

#### 参数

```
+-----------------+---------------------------------+------------------------+----------+
|                                    zealot Options                                     |
+-----------------+---------------------------------+------------------------+----------+
| Key             | Description                     | Env Var                | Default  |
+-----------------+---------------------------------+------------------------+----------+
| endpoint        | The endpoint of zealot          | ZEALOT_ENDPOINT        |          |
| token           | The token of user               | ZEALOT_TOKEN           |          |
| channel_key     | The key of app's channel        | ZEALOT_CHANNEL_KEY     |          |
| file            | The path of app file. Optional  | ZEALOT_FILE            |          |
|                 | if you use the `gym`, `ipa`,    |                        |          |
|                 | `xcodebuild` or `gradle`        |                        |          |
|                 | action.                         |                        |          |
| name            | The name of app to display on   | ZEALOT_NAME            |          |
|                 | zealot                          |                        |          |
| changelog       | The changelog of app            | ZEALOT_CHANGELOG       |          |
| slug            | The slug of app                 | ZEALOT_SLUG            |          |
| release_type    | The release type of app         | ZEALOT_RELEASE_TYPE    |          |
| branch          | The name of git branch          | ZEALOT_BRANCH          |          |
| git_commit      | The hash of git commit          | ZEALOT_GIT_COMMIT      |          |
| custom_fields   | The key-value hash of custom    | ZEALOT_CUSTOM_FIELDS   |          |
|                 | fields                          |                        |          |
| password        | The password of app to download | ZEALOT_PASSWORD        |          |
| source          | The name of upload source       | ZEALOT_SOURCE          | fastlane |
| ci_url          | The name of upload source       | ZEALOT_CI_CURL         |          |
| timeout         | Request timeout in seconds      | ZEALOT_TIMEOUT         |          |
| hide_user_token | replase user token to *** to    | ZEALOT_HIDE_USER_TOKEN | true     |
|                 | keep secret                     |                        |          |
| verify_ssl      | Should verify SSL of zealot     | ZEALOT_VERIFY_SSL      | true     |
|                 | service                         |                        |          |
| fail_on_error   | Should an error uploading app   | ZEALOT_FAIL_ON_ERROR   | false    |
|                 | cause a failure                 |                        |          |
+-----------------+---------------------------------+------------------------+----------+
* = default value is dependent on the user's system

+-----------------------+---------------------------------------------+
|                       zealot Output Variables                       |
+-----------------------+---------------------------------------------+
| Key                   | Description                                 |
+-----------------------+---------------------------------------------+
| ZEALOT_APP_ID         | The id of app                               |
| ZEALOT_RELEASE_ID     | The id of app's release                     |
| ZEALOT_RELEASE_URL    | The release URL of the newly uploaded build |
| ZEALOT_INSTALL_URL    | The install URL of the newly uploaded build |
| ZEALOT_QRCODE_URL     | The QRCode URL of the newly uploaded build  |
| ZEAALOT_ERROR_MESSAGE | The error message during upload process     |
+-----------------------+---------------------------------------------+
```

#### 环境变量

```
+-----------------------+---------------------------------------------+
|                       zealot Output Variables                       |
+-----------------------+---------------------------------------------+
| Key                   | Description                                 |
+-----------------------+---------------------------------------------+
| ZEALOT_APP_ID         | The id of app                               |
| ZEALOT_RELEASE_ID     | The id of app's release                     |
| ZEALOT_RELEASE_URL    | The release URL of the newly uploaded build |
| ZEALOT_INSTALL_URL    | The install URL of the newly uploaded build |
| ZEALOT_QRCODE_URL     | The QRCode URL of the newly uploaded build  |
| ZEAALOT_ERROR_MESSAGE | The error message during upload process     |
+-----------------------+---------------------------------------------+
Access the output values using `lane_context[SharedValues::VARIABLE_NAME]`
```

### zealot_version_check

检查当前版本是否已经上传，减少重复打包和上传。

```ruby
zealot_version_check(
  endpoint: 'https://zealot.com',
  token: '...',
  bundle_id: 'com.example.app.name',
  release_version: '1.0.0',
  build_version: '1'
)
```

#### 参数

```
+-----------------+---------------------------------+------------------------+---------+
|                             zealot_version_check Options                             |
+-----------------+---------------------------------+------------------------+---------+
| Key             | Description                     | Env Var                | Default |
+-----------------+---------------------------------+------------------------+---------+
| endpoint        | The endpoint of zealot          | ZEALOT_ENDPOINT        |         |
| token           | The token of user               | ZEALOT_TOKEN           |         |
| channel_key     | The key of app's channel        | ZEALOT_CHANNEL_KEY     |         |
| bundle_id       | The bundle id(package name) of  | ZEALOT_BUNDLE_ID       |         |
|                 | app                             |                        |         |
| release_version | The release version of app      | ZEALOT_RELEASE_VERSION |         |
| build_version   | The build version of app        | ZEALOT_BUILD_VERSION   |         |
| git_commit      | The latest git commit of app    | ZEALOT_GIT_COMMIT      |         |
| verify_ssl      | Should verify SSL of zealot     | ZEALOT_VERIFY_SSL      | true    |
|                 | service                         |                        |         |
| fail_on_error   | Should an error http request    | ZEALOT_FAIL_ON_ERROR   | false   |
|                 | cause a failure? (true/false)   |                        |         |
+-----------------+---------------------------------+------------------------+---------+
```

### zealot_debug_file

> fastlane-plugin-zealot `0.2.0` 版本开始支持。

自动寻找调试文件并打 zip 包上传（iOS 是 dSYM，Android 是 Proguard）

#### 参数

```
+--------------------+---------------------------------+---------------------------+---------+
|                                 zealot_debug_file Options                                  |
+--------------------+---------------------------------+---------------------------+---------+
| Key                | Description                     | Env Var                   | Default |
+--------------------+---------------------------------+---------------------------+---------+
| endpoint           | The endpoint of zealot          | ZEALOT_ENDPOINT           |         |
| token              | The token of user               | ZEALOT_TOKEN              |         |
| channel_key        | The key of app's channel        | ZEALOT_CHANNEL_KEY        |         |
| platform           | The name of platfrom, avaiable  | ZEALOT_PLATFORM           |         |
|                    | value are                       |                           |         |
|                    | ios,mac,macos,osx,android       |                           |         |
| path               | The path of debug file          | ZEALOT_PATH               |         |
|                    | (iOS/macOS is archive path for  |                           |         |
|                    | Xcode, Android is path for app  |                           |         |
|                    | project)                        |                           |         |
| xcode_scheme       | The scheme name of app          | ZEALOT_XCODE_SCHEME       |         |
| android_build_type | The build type of app           | ZEALOT_ANDROID_BUILD_TYPE | release |
| android_flavor     | The product flavor of app       | ZEALOT_ANDROID_FLAVOR     |         |
| extra_files        | A set file names                | ZEALOT_EXTRA_FILES        | []      |
| output_path        | The output path of compressed   | DF_DSYM_OUTPUT_PATH       | .       |
|                    | dSYM file                       |                           |         |
| release_version    | The release version of app      | ZEALOT_RELEASE_VERSION    |         |
| build_version      | The build version of app        | ZEALOT_BUILD_VERSION      |         |
| overwrite          | Overwrite output compressed     | DF_DSYM_OVERWRITE         | false   |
|                    | file if it existed              |                           |         |
| timeout            | Request timeout in seconds      | ZEALOT_TIMEOUT            |         |
| verify_ssl         | Should verify SSL of zealot     | ZEALOT_VERIFY_SSL         | true    |
|                    | service                         |                           |         |
| fail_on_error      | Should an error uploading app   | ZEALOT_FAIL_ON_ERROR      | false   |
|                    | cause a failure? (true/false)   |                           |         |
+--------------------+---------------------------------+---------------------------+---------+
```

### zealot_sync_devices

> fastlane-plugin-zealot `0.4.1` 版本开始支持。

同步并关联苹果开发者中心（Apple Developer Portal)的测试设备名称到 Zealot 服务。

```ruby
zealot_sync_devices(
  endpoint: 'https://zealot.com',
  token: '...',
  username: 'user@example.com',
  team_id: '...'
)
```

#### 参数

```
+---------------+----------------------------------+------------------------+---------+
|                             zealot_sync_devices Options                             |
+---------------+----------------------------------+------------------------+---------+
| Key           | Description                      | Env Var                | Default |
+---------------+----------------------------------+------------------------+---------+
| endpoint      | The endpoint of zealot           | ZEALOT_ENDPOINT        |         |
| token         | The token of user                | ZEALOT_TOKEN           |         |
| username      | The apple id (username) of       | DELIVER_USER           | *       |
|               | Apple Developer Portal           |                        |         |
| team_id       | The ID of your Developer Portal  | ZEALOT_APPLE_TEAM_ID   | *       |
|               | team if you're in multiple       |                        |         |
|               | teams                            |                        |         |
| team_name     | The name of your Developer       | ZEALOT_APPLE_TEAM_NAME | *       |
|               | Portal team if you're in         |                        |         |
|               | multiple teams                   |                        |         |
| platform      | The platform to use (optional)   | ZEALOT_APPLE_PLATFORM  | ios     |
| verify_ssl    | Should verify SSL of zealot      | ZEALOT_VERIFY_SSL      | true    |
|               | service                          |                        |         |
| timeout       | Request timeout in seconds       | ZEALOT_TIMEOUT         |         |
| fail_on_error | Should an error http request     | ZEALOT_FAIL_ON_ERROR   | false   |
|               | cause a failure? (true/false)    |                        |         |
+---------------+----------------------------------+------------------------+---------+
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
