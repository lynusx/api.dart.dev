# InternetAddressType

```dart
final class InternetAddressType {}
```

The type, or address family, of an [InternetAddress].

Currently, IP version 4 (IPv4), IP version 6 (IPv6) and Unix domain address are supported. Unix domain sockets are available only on Linux, MacOS and Android.

### IPv4

```dart
InternetAddressType IPv4
```

### IPv6

```dart
InternetAddressType IPv6
```

### unix

```dart
InternetAddressType unix
```

### any

```dart
InternetAddressType any
```

### name

```dart
String get name
```

Get the name of the type, e.g. "IPv4" or "IPv6".

### toString()

```dart
String toString()
```

# InternetAddress

```dart
abstract interface class InternetAddress {}
```

An internet address or a Unix domain address.

This object holds an internet address. If this internet address is the result of a DNS lookup, the address also holds the hostname used to make the lookup. An Internet address combined with a port number represents an endpoint to which a socket can connect or a listening socket can bind.

### loopbackIPv4

```dart
InternetAddress get loopbackIPv4
```

IP version 4 loopback address.

Use this address when listening on or connecting to the loopback adapter using IP version 4 (IPv4).

### loopbackIPv6

```dart
InternetAddress get loopbackIPv6
```

IP version 6 loopback address.

Use this address when listening on or connecting to the loopback adapter using IP version 6 (IPv6).

### anyIPv4

```dart
InternetAddress get anyIPv4
```

IP version 4 any address.

Use this address when listening on the addresses of all adapters using IP version 4 (IPv4).

### anyIPv6

```dart
InternetAddress get anyIPv6
```

IP version 6 any address.

Use this address when listening on the addresses of all adapters using IP version 6 (IPv6).

### type

```dart
InternetAddressType get type
```

The address family of the [InternetAddress].

### address

```dart
String get address
```

The numeric address of the host.

For IPv4 addresses this is using the dotted-decimal notation. For IPv6 it is using the hexadecimal representation. For Unix domain addresses, this is a file path.

### host

```dart
String get host
```

The host used to lookup the address.

If there is no host associated with the address this returns the [address].

### rawAddress

```dart
Uint8List get rawAddress
```

The raw address of this [InternetAddress].

For an IP address, the result is either a 4 or 16 byte long list. For a Unix domain address, UTF-8 encoded byte sequences that represents [address] is returned.

The returned list is a fresh copy, making it possible to change the list without modifying the [InternetAddress].

### isLoopback

```dart
bool get isLoopback
```

Whether the [InternetAddress] is a loopback address.

### isLinkLocal

```dart
bool get isLinkLocal
```

Whether the scope of the [InternetAddress] is a link-local.

### isMulticast

```dart
bool get isMulticast
```

Whether the scope of the [InternetAddress] is multicast.

### InternetAddress()

```dart
InternetAddress(String address, {InternetAddressType? type})
```

Creates a new [InternetAddress] from a numeric address or a file path.

If [type] is [InternetAddressType.IPv4], [address] must be a numeric IPv4 address (dotted-decimal notation). If [type] is [InternetAddressType.IPv6], [address] must be a numeric IPv6 address (hexadecimal notation). If [type] is [InternetAddressType.unix], [address] must be a valid file path. If [type] is omitted, [address] must be either a numeric IPv4 or IPv6 address and the type is inferred from the format.

### InternetAddress.fromRawAddress()

```dart
InternetAddress.fromRawAddress(Uint8List rawAddress, {InternetAddressType? type})
```

Creates a new [InternetAddress] from the provided raw address bytes.

If the [type] is [InternetAddressType.IPv4], the [rawAddress] must have length 4. If the [type] is [InternetAddressType.IPv6], the [rawAddress] must have length 16. If the [type] is [InternetAddressType.unix], the [rawAddress] must be a valid UTF-8 encoded file path.

If [type] is omitted, the [rawAddress] must have a length of either 4 or 16, in which case the type defaults to [InternetAddressType.IPv4] or [InternetAddressType.IPv6] respectively.

### reverse()

```dart
Future<InternetAddress> reverse()
```

Performs a reverse DNS lookup on this [address]

Returns a new [InternetAddress] with the same address, but where the [host] field set to the result of the lookup.

If this address is Unix domain addresses, no lookup is performed and this address is returned directly.

### lookup()

```dart
Future<List<InternetAddress>> lookup(String host, {InternetAddressType type = InternetAddressType.any})
```

Looks up the addresses of a host.

If [type] is [InternetAddressType.any], it will lookup both IP version 4 (IPv4) and IP version 6 (IPv6) addresses. If [type] is either [InternetAddressType.IPv4] or [InternetAddressType.IPv6] it will only lookup addresses of the specified type. The order of the list can, and most likely will, change over time.

### tryParse()

```dart
InternetAddress? tryParse(String address)
```

Attempts to parse [address] as a numeric address.

Returns `null` If [address] is not a numeric IPv4 (dotted-decimal notation) or IPv6 (hexadecimal representation) address.

