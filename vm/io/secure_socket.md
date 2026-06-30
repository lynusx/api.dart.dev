# SecureSocket

```dart
abstract interface class SecureSocket implements Socket {}
```

A TCP socket using TLS and SSL.

See [Socket] for more information.

### connect()

```dart
Future<SecureSocket> connect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols, Duration? timeout})
```

Constructs a new secure client socket and connects it to the given [host] on port [port].

The returned Future will complete with a [SecureSocket] that is connected and ready for subscription.

The certificate provided by the server is checked using the trusted certificates set in the SecurityContext object. The default SecurityContext object contains a built-in set of trusted root certificates for well-known certificate authorities.

[onBadCertificate] is an optional handler for unverifiable certificates. The handler receives the [X509Certificate], and can inspect it and decide (or let the user decide) whether to accept the connection or not. The handler should return true to continue the [SecureSocket] connection.

[keyLog] is an optional callback that will be called when new TLS keys are exchanged with the server. [keyLog] will receive one line of text in [NSS Key Log Format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) for each call. Writing these lines to a file will allow tools (such as [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)) to decrypt content sent through this socket. This is meant to allow network-level debugging of secure sockets and should not be used in production code. For example:

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] is an optional list of protocols (in decreasing order of preference) to use during the ALPN protocol negotiation with the server. Example values are "http/1.1" or "h2". The selected protocol can be obtained via [SecureSocket.selectedProtocol].

The argument [timeout] is used to specify the maximum allowed time to wait for a connection to be established. If [timeout] is longer than the system level timeout duration, a timeout may occur sooner than specified in [timeout]. On timeout, a [SocketException] is thrown and all ongoing connection attempts to [host] are cancelled.

### startConnect()

