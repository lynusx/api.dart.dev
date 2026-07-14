# ServiceProtocolInfo

```dart
final class ServiceProtocolInfo {}
```

服务协议（Service protocol）是客户端（如 Observatory）用于访问 Dart VM 为调试和检查 Dart 程序而提供的各项服务的协议。此类封装了访问该服务所需的版本号和 Uri。

### majorVersion

```dart
int majorVersion
```

协议的主版本号。如果正在运行的 Dart 环境不支持服务协议，则此值为 0。

### minorVersion

```dart
int minorVersion
```

协议的次版本号。如果正在运行的 Dart 环境不支持服务协议，则此值为 0。

### serverUri

```dart
Uri? serverUri
```

用于连接由服务托管的调试器客户端的 Uri。如果 Web 服务器未运行，则此值为 null。

### serverWebSocketUri

```dart
Uri? get serverWebSocketUri
```

用于通过 WebSocket 连接服务的 Uri。如果 Web 服务器未运行，则此值为 null。

### ServiceProtocolInfo()

```dart
ServiceProtocolInfo(Uri? serverUri)
```

### toString()

```dart
String toString()
```

# Service

```dart
final class Service {}
```

访问有关服务协议的信息，并控制提供 Dart VM 调试和检查服务访问的 Web 服务器。

### getInfo()

```dart
Future<ServiceProtocolInfo> getInfo()
```

获取有关服务协议的信息（版本号以及访问该服务所需的 Uri）。

### controlWebServer()

```dart
Future<ServiceProtocolInfo> controlWebServer({bool enable = false, bool? silenceOutput})
```

控制通过其访问服务协议的 Web 服务器。[enable] 用作开关，用于启用或禁用该 Web 服务器处理请求。如果提供了 [silenceOutput] 且其值为 true，服务器将不会向控制台输出信息。

### getIsolateId()

```dart
String? getIsolateId(Isolate isolate)
```

返回代表 [isolate] ID 的 [String](https://www.yuque.com/thyname/dart.core/string) 令牌。

如果正在运行的 Dart 环境不支持服务协议，则返回 null。

要获取当前 isolate 的 ID，请将 [Isolate.current] 作为 [isolate] 参数传入。

### getIsolateID()

```dart
String? getIsolateID(Isolate isolate)
```

返回代表 [isolate] ID 的 [String](https://www.yuque.com/thyname/dart.core/string) 令牌。

如果正在运行的 Dart 环境不支持服务协议，则返回 null。

### getObjectId()

```dart
String? getObjectId(Object object)
```

返回代表 [object] ID 的 [String](https://www.yuque.com/thyname/dart.core/string) 令牌。

如果正在运行的 Dart 环境不支持服务协议，则返回 null。