# InterfaceAddress

```dart
abstract interface class InterfaceAddress implements InternetAddress {}
```

An [InternetAddress] bound to a [NetworkInterface], extended with network configuration details.

In addition to the address itself, provides the prefix length (also known as subnet mask length) and, for IPv4 addresses, the broadcast address of the network.

### prefixLength

```dart
int get prefixLength
```

The prefix length of the network, also known as the subnet mask length.

For example, a prefix length of `24` corresponds to a subnet mask of `255.255.255.0` in IPv4. For IPv6, a prefix length of `64` means the first 64 bits of the address are the network prefix.

### broadcast

```dart
InternetAddress? get broadcast
```

The broadcast address of this network.

Only IPv4 networks have broadcast addresses. Returns `null` for IPv6 addresses.

# NetworkInterface

```dart
abstract interface class NetworkInterface {}
```

A [NetworkInterface] represents an active network interface on the current system. It contains a list of [InterfaceAddress]es that are bound to the interface.

### name

```dart
String get name
```

The name of the [NetworkInterface].

### index

```dart
int get index
```

The index of the [NetworkInterface].

### addresses

```dart
List<InterfaceAddress> get addresses
```

The list of [InterfaceAddress]es currently bound to this [NetworkInterface].

### listSupported

```dart
bool get listSupported
```

Whether the [list] method is supported.

The [list] method is supported on all platforms supported by Dart so this property is always true.

### list()

```dart
Future<List<NetworkInterface>> list({bool includeLoopback = false, bool includeLinkLocal = false, InternetAddressType type = InternetAddressType.any})
```

Query the system for [NetworkInterface]s.

If [includeLoopback] is `true`, the returned list will include the loopback device. Default is `false`.

If [includeLinkLocal] is `true`, the list of addresses of the returned [NetworkInterface]s, may include link local addresses. Default is `false`.

If [type] is either [InternetAddressType.IPv4] or [InternetAddressType.IPv6] it will only lookup addresses of the specified type. Default is [InternetAddressType.any].

# RawServerSocket

```dart
abstract interface class RawServerSocket implements Stream<RawSocket> {}
```

A listening socket.

A `RawServerSocket` and provides a stream of low-level [RawSocket] objects, one for each connection made to the listening socket.

See [RawSocket] for more information.

### bind()

```dart
Future<RawServerSocket> bind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

Listens on a given address and port.

When the returned future completes the server socket is bound to the given [address] and [port] and has started listening on it.

The [address] can either be a [String] or an [InternetAddress]. If [address] is a [String], [bind] will perform a [InternetAddress.lookup] and use the first value in the list. To listen on the loopback adapter, which will allow only incoming connections from the local host, use the value [InternetAddress.loopbackIPv4] or [InternetAddress.loopbackIPv6]. To allow for incoming connection from the network use either one of the values [InternetAddress.anyIPv4] or [InternetAddress.anyIPv6] to bind to all interfaces or the IP address of a specific interface.

If an IP version 6 (IPv6) address is used, both IP version 6 (IPv6) and version 4 (IPv4) connections will be accepted. To restrict this to version 6 (IPv6) only, use [v6Only] to set version 6 only.

If [port] has the value `0` an ephemeral port will be chosen by the system. The actual port used can be retrieved using the `port` getter.

The optional argument [backlog] can be used to specify the listen backlog for the underlying OS listen setup. If [backlog] has the value of `0` (the default) a reasonable value will be chosen by the system.

The optional argument [shared] specifies whether additional RawServerSocket objects can bind to the same combination of [address], [port] and [v6Only]. If [shared] is `true` and more [RawServerSocket]s from this isolate or other isolates are bound to the port, then the incoming connections will be distributed among all the bound [RawServerSocket]s. Connections can be distributed over multiple isolates this way.

### port

```dart
int get port
```

The port used by this socket.

### address

```dart
InternetAddress get address
```

The address used by this socket.

### close()

```dart
Future<RawServerSocket> close()
```

Closes the socket.

The returned future completes when the socket is fully closed and is no longer bound.

# ServerSocket

```dart
abstract interface class ServerSocket implements ServerSocketBase<Socket> {}
```

A listening socket.

A [ServerSocket] provides a stream of [Socket] objects, one for each connection made to the listening socket.

See [Socket] for more info.

### bind()

```dart
Future<ServerSocket> bind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

Listens on a given address and port.

When the returned future completes the server socket is bound to the given [address] and [port] and has started listening on it.

The [address] can either be a [String] or an [InternetAddress]. If [address] is a [String], [bind] will perform a [InternetAddress.lookup] and use the first value in the list. To listen on the loopback adapter, which will allow only incoming connections from the local host, use the value [InternetAddress.loopbackIPv4] or [InternetAddress.loopbackIPv6]. To allow for incoming connection from the network use either one of the values [InternetAddress.anyIPv4] or [InternetAddress.anyIPv6] to bind to all interfaces or the IP address of a specific interface.

