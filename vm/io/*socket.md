# InternetAddressType

```dart
final class InternetAddressType {}
```

[InternetAddress] 的类型（或地址族）。

目前支持 IP 版本 4（IPv4）、IP 版本 6（IPv6）和 Unix 域地址。Unix 域套接字仅在 Linux、MacOS 和 Android 上可用。

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

获取类型的名称，例如 "IPv4" 或 "IPv6"。

### toString()

```dart
String toString()
```

# InternetAddress

```dart
abstract interface class InternetAddress {}
```

一个互联网地址或 Unix 域地址。

此对象保存一个互联网地址。如果此互联网地址是 DNS 查找的结果，该地址还会保存用于查找的主机名。互联网地址与端口号结合，表示套接字可以连接的端点，或监听套接字可以绑定的端点。

### loopbackIPv4

```dart
InternetAddress get loopbackIPv4
```

IP 版本 4 环回地址。

在使用 IP 版本 4（IPv4）监听或连接到环回适配器时使用此地址。

### loopbackIPv6

```dart
InternetAddress get loopbackIPv6
```

IP 版本 6 环回地址。

在使用 IP 版本 6（IPv6）监听或连接到环回适配器时使用此地址。

### anyIPv4

```dart
InternetAddress get anyIPv4
```

IP 版本 4 任意地址。

在使用 IP 版本 4（IPv4）监听所有适配器的地址时使用此地址。

### anyIPv6

```dart
InternetAddress get anyIPv6
```

IP 版本 6 任意地址。

在使用 IP 版本 6（IPv6）监听所有适配器的地址时使用此地址。

### type

```dart
InternetAddressType get type
```

[InternetAddress] 的地址族。

### address

```dart
String get address
```

主机的数字地址。

对于 IPv4 地址，使用点分十进制表示法。对于 IPv6，使用十六进制表示法。对于 Unix 域地址，这是一个文件路径。

### host

```dart
String get host
```

用于查找该地址的主机。

如果该地址没有关联的主机，则返回 [address]。

### rawAddress

```dart
Uint8List get rawAddress
```

此 [InternetAddress] 的原始地址。

对于 IP 地址，结果是长度为 4 或 16 字节的列表。对于 Unix 域地址，返回表示 [address] 的 UTF-8 编码字节序列。

返回的列表是一份新的副本，因此可以在不修改 [InternetAddress] 的情况下更改该列表。

### isLoopback

```dart
bool get isLoopback
```

[InternetAddress] 是否为环回地址。

### isLinkLocal

```dart
bool get isLinkLocal
```

[InternetAddress] 的作用域是否为链路本地。

### isMulticast

```dart
bool get isMulticast
```

[InternetAddress] 的作用域是否为组播。

### InternetAddress()

```dart
InternetAddress(String address, {InternetAddressType? type})
```

从数字地址或文件路径创建一个新的 [InternetAddress]。

如果 [type] 为 [InternetAddressType.IPv4]，则 [address] 必须是数字 IPv4 地址（点分十进制表示法）。如果 [type] 为 [InternetAddressType.IPv6]，则 [address] 必须是数字 IPv6 地址（十六进制表示法）。如果 [type] 为 [InternetAddressType.unix]，则 [address] 必须是有效的文件路径。如果省略 [type]，则 [address] 必须是数字 IPv4 或 IPv6 地址，类型将根据格式推断。

### InternetAddress.fromRawAddress()

```dart
InternetAddress.fromRawAddress(Uint8List rawAddress, {InternetAddressType? type})
```

根据提供的原始地址字节创建一个新的 [InternetAddress]。

如果 [type] 为 [InternetAddressType.IPv4]，则 [rawAddress] 的长度必须为 4。如果 [type] 为 [InternetAddressType.IPv6]，则 [rawAddress] 的长度必须为 16。如果 [type] 为 [InternetAddressType.unix]，则 [rawAddress] 必须是有效的 UTF-8 编码文件路径。

如果省略 [type]，则 [rawAddress] 的长度必须为 4 或 16，此时类型将分别默认为 [InternetAddressType.IPv4] 或 [InternetAddressType.IPv6]。

### reverse()

```dart
Future<InternetAddress> reverse()
```

对此 [address] 执行反向 DNS 查找。

返回一个新的 [InternetAddress]，其地址相同，但 [host] 字段设置为查找结果。

如果此地址为 Unix 域地址，则不执行查找，直接返回此地址。

### lookup()

```dart
Future<List<InternetAddress>> lookup(String host, {InternetAddressType type = InternetAddressType.any})
```

查找主机的地址。

如果 [type] 为 [InternetAddressType.any]，将同时查找 IP 版本 4（IPv4）和 IP 版本 6（IPv6）地址。如果 [type] 为 [InternetAddressType.IPv4] 或 [InternetAddressType.IPv6]，则只查找指定类型的地址。列表的顺序可能（且很可能）会随时间变化。

### tryParse()

```dart
InternetAddress? tryParse(String address)
```

尝试将 [address] 解析为数字地址。

如果 [address] 不是数字 IPv4（点分十进制表示法）或 IPv6（十六进制表示法）地址，则返回 `null`。

# InterfaceAddress

```dart
abstract interface class InterfaceAddress implements InternetAddress {}
```

绑定到 [NetworkInterface] 的 [InternetAddress]，并扩展了网络配置详细信息。

除地址本身外，还提供前缀长度（也称为子网掩码长度），以及对于 IPv4 地址而言的网络广播地址。

### prefixLength

```dart
int get prefixLength
```

网络的前缀长度，也称为子网掩码长度。

例如，前缀长度 `24` 对应 IPv4 中的子网掩码 `255.255.255.0`。对于 IPv6，前缀长度 `64` 表示地址的前 64 位是网络前缀。

### broadcast

```dart
InternetAddress? get broadcast
```

此网络的广播地址。

只有 IPv4 网络具有广播地址。IPv6 地址返回 `null`。

# NetworkInterface

```dart
abstract interface class NetworkInterface {}
```

[NetworkInterface] 表示当前系统上的一个活动网络接口。它包含绑定到该接口的 [InterfaceAddress] 列表。

### name

```dart
String get name
```

[NetworkInterface] 的名称。

### index

```dart
int get index
```

[NetworkInterface] 的索引。

### addresses

```dart
List<InterfaceAddress> get addresses
```

当前绑定到此 [NetworkInterface] 的 [InterfaceAddress] 列表。

### listSupported

```dart
bool get listSupported
```

是否支持 [list] 方法。

Dart 支持的所有平台都支持 [list] 方法，因此该属性始终为 true。

### list()

```dart
Future<List<NetworkInterface>> list({bool includeLoopback = false, bool includeLinkLocal = false, InternetAddressType type = InternetAddressType.any})
```

查询系统中的 [NetworkInterface]。

如果 [includeLoopback] 为 `true`，返回的列表将包含环回设备。默认值为 `false`。

如果 [includeLinkLocal] 为 `true`，返回的 [NetworkInterface] 的地址列表可能包含链路本地地址。默认值为 `false`。

如果 [type] 为 [InternetAddressType.IPv4] 或 [InternetAddressType.IPv6]，将只查找指定类型的地址。默认值为 [InternetAddressType.any]。

# RawServerSocket

```dart
abstract interface class RawServerSocket implements Stream<RawSocket> {}
```

一个监听套接字。

`RawServerSocket` 提供一个低级 [RawSocket] 对象的流，每个连接到监听套接字的连接对应一个 [RawSocket] 对象。

更多信息参见 [RawSocket]。

### bind()

```dart
Future<RawServerSocket> bind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

在给定地址和端口上监听。

当返回的 Future 完成时，服务器套接字已绑定到给定的 [address] 和 [port]，并已开始监听。

[address] 可以是 [String] 或 [InternetAddress]。如果 [address] 是 [String]，[bind] 将执行 [InternetAddress.lookup] 并使用列表中的第一个值。要监听环回适配器（只允许来自本地主机的传入连接），请使用 [InternetAddress.loopbackIPv4] 或 [InternetAddress.loopbackIPv6]。要允许来自网络的传入连接，请使用 [InternetAddress.anyIPv4] 或 [InternetAddress.anyIPv6] 以绑定到所有接口，或使用特定接口的 IP 地址。

如果使用 IP 版本 6（IPv6）地址，将同时接受 IP 版本 6（IPv6）和版本 4（IPv4）连接。要将其限制为仅版本 6（IPv6），请使用 [v6Only] 设置仅版本 6。

如果 [port] 的值为 `0`，系统将选择一个临时端口。可以使用 `port` 获取器获取实际使用的端口。

可选参数 [backlog] 用于指定底层操作系统监听设置的监听队列长度。如果 [backlog] 的值为 `0`（默认值），系统将选择一个合理的值。

可选参数 [shared] 指定是否允许其他 RawServerSocket 对象绑定到相同的 [address]、[port] 和 [v6Only] 组合。如果 [shared] 为 `true`，且来自此隔离区（isolate）或其他隔离区的更多 [RawServerSocket] 绑定到该端口，则传入连接将在所有绑定的 [RawServerSocket] 之间分配。通过这种方式，连接可以分布在多个隔离区上。

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
Future<RawServerSocket> close()
```

关闭套接字。

当套接字完全关闭且不再绑定时，返回的 Future 完成。

# ServerSocket

```dart
abstract interface class ServerSocket implements ServerSocketBase<Socket> {}
```

一个监听套接字。

[ServerSocket] 提供一个 [Socket] 对象的流，每个连接到监听套接字的连接对应一个 [Socket] 对象。

更多信息参见 [Socket]。

### bind()

```dart
Future<ServerSocket> bind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

在给定地址和端口上监听。

当返回的 Future 完成时，服务器套接字已绑定到给定的 [address] 和 [port]，并已开始监听。

[address] 可以是 [String] 或 [InternetAddress]。如果 [address] 是 [String]，[bind] 将执行 [InternetAddress.lookup] 并使用列表中的第一个值。要监听环回适配器（只允许来自本地主机的传入连接），请使用 [InternetAddress.loopbackIPv4] 或 [InternetAddress.loopbackIPv6]。要允许来自网络的传入连接，请使用 [InternetAddress.anyIPv4] 或 [InternetAddress.anyIPv6] 以绑定到所有接口，或使用特定接口的 IP 地址。

如果使用 IP 版本 6（IPv6）地址，将同时接受 IP 版本 6（IPv6）和版本 4（IPv4）连接。要将其限制为仅版本 6（IPv6），请使用 [v6Only] 设置仅版本 6。

如果 [port] 的值为 `0`，系统将选择一个临时端口。可以使用 [port] 获取器获取实际使用的端口。

可选参数 [backlog] 用于指定底层操作系统监听设置的监听队列长度。如果 [backlog] 的值为 `0`（默认值），系统将选择一个合理的值。

可选参数 [shared] 指定是否允许其他 ServerSocket 对象绑定到相同的 [address]、[port] 和 [v6Only] 组合。如果 [shared] 为 `true`，且来自此隔离区或其他隔离区的更多服务器套接字绑定到该端口，则传入连接将在所有绑定的服务器套接字之间分配。通过这种方式，连接可以分布在多个隔离区上。

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
Future<ServerSocket> close()
```

关闭套接字。

当套接字完全关闭且不再绑定时，返回的 Future 完成。

# SocketDirection

```dart
final class SocketDirection {}
```

[SocketDirection] 用作 [Socket.close] 和 [RawSocket.close] 的参数，用于在指定方向上关闭套接字。

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

用于配置套接字的选项，通过 [Socket.setOption] 进行配置。

[SocketOption] 用作 [Socket.setOption] 和 [RawSocket.setOption] 的参数，用于自定义底层套接字的行为。

### tcpNoDelay

```dart
SocketOption tcpNoDelay
```

启用或禁用套接字的无延迟（no-delay）选项。如果启用了 tcpNoDelay，套接字将不在内部缓冲数据，而是将每个数据块作为单独的 TCP 数据包写入。

tcpNoDelay 默认禁用。

# RawSocketOption

```dart
final class RawSocketOption {}
```

[RawSocketOption] 用作 [Socket.setRawOption] 和 [RawSocket.setRawOption] 的参数，用于自定义底层套接字的行为。

它允许对套接字选项进行精细控制，其值将传递给底层平台的 setsockopt 和 getsockopt 实现。

### RawSocketOption()

```dart
RawSocketOption(int level, int option, Uint8List value)
```

为 [RawSocket.getRawOption] 和 [RawSocket.setRawOption] 创建一个 [RawSocketOption]。

[level] 和 [option] 参数对应于原生 `getsockopt()` 和 `setsockopt()` 调用中的 `level` 和 `optname` 参数。

value 参数及其长度对应于原生调用中的 optval 和 length 参数。

对于 [RawSocket.getRawOption] 调用，调用成功后 value 参数将被更新（尽管其长度不会改变）。

对于 [RawSocket.setRawOption] 调用，value 参数将用于设置选项。

### RawSocketOption.fromInt()

```dart
RawSocketOption.fromInt(int level, int option, int value)
```

用于创建基于整数的 [RawSocketOption] 的便捷构造函数。

### RawSocketOption.fromBool()

```dart
RawSocketOption.fromBool(int level, int option, bool value)
```

用于创建基于布尔值的 [RawSocketOption] 的便捷构造函数。

### level

```dart
int level
```

要设置或获取的选项的级别。

另请参阅：

- [RawSocketOption.levelSocket]
- [RawSocketOption.levelIPv4]
- [RawSocketOption.levelIPv6]
- [RawSocketOption.levelTcp]
- [RawSocketOption.levelUdp]

### option

```dart
int option
```

要设置或获取的选项的数字 ID。

### value

```dart
Uint8List value
```

要设置的原始数据，或用于写入当前选项值的数组。

此列表长度必须与预期选项的长度一致。对于大多数接受 [int] 或 [bool] 值的选项，长度应为 4。对于需要结构体（例如 in_addr_t）的选项，长度应为该结构体的正确长度。

### levelSocket

```dart
int get levelSocket
```

`SOL_SOCKET` 的套接字级别选项。

### levelIPv4

```dart
int get levelIPv4
```

`IPPROTO_IP` 的套接字级别选项。

### IPv4MulticastInterface

```dart
int get IPv4MulticastInterface
```

`IP_MULTICAST_IF` 的套接字选项。

### levelIPv6

```dart
int get levelIPv6
```

`IPPROTO_IPV6` 的套接字级别选项。

### IPv6MulticastInterface

```dart
int get IPv6MulticastInterface
```

`IPV6_MULTICAST_IF` 的套接字选项。

### levelTcp

```dart
int get levelTcp
```

`IPPROTO_TCP` 的套接字级别选项。

### levelUdp

```dart
int get levelUdp
```

`IPPROTO_UDP` 的套接字级别选项。

# RawSocketEvent

```dart
class RawSocketEvent {}
```

[RawDatagramSocket]、[RawSecureSocket] 和 [RawSocket] 的事件。

当套接字状态发生变化时，这些事件对象由套接字的 [Stream] 行为使用（例如 [RawSocket.listen]、[RawSocket.forEach]）。

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

表示套接字已准备好读取的事件。

### write

```dart
RawSocketEvent write
```

表示套接字已准备好写入的事件。

### readClosed

```dart
RawSocketEvent readClosed
```

表示套接字的读取已关闭的事件。

### closed

```dart
RawSocketEvent closed
```

表示套接字已关闭的事件。

### toString()

```dart
String toString()
```

# ConnectionTask

```dart
final class ConnectionTask<S> {}
```

一个可取消的连接尝试。

由客户端套接字类型 `S` 上的 `startConnect` 方法返回，`ConnectionTask<S>` 允许取消尝试连接到主机的操作。

### socket

```dart
Future<S> socket
```

一个 `Future`，其完成值与 `S.connect()` 的返回值相同，除非在此 [ConnectionTask] 上调用了 [cancel]。

如果调用了 [cancel]，该 Future 将以 [SocketException] 错误完成，该错误消息表明连接尝试已被取消。

### fromSocket()

```dart
ConnectionTask<T> fromSocket<T extends Socket>(Future<T> socket, void Function() onCancel)
```

从现有的 `Future<Socket>` 创建一个 `ConnectionTask`。

可以使用此方法在 [HttpClient.connectionFactory] 中返回现有的套接字连接。

例如：

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

取消连接尝试。

这也会导致 [socket] `Future` 以 [SocketException] 错误完成。

# RawSocket

```dart
abstract interface class RawSocket implements Stream<RawSocketEvent> {}
```

一个 TCP 连接。

一个 _套接字连接_ 将一个 _本地_ 套接字连接到一个 _远程_ 套接字。数据以 [Uint8List] 的形式由本地套接字接收，并通过 [read] 方法提供，也可以通过 [write] 方法发送到远程套接字。

此类的 [Stream] 接口提供关于特定变化发生时的事件通知，例如数据变为可用时（[RawSocketEvent.read]）或远程端停止监听时（[RawSocketEvent.closed]）。

### readEventsEnabled

```dart
bool readEventsEnabled
```

设置或获取 [RawSocket] 是否应监听 [RawSocketEvent.read] 和 [RawSocketEvent.readClosed] 事件。默认值为 `true`。

警告：将 [readEventsEnabled] 设置为 `false` 可能会阻止套接字在 [SocketDirection.receive] 和 [SocketDirection.send] 方向被独立关闭时完全关闭。更多详情参见 [shutdown]。

### writeEventsEnabled

```dart
bool writeEventsEnabled
```

设置或获取 [RawSocket] 是否应监听 [RawSocketEvent.write] 事件。默认值为 `true`。这是一次性监听器，必须再次将 writeEventsEnabled 设置为 true 才能接收另一个写入事件。

### connect()

```dart
Future<RawSocket> connect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

创建一个到主机和端口的新套接字连接。

返回一个 [Future]，一旦连接完成将以 [RawSocket] 完成，如果主机查找或连接失败，则以错误完成。

[host] 可以是 [String] 或 [InternetAddress]。如果 [host] 是 [String]，[connect] 将执行 [InternetAddress.lookup] 并尝试所有返回的 [InternetAddress]，直到连接成功。如果 IPv4 和 IPv6 地址都可用，则优先使用 IPv4 连接。如果无法建立连接，则返回第一个失败连接的错误。

[sourceAddress] 参数用于指定进行连接时要绑定的本地地址。[sourceAddress] 可以是 [String] 或 [InternetAddress]。如果传入的是 [String]，则必须是数字 IP 地址。

[sourcePort] 定义要绑定的本地端口。如果未指定 [sourcePort] 或为零，将选择一个端口。

[timeout] 参数用于指定等待建立连接的最大允许时间。如果 [timeout] 长于系统级超时时间，超时可能会比指定的 [timeout] 更早发生。超时时，将抛出 [SocketException]，并且所有正在进行的到 [host] 的连接尝试都将被取消。

### startConnect()

```dart
Future<ConnectionTask<RawSocket>> startConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

类似于 [connect]，但返回一个以 [ConnectionTask] 完成的 [Future]，如果不再需要 [RawSocket]，可以取消该任务。

### available()

```dart
int available()
```

套接字中已接收但尚未读取的字节数，可供读取。

### read()

```dart
Uint8List? read([int? len])
```

从套接字中读取最多 [len] 字节。

此函数是非阻塞的，只有在数据可用时才会返回数据。如果立即可读的字节较少，读取的字节数可能小于 [len]。如果没有可用数据，则返回 `null`。

### readMessage()

```dart
SocketMessage? readMessage([int? count])
```

从套接字中读取包含最多 [count] 字节的消息。

此函数与 [read] 的不同之处在于，它还会返回已发送的任何 [SocketControlMessage]。

此函数是非阻塞的，只有在数据可用时才会返回数据。如果立即可读的字节较少，读取的字节数可能小于 [count]。[SocketMessage] 中数据缓冲区的长度表示已读取的字节数。

如果没有可用数据，则返回 `null`。

[RawSecureSocket] 不支持此方法。

在 Android、Fuchsia、Windows 上不支持。

### write()

```dart
int write(List<int> buffer, [int offset = 0, int? count])
```

将缓冲区从 [offset] 缓冲区偏移量开始的最多 [count] 字节写入套接字。

返回成功写入的字节数。此函数是非阻塞的，只有在套接字中有可用的缓冲区空间时才会写入数据。这意味着成功写入的字节数可能小于 `count`，甚至为 0。

除非通过 [RawSocket.setOption] 设置了 [SocketOption.tcpNoDelay]，否则缓冲区的传输可能会被延迟。

[offset] 的默认值为 0，[count] 的默认值为 `buffer.length - offset`。

### sendMessage()

```dart
int sendMessage(List<SocketControlMessage> controlMessages, List<int> data, [int offset = 0, int? count])
```

向套接字写入套接字控制消息和数据字节。

从 [offset] 开始，写入 [controlMessages] 和最多 [count] 字节的 [data]。如果未提供 [count]，将写入尽可能多的字节。如果不需要发送控制消息，请改用 [write]。

当发送的控制消息被接收时，它们将被保留，直到下一次调用 [readMessage]，此时所有当前可用的控制消息都将作为返回的 [SocketMessage] 的一部分提供。调用 [read] 只会读取数据字节，不会影响控制消息。

[count] 必须为正数（大于零）。

返回写入的字节数，该数不能大于 [count]，也不能大于 `data.length - offset`。返回值为零表示未发送控制消息。

此函数是非阻塞的，只有在套接字中有可用的缓冲区空间时才会写入数据。

如果消息无法发送，将抛出 [OSError]。

[RawSecureSocket] 不支持此方法。

在 Android、Fuchsia、Windows 上不支持。

### port

```dart
int get port
```

此套接字使用的端口。

如果套接字已关闭，则抛出 [SocketException]。

### remotePort

```dart
int get remotePort
```

此套接字连接到的远程端口。

如果套接字已关闭，则抛出 [SocketException]。

### address

```dart
InternetAddress get address
```

用于连接此套接字的 [InternetAddress]。

如果套接字已关闭，则抛出 [SocketException]。

### remoteAddress

```dart
InternetAddress get remoteAddress
```

此套接字连接到的远程 [InternetAddress]。

如果套接字已关闭，则抛出 [SocketException]。

### close()

```dart
Future<RawSocket> close()
```

关闭套接字。

返回一个 Future，当底层连接被完全销毁时，该 Future 以此套接字完成。

调用 [close] 永远不会抛出异常，并且支持多次调用。调用 [close] 可能导致产生 [RawSocketEvent.readClosed] 事件。

### shutdown()

```dart
void shutdown(SocketDirection direction)
```

在指定的 [direction] 上关闭套接字。

调用 [shutdown] 永远不会抛出异常，并且支持多次调用。使用 [SocketDirection.both] 或 [SocketDirection.receive] 调用 shutdown 可能导致产生 [RawSocketEvent.readClosed] 事件。

警告：只有在所有可用数据都被排空且已派发 [RawSocketEvent.readClosed] 之后，[SocketDirection.receive] 方向才被视为完全关闭。如果在未排空数据的情况下分别关闭 [SocketDirection.receive] 和 [SocketDirection.send] 方向，套接字将保持存在，直到数据被排空。如果 [readEventsEnabled] 设置为 `false`，或者接收到的数据未响应这些事件被 [read]，就会发生这种情况。这不适用于使用 [SocketDirection.both] 同时关闭两个方向的情况，此时会直接丢弃所有已接收的数据。

### setOption()

```dart
bool setOption(SocketOption option, bool enabled)
```

自定义 [RawSocket]。

可用选项参见 [SocketOption]。

如果选项设置成功，返回 `true`，否则返回 `false`。

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

读取有关 [RawSocket] 的底层信息。

可用选项参见 [RawSocketOption]。

成功时返回 [RawSocketOption.value]。

失败时抛出 [OSError]。

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

自定义 [RawSocket]。

可用选项参见 [RawSocketOption]。

失败时抛出 [OSError]。

# Socket

```dart
abstract interface class Socket implements Stream<Uint8List>, IOSink {}
```

两个套接字之间的 TCP 连接。

一个 _套接字连接_ 将一个 _本地_ 套接字连接到一个 _远程_ 套接字。数据以 [Uint8List] 的形式由本地套接字接收，并通过此类的 [Stream] 接口提供，也可以通过此类的 [IOSink] 接口发送到远程套接字。

除非通过 [Socket.setOption] 设置了 [SocketOption.tcpNoDelay]，否则通过 [IOSink] 接口发送的数据的传输可能会被延迟。

### connect()

```dart
Future<Socket> connect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

创建一个到主机和端口的新套接字连接，并返回一个 [Future]，一旦连接完成将以 [Socket] 完成，如果主机查找或连接失败，则以错误完成。

[host] 可以是 [String] 或 [InternetAddress]。如果 [host] 是 [String]，[connect] 将执行 [InternetAddress.lookup] 并尝试所有返回的 [InternetAddress]，直到连接成功。除非成功建立连接，否则返回第一个失败连接的错误。

[sourceAddress] 参数用于指定进行连接时要绑定的本地地址。[sourceAddress] 可以是 [String] 或 [InternetAddress]。如果传入的是 [String]，则必须是数字 IP 地址。

[sourcePort] 定义要绑定的本地端口。如果未指定 [sourcePort] 或为零，将选择一个端口。

[timeout] 参数用于指定等待建立连接的最大允许时间。如果 [timeout] 长于系统级超时时间，超时可能会比指定的 [timeout] 更早发生。超时时，将抛出 [SocketException]，并且所有正在进行的到 [host] 的连接尝试都将被取消。

### startConnect()

```dart
Future<ConnectionTask<Socket>> startConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

类似于 [connect]，但返回一个以 [ConnectionTask] 完成的 [Future]，如果不再需要 [Socket]，可以取消该任务。

### destroy()

```dart
void destroy()
```

在两个方向上销毁套接字。

调用 [destroy] 将在流上产生一个关闭事件，并且不再响应传输给它的数据。

调用（从 [IOSink] 继承的）[close] 只会关闭 [Socket] 的发送功能。

### setOption()

```dart
bool setOption(SocketOption option, bool enabled)
```

自定义 [RawSocket]。

可用选项参见 [SocketOption]。

如果选项设置成功，返回 `true`，否则返回 false。

如果套接字已被销毁或升级为安全套接字，则抛出 [SocketException]。

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

读取有关 [RawSocket] 的底层信息。

可用选项参见 [RawSocketOption]。

成功时返回 [RawSocketOption.value]。

失败时抛出 [OSError]，如果套接字已被销毁或升级为安全套接字，则抛出 [SocketException]。

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

自定义 [RawSocket]。

可用选项参见 [RawSocketOption]。

失败时抛出 [OSError]，如果套接字已被销毁或升级为安全套接字，则抛出 [SocketException]。

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

套接字不支持的操作。

此方法继承自 [IOSink]，套接字不支持，**不得**调用。套接字没有报告错误的方式，因此使用 [addError] 传入的任何错误都会丢失。

### port

```dart
int get port
```

此套接字使用的端口。

如果套接字已关闭，则抛出 [SocketException]。如果套接字是 Unix 域套接字，端口为 0。

### remotePort

```dart
int get remotePort
```

此套接字连接到的远程端口。

如果套接字已关闭，则抛出 [SocketException]。如果套接字是 Unix 域套接字，端口为 0。

### address

```dart
InternetAddress get address
```

用于连接此套接字的 [InternetAddress]。

如果套接字已关闭，则抛出 [SocketException]。

### remoteAddress

```dart
InternetAddress get remoteAddress
```

此套接字连接到的远程 [InternetAddress]。

如果套接字已关闭，则抛出 [SocketException]。

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

由 [RawDatagramSocket] 接收的数据包。

### data

```dart
Uint8List data
```

消息的实际字节。

### address

```dart
InternetAddress address
```

发送数据的套接字的地址。

### port

```dart
int port
```

发送数据的套接字的端口。

### Datagram()

```dart
Datagram(Uint8List data, InternetAddress address, int port)
```

# ResourceHandle

```dart
abstract interface class ResourceHandle {}
```

围绕操作系统资源句柄的包装器，以便可以通过 Socket 作为 [SocketMessage] 的一部分传递。

### ResourceHandle.fromFile()

```dart
ResourceHandle.fromFile(RandomAccessFile file)
```

创建围绕已打开文件的包装器。

### ResourceHandle.fromSocket()

```dart
ResourceHandle.fromSocket(Socket socket)
```

创建围绕已打开套接字的包装器。

### ResourceHandle.fromRawSocket()

```dart
ResourceHandle.fromRawSocket(RawSocket socket)
```

创建围绕已打开原始套接字的包装器。

### ResourceHandle.fromRawDatagramSocket()

```dart
ResourceHandle.fromRawDatagramSocket(RawDatagramSocket socket)
```

创建围绕已打开原始数据报套接字的包装器。

### ResourceHandle.fromStdin()

```dart
ResourceHandle.fromStdin(Stdin stdin)
```

创建围绕当前 stdin 的包装器。

### ResourceHandle.fromStdout()

```dart
ResourceHandle.fromStdout(Stdout stdout)
```

创建围绕当前 stdout 的包装器。

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

从资源句柄中提取已打开的文件。

这也可用于接收 stdin 和 stdout 句柄以及读写管道。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是文件或 stdio 句柄，则返回的 [RandomAccessFile] 的行为完全未指定。请务必小心，避免错误地使用句柄。

### toSocket()

```dart
Socket toSocket()
```

从资源句柄中提取已打开的套接字。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是套接字句柄，则返回的 [Socket] 的行为完全未指定。请务必小心，避免错误地使用句柄。

### toRawSocket()

```dart
RawSocket toRawSocket()
```

从资源句柄中提取已打开的原始套接字。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是套接字句柄，则返回的 [RawSocket] 的行为完全未指定。请务必小心，避免错误地使用句柄。

### toRawDatagramSocket()

```dart
RawDatagramSocket toRawDatagramSocket()
```

从资源句柄中提取已打开的原始数据报套接字。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是数据报套接字句柄，则返回的 [RawDatagramSocket] 的行为完全未指定。请务必小心，避免错误地使用句柄。

### toReadPipe()

```dart
ReadPipe toReadPipe()
```

从资源句柄中提取读管道。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是可读管道，则返回的 [ReadPipe] 的行为完全未指定。请务必小心，避免错误地使用句柄。

### toWritePipe()

```dart
WritePipe toWritePipe()
```

从资源句柄中提取写管道。

由于 [ResourceHandle] 表示单个操作系统资源，在调用此方法之后，不能再调用 [toFile]、[toSocket]、[toRawSocket]、[toRawDatagramSocket]、[toReadPipe]、[toWritePipe] 中的任何一个。

如果此资源句柄不是可写管道，则返回的 [ReadPipe] 的行为完全未指定。请务必小心，避免错误地使用句柄。

# SocketControlMessage

```dart
abstract interface class SocketControlMessage {}
```

通过调用 [RawSocket.readMessage] 接收的 [SocketMessage] 中的控制消息部分。

控制消息可以携带不同的信息，包括 [ResourceHandle]。如果此消息中包含 [ResourceHandle]，可以通过 [extractHandles] 提取。

### SocketControlMessage.fromHandles()

```dart
SocketControlMessage.fromHandles(List<ResourceHandle> handles)
```

创建一个包含所提供的 [handles] 的控制消息。

这在发送方通过套接字发送句柄时使用。接收方可以使用 [extractHandles] 从消息中提取句柄。

### extractHandles()

```dart
List<ResourceHandle> extractHandles()
```

提取此消息中嵌入的句柄列表。

此方法只能用于从套接字接收到的消息中提取句柄。不得用于本地创建、且尚未通过 [RawSocket.sendMessage] 发送的套接字控制消息。

此方法只能调用一次。多次调用可能导致句柄重复，行为未指定。

### level

```dart
int get level
```

用于确定控制消息种类的平台特定值。

与 [type] 一起，这两个整数以平台特定的方式标识控制消息的种类。例如，在 Linux 上，这些值的某些组合表示该控制消息携带 [ResourceHandle]。

### type

```dart
int get type
```

用于确定控制消息种类的平台特定值。

与 [level] 一起，这两个整数以平台特定的方式标识控制消息的种类。例如，在 Linux 上，这些值的某些组合表示该控制消息携带 [ResourceHandle]。

### data

```dart
Uint8List get data
```

底层平台作为控制消息一部分传递的实际字节。

这些字节的解释方式因 [level] 和 [type] 而异。这些实际字节可用于检查和解释不携带句柄的消息。

# SocketMessage

```dart
final class SocketMessage {}
```

由 [RawDatagramSocket] 接收的套接字消息。

套接字消息由 [data] 字节和 [controlMessages] 组成。

### data

```dart
Uint8List data
```

消息的实际字节。

### controlMessages

```dart
List<SocketControlMessage> controlMessages
```

作为此消息一部分发送的控制消息。

此列表可以为空。

### SocketMessage()

```dart
SocketMessage(Uint8List data, List<SocketControlMessage> controlMessages)
```

# RawDatagramSocket

```dart
abstract interface class RawDatagramSocket extends Stream<RawSocketEvent> {}
```

UDP 套接字的无缓冲接口。

原始数据报套接字以底层操作系统接收数据的相同分块方式，传递 [RawSocketEvent] 的 [Stream]。

请注意，UDP 套接字无法被远程对等方关闭，因此永远不会收到 [RawSocketEvent.readClosed] 事件。

它与 [POSIX 原始套接字](http://man7.org/linux/man-pages/man7/raw.7.html) 不同。

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

[RawDatagramSocket] 是否应监听 [RawSocketEvent.read] 事件。

默认值为 `true`。

### writeEventsEnabled

```dart
bool writeEventsEnabled
```

[RawDatagramSocket] 是否应监听 [RawSocketEvent.write] 事件。

默认值为 `true`。这是一次性监听器，必须再次将 [writeEventsEnabled] 设置为 true 才能接收另一个写入事件。

### multicastLoopback

```dart
bool multicastLoopback
```

组播流量是否回环到主机。

默认情况下启用组播回环。

### multicastHops

```dart
int multicastHops
```

源自此套接字的组播数据包的最大网络跳数。

对于 IPv4，这被称为 TTL（生存时间）。

默认情况下此值为 1，使组播流量保持在本地网络中。

### multicastInterface

```dart
NetworkInterface? multicastInterface
```

用于传出组播数据包的网络接口。

值为 `null` 表示由系统选择要使用的网络接口。

默认情况下此值为 `null`。

### broadcastEnabled

```dart
bool broadcastEnabled
```

是否启用 IPv4 广播。

发送方需要启用 IPv4 广播才能发送 IPv4 广播数据包。默认情况下禁用 IPv4 广播。

对于 IPv6，没有通用的广播机制。请改用组播。

### bind()

```dart
Future<RawDatagramSocket> bind(dynamic host, int port, {bool reuseAddress = true, bool reusePort = false, int ttl = 1})
```

将套接字绑定到给定的 [host] 和 [port]。

当套接字绑定完成并开始监听 [port] 时，返回的 Future 将以绑定套接字的 [RawDatagramSocket] 完成。

[host] 可以是 [String] 或 [InternetAddress]。如果 [host] 是 [String]，[bind] 将执行 [InternetAddress.lookup] 并使用列表中的第一个值。要监听环回接口（只允许来自本地主机的传入连接），请使用 [InternetAddress.loopbackIPv4] 或 [InternetAddress.loopbackIPv6]。要允许来自任何网络的传入连接，请使用 [InternetAddress.anyIPv4] 或 [InternetAddress.anyIPv6] 以绑定到所有接口，或使用特定接口的 IP 地址。

[reuseAddress] 应为所有绑定到同一地址的监听器设置。否则，将失败并抛出 [SocketException]。

[reusePort] 指定端口是否可以重用。

[ttl] 设置在套接字上发送的数据报的"生存时间"。

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
void close()
```

关闭数据报套接字。

### send()

```dart
int send(List<int> buffer, InternetAddress address, int port)
```

异步发送数据报。

返回写入的字节数。这将始终是 [buffer] 的大小或 `0`。

返回值为 `0` 表示发送数据报将阻塞，可以再次尝试 [send] 调用。

返回值为 [buffer] 的大小表示已向操作系统发出传输数据报的请求。这并不表示操作系统已成功发送数据报。如果本地发送数据报失败，将向 [Stream] 添加一个错误事件。如果发生网络或远程故障，则不会报告。

IPv4 UDP 数据报的最大大小为 65535 字节（包括数据和头部），但由于操作系统限制和网络的最大传输单元（MTU），实际最大大小可能会低得多。

某些 IPv6 实现可能支持高达 4GB 的负载（参见 RFC-2675），但该支持有限（参见 RFC-6434），并已在后续标准中移除（参见 RFC-8504）。

[Chromium 团队的实证测试](https://groups.google.com/a/chromium.org/g/proto-quic/c/uKWLRh9JPCo) 表明，超过 1350 字节的负载无法可靠接收。

### receive()

```dart
Datagram? receive()
```

接收一个数据报。

如果没有可用的数据报，返回 `null`。

### joinMulticast()

```dart
void joinMulticast(InternetAddress group, [NetworkInterface? interface])
```

加入一个组播组。

如果尝试加入组播组时发生错误，将抛出异常。

### leaveMulticast()

```dart
void leaveMulticast(InternetAddress group, [NetworkInterface? interface])
```

离开一个组播组。

如果尝试加入组播组时发生错误，将抛出异常。

### getRawOption()

```dart
Uint8List getRawOption(RawSocketOption option)
```

读取有关 [RawSocket] 的底层信息。

可用选项参见 [RawSocketOption]。

成功时返回 [RawSocketOption.value]。

失败时抛出 [OSError]。

### setRawOption()

```dart
void setRawOption(RawSocketOption option)
```

自定义 [RawSocket]。

可用选项参见 [RawSocketOption]。

失败时抛出 [OSError]。

# SocketException

```dart
class SocketException implements IOException {}
```

套接字操作失败时抛出的异常。

### message

```dart
String message
```

错误描述。

### osError

```dart
OSError? osError
```

底层操作系统错误。

如果此异常不是由操作系统错误引起的，则该值为 `null`。

### address

```dart
InternetAddress? address
```

引发异常的套接字地址。

这可以是套接字的源地址或目标地址，如果异常原因不涉及套接字端点，也可以为 `null`。

### port

```dart
int? port
```

引发异常的套接字端口。

这可以是套接字的源地址或目标地址（端口），如果异常原因不涉及套接字端点，也可以为 `null`。

### SocketException()

```dart
SocketException(String message, {OSError? osError, InternetAddress? address, int? port})
```

使用提供的值创建一个 [SocketException]。

### SocketException.closed()

```dart
SocketException.closed()
```

创建一个异常，报告套接字在关闭后被使用。

### toString()

```dart
String toString()
```
