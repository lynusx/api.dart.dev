# ServiceExtensionResponse

```dart
final class ServiceExtensionResponse {}
```

服务协议扩展 RPC 的响应。

如果 RPC 成功，请使用 [ServiceExtensionResponse.result]，否则请使用 [ServiceExtensionResponse.error]。

### result

```dart
String? result
```

成功的服务协议扩展 RPC 的结果。

### errorCode

```dart
int? errorCode
```

与失败的服务协议扩展 RPC 关联的错误代码。

### errorDetail

```dart
String? errorDetail
```

失败的服务协议扩展 RPC 的详细信息。

### ServiceExtensionResponse.result()

```dart
ServiceExtensionResponse.result(String result)
```

为成功的服务协议扩展 RPC 创建一个响应。

要求 [result] 是一个编码为字符串的 JSON 对象。构造 JSON-RPC 消息时，[result] 将被直接内联。

### ServiceExtensionResponse.error()

```dart
ServiceExtensionResponse.error(int errorCode, String errorDetail)
```

为失败的服务协议扩展 RPC 创建一个错误响应。

要求 [errorCode] 为 [invalidParams]，或介于 [extensionErrorMin] 和 [extensionErrorMax] 之间。要求 [errorDetail] 是一个编码为字符串的 JSON 对象。构造 JSON-RPC 消息时，[errorDetail] 将被直接内联。

### invalidParams

```dart
dynamic invalidParams
```

无效方法参数的错误代码。

### extensionError

```dart
dynamic extensionError
```

通用扩展错误代码。

### extensionErrorMax

```dart
dynamic extensionErrorMax
```

扩展提供的最大错误代码。

### extensionErrorMin

```dart
dynamic extensionErrorMin
```

扩展提供的最小错误代码。

### isError()

```dart
bool isError()
```

判断此响应是否表示一个错误。

服务协议扩展处理器。通过 [registerExtension](https://www.yuque.com/thyname/dart.developer/registerextension) 注册。

必须以 [ServiceExtensionResponse](https://www.yuque.com/thyname/dart.developer/serviceextensionresponse) 完成。[method] 为服务协议请求的方法名，[parameters] 为保存服务协议请求参数的映射。

_注意_：所有参数名和参数值均以字符串形式编码。

# registerExtension()

```dart
void registerExtension(String method, ServiceExtensionHandler handler)
```

注册一个 [ServiceExtensionHandler]，该处理器将在此 isolate 中针对 [method] 被调用。_注意_：服务协议扩展必须在每个 isolate 中分别注册。

_注意_：[method] 必须以 'ext.' 开头，建议使用以下结构以避免与其他包冲突：'ext.package.command'。即紧跟在 'ext.' 前缀之后的应为注册包的名称，随后再跟一个句点（'.'）和命令名称。例如：'ext.dart.io.getOpenFiles'。

由于服务扩展是特定于 isolate 的，使用扩展的客户端在每次 RPC 调用中都必须包含一个 'isolateId' 参数。

# extensionStreamHasListener

```dart
bool get extensionStreamHasListener
```

“Extension”流当前是否至少有一个监听者。

VM 服务的客户端可以使用 `listenStream` 方法注册为扩展流的监听者。只要至少有一个这样的客户端注册为监听者且尚未断开连接，扩展流就处于有监听者的状态。

当流存在监听者时调用 [postEvent](https://www.yuque.com/thyname/dart.developer/postevent) 将尝试把该事件传递给所有当前监听者，尽管某个监听者可能在事件送达前断开连接。当流没有监听者时调用 [postEvent](https://www.yuque.com/thyname/dart.developer/postevent) 意味着没有人会收到该事件，此调用实际上是一个空操作。

# postEvent()

```dart
void postEvent(String eventKind, Map eventData, {String stream = 'Extension'})
```

向 “Extension” 事件流发布一个类型为 [eventKind]、负载为 [eventData] 的事件。

如果 [extensionStreamHasListener](https://www.yuque.com/thyname/dart.developer/extensionstreamhaslistener) 为 false，此方法为空操作。可通过覆盖 [stream] 设置事件应发布到的目标流。[stream] 不得以下划线开头，也不得是 VM Service 的核心流。