If an IP version 6 (IPv6) address is used, both IP version 6 (IPv6) and version 4 (IPv4) connections will be accepted. To restrict this to version 6 (IPv6) only, use [v6Only] to set version 6 only.

If [port] has the value `0` an ephemeral port will be chosen by the system. The actual port used can be retrieved using the [port] getter.

The optional argument [backlog] can be used to specify the listen backlog for the underlying OS listen setup. If [backlog] has the value of `0` (the default) a reasonable value will be chosen by the system.

The optional argument [shared] specifies whether additional ServerSocket objects can bind to the same combination of [address], [port] and [v6Only]. If [shared] is `true` and more server sockets from this isolate or other isolates are bound to the port, then the incoming connections will be distributed among all the bound server sockets. Connections can be distributed over multiple isolates this way.

### port

```dart
int get port
```

The port used by this socket.

### address

```dart
InternetAddress get address
```

The address used by this socket.

### close()

```dart
Future<ServerSocket> close()
```

Closes the socket.

The returned future completes when the socket is fully closed and is no longer bound.

# SocketDirection

```dart
final class SocketDirection {}
```

The [SocketDirection] is used as a parameter to [Socket.close] and [RawSocket.close] to close a socket in the specified direction(s).

### receive

```dart
SocketDirection receive
```

### send

```dart
SocketDirection send
```

### both

```dart
SocketDirection both
```

# SocketOption

```dart
final class SocketOption {}
```

An option for a socket which is configured using [Socket.setOption].

The [SocketOption] is used as a parameter to [Socket.setOption] and [RawSocket.setOption] to customize the behaviour of the underlying socket.

### tcpNoDelay

```dart
SocketOption tcpNoDelay
```

Enable or disable no-delay on the socket. If tcpNoDelay is enabled, the socket will not buffer data internally, but instead write each data chunk as an individual TCP packet.

tcpNoDelay is disabled by default.

# RawSocketOption

```dart
final class RawSocketOption {}
```

The [RawSocketOption] is used as a parameter to [Socket.setRawOption] and [RawSocket.setRawOption] to customize the behaviour of the underlying socket.

It allows for fine grained control of the socket options, and its values will be passed to the underlying platform's implementation of setsockopt and getsockopt.

### RawSocketOption()

```dart
RawSocketOption(int level, int option, Uint8List value)
```

Creates a [RawSocketOption] for [RawSocket.getRawOption] and [RawSocket.setRawOption].

The [level] and [option] arguments correspond to `level` and `optname` arguments on the `getsockopt()` and `setsockopt()` native calls.

The value argument and its length correspond to the optval and length arguments on the native call.

For a [RawSocket.getRawOption] call, the value parameter will be updated after a successful call (although its length will not be changed).

For a [RawSocket.setRawOption] call, the value parameter will be used set the option.

### RawSocketOption.fromInt()

```dart
RawSocketOption.fromInt(int level, int option, int value)
```

Convenience constructor for creating an integer based [RawSocketOption].

### RawSocketOption.fromBool()

```dart
RawSocketOption.fromBool(int level, int option, bool value)
```

Convenience constructor for creating a boolean based [RawSocketOption].

### level

```dart
int level
```

The level for the option to set or get.

See also:

- [RawSocketOption.levelSocket]
- [RawSocketOption.levelIPv4]
- [RawSocketOption.levelIPv6]
- [RawSocketOption.levelTcp]
- [RawSocketOption.levelUdp]

### option

```dart
int option
```

The numeric ID of the option to set or get.

### value

```dart
Uint8List value
```

The raw data to set, or the array to write the current option value into.

This list must be the correct length for the expected option. For most options that take [int] or [bool] values, the length should be 4. For options that expect a struct (such as an in_addr_t), the length should be the correct length for that struct.

### levelSocket

```dart
int get levelSocket
```

Socket level option for `SOL_SOCKET`.

### levelIPv4

```dart
int get levelIPv4
```

Socket level option for `IPPROTO_IP`.

### IPv4MulticastInterface

```dart
int get IPv4MulticastInterface
```

Socket option for `IP_MULTICAST_IF`.

### levelIPv6

```dart
int get levelIPv6
```

Socket level option for `IPPROTO_IPV6`.

### IPv6MulticastInterface

```dart
int get IPv6MulticastInterface
```

Socket option for `IPV6_MULTICAST_IF`.

### levelTcp

```dart
int get levelTcp
```

Socket level option for `IPPROTO_TCP`.

### levelUdp

```dart
int get levelUdp
```

Socket level option for `IPPROTO_UDP`.

# RawSocketEvent

```dart
class RawSocketEvent {}
```

Events for the [RawDatagramSocket], [RawSecureSocket], and [RawSocket].

These event objects are used by the [Stream] behavior of the sockets (for example [RawSocket.listen], [RawSocket.forEach]) when the socket's state change.