```dart
Future<ConnectionTask<SecureSocket>> startConnect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

Like [connect], but returns a [Future] that completes with a [ConnectionTask] that can be cancelled if the [SecureSocket] is no longer needed.

### secure()

```dart
Future<SecureSocket> secure(Socket socket, {dynamic host, SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

Initiates TLS on an existing connection.

Takes an already connected [socket] and starts client side TLS handshake to make the communication secure. When the returned future completes the [SecureSocket] has completed the TLS handshake. Using this function requires that the other end of the connection is prepared for TLS handshake.

If the [socket] already has a subscription, this subscription will no longer receive and events. In most cases calling [StreamSubscription.pause] on this subscription before starting TLS handshake is the right thing to do.

The given [socket] is closed and may not be used anymore.

If the [host] argument is passed it will be used as the host name for the TLS handshake. If [host] is not passed the host name from the [socket] will be used. The [host] can be either a [String] or an [InternetAddress].

[onBadCertificate] is an optional handler for unverifiable certificates. The handler receives the [X509Certificate], and can inspect it and decide (or let the user decide) whether to accept the connection or not. The handler should return true to continue the [SecureSocket] connection.

[keyLog] is an optional callback that will be called when new TLS keys are exchanged with the server. [keyLog] will receive one line of text in [NSS Key Log Format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) for each call. Writing these lines to a file will allow tools (such as [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)) to decrypt content sent through this socket. This is meant to allow network-level debugging of secure sockets and should not be used in production code. For example:

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] is an optional list of protocols (in decreasing order of preference) to use during the ALPN protocol negotiation with the server. Example values are "http/1.1" or "h2". The selected protocol can be obtained via [SecureSocket.selectedProtocol].

Calling this function will _not_ cause a DNS host lookup. If the [host] passed is a [String], the [InternetAddress] for the resulting [SecureSocket] will have the passed in [host] as its host value and the internet address of the already connected socket as its address value.

See [connect] for more information on the arguments.

### secureServer()

```dart
Future<SecureSocket> secureServer(Socket socket, SecurityContext? context, {List<int>? bufferedData, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols})
```

Initiates TLS on an existing server connection.

Takes an already connected [socket] and starts server side TLS handshake to make the communication secure. When the returned future completes the [SecureSocket] has completed the TLS handshake. Using this function requires that the other end of the connection is going to start the TLS handshake.

If the [socket] already has a subscription, this subscription will no longer receive and events. In most cases calling [StreamSubscription.pause] on this subscription before starting TLS handshake is the right thing to do.

If some of the data of the TLS handshake has already been read from the socket this data can be passed in the [bufferedData] parameter. This data will be processed before any other data available on the socket.

See [SecureServerSocket.bind] for more information on the arguments.

### peerCertificate

```dart
X509Certificate? get peerCertificate
```

The peer certificate for a connected SecureSocket.

If this [SecureSocket] is the server end of a secure socket connection, [peerCertificate] will return the client certificate, or `null` if no client certificate was received. If this socket is the client end, [peerCertificate] will return the server's certificate.

### selectedProtocol

```dart
String? get selectedProtocol
```

The protocol which was selected during ALPN protocol negotiation.

Returns `null` if one of the peers does not have support for ALPN, did not specify a list of supported ALPN protocols or there was no common protocol between client and server.

### renegotiate()

```dart
void renegotiate({bool useSessionCache = true, bool requestClientCertificate = false, bool requireClientCertificate = false})
```

Does nothing.

The original intent was to allow TLS renegotiation of existing secure connections.

# RawSecureSocket

```dart
abstract interface class RawSecureSocket implements RawSocket {}
```

`RawSecureSocket` provides a secure (SSL or TLS) network connection.

Client connections to a server are provided by calling RawSecureSocket.connect. A secure server, created with [RawSecureServerSocket], also returns `RawSecureSocket` objects representing the server end of a secure connection. The certificate provided by the server is checked using the trusted certificates set in the [SecurityContext] object. The default [SecurityContext] object contains a built-in set of trusted root certificates for well-known certificate authorities.

See [RawSocket] for more information.

### connect()

```dart
Future<RawSecureSocket> connect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols, Duration? timeout})
```

Constructs a new secure client socket and connect it to the given host on the given port.

The returned [Future] is completed with the [RawSecureSocket] when it is connected and ready for subscription.

The certificate provided by the server is checked using the trusted certificates set in the SecurityContext object If a certificate and key are set on the client, using [SecurityContext.useCertificateChain] and [SecurityContext.usePrivateKey], and the server asks for a client certificate, then that client certificate is sent to the server.

[onBadCertificate] is an optional handler for unverifiable certificates. The handler receives the [X509Certificate], and can inspect it and decide (or let the user decide) whether to accept the connection or not. The handler should return true to continue the [RawSecureSocket] connection.

[onBadCertificate] is an optional handler for unverifiable certificates. The handler receives the [X509Certificate], and can inspect it and decide (or let the user decide) whether to accept the connection or not. The handler should return true to continue the [SecureSocket] connection.

[keyLog] is an optional callback that will be called when new TLS keys are exchanged with the server. [keyLog] will receive one line of text in [NSS Key Log Format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) for each call. Writing these lines to a file will allow tools (such as [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)) to decrypt content sent through this socket. This is meant to allow network-level debugging of secure sockets and should not be used in production code. For example:

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] is an optional list of protocols (in decreasing order of preference) to use during the ALPN protocol negotiation with the server. Example values are "http/1.1" or "h2". The selected protocol can be obtained via [RawSecureSocket.selectedProtocol].

### startConnect()

