# 应用接口

## 上传应用

上传应用，仅支持 iOS 和 Android 类型。

```
POST /api/apps/upload
```

### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| file | `File` | true | 应用本地路径的内容 |
| name | `String` | false | 应用名称，为空时取 App 的信息 |
| release_type | `String` | false | 应用类型，比如 debug, beta, adhoc, release, enterprise 等 |
| source | `String` | false | 上传渠道名称，默认是 api |
| changelog | `String` | false | 变更日志 |
| git_commit | `String` | false | 上传应用时的 git commit hash |
| ci_url | `String` | false | CI 项目构建地址 |

#### 返回样例

> TODO

### 应用列表

获取创建的应用列表，支持分页

```
GET /api/apps
```

#### 参数

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| page | `Integer` | false | 页数|
| per_page | `Integer` | false | 每页返回最大数目 |

#### 返回样例

> TODO

### 应用详情

查看应用的明细：应用类型、渠道等信息

```
GET /api/apps/:id
```

#### 参数

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| id | `String` | true | 应用 ID |

#### 返回样例

> TODO

### 应用版本列表

获取应用已上传的版本列表，按照上传时间倒序排列

```
GET /api/apps/versions
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| page | `Integer` | false | 页数|
| per_page | `Integer` | false | 每页返回最大数目 |

#### 返回样例

> TODO

### 应用最新版本

获取指定应用的最新版本信息

```
GET /api/apps/latest
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| release_version | `String` | true | 应用的发布版本 |
| build_version | `String` | true | 应用的构建版本 |

#### 返回样例

> TODO

### 检查当前版本是否存在

使用 bundle_id、release_version、build_verion 或 bundle_id、git_commit 组合检查当前版本是否存在

```
GET /api/apps/version_exist
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| bundle_id | `String` | true | 应用的包名，iOS 取 bundle_id，Android 取 package_name |
| release_version | `String` | false | 应用的发布版本 |
| build_version | `String` | false | 应用的构建版本 |
| git_commit | `String` | false | 上传应用时的 git commit hash |

#### 返回样例

> TODO