```dart
import 'dart:convert';
import 'dart:io';

void main() async {
  final socket = await RawSocket.connect("example.com", 80);

  socket.listen((event) {
    switch (event) {
      case RawSocketEvent.read:
        final data = socket.read();
        if (data != null) {
          print(ascii.decode(data));
        }
        break;
      case RawSocketEvent.write:
        socket.write(ascii.encode('GET /\r\nHost: example.com\r\n\r\n'));
        socket.writeEventsEnabled = false;
        break;
      case RawSocketEvent.readClosed:
        socket.close();
        break;
      case RawSocketEvent.closed:
        break;
      default:
        throw "Unexpected event $event";
    }
  });
}
```

### read

```dart
RawSocketEvent read
```

An event indicates the socket is ready to be read.

### write

```dart
RawSocketEvent write
```

An event indicates the socket is ready to write.

### readClosed

```dart
RawSocketEvent readClosed
```

An event indicates the reading from the socket is closed

### closed

```dart
RawSocketEvent closed
```

An event indicates the socket is closed.

### toString()

```dart
String toString()
```

# ConnectionTask

```dart
final class ConnectionTask<S> {}
```

A cancelable connection attempt.

Returned by the `startConnect` methods on client-side socket types `S`, `ConnectionTask<S>` allows canceling an attempt to connect to a host.

### socket

```dart
Future<S> socket
```

A `Future` that completes with value that `S.connect()` would return unless [cancel] is called on this [ConnectionTask].

If [cancel] is called, the future completes with a [SocketException] error whose message indicates that the connection attempt was cancelled.

### fromSocket()

```dart
ConnectionTask<T> fromSocket<T extends Socket>(Future<T> socket, void Function() onCancel)
```

Create a `ConnectionTask` from an existing `Future<Socket>`.

You can use this method to return existing socket connections in [HttpClient.connectionFactory].

For example:

```dart
final clientSocketFuture = Socket.connect(
    serverUri.host, serverUri.port);
final client = HttpClient()
 ..connectionFactory = (uri, proxyHost, proxyPort) {
   return Future.value(
       ConnectionTask.fromSocket(clientSocketFuture, () {}));
final response = await client.getUrl(serverUri);
```

### cancel()

```dart
void cancel()
```

Cancels the connection attempt.

This also causes the [socket] `Future` to complete with a [SocketException] error.

# RawSocket

```dart
abstract interface class RawSocket implements Stream<RawSocketEvent> {}
```

A TCP connection.

A _socket connection_ connects a _local_ socket to a _remote_ socket. Data, as [Uint8List]s, is received by the local socket and made available by the [read] method, and can be sent to the remote socket through the [write] method.

The [Stream] interface of this class provides event notification about when a certain change has happened, for example when data has become available ([RawSocketEvent.read]) or when the remote end has stopped listening ([RawSocketEvent.closed]).

### readEventsEnabled

```dart
bool readEventsEnabled
```

Set or get, if the [RawSocket] should listen for [RawSocketEvent.read] and [RawSocketEvent.readClosed] events. Default is `true`.

Warning: setting [readEventsEnabled] to `false` might prevent socket from fully closing when [SocketDirection.receive] and [SocketDirection.send] directions are shutdown independently. See [shutdown] for more details.

### writeEventsEnabled

```dart
bool writeEventsEnabled
```

Set or get, if the [RawSocket] should listen for [RawSocketEvent.write] events. Default is `true`. This is a one-shot listener, and writeEventsEnabled must be set to true again to receive another write event.

### connect()

```dart
Future<RawSocket> connect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

Creates a new socket connection to the host and port.

Returns a [Future] that will complete with either a [RawSocket] once connected, or an error if the host-lookup or connection failed.

The [host] can either be a [String] or an [InternetAddress]. If [host] is a [String], [connect] will perform a [InternetAddress.lookup] and try all returned [InternetAddress]es, until connected. If IPv4 and IPv6 addresses are both available then connections over IPv4 are preferred. If no connection can be established then the error from the first failing connection is returned.

The argument [sourceAddress] can be used to specify the local address to bind when making the connection. The [sourceAddress] can either be a [String] or an [InternetAddress]. If a [String] is passed it must hold a numeric IP address.

The [sourcePort] defines the local port to bind to. If [sourcePort] is not specified or zero, a port will be chosen.

The argument [timeout] is used to specify the maximum allowed time to wait for a connection to be established. If [timeout] is longer than the system level timeout duration, a timeout may occur sooner than specified in [timeout]. On timeout, a [SocketException] is thrown and all ongoing connection attempts to [host] are cancelled.

### startConnect()

```dart
Future<ConnectionTask<RawSocket>> startConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

Like [connect], but returns a [Future] that completes with a [ConnectionTask] that can be cancelled if the [RawSocket] is no longer needed.

### available()

```dart
int available()
```

The number of received and non-read bytes in the socket that can be read.

### read()

```dart
Uint8List? read([int? len])
```

Read up to [len] bytes from the socket.

This function is non-blocking and will only return data if data is available. The number of bytes read can be less than [len] if fewer bytes are available for immediate reading. If no data is available `null` is returned.

### readMessage()

```dart
SocketMessage? readMessage([int? count])
```

Reads a message containing up to [count] bytes from the socket.