```dart
Future<ConnectionTask<RawSecureSocket>> startConnect(dynamic host, int port, {SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

Like [connect], but returns a [Future] that completes with a [ConnectionTask] that can be cancelled if the [RawSecureSocket] is no longer needed.

### secure()

```dart
Future<RawSecureSocket> secure(RawSocket socket, {StreamSubscription<RawSocketEvent>? subscription, dynamic host, SecurityContext? context, bool onBadCertificate(X509Certificate certificate), void keyLog(String line), List<String>? supportedProtocols})
```

Initiates TLS on an existing connection.

Takes an already connected [socket] and starts client side TLS handshake to make the communication secure. When the returned future completes the [RawSecureSocket] has completed the TLS handshake. Using this function requires that the other end of the connection is prepared for TLS handshake.

If the [socket] already has a subscription, pass the existing subscription in the [subscription] parameter. The [secure] operation will take over the subscription by replacing the handlers with it own secure processing. The caller must not touch this subscription anymore. Passing a paused subscription is an error.

If the [host] argument is passed it will be used as the host name for the TLS handshake. If [host] is not passed the host name from the [socket] will be used. The [host] can be either a [String] or an [InternetAddress].

[onBadCertificate] is an optional handler for unverifiable certificates. The handler receives the [X509Certificate], and can inspect it and decide (or let the user decide) whether to accept the connection or not. The handler should return true to continue the [SecureSocket] connection.

[keyLog] is an optional callback that will be called when new TLS keys are exchanged with the server. [keyLog] will receive one line of text in [NSS Key Log Format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) for each call. Writing these lines to a file will allow tools (such as [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption)) to decrypt content sent through this socket. This is meant to allow network-level debugging of secure sockets and should not be used in production code. For example:

```dart
final log = File('keylog.txt');
final socket = await SecureSocket.connect('www.example.com', 443,
    keyLog: (line) => log.writeAsStringSync(line, mode: FileMode.append));
```

[supportedProtocols] is an optional list of protocols (in decreasing order of preference) to use during the ALPN protocol negotiation with the server. Example values are "http/1.1" or "h2". The selected protocol can be obtained via [SecureSocket.selectedProtocol].

Calling this function will _not_ cause a DNS host lookup. If the [host] passed is a [String] the [InternetAddress] for the resulting [SecureSocket] will have this passed in [host] as its host value and the internet address of the already connected socket as its address value.

See [connect] for more information on the arguments.

### secureServer()

```dart
Future<RawSecureSocket> secureServer(RawSocket socket, SecurityContext? context, {StreamSubscription<RawSocketEvent>? subscription, List<int>? bufferedData, bool requestClientCertificate = false, bool requireClientCertificate = false, List<String>? supportedProtocols})
```

Initiates TLS on an existing server connection.

Takes an already connected [socket] and starts server side TLS handshake to make the communication secure. When the returned future completes the [RawSecureSocket] has completed the TLS handshake. Using this function requires that the other end of the connection is going to start the TLS handshake.

If the [socket] already has a subscription, pass the existing subscription in the [subscription] parameter. The [secureServer] operation will take over the subscription by replacing the handlers with it own secure processing. The caller must not touch this subscription anymore. Passing a paused subscription is an error.

If some of the data of the TLS handshake has already been read from the socket this data can be passed in the [bufferedData] parameter. This data will be processed before any other data available on the socket.

See [RawSecureServerSocket.bind] for more information on the arguments.

### renegotiate()

```dart
void renegotiate({bool useSessionCache = true, bool requestClientCertificate = false, bool requireClientCertificate = false})
```

Does nothing.

The original intent was to allow TLS renegotiation of existing secure connections.

### peerCertificate

```dart
X509Certificate? get peerCertificate
```

Get the peer certificate for a connected RawSecureSocket. If this RawSecureSocket is the server end of a secure socket connection, [peerCertificate] will return the client certificate, or null, if no client certificate was received. If it is the client end, [peerCertificate] will return the server's certificate.

### selectedProtocol

```dart
String? get selectedProtocol
```

The protocol which was selected during protocol negotiation.

Returns null if one of the peers does not have support for ALPN, did not specify a list of supported ALPN protocols or there was no common protocol between client and server.

# X509Certificate

```dart
abstract interface class X509Certificate {}
```

X509Certificate represents an SSL certificate, with accessors to get the fields of the certificate.

### der

```dart
Uint8List get der
```

The DER encoded bytes of the certificate.

### pem

```dart
String get pem
```

The PEM encoded String of the certificate.

### sha1

```dart
Uint8List get sha1
```

The SHA1 hash of the certificate.

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

A secure networking exception caused by a failure in the TLS/SSL protocol.

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

An exception that happens in the handshake phase of establishing a secure network connection.

### HandshakeException()

```dart
HandshakeException([String message = "", OSError? osError])
```

# CertificateException

```dart
class CertificateException extends TlsException {}
```

An exception that happens in the handshake phase of establishing a secure network connection, when looking up or verifying a certificate.

### CertificateException()

```dart
CertificateException([String message = "", OSError? osError])
```
