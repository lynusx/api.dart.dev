# SecureServerSocket

```dart
class SecureServerSocket extends Stream<SecureSocket> implements ServerSocketBase<SecureSocket> {}
```

一个服务器套接字，提供高层级 [Socket] 的流。

更多信息请参见 [SecureSocket]。

### bind()

```dart
Future<SecureServerSocket> bind(dynamic address, int port, SecurityContext? context, {int backlog = 0, bool v6Only = false, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols, bool shared = false})
```

在给定的地址和端口上监听。

当返回的 Future 完成时，服务器套接字已绑定到给定的 [address] 和 [port]，并已开始监听。

[address] 可以是 [String] 或 [InternetAddress]。如果 [address] 是 [String]，[bind] 将执行 [InternetAddress.lookup] 并使用列表中的第一个值。要监听环回适配器（只允许来自本地主机的传入连接），请使用 [InternetAddress.loopbackIPv4] 或 [InternetAddress.loopbackIPv6] 的值。要允许来自网络的传入连接，可使用 [InternetAddress.anyIPv4] 或 [InternetAddress.anyIPv6] 的值，以绑定到所有接口，或使用特定接口的 IP 地址。

如果 [port] 的值为 `0`，系统将选择一个临时端口。可以通过 [port] getter 获取实际使用的端口。

可选参数 [backlog] 可用于为底层操作系统的监听设置指定监听队列长度。如果 [backlog] 的值为 `0`（默认值），系统将选择一个合理的值。

传入的客户端连接将使用 [context] 中设置的服务器证书和密钥升级为安全连接。

[address] 必须以数字地址给出，而非主机名。

要请求或要求客户端通过提供 SSL（TLS）客户端证书进行身份验证，请将可选参数 [requestClientCertificate] 或 [requireClientCertificate] 设为 true。要求证书意味着同时请求证书，因此同时设置两者是多余的。要检查是否收到了客户端证书，请在连接后检查 [SecureSocket.peerCertificate]。如果未收到证书，结果将为 null。

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与客户端进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [SecureSocket.selectedProtocol] 获取协商选定的协议。

可选参数 [shared] 指定是否允许其他 [SecureServerSocket] 对象绑定到相同的 [address]、[port] 和 [v6Only] 组合。如果 [shared] 为 `true`，并且来自此 isolate 或其他 isolate 的更多 [SecureServerSocket] 绑定到同一端口，则传入的连接将在所有已绑定的 `SecureServerSocket` 之间分配。通过这种方式，连接可以分布到多个 isolate 上。

### listen()

```dart
StreamSubscription<SecureSocket> listen(void onData(SecureSocket socket), {Function? onError, void onDone(), bool? cancelOnError})
```

### port

```dart
int get port
```

此套接字使用的端口。

### address

```dart
InternetAddress get address
```

此套接字使用的地址。

### close()

```dart
Future<SecureServerSocket> close()
```

关闭此套接字。

返回的 Future 在套接字完全关闭且不再绑定时完成。

# RawSecureServerSocket

```dart
class RawSecureServerSocket extends Stream<RawSecureSocket> {}
```

一个服务器套接字，提供低层级 [RawSecureSocket] 的流。

更多信息请参见 [RawSecureSocket]。

### requestClientCertificate

```dart
bool requestClientCertificate
```

### requireClientCertificate

```dart
bool requireClientCertificate
```

### supportedProtocols

```dart
List<String>? supportedProtocols
```

### bind()

```dart
Future<RawSecureServerSocket> bind(dynamic address, int port, SecurityContext? context, {int backlog = 0, bool v6Only = false, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols, bool shared = false})
```

在给定的地址和端口上监听。

当返回的 Future 完成时，服务器套接字已绑定到给定的 [address] 和 [port]，并已开始监听。

[address] 可以是 [String] 或 [InternetAddress]。如果 [address] 是 [String]，[bind] 将执行 [InternetAddress.lookup] 并使用列表中的第一个值。要监听环回适配器（只允许来自本地主机的传入连接），请使用 [InternetAddress.loopbackIPv4] 或 [InternetAddress.loopbackIPv6] 的值。要允许来自网络的传入连接，可使用 [InternetAddress.anyIPv4] 或 [InternetAddress.anyIPv6] 的值，以绑定到所有接口，或使用特定接口的 IP 地址。

如果 [port] 的值为 `0`，系统将选择一个临时端口。可以通过 [port] getter 获取实际使用的端口。

可选参数 [backlog] 可用于为底层操作系统的监听设置指定监听队列长度。如果 [backlog] 的值为 `0`（默认值），系统将选择一个合理的值。

传入的客户端连接将使用 [context] 中设置的服务器证书和密钥升级为安全连接。

[address] 必须以数字地址给出，而非主机名。

要请求或要求客户端通过提供 SSL（TLS）客户端证书进行身份验证，请将可选参数 requestClientCertificate 或 requireClientCertificate 设为 true。要求证书隐含了同时请求证书，因此无需同时指定两者。要检查是否收到了客户端证书，请在连接后检查 SecureSocket.peerCertificate。如果未收到证书，结果将为 null。

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与客户端进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [RawSecureSocket.selectedProtocol] 获取协商选定的协议。

可选参数 [shared] 指定是否允许其他 [RawSecureServerSocket] 对象绑定到相同的 [address]、[port] 和 [v6Only] 组合。如果 [shared] 为 `true`，并且来自此 isolate 或其他 isolate 的更多 [RawSecureServerSocket] 绑定到该端口，则传入的连接将在所有已绑定的 [RawSecureServerSocket] 之间分配。通过这种方式，连接可以分布到多个 isolate 上。

### listen()

```dart
StreamSubscription<RawSecureSocket> listen(void onData(RawSecureSocket s), {Function? onError, void onDone(), bool? cancelOnError})
```

### port

```dart
int get port
```

此套接字使用的端口。

### address

```dart
InternetAddress get address
```

此套接字使用的地址。

### close()

```dart
Future<RawSecureServerSocket> close()
```

关闭此套接字。

返回的 Future 在套接字完全关闭且不再绑定时完成。