This function differs from [read] in that it will also return any [SocketControlMessage] that have been sent.

This function is non-blocking and will only return data if data is available. The number of bytes read can be less than [count] if fewer bytes are available for immediate reading. Length of data buffer in [SocketMessage] indicates number of bytes read.

Returns `null` if no data is available.

Unsupported by [RawSecureSocket].

Unsupported on Android, Fuchsia, Windows.

### write()

```dart
int write(List<int> buffer, [int offset = 0, int? count])
```

Writes up to [count] bytes of the buffer from [offset] buffer offset to the socket.

The number of successfully written bytes is returned. This function is non-blocking and will only write data if buffer space is available in the socket. This means that the number of successfully written bytes may be less than `count` or even 0.

Transmission of the buffer may be delayed unless [SocketOption.tcpNoDelay] is set with [RawSocket.setOption].

The default value for [offset] is 0, and the default value for [count] is `buffer.length - offset`.

### sendMessage()

```dart
int sendMessage(List<SocketControlMessage> controlMessages, List<int> data, [int offset = 0, int? count])
```

Writes socket control messages and data bytes to the socket.

Writes [controlMessages] and up to [count] bytes of [data], starting at [offset], to the socket. If [count] is not provided, as many bytes as possible are written. Use [write] instead if no control messages are required to be sent.

When sent control messages are received, they are retained until the next call to [readMessage], where all currently available control messages are provided as part of the returned [SocketMessage]. Calling [read] will read only data bytes, and will not affect control messages.

The [count] must be positive (greater than zero).

Returns the number of bytes written, which cannot be greater than [count], nor greater than `data.length - offset`. Return value of zero indicates that control messages were not sent.

This function is non-blocking and will only write data if buffer space is available in the socket.

Throws an [OSError] if message could not be sent out.

Unsupported by [RawSecureSocket].

Unsupported on Android, Fuchsia, Windows.

### port

```dart
int get port
```

The port used by this socket.

Throws a [SocketException] if the socket is closed.

### remotePort

```dart
int get remotePort
```

The remote port connected to by this socket.

Throws a [SocketException] if the socket is closed.

### address

```dart
InternetAddress get address
```

The [InternetAddress] used to connect this socket.

Throws a [SocketException] if the socket is closed.

### remoteAddress

```dart
InternetAddress get remoteAddress
```

The remote [InternetAddress] connected to by this socket.

Throws a [SocketException] if the socket is closed.

### close()

```dart
Future<RawSocket> close()
```

Closes the socket.

Returns a future that completes with this socket when the underlying connection is completely destroyed.

Calling [close] will never throw an exception and calling it several times is supported. Calling [close] can result in a [RawSocketEvent.readClosed] event.

### shutdown()

```dart
void shutdown(SocketDirection direction)
```

Shuts down the socket in the [direction].

Calling [shutdown] will never throw an exception and calling it several times is supported. Calling shutdown with either [SocketDirection.both] or [SocketDirection.receive] can result in a [RawSocketEvent.readClosed] event.

Warning: [SocketDirection.receive] direction is only considered to be to be fully shutdown once all available data is drained and [RawSocketEvent.readClosed] is dispatched. Shutting down [SocketDirection.receive] and [SocketDirection.send] directions separately without draining the data will lead to socket staying around until the data is drained. This can happen if [readEventsEnabled] is set to `false` or if received data is not [read] in response to these events. This does not apply to shutting down both directions simultaneously using [SocketDirection.both] which will discard all received data instead.

### setOption()

```dart
bool setOption(SocketOption option, bool enabled)
```

Customize the [RawSocket].

See [SocketOption] for available options.

Returns `true` if the option was set successfully, `false` otherwise.

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

Reads low level information about the [RawSocket].

See [RawSocketOption] for available options.

Returns the [RawSocketOption.value] on success.

Throws an [OSError] on failure.

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

Customizes the [RawSocket].

See [RawSocketOption] for available options.

Throws an [OSError] on failure.

# Socket

```dart
abstract interface class Socket implements Stream<Uint8List>, IOSink {}
```

A TCP connection between two sockets.

A _socket connection_ connects a _local_ socket to a _remote_ socket. Data, as [Uint8List]s, is received by the local socket, made available by the [Stream] interface of this class, and can be sent to the remote socket through the [IOSink] interface of this class.

Transmission of the data sent through the [IOSink] interface may be delayed unless [SocketOption.tcpNoDelay] is set with [Socket.setOption].

### connect()

```dart
Future<Socket> connect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

Creates a new socket connection to the host and port and returns a [Future] that will complete with either a [Socket] once connected or an error if the host-lookup or connection failed.

[host] can either be a [String] or an [InternetAddress]. If [host] is a [String], [connect] will perform a [InternetAddress.lookup] and try all returned [InternetAddress]es, until connected. Unless a connection was established, the error from the first failing connection is returned.

The argument [sourceAddress] can be used to specify the local address to bind when making the connection. The [sourceAddress] can either be a [String] or an [InternetAddress]. If a [String] is passed it must hold a numeric IP address.

The [sourcePort] defines the local port to bind to. If [sourcePort] is not specified or zero, a port will be chosen.

The argument [timeout] is used to specify the maximum allowed time to wait for a connection to be established. If [timeout] is longer than the system level timeout duration, a timeout may occur sooner than specified in [timeout]. On timeout, a [SocketException] is thrown and all ongoing connection attempts to [host] are cancelled.

### startConnect()

```dart
Future<ConnectionTask<Socket>> startConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

