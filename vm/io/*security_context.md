# TlsProtocolVersion

```dart
class TlsProtocolVersion {}
```

传输层安全性（TLS）版本。

仅包含 `dart:io` 支持的 TLS 版本。

### tls1_2

```dart
dynamic tls1_2
```

传输层安全性（TLS）协议版本 1.2。

参见 RFC-5246。

### tls1_3

```dart
dynamic tls1_3
```

传输层安全性（TLS）协议版本 1.3。

参见 RFC-8446。

# SecurityContext

```dart
abstract final class SecurityContext {}
```

包含建立安全客户端连接时要信任的证书，以及安全服务器提供服务时使用的证书链和私钥的对象。

[SecureSocket] 和 [SecureServerSocket] 类将 SecurityContext 作为其 connect 和 bind 方法的参数。

可以通过 PEM 或 PKCS12 容器向 SecurityContext 添加证书和密钥。

iOS 注意事项：一些添加、删除和查看证书的方法尚未实现。不过，可以通过 [SecurityContext.defaultContext] 使用平台内置的受信任证书。

### SecurityContext()

```dart
SecurityContext({bool withTrustedRoots = false})
```

创建一个新的 [SecurityContext]。

默认情况下，创建的 [SecurityContext] 不包含任何密钥或证书。可以通过调用此类的方法来添加它们。

如果 [withTrustedRoots] 传入 `true`，[SecurityContext] 将使用下文所述提供的受信任根证书进行初始化。要获取包含受信任根证书的 [SecurityContext]，通常使用 [SecurityContext.defaultContext] 就已足够，应优先使用它。但是，如果必须针对每个连接修改包含受信任根证书的 [SecurityContext]，则应使用 [withTrustedRoots]。

### defaultContext

```dart
SecurityContext get defaultContext
```

大多数需要安全上下文的操作所使用的默认安全上下文。

带有可选 `context` 参数的安全网络类，在省略该参数时使用 [defaultContext] 对象。也可以直接访问和修改该对象。每个 isolate 拥有不同的 [defaultContext] 对象。[defaultContext] 对象使用一份知名受信任证书颁发机构列表作为其受信任根证书。在 Linux 和 Windows 上，此列表取自 Mozilla，由其作为 Firefox 的一部分维护。在 MacOS、iOS 和 Android 上，此列表来自平台内置的受信任证书存储。

### usePrivateKey()

```dart
void usePrivateKey(String file, {String? password})
```

设置服务器证书或客户端证书的私钥。

使用此 SecurityContext 的安全连接将使用此密钥与服务器或客户端证书一起对消息进行签名和解密。[file] 是包含使用 [password] 加密的私钥的 PEM 或 PKCS12 文件的路径。假设格式正确，[file] 的所有其他内容都将被忽略。也可以使用未加密的文件，但这并不常见。

注意：此函数调用 [File.readAsBytesSync]，将阻塞于文件 IO。建议使用 [usePrivateKeyBytes]。

iOS 注意事项：仅支持 PKCS12 数据。它应同时包含私钥和证书链。在 iOS 上，使用一次带此数据的 [usePrivateKey] 调用即可，取代对 [useCertificateChain] 和 [usePrivateKey] 的两次调用。

### usePrivateKeyBytes()

```dart
void usePrivateKeyBytes(List<int> keyBytes, {String? password})
```

设置服务器证书或客户端证书的私钥。

与 [usePrivateKey] 类似，但接受文件内容的字节列表。

### setTrustedCertificates()

```dart
void setTrustedCertificates(String file, {String? password})
```

向 [SecureSocket] 客户端连接使用的受信任 X509 证书集合中添加证书。

[file] 是包含 X509 证书的 PEM 或 PKCS12 文件的路径，通常是来自证书颁发机构的根证书。对于 PKCS12 文件，[password] 是该文件的密码。对于 PEM 文件，[password] 将被忽略。假设格式正确，[file] 的所有其他内容都将被忽略。

注意：此函数调用 [File.readAsBytesSync]，将阻塞于文件 IO。建议使用 [setTrustedCertificatesBytes]。

iOS 注意事项：在 iOS 上，此调用仅接受单个 DER 编码的 X509 证书的字节。可以多次调用它，以向上下文添加多个受信任证书。可以使用 openssl 工具从 PEM 编码的证书获取 DER 编码的证书：

```bash
$ openssl x509 -outform der -in cert.pem -out cert.der
```

### setTrustedCertificatesBytes()

```dart
void setTrustedCertificatesBytes(List<int> certBytes, {String? password})
```

向 [SecureSocket] 客户端连接使用的受信任 X509 证书集合中添加证书。

与 [setTrustedCertificates] 类似，但接受文件内容。

### useCertificateChain()

```dart
void useCertificateChain(String file, {String? password})
```

设置 [SecureServerSocket] 在建立安全连接时提供的 X509 证书链，包括服务器证书。

[file] 是包含 X509 证书的 PEM 或 PKCS12 文件，从根证书颁发机构和中间证书颁发机构开始，形成签名链直至服务器证书，最后以服务器证书结束。服务器证书的私钥由 [usePrivateKey] 设置。对于 PKCS12 文件，[password] 是该文件的密码。对于 PEM 文件，[password] 将被忽略。假设格式正确，[file] 的所有其他内容都将被忽略。

注意：此函数调用 [File.readAsBytesSync]，将阻塞于文件 IO。建议使用 [useCertificateChainBytes]。

iOS 注意事项：如上所述，[usePrivateKey] 同时完成了该调用和此调用的工作。在 iOS 上，此调用为空操作。

### useCertificateChainBytes()

```dart
void useCertificateChainBytes(List<int> chainBytes, {String? password})
```

设置 [SecureServerSocket] 在建立安全连接时提供的 X509 证书链，包括服务器证书。

与 [useCertificateChain] 类似，但接受文件内容。

### setClientAuthorities()

```dart
void setClientAuthorities(String file, {String? password})
```

设置 [SecureServerSocket] 在向连接的客户端请求客户端证书时，将通告为可接受的颁发机构名称列表。

[file] 是包含可接受签名颁发机构证书的 PEM 或 PKCS12 文件——颁发机构名称从证书中提取。对于 PKCS12 文件，[password] 是该文件的密码。对于 PEM 文件，[password] 将被忽略。假设格式正确，[file] 的所有其他内容都将被忽略。

注意：此函数调用 [File.readAsBytesSync]，将阻塞于文件 IO。建议使用 [setClientAuthoritiesBytes]。

iOS 注意事项：不支持此调用。

### setClientAuthoritiesBytes()

```dart
void setClientAuthoritiesBytes(List<int> authCertBytes, {String? password})
```

设置 [SecureServerSocket] 在向连接的客户端请求客户端证书时，将通告为可接受的颁发机构名称列表。

与 [setClientAuthorities] 类似，但接受文件内容。

### alpnSupported

```dart
bool get alpnSupported
```

平台是否支持 ALPN。此值始终返回 true，并将在未来版本中移除。

### setAlpnProtocols()

```dart
void setAlpnProtocols(List<String> protocols, bool isServer)
```

设置客户端连接或服务器连接所支持的应用层协议列表。TLS 的 ALPN（应用层协议协商）扩展允许客户端在 TLS client hello 消息中发送协议列表，服务器从中选择一个并在其 server hello 消息中送回所选协议。

可以使用同一个 SecurityContext，为客户端连接和服务器连接分别发送不同的协议列表。可选布尔参数 [isServer] 指定是为服务器连接还是为客户端连接设置该列表。

### allowLegacyUnsafeRenegotiation

```dart
bool allowLegacyUnsafeRenegotiation
```

如果为 `true`，[SecurityContext] 将允许 TLS 重新协商。重新协商仅在作为客户端时受支持，并且 HelloRequest 必须在应用协议的空闲点收到。这足以支持在（非流水线的）HTTP/1.1 中，在一个 HTTP 请求和响应之间请求新客户端证书的旧用例。注意：重新协商是一个存在严重问题的协议特性，应仅在已知安全的环境中用于与旧版服务器通信。

### minimumTlsProtocolVersion

```dart
TlsProtocolVersion minimumTlsProtocolVersion
```

建立安全连接时使用的最低 TLS 版本。

如果对端不支持 `minimumTlsProtocolVersion` 或更高版本，[SecureSocket.connect] 将抛出 [TlsException]。

如果更改了此值，仅影响新的连接。现有连接将继续使用与对端协商好的协议。

默认值为 [TlsProtocolVersion.tls1_2]。
