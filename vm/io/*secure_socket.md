# SecureSocket

```dart
abstract interface class SecureSocket implements Socket {}
```

使用 TLS 和 SSL 的 TCP 套接字。

更多信息请参见 [Socket]。

### connect()

```dart
Future<SecureSocket> connect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols, Duration? timeout})
```

构造一个新的安全客户端套接字，并将其连接到给定端口 [port] 上的给定主机 [host]。

返回的 Future 将以已连接且可供订阅的 [SecureSocket] 完成。

服务器提供的证书将使用 SecurityContext 对象中设置的受信任证书进行检查。默认的 SecurityContext 对象包含一组内置的、针对知名证书颁发机构的受信任根证书。

[onBadCertificate] 是用于处理无法验证的证书的可选处理程序。该处理程序接收 [X509Certificate]，可以检查它并决定（或让用户决定）是否接受该连接。该处理程序应返回 true 以继续 [SecureSocket] 连接。

[keyLog] 是一个可选回调，在与服务器交换新的 TLS 密钥时会被调用。每次调用，[keyLog] 都会接收一行采用 [NSS 密钥日志格式](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) 的文本。将这些行写入文件可使工具（如 [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)）解密通过此套接字发送的内容。这旨在支持对安全套接字进行网络层调试，不应在生产代码中使用。例如：

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与服务器进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [SecureSocket.selectedProtocol] 获取协商选定的协议。

参数 [timeout] 用于指定等待建立连接所允许的最长时间。如果 [timeout] 长于系统级别的超时时长，超时可能会比 [timeout] 指定的时间更早发生。超时时，将抛出 [SocketException]，并取消所有正在进行的、连接到 [host] 的连接尝试。

### startConnect()

```dart
Future<ConnectionTask<SecureSocket>> startConnect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

类似于 [connect]，但返回一个 [Future]，该 Future 以一个 [ConnectionTask] 完成，如果不再需要 [SecureSocket]，可以取消该任务。

### secure()

```dart
Future<SecureSocket> secure(Socket socket, {dynamic host, SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

在现有连接上启动 TLS。

接受一个已连接的 [socket]，并启动客户端侧的 TLS 握手，以使通信安全。当返回的 Future 完成时，[SecureSocket] 已完成 TLS 握手。使用此函数要求连接的另一端已准备好进行 TLS 握手。

如果 [socket] 已经有订阅，此订阅将不再接收任何事件。在大多数情况下，在启动 TLS 握手之前，对该订阅调用 [StreamSubscription.pause] 是正确的做法。

如果传入了 [host] 参数，它将用作 TLS 握手的主机名。如果未传入 [host]，将使用 [socket] 的主机名。[host] 可以是 [String] 或 [InternetAddress]。

[onBadCertificate] 是用于处理无法验证的证书的可选处理程序。该处理程序接收 [X509Certificate]，可以检查它并决定（或让用户决定）是否接受该连接。该处理程序应返回 true 以继续 [SecureSocket] 连接。

[keyLog] 是一个可选回调，在与服务器交换新的 TLS 密钥时会被调用。每次调用，[keyLog] 都会接收一行采用 [NSS 密钥日志格式](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) 的文本。将这些行写入文件可使工具（如 [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)）解密通过此套接字发送的内容。这旨在支持对安全套接字进行网络层调试，不应在生产代码中使用。例如：

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与服务器进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [SecureSocket.selectedProtocol] 获取协商选定的协议。

调用此函数**不会**触发 DNS 主机名查找。如果传入的 [host] 是 [String]，生成的 [SecureSocket] 的 [InternetAddress] 将以传入的 [host] 作为其主机值，并以已连接套接字的网际地址作为其地址值。

有关参数的更多信息，请参见 [connect]。

### secureServer()

```dart
Future<SecureSocket> secureServer(Socket socket, SecurityContext? context, {List<int>? bufferedData, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols})
```

在现有服务器连接上启动 TLS。

接受一个已连接的 [socket]，并启动服务器侧的 TLS 握手，以使通信安全。当返回的 Future 完成时，[SecureSocket] 已完成 TLS 握手。使用此函数要求连接的另一端即将启动 TLS 握手。

如果 [socket] 已经有订阅，此订阅将不再接收任何事件。在大多数情况下，在启动 TLS 握手之前，对该订阅调用 [StreamSubscription.pause] 是正确的做法。

如果部分 TLS 握手数据已从套接字中读取，可以通过 [bufferedData] 参数传入这些数据。这些数据将在套接字上任何其他可用数据之前被处理。

有关参数的更多信息，请参见 [SecureServerSocket.bind]。

### peerCertificate

```dart
X509Certificate? get peerCertificate
```

已连接 SecureSocket 的对端证书。

如果此 [SecureSocket] 是安全套接字连接的服务器端，[peerCertificate] 将返回客户端证书；如果未收到客户端证书，则返回 `null`。如果此套接字是客户端，[peerCertificate] 将返回服务器的证书。

### selectedProtocol

```dart
String? get selectedProtocol
```

在 ALPN 协议协商期间选定的协议。

如果其中一方不支持 ALPN、未指定支持的 ALPN 协议列表，或者客户端和服务器之间没有共同协议，则返回 `null`。

### renegotiate()

```dart
void renegotiate({bool useSessionCache = true, bool requestClientCertificate = false, bool requireClientCertificate = false})
```

不执行任何操作。

最初的意图是允许对现有安全连接进行 TLS 重新协商。

# RawSecureSocket

```dart
abstract interface class RawSecureSocket implements RawSocket {}
```

`RawSecureSocket` 提供了一个安全（SSL 或 TLS）网络连接。

通过调用 RawSecureSocket.connect 提供到服务器的客户端连接。使用 [RawSecureServerSocket] 创建的安全服务器也会返回代表安全连接服务器端的 `RawSecureSocket` 对象。服务器提供的证书将使用 [SecurityContext] 对象中设置的受信任证书进行检查。默认的 [SecurityContext] 对象包含一组内置的、针对知名证书颁发机构的受信任根证书。

更多信息请参见 [RawSocket]。

### connect()

```dart
Future<RawSecureSocket> connect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols, Duration? timeout})
```

构造一个新的安全客户端套接字，并将其连接到给定端口上的给定主机。

返回的 [Future] 在连接完成且可供订阅时以 [RawSecureSocket] 完成。

服务器提供的证书将使用 SecurityContext 对象中设置的受信任证书进行检查。如果客户端使用 [SecurityContext.useCertificateChain] 和 [SecurityContext.usePrivateKey] 设置了证书和密钥，并且服务器要求客户端证书，则该客户端证书将被发送给服务器。

[onBadCertificate] 是用于处理无法验证的证书的可选处理程序。该处理程序接收 [X509Certificate]，可以检查它并决定（或让用户决定）是否接受该连接。该处理程序应返回 true 以继续 [RawSecureSocket] 连接。

[onBadCertificate] 是用于处理无法验证的证书的可选处理程序。该处理程序接收 [X509Certificate]，可以检查它并决定（或让用户决定）是否接受该连接。该处理程序应返回 true 以继续 [SecureSocket] 连接。

[keyLog] 是一个可选回调，在与服务器交换新的 TLS 密钥时会被调用。每次调用，[keyLog] 都会接收一行采用 [NSS 密钥日志格式](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) 的文本。将这些行写入文件可使工具（如 [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)）解密通过此套接字发送的内容。这旨在支持对安全套接字进行网络层调试，不应在生产代码中使用。例如：

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与服务器进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [RawSecureSocket.selectedProtocol] 获取协商选定的协议。

### startConnect()

```dart
Future<ConnectionTask<RawSecureSocket>> startConnect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

类似于 [connect]，但返回一个 [Future]，该 Future 以一个 [ConnectionTask] 完成，如果不再需要 [RawSecureSocket]，可以取消该任务。

### secure()

```dart
Future<RawSecureSocket> secure(RawSocket socket, {StreamSubscription<RawSocketEvent>? subscription, dynamic host, SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

在现有连接上启动 TLS。

接受一个已连接的 [socket]，并启动客户端侧的 TLS 握手，以使通信安全。当返回的 Future 完成时，[RawSecureSocket] 已完成 TLS 握手。使用此函数要求连接的另一端已准备好进行 TLS 握手。

如果 [socket] 已经有订阅，请通过 [subscription] 参数传入该已有的订阅。[secure] 操作将通过替换其处理程序来接管该订阅。调用者此后不得再操作此订阅。传入已暂停的订阅是错误的。

如果传入了 [host] 参数，它将用作 TLS 握手的主机名。如果未传入 [host]，将使用 [socket] 的主机名。[host] 可以是 [String] 或 [InternetAddress]。

[onBadCertificate] 是用于处理无法验证的证书的可选处理程序。该处理程序接收 [X509Certificate]，可以检查它并决定（或让用户决定）是否接受该连接。该处理程序应返回 true 以继续 [SecureSocket] 连接。

[keyLog] 是一个可选回调，在与服务器交换新的 TLS 密钥时会被调用。每次调用，[keyLog] 都会接收一行采用 [NSS 密钥日志格式](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) 的文本。将这些行写入文件可使工具（如 [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)）解密通过此套接字发送的内容。这旨在支持对安全套接字进行网络层调试，不应在生产代码中使用。例如：

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] 是一个可选的协议列表（按优先级从高到低排列），在与服务器进行 ALPN 协议协商期间使用。示例值为 "http/1.1" 或 "h2"。可以通过 [SecureSocket.selectedProtocol] 获取协商选定的协议。

调用此函数**不会**触发 DNS 主机名查找。如果传入的 [host] 是 [String]，生成的 [SecureSocket] 的 [InternetAddress] 将以传入的 [host] 作为其主机值，并以已连接套接字的网际地址作为其地址值。

有关参数的更多信息，请参见 [connect]。

### secureServer()

```dart
Future<RawSecureSocket> secureServer(RawSocket socket, SecurityContext? context, {StreamSubscription<RawSocketEvent>? subscription, List<int>? bufferedData, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols})
```

在现有服务器连接上启动 TLS。

接受一个已连接的 [socket]，并启动服务器侧的 TLS 握手，以使通信安全。当返回的 Future 完成时，[RawSecureSocket] 已完成 TLS 握手。使用此函数要求连接的另一端即将启动 TLS 握手。

如果 [socket] 已经有订阅，请通过 [subscription] 参数传入该已有的订阅。[secureServer] 操作将通过替换其处理程序来接管该订阅。调用者此后不得再操作此订阅。传入已暂停的订阅是错误的。

如果部分 TLS 握手数据已从套接字中读取，可以通过 [bufferedData] 参数传入这些数据。这些数据将在套接字上任何其他可用数据之前被处理。

有关参数的更多信息，请参见 [RawSecureServerSocket.bind]。

### renegotiate()

```dart
void renegotiate({bool useSessionCache = true, bool requestClientCertificate = false, bool requireClientCertificate = false})
```

不执行任何操作。

最初的意图是允许对现有安全连接进行 TLS 重新协商。

### peerCertificate

```dart
X509Certificate? get peerCertificate
```

获取已连接 RawSecureSocket 的对端证书。如果此 RawSecureSocket 是安全套接字连接的服务器端，[peerCertificate] 将返回客户端证书；如果未收到客户端证书，则返回 null。如果它是客户端，[peerCertificate] 将返回服务器的证书。

### selectedProtocol

```dart
String? get selectedProtocol
```

在协议协商期间选定的协议。

如果其中一方不支持 ALPN、未指定支持的 ALPN 协议列表，或者客户端和服务器之间没有共同协议，则返回 null。

# X509Certificate

```dart
abstract interface class X509Certificate {}
```

X509Certificate 表示一个 SSL 证书，并提供访问器以获取证书的各个字段。

### der

```dart
Uint8List get der
```

证书的 DER 编码字节。

### pem

```dart
String get pem
```

证书的 PEM 编码字符串。

### sha1

```dart
Uint8List get sha1
```

证书的 SHA1 哈希值。

### subject

```dart
String get subject
```

### issuer

```dart
String get issuer
```

### startValidity

```dart
DateTime get startValidity
```

### endValidity

```dart
DateTime get endValidity
```

# TlsException

```dart
class TlsException implements IOException {}
```

由 TLS/SSL 协议失败引起的安全网络异常。

### type

```dart
String type
```

### message

```dart
String message
```

### osError

```dart
OSError? osError
```

### TlsException()

```dart
TlsException([String message = "", OSError? osError])
```

### toString()

```dart
String toString()
```

# HandshakeException

```dart
class HandshakeException extends TlsException {}
```

在建立安全网络连接的握手阶段发生的异常。

### HandshakeException()

```dart
HandshakeException([String message = "", OSError? osError])
```

# CertificateException

```dart
class CertificateException extends TlsException {}
```

在建立安全网络连接的握手阶段，查找或验证证书时发生的异常。

### CertificateException()

```dart
CertificateException([String message = "", OSError? osError])
```