Like [connect], but returns a [Future] that completes with a [ConnectionTask] that can be cancelled if the [Socket] is no longer needed.

### destroy()

```dart
void destroy()
```

Destroys the socket in both directions.

Calling [destroy] will make the send a close event on the stream and will no longer react on data being piped to it.

Call [close] (inherited from [IOSink]) to only close the [Socket] for sending data.

### setOption()

```dart
bool setOption(SocketOption option, bool enabled)
```

Customizes the [RawSocket].

See [SocketOption] for available options.

Returns `true` if the option was set successfully, false otherwise.

Throws a [SocketException] if the socket has been destroyed or upgraded to a secure socket.

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

Reads low level information about the [RawSocket].

See [RawSocketOption] for available options.

Returns the [RawSocketOption.value] on success.

Throws an [OSError] on failure and a [SocketException] if the socket has been destroyed or upgraded to a secure socket.

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

Customizes the [RawSocket].

See [RawSocketOption] for available options.

Throws an [OSError] on failure and a [SocketException] if the socket has been destroyed or upgraded to a secure socket.

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

Unsupported operation on sockets.

This method, which is inherited from [IOSink], is not supported on sockets, and **must not** be called. Sockets have no way to report errors, so any error passed in to a socket using [addError] would be lost.

### port

```dart
int get port
```

The port used by this socket.

Throws a [SocketException] if the socket is closed. The port is 0 if the socket is a Unix domain socket.

### remotePort

```dart
int get remotePort
```

The remote port connected to by this socket.

Throws a [SocketException] if the socket is closed. The port is 0 if the socket is a Unix domain socket.

### address

```dart
InternetAddress get address
```

The [InternetAddress] used to connect this socket.

Throws a [SocketException] if the socket is closed.

### remoteAddress

```dart
InternetAddress get remoteAddress
```

The remote [InternetAddress] connected to by this socket.

Throws a [SocketException] if the socket is closed.

### close()

```dart
Future close()
```

### done

```dart
Future get done
```

# Datagram

```dart
final class Datagram {}
```

A data packet received by a [RawDatagramSocket].

### data

```dart
Uint8List data
```

The actual bytes of the message.

### address

```dart
InternetAddress address
```

The address of the socket which sends the data.

### port

```dart
int port
```

The port of the socket which sends the data.

### Datagram()

```dart
Datagram(Uint8List data, InternetAddress address, int port)
```

# ResourceHandle

```dart
abstract interface class ResourceHandle {}
```

A wrapper around OS resource handle so it can be passed via Socket as part of [SocketMessage].

### ResourceHandle.fromFile()

```dart
ResourceHandle.fromFile(RandomAccessFile file)
```

Creates wrapper around opened file.

### ResourceHandle.fromSocket()

```dart
ResourceHandle.fromSocket(Socket socket)
```

Creates wrapper around opened socket.

### ResourceHandle.fromRawSocket()

```dart
ResourceHandle.fromRawSocket(RawSocket socket)
```

Creates wrapper around opened raw socket.

### ResourceHandle.fromRawDatagramSocket()

```dart
ResourceHandle.fromRawDatagramSocket(RawDatagramSocket socket)
```

Creates wrapper around opened raw datagram socket.

### ResourceHandle.fromStdin()

```dart
ResourceHandle.fromStdin(Stdin stdin)
```

Creates wrapper around current stdin.

### ResourceHandle.fromStdout()

```dart
ResourceHandle.fromStdout(Stdout stdout)
```

Creates wrapper around current stdout.

### ResourceHandle.fromReadPipe()

```dart
ResourceHandle.fromReadPipe(ReadPipe pipe)
```

### ResourceHandle.fromWritePipe()

```dart
ResourceHandle.fromWritePipe(WritePipe pipe)
```

### toFile()

```dart
RandomAccessFile toFile()
```

Extracts opened file from resource handle.

This can also be used when receiving stdin and stdout handles and read and write pipes.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a file or stdio handle, the behavior of the returned [RandomAccessFile] is completely unspecified. Be very careful to avoid using a handle incorrectly.

### toSocket()

```dart
Socket toSocket()
```

Extracts opened socket from resource handle.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a socket handle, the behavior of the returned [Socket] is completely unspecified. Be very careful to avoid using a handle incorrectly.

### toRawSocket()

```dart
RawSocket toRawSocket()
```

Extracts opened raw socket from resource handle.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a socket handle, the behavior of the returned [RawSocket] is completely unspecified. Be very careful to avoid using a handle incorrectly.

