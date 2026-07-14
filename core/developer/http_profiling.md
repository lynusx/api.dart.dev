# addHttpClientProfilingData()

```dart
void addHttpClientProfilingData(Map<String, dynamic> requestProfile)
```

记录与某个 HTTP 请求相关的数据，用于性能分析。

不应直接调用此函数，而应使用 [package:http_profile](https://pub.dev/packages/http_profile)。

# getHttpClientProfilingData()

```dart
List<Map<String, dynamic>> getHttpClientProfilingData()
```

返回通过 [addHttpClientProfilingData](https://www.yuque.com/thyname/dart.developer/addhttpclientprofilingdata) 添加的数据。

此函数仅供网络性能分析工具使用，其返回数据的格式可能会随时间变化。
