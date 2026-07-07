# RawSynchronousSocket

```dart
abstract interface class RawSynchronousSocket {}
```

用于通过 TCP 套接字进行同步通信的底层类。

警告：[RawSynchronousSocket] 可能应该只用于连接到 'localhost'。以下操作会阻塞调用线程，以等待网络响应。在这些操作完成之前，该线程无法处理任何其他事件。[RawSynchronousSocket] 不适用于需要高性能或异步 I/O（例如服务器）的应用程序。此类应用程序应改用 [Socket] 或 [RawSocket] 类中的非阻塞套接字和异步操作。

### connectSync()

```dart
RawSynchronousSocket connectSync(dynamic host, int port)
```

创建一个新的套接字连接，并返回一个 [RawSynchronousSocket]。

[host] 可以是 [String] 或 [InternetAddress]。如果 [host] 是 [String]，[connectSync] 将执行 [InternetAddress.lookup]，并依次尝试所有返回的 [InternetAddress]，直到连接成功。除非成功建立连接，否则将返回第一个连接失败时的错误。

### available()

```dart
int available()
```

套接字中已接收但尚未读取的字节数。

### closeSync()

```dart
void closeSync()
```

关闭 [RawSynchronousSocket]。

一旦调用了 [closeSync]，再尝试调用 [readSync]、[readIntoSync]、[writeFromSync]、[remoteAddress] 和 [remotePort] 将导致抛出 [SocketException]。

### readIntoSync()

```dart
int readIntoSync(List<int> buffer, [int start = 0, int? end])
```

将字节读取到现有的 [buffer] 中。

读取字节并将其写入 [buffer] 中从 [start] 到 [end] 的范围内。[start] 必须为非负数，且不超过 [buffer].length。如果省略 [end]，默认为 [buffer].length。否则 [end] 必须不小于 [start]，且不超过 [buffer].length。

返回实际读取的字节数。如果文件中没有那么多字节可读，该值可能小于 `end - start`。

### readSync()

```dart
List<int>? readSync(int bytes)
```

从套接字中最多读取 [bytes] 个字节。

阻塞并等待套接字发送的、最多指定字节数的响应。[bytes] 指定要读取的最大字节数。返回读取到的字节列表，其长度可能小于 [bytes] 指定的值。

### shutdown()

```dart
void shutdown(SocketDirection direction)
```

按指定方向关闭套接字。

调用 shutdown 永远不会抛出异常，且支持多次调用。如果 [SocketDirection.receive] 和 [SocketDirection.send] 两个方向都已关闭，套接字将完全关闭，效果等同于调用了 [closeSync]。

### writeFromSync()

```dart
void writeFromSync(List<int> buffer, [int start = 0, int? end])
```

将 [buffer] 中的内容写入套接字。

将读取 [buffer] 中从索引 [start] 到索引 [end] 的内容。[start] 必须为非负数，且不超过 [buffer].length。如果省略 [end]，默认为 [buffer].length。否则 [end] 必须不小于 [start]，且不超过 [buffer].length。

### port

```dart
int get port
```

此套接字使用的端口。

### remotePort

```dart
int get remotePort
```

此套接字所连接的远程端口。

### address

```dart
InternetAddress get address
```

此套接字用于连接的 [InternetAddress]。

### remoteAddress

```dart
InternetAddress get remoteAddress
```

此套接字所连接的远程 [InternetAddress]。