### toRawDatagramSocket()

```dart
RawDatagramSocket toRawDatagramSocket()
```

Extracts opened raw datagram socket from resource handle.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a datagram socket handle, the behavior of the returned [RawDatagramSocket] is completely unspecified. Be very careful to avoid using a handle incorrectly.

### toReadPipe()

```dart
ReadPipe toReadPipe()
```

Extracts a read pipe from resource handle.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a readable pipe, the behavior of the returned [ReadPipe] is completely unspecified. Be very careful to avoid using a handle incorrectly.

### toWritePipe()

```dart
WritePipe toWritePipe()
```

Extracts a write pipe from resource handle.

Since the [ResourceHandle] represents a single OS resource, none of [toFile], [toSocket], [toRawSocket], or [toRawDatagramSocket], [toReadPipe], [toWritePipe], can be called after a call to this method.

If this resource handle is not a writeable pipe, the behavior of the returned [ReadPipe] is completely unspecified. Be very careful to avoid using a handle incorrectly.

# SocketControlMessage

```dart
abstract interface class SocketControlMessage {}
```

Control message part of the [SocketMessage] received by a call to [RawSocket.readMessage].

Control messages could carry different information including [ResourceHandle]. If [ResourceHandle]s are available as part of this message, they can be extracted via [extractHandles].

### SocketControlMessage.fromHandles()

```dart
SocketControlMessage.fromHandles(List<ResourceHandle> handles)
```

Creates a control message containing the provided [handles].

This is used by the sender when it sends handles across the socket. Receiver can extract the handles from the message using [extractHandles].

### extractHandles()

```dart
List<ResourceHandle> extractHandles()
```

Extracts the list of handles embedded in this message.

This method must only be used to extract handles from messages received on a socket. It must not be used on a socket control message that is created locally, and has not been sent using [RawSocket.sendMessage].

This method must only be called once. Calling it multiple times may cause duplicated handles with unspecified behavior.

### level

```dart
int get level
```

A platform specific value used to determine the kind of control message.

Together with [type], these two integers identify the kind of control message in a platform specific way. For example, on Linux certain combinations of these values indicate that this is a control message that carries [ResourceHandle]s.

### type

```dart
int get type
```

A platform specific value used to determine the kind of control message.

Together with [level], these two integers identify the kind of control message in a platform specific way. For example, on Linux certain combinations of these values indicate that this is a control message that carries [ResourceHandle]s.

### data

```dart
Uint8List get data
```

Actual bytes that were passed as part of the control message by the underlying platform.

The bytes are interpreted differently depending on the [level] and [type]. These actual bytes can be used to inspect and interpret non-handle-carrying messages.

# SocketMessage

```dart
final class SocketMessage {}
```

A socket message received by a [RawDatagramSocket].

A socket message consists of [data] bytes and [controlMessages].

### data

```dart
Uint8List data
```

The actual bytes of the message.

### controlMessages

```dart
List<SocketControlMessage> controlMessages
```

The control messages sent as part of this socket message.

This list can be empty.

### SocketMessage()

```dart
SocketMessage(Uint8List data, List<SocketControlMessage> controlMessages)
```

# RawDatagramSocket

```dart
abstract interface class RawDatagramSocket extends Stream<RawSocketEvent> {}
```

An unbuffered interface to a UDP socket.

The raw datagram socket delivers a [Stream] of [RawSocketEvent]s in the same chunks as the underlying operating system receives them.

Note that the event [RawSocketEvent.readClosed] will never be received as an UDP socket cannot be closed by a remote peer.

