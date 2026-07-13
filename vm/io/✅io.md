适用于非 Web 应用程序的文件、套接字、HTTP 及其他 I/O 支持。

**重要说明：** 基于浏览器的应用程序不能使用此库。只有以下应用程序可以导入并使用 dart:io 库：

- 服务器
- 命令行脚本
- Flutter 移动应用
- Flutter 桌面应用

该库允许你处理文件、目录、套接字、进程、HTTP 服务器和客户端等。许多与输入输出相关的操作是异步的，通过 [Future](https://www.yuque.com/thyname/dart.async/future) 或 [Stream](https://www.yuque.com/thyname/dart.async/stream) 处理，两者都定义在 [dart:async 库](https://www.yuque.com/thyname/dart.async) 中。

要在代码中使用 dart:io 库：

```dart
import 'dart:io';
```

有关 Dart 中 I/O 的介绍，请参阅 [dart:io 库导览](https://dart.dev/guides/libraries/library-tour#dartio)。

## File、Directory 和 Link

[File](https://www.yuque.com/thyname/dart.io/file)、[Directory](https://www.yuque.com/thyname/dart.io/directory) 或 [Link](https://www.yuque.com/thyname/dart.io/link) 的实例分别表示原生文件系统中的文件、目录或链接。

你可以通过这些类型的对象操作文件系统。例如，你可以重命名文件或目录：

```dart
File myFile = File('myFile.txt');
myFile.rename('yourFile.txt').then((_) => print('file renamed'));
```

[File](https://www.yuque.com/thyname/dart.io/file)、[Directory](https://www.yuque.com/thyname/dart.io/directory) 和 [Link](https://www.yuque.com/thyname/dart.io/link) 类提供的许多方法都是异步运行的，并返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)。

## FileSystemEntity

[File](https://www.yuque.com/thyname/dart.io/file)、[Directory](https://www.yuque.com/thyname/dart.io/directory) 和 [Link](https://www.yuque.com/thyname/dart.io/link) 都继承自 [FileSystemEntity](https://www.yuque.com/thyname/dart.io/filesystementity)。除了作为这些类的父类之外，FileSystemEntity 还提供了许多用于处理路径的静态方法。

要获取路径的相关信息，可以使用 [FileSystemEntity](https://www.yuque.com/thyname/dart.io/filesystementity) 的静态方法，例如 [FileSystemEntity.isDirectory](https://www.yuque.com/thyname/dart.io/filesystementity)、[FileSystemEntity.isFile](https://www.yuque.com/thyname/dart.io/filesystementity) 和 [FileSystemEntity.exists](https://www.yuque.com/thyname/dart.io/filesystementity)。由于访问文件系统涉及 I/O 操作，这些方法都是异步的，返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)。

```dart
FileSystemEntity.isDirectory(myPath).then((isDir) {
  if (isDir) {
    print('$myPath is a directory');
  } else {
    print('$myPath is not a directory');
  }
});
```

## HttpServer 和 HttpClient

[HttpClient](https://www.yuque.com/thyname/dart.io/httpclient) 和 [HttpServer](https://www.yuque.com/thyname/dart.io/httpserver) 类提供了底层的 HTTP 功能。

建议不要直接使用这些类，而是考虑使用相关软件包中更易用、可组合的 API。

关于 HTTP 客户端，请参阅 [`package:http`](https://pub.dev/packages/http)。

关于 HTTP 服务器，请参阅 [dart.dev](https://dart.dev/) 上的[编写 HTTP 服务器](https://dart.dev/tutorials/server/httpserver)。

## Process

[Process](https://www.yuque.com/thyname/dart.io/process) 类提供了在本机上运行进程的方式。例如，以下代码生成一个进程，递归列出 `web` 目录下的文件。

```dart
Process.start('ls', ['-R', 'web']).then((process) {
  stdout.addStream(process.stdout);
  stderr.addStream(process.stderr);
  process.exitCode.then(print);
});
```

使用 [Process.start](https://www.yuque.com/thyname/dart.io/process) 会返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，该进程启动后会以一个 [Process](https://www.yuque.com/thyname/dart.io/process) 对象完成。这个 [Process](https://www.yuque.com/thyname/dart.io/process) 对象允许你在进程运行时与其交互。使用 [Process.run](https://www.yuque.com/thyname/dart.io/process) 会返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，该 Future 会在生成的进程终止后以一个 [ProcessResult](https://www.yuque.com/thyname/dart.io/processresult) 对象完成。这个 [ProcessResult](https://www.yuque.com/thyname/dart.io/processresult) 对象收集了进程的输出和退出代码。

使用 [Process.start](https://www.yuque.com/thyname/dart.io/process) 时，你需要读取 [Process.stdout](https://www.yuque.com/thyname/dart.io/process) 和 [Process.stderr](https://www.yuque.com/thyname/dart.io/process) 流上的所有数据，否则系统资源不会被释放。

## WebSocket

[WebSocket](https://www.yuque.com/thyname/dart.io/websocket) 类提供了对 Web 套接字协议的支持，允许客户端和服务器应用程序之间进行全双工通信。

Web 套接字服务器使用普通的 HTTP 服务器来接受 Web 套接字连接。初始握手是一个 HTTP 请求，随后升级为 Web 套接字连接。服务器使用 [WebSocketTransformer](https://www.yuque.com/thyname/dart.io/websockettransformer) 升级请求，并在返回的 Web 套接字上监听数据。例如，下面是一个监听 'ws' 数据的迷你服务器：

```dart import:async
runZoned(() async {
  var server = await HttpServer.bind('127.0.0.1', 4040);
  server.listen((HttpRequest req) async {
    if (req.uri.path == '/ws') {
      var socket = await WebSocketTransformer.upgrade(req);
      socket.listen(handleMsg);
    }
  });
}, onError: (e) => print("An error occurred."));
```

客户端使用 [WebSocket.connect](https://www.yuque.com/thyname/dart.io/websocket) 方法和使用 Web 套接字协议的 URI 连接到 [WebSocket](https://www.yuque.com/thyname/dart.io/websocket)。客户端可以使用 [WebSocket.add](https://www.yuque.com/thyname/dart.io/websocket) 方法向 [WebSocket](https://www.yuque.com/thyname/dart.io/websocket) 写入数据。例如：

```dart
var socket = await WebSocket.connect('ws://127.0.0.1:4040/ws');
socket.add('Hello, World!');
```

查看 [websocket_sample](https://github.com/dart-lang/dart-samples/tree/master/html5/web/websockets/basics) 应用，该应用使用 [WebSocket](https://www.yuque.com/thyname/dart.io/websocket) 与服务器通信。

## Socket 和 ServerSocket

客户端和服务器使用 [Socket](https://www.yuque.com/thyname/dart.io/socket) 通过 TCP 协议进行通信。服务器端使用 [ServerSocket](https://www.yuque.com/thyname/dart.io/serversocket)，客户端使用 [Socket](https://www.yuque.com/thyname/dart.io/socket)。服务器使用 `bind()` 方法创建一个监听套接字，然后在该套接字上监听传入的连接。例如：

```dart import:convert
ServerSocket.bind('127.0.0.1', 4041)
  .then((serverSocket) {
    serverSocket.listen((socket) {
      socket.transform(utf8.decoder).listen(print);
    });
  });
```

客户端使用 `connect()` 方法连接 [Socket](https://www.yuque.com/thyname/dart.io/socket)，该方法返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)。使用 `write()`、`writeln()` 或 `writeAll()` 是通过套接字发送数据的最简单方式。例如：

```dart
Socket.connect('127.0.0.1', 4041).then((socket) {
  socket.write('Hello, World!');
});
```

除了 [Socket](https://www.yuque.com/thyname/dart.io/socket) 和 [ServerSocket](https://www.yuque.com/thyname/dart.io/serversocket) 之外，[RawSocket](https://www.yuque.com/thyname/dart.io/rawsocket) 和 [RawServerSocket](https://www.yuque.com/thyname/dart.io/rawserversocket) 类也可用于对异步套接字 I/O 进行更底层的访问。

## 标准输出、错误和输入流

该库提供了标准输出、错误和输入流，分别命名为 [stdout](https://www.yuque.com/thyname/dart.io/stdout)、[stderr](https://www.yuque.com/thyname/dart.io/stderr) 和 [stdin](https://www.yuque.com/thyname/dart.io/stdin)。

[stdout](https://www.yuque.com/thyname/dart.io/stdout) 和 [stderr](https://www.yuque.com/thyname/dart.io/stderr) 流都是 [IOSink](https://www.yuque.com/thyname/dart.io/iosink)，具有相同的方法和属性集合。

向 [stdout](https://www.yuque.com/thyname/dart.io/stdout) 写入字符串：

```dart
stdout.writeln('Hello, World!');
```

向 [stderr](https://www.yuque.com/thyname/dart.io/stderr) 写入一组对象：

```dart
stderr.writeAll([ 'That ', 'is ', 'an ', 'error.', '\n']);
```

标准输入流是一个真正的 [Stream](https://www.yuque.com/thyname/dart.async/stream)，因此它继承了 [Stream](https://www.yuque.com/thyname/dart.async/stream) 类的属性和方法。

要从命令行同步读取文本（程序会阻塞，等待用户输入信息）：

```dart
String? inputText = stdin.readLineSync();
```
