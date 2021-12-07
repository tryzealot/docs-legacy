# Modules

Zealot offers a rich set of components for iOS, Android, and all aspects of the packaging process.

## iOS SDK

The iOS component provides a service to check for new versions and installations for Zealot,
supporting Swift and Objective-C.

### Install

#### Cocoapods

Adding below code into `Podfile`:

```ruby
pod 'Zealot', :git => 'https://github.com/tryzealot/zealot-ios.git', :branch => 'master'
```

Install it：

```bash
pod install
```

### Usages

1. Add the code in your `AppDelegate`：

```swift
// Swift
import Zealot
```

```objectivec
// Objective-C
#import <Zealot/Zealot-Swift.h>
```

2. Add the following code in  `application:didFinishLaunchingWithOptions:` method block：

```swift
// Swift
// Single channel
let zealot = Zealot(endpoint: "http://zealot.com", channelKey: "...")
zealot.checkVersion()

// Multi-channel, such as beta, adhoc versions
let zealot = Zealot(endpoint: "http://zealot.com",
                 channelKeys: [
                   "beta": "xxxxxxx",
                   "test": "yyyyyyy"],
          default_enviroment: "beta")

// Active it
zealot.checkVersion()
```

```objectivec
// Objective-C
// Single channel
Zealot *zealot = [[Zealot alloc] initWithEndpoint:@"http://zealot.com"
                                       channelKey:@"..."];

// Multi-channel, such as beta, adhoc versions
Zealot *zealot = [[Zealot alloc] initWithEndpoint:@"http://zealot.com"
                                      channelKeys:@{
                                              @"beta": @"xxxxxxx",
                                              @"gray": @"yyyyyyy"
                                          }
                               default_enviroment:@"beta"];

// Active it
[zealot checkVersion];
```

## Android SDK

The Android component provides a service to check for new versions and installations for Zealot, supporting both Kotlin and Java.

### Install

#### JitPack

Using [jitpack](https://jitpack.io) to install：

```groovy
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```

In `build.gradle` file of main app project add:

```groovy
dependencies {
  implementation 'com.github.tryzealot:zealot-android:master-SNAPSHOT'
}
```

### Usages

Add the start code to the `onCreate` method block of your `Application` file:

```kotlin
// Kotlin

// Single channel
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("...")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch()

// Multi-channel, such as beta, adhoc versions
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("xxxxxxx", "beta")
      .setCHannelKey("yyyyyyy", "test")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch()
```

```java
// Java

// Single channel
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("...")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch();

// Multi-channel, such as beta, adhoc versions
Zealot.create(getActivity())
      .setEndpoint("https://zealot.com")
      .setChannelKey("xxxxxxx", "beta")
      .setCHannelKey("yyyyyyy", "test")
      .setBuildType(BuildConfig.BUILD_TYPE)
      .launch();
```

### Permission

使用 Zealot SDK 需要开启网络权限

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## Fastlane plugins

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

#### Parameters

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

#### Parameters

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

#### Parameters

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

#### Parameters

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

#### Usefully plugins

除此之外，作为项目的作者还开源了不少其他的 fastlane 插件总有一个你会用到：

插件 | 说明 |
---|---
[fastlane-plugin-ci_changelog](https://github.com/icyleaf/fastlane-plugin-ci_changelog)  | 支持多种 CI 系统自动生成变更历史
[fastlane-plugin-update_jenkins_build](https://github.com/icyleaf/fastlane-plugin-update_jenkins_build)  | 自动更新 Jenkins Build 描述
[fastlane-plugin-humanable_build_number](https://github.com/icyleaf/fastlane-plugin-humanable_build_number)  | 生成开发可识别的构建版本号
[fastlane-plugin-app_info](https://github.com/icyleaf/fastlane-plugin-app_info) | 解析 apk/ipa 包的 metadata 并打印
[fastlane-plugin-android_channels](https://github.com/icyleaf/fastlane-plugin-android_channels)  | 通用性 Android 多渠道打包
[fastlane-plugin-ram_disk](https://github.com/icyleaf/fastlane-plugin-ram_disk)  | 利用内存做虚拟磁盘加速打包