It is not the same as a [POSIX raw socket](http://man7.org/linux/man-pages/man7/raw.7.html).

```dart
import 'dart:io';
import 'dart:typed_data';

void main() async {
  // Read the current time from an NTP server.
  final serverAddress = (await InternetAddress.lookup('pool.ntp.org')).first;
  final clientSocket = await RawDatagramSocket.bind(
      serverAddress.type == InternetAddressType.IPv6
          ? InternetAddress.anyIPv6
          : InternetAddress.anyIPv4,
      0);
  final ntpQuery = Uint8List(48);
  ntpQuery[0] = 0x23; // See RFC 5905 7.3

  clientSocket.listen((event) {
    switch (event) {
      case RawSocketEvent.read:
        final datagram = clientSocket.receive();
        // Parse `datagram.data`
        clientSocket.close();
        break;
      case RawSocketEvent.write:
        if (clientSocket.send(ntpQuery, serverAddress, 123) > 0) {
          clientSocket.writeEventsEnabled = false;
        }
        break;
      case RawSocketEvent.closed:
        break;
      default:
        throw "Unexpected event $event";
    }
  });
}
```

### readEventsEnabled

```dart
bool readEventsEnabled
```

Whether the [RawDatagramSocket] should listen for [RawSocketEvent.read] events.

Default is `true`.

### writeEventsEnabled

```dart
bool writeEventsEnabled
```

Whether the [RawDatagramSocket] should listen for [RawSocketEvent.write] events.

Default is `true`. This is a one-shot listener, and [writeEventsEnabled] must be set to true again to receive another write event.

### multicastLoopback

```dart
bool multicastLoopback
```

Whether multicast traffic is looped back to the host.

By default multicast loopback is enabled.

### multicastHops

```dart
int multicastHops
```

The maximum network hops for multicast packages originating from this socket.

For IPv4 this is referred to as TTL (time to live).

By default this value is 1 causing multicast traffic to stay on the local network.

### multicastInterface

```dart
NetworkInterface? multicastInterface
```

The network interface used for outgoing multicast packages.

A value of `null` indicate that the system chooses the network interface to use.

By default this value is `null`

### broadcastEnabled

```dart
bool broadcastEnabled
```

Whether IPv4 broadcast is enabled.

IPv4 broadcast needs to be enabled by the sender for sending IPv4 broadcast packages. By default IPv4 broadcast is disabled.

For IPv6 there is no general broadcast mechanism. Use multicast instead.

### bind()

```dart
Future<RawDatagramSocket> bind(dynamic host, int port, {bool reuseAddress = true, bool reusePort = false, int ttl = 1})
```

Binds a socket to the given [host] and [port].

When the socket is bound and has started listening on [port], the returned future completes with the [RawDatagramSocket] of the bound socket.

The [host] can either be a [String] or an [InternetAddress]. If [host] is a [String], [bind] will perform a [InternetAddress.lookup] and use the first value in the list. To listen on the loopback interface, which will allow only incoming connections from the local host, use the value [InternetAddress.loopbackIPv4] or [InternetAddress.loopbackIPv6]. To allow for incoming connection from any network use either one of the values [InternetAddress.anyIPv4] or [InternetAddress.anyIPv6] to bind to all interfaces, or use the IP address of a specific interface.

The [reuseAddress] should be set for all listeners that bind to the same address. Otherwise, it will fail with a [SocketException].

The [reusePort] specifies whether the port can be reused.

The [ttl] sets `time to live` of a datagram sent on the socket.

### port

```dart
int get port
```

The port used by this socket.

### address

```dart
InternetAddress get address
```

The address used by this socket.

### close()

```dart
void close()
```

Closes the datagram socket.

### send()

```dart
int send(List<int> buffer, InternetAddress address, int port)
```

Asynchronously sends a datagram.

Returns the number of bytes written. This will always be either the size of [buffer] or `0`.

A return value of `0` indicates that sending the datagram would block and that the [send] call can be tried again.

A return value of the size of [buffer] indicates that a request to transmit the datagram was made to the operating system. It does not indicate that the operating system successfully sent the datagram. If a local failure to send the datagram occurs then an error event will be added to the [Stream]. If a networking or remote failure occurs then it will not be reported.

The maximum size of a IPv4 UDP datagram is 65535 bytes (including both data and headers) but the practical maximum size is likely to be much lower due to operating system limits and the network's maximum transmission unit (MTU).

Some IPv6 implementations may support payloads up to 4GB (see RFC-2675) but that support is limited (see RFC-6434) and has been removed in later standards (see RFC-8504).

[Emperical testing by the Chromium team](https://groups.google.com/a/chromium.org/g/proto-quic/c/uKWLRh9JPCo) suggests that payloads later than 1350 cannot be reliably received.

### receive()

```dart
Datagram? receive()
```

Receives a datagram.

Returns `null` if there are no datagrams available.

### joinMulticast()

```dart
void joinMulticast(InternetAddress group, [NetworkInterface? interface])
```

Joins a multicast group.

If an error occur when trying to join the multicast group, an exception is thrown.

### leaveMulticast()

```dart
void leaveMulticast(InternetAddress group, [NetworkInterface? interface])
```

Leaves a multicast group.

If an error occur when trying to join the multicast group, an exception is thrown.

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

Reads low level information about the [RawSocket].

See [RawSocketOption] for available options.

Returns [RawSocketOption.value] on success.

Throws an [OSError] on failure.

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

Customizes the [RawSocket].

See [RawSocketOption] for available options.

Throws an [OSError] on failure.

# SocketException

```dart
class SocketException implements IOException {}
```

Exception thrown when a socket operation fails.

### message

```dart
String message
```

Description of the error.

### osError

```dart
OSError? osError
```

The underlying OS error.

If this exception is not thrown due to an OS error, the value is `null`.

### address

```dart
InternetAddress? address
```

The address of the socket giving rise to the exception.

This is either the source or destination address of a socket, or it can be `null` if no socket end-point was involved in the cause of the exception.

### port

```dart
int? port
```

The port of the socket giving rise to the exception.

This is either the source or destination address of a socket, or it can be `null` if no socket end-point was involved in the cause of the exception.

### SocketException()

```dart
SocketException(String message, {OSError? osError, InternetAddress? address, int? port})
```

Creates a [SocketException] with the provided values.

### SocketException.closed()

```dart
SocketException.closed()
```

Creates an exception reporting that a socket was used after it was closed.

### toString()

```dart
String toString()
```
