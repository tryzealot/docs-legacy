# 调试文件接口

## 上传调试文件

上传 iOS 和 Android 的调试文件：

- iOS: 使用 Zip 压缩后的 dSYM 文件
- Android: 使用 Zip 压缩后包含 mapping.txt、R.txt 和 AndroidManifest.xml 的文件

```
POST /api/debug_files/upload
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| file | `File` | true | Zip 压缩文件后的调试文件 |
| release_version | `String` | true | 发布版本号，iOS 类型可忽略该参数 |
| build_version | `String` | true | 内部版本号，iOS 类型可忽略该参数 |

#### 返回样例

> TODO

### 调试文件列表

获取创建的应用列表，支持分页

```
GET /api/debug_files
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| page | `Integer` | false | 页数 |
| per_page | `Integer` | false | 每页返回最大数目 |

#### 返回样例

> TODO

### 调试文件详情

查看调试文件的明细，包含上传调试文件的具体解析。

```
GET /api/debug_files/:id
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| id | `String` | true | 调试文件 ID |

#### 返回样例

> TODO

### 更新调试文件

更新调试文件

```
PUT /api/debug_files/:id
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| id | `String` | true | 调试文件 ID |
| file | `File` | true | Zip 压缩文件后的调试文件 |
| release_version | `String` | true | 发布版本号，iOS 类型可忽略该参数 |
| build_version | `String` | true | 内部版本号，iOS 类型可忽略该参数 |

#### 返回样例

> TODO

### 删除调试文件

删除指定调试文件

```
DELETE /api/debug_files/:id
```

#### 参数

!> 需要[用户认证](api#接口认证)。

| 名称 | 类型 | 是否必须 | 描述 |
|---|---|---|---|
| channel_key | `String` | true | 应用具体渠道的 Key |
| id | `String` | true | 调试文件 ID |

#### 返回样例

> TODO
