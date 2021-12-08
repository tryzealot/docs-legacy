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

Zealot needs internet permission

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## Fastlane plugins

> [fastlane]((https://fastlane.tools/)) is an open source platform aimed at simplifying Android and iOS deployment.
> fastlane lets you automate every aspect of your development and release workflow,
> customize your deployment workflows using the hundreds of community built fastlane actions and plugins.

[fastlane-plugin-zealot](https://github.com/tryzealot/fastlane-plugin-zealot) provides upload app, debug_file and
version check actions to Zealot, install it in shell：

```bash
$ fastlane add_plugin zealot
```

The plugins includes multi-actions:

- `zealot`: Provides upload iOS and Android app
- `zealot_debug_file`: Provides upload debug file (dSYM for iOS, Proguard for Android)
- `zealot_version_check`: Check the build exists or not on Zealot service
- `zealot_sync_devices`: Syncing iOS UDID devices to relates existed value in Zealot

### zealot

Uploading iOS or Android app, it requires three params:

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

#### Output Variables

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

Check given build exists or not on Zealot

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

> Available begin fastlane-plugin-zealot `0.2.0`.

Searching, zipping and uploading debug file automatily (dSYM for iOS, Proguard for Android)

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

> Available begin fastlane-plugin-zealot `0.4.1`.

Use Apple Developer account to syncing UDIDs and relates the value in Zealot.

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

In addition, as the author of the project also open source a number of other fastlane plug-ins there is always one you will use:

Plugin | Description |
---|---
[fastlane-plugin-ci_changelog](https://github.com/icyleaf/fastlane-plugin-ci_changelog) | Automate generate changelog between previous and the latest commit of SCM during the CI services.
[fastlane-plugin-update_jenkins_build](https://github.com/icyleaf/fastlane-plugin-update_jenkins_build)  | Update build's description of jenkins.
[fastlane-plugin-humanable_build_number](https://github.com/icyleaf/fastlane-plugin-humanable_build_number)  | Automatic generate app build number unque and human readable friendly
[fastlane-plugin-app_info](https://github.com/icyleaf/fastlane-plugin-app_info) | Teardown tool for mobile app(ipa, apk and aab file), analysis metedata like version, name, icon etc.
[fastlane-plugin-android_channels](https://github.com/icyleaf/fastlane-plugin-android_channels)  | Package unsign apk with channels and write empty file to META-INF with channel in general way
[fastlane-plugin-ram_disk](https://github.com/icyleaf/fastlane-plugin-ram_disk)  | Use a virtual ram disk to do anything else
