# IOOverrides

```dart
abstract base class IOOverrides {}
```

用于以模拟（mock）实现覆盖 `dart:io` 各种 API 的工具。

这个抽象基类应通过为构建模拟对象所需的操作提供覆盖来进行扩展。此基类中的实现默认使用实际的 `dart:io` 实现。例如：

```dart
class MyDirectory implements Directory {
  ...
  // Directory 接口的一个实现
  ...
}

void main() {
  IOOverrides.runZoned(() {
    ...
    // 只要使用 Directory，操作将使用 MyDirectory
    // 而不是 dart:io 的 Directory 实现。
    ...
  }, createDirectory: (String path) => new MyDirectory(path));
}
```

### current

```dart
IOOverrides? get current
```

### global

```dart
set global(IOOverrides? overrides)
```

在根 [Zone](https://www.yuque.com/thyname/dart.async/zone) 中使用的 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides)。

这些是将在根 [Zone](https://www.yuque.com/thyname/dart.async/zone) 中使用的 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides)，也用于那些未设置 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides) 且其祖先一直到根 [Zone](https://www.yuque.com/thyname/dart.async/zone) 也都未设置 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides) 的 [Zone](https://www.yuque.com/thyname/dart.async/zone)。

### runZoned()

```dart
R runZoned<R>(R body(), {Directory Function(String)? createDirectory, Directory Function()? getCurrentDirectory, void Function(String)? setCurrentDirectory, Directory Function()? getSystemTempDirectory, File Function(String)? createFile, Future<FileStat> Function(String)? stat, FileStat Function(String)? statSync, Future<bool> Function(String, String)? fseIdentical, bool Function(String, String)? fseIdenticalSync, Future<FileSystemEntityType> Function(String, bool)? fseGetType, FileSystemEntityType Function(String, bool)? fseGetTypeSync, Stream<FileSystemEvent> Function(String, int, bool)? fsWatch, bool Function()? fsWatchIsSupported, Link Function(String)? createLink, Future<Socket> Function(dynamic, int, {dynamic sourceAddress, int sourcePort, Duration? timeout})? socketConnect, Future<ConnectionTask<Socket>> Function(dynamic, int, {dynamic sourceAddress, int sourcePort})? socketStartConnect, Future<ServerSocket> Function(dynamic, int, {int backlog, bool v6Only, bool shared})? serverSocketBind, Stdin Function()? stdin, Stdout Function()? stdout, Stdout Function()? stderr, Never Function(int)? exit})
```

在使用所提供覆盖的新建 [Zone](https://www.yuque.com/thyname/dart.async/zone) 中运行 [body]。

有关这些可选参数的作用，请参阅 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides) 相应方法的文档。

### runWithIOOverrides()

```dart
R runWithIOOverrides<R>(R body(), IOOverrides overrides)
```

在使用 [overrides] 中所包含覆盖的新建 [Zone](https://www.yuque.com/thyname/dart.async/zone) 中运行 [body]。

注意，[overrides] 应为扩展自 [IOOverrides](https://www.yuque.com/thyname/dart.io/iooverrides) 的类的实例。

### createDirectory()

```dart
Directory createDirectory(String path)
```

为给定的 [path] 创建一个新的 [Directory](https://www.yuque.com/thyname/dart.io/directory) 对象。

安装此覆盖后，此函数将覆盖 `new Directory()` 和 `new Directory.fromUri()` 的行为。

### getCurrentDirectory()

```dart
Directory getCurrentDirectory()
```

返回当前工作目录。

安装此覆盖后，此函数将覆盖静态获取器 `Directory.current` 的行为。

### setCurrentDirectory()

```dart
void setCurrentDirectory(String path)
```

将当前工作目录设置为 [path]。

安装此覆盖后，此函数将覆盖设置器 `Directory.current` 的行为。

### getSystemTempDirectory()

```dart
Directory getSystemTempDirectory()
```

返回系统临时目录。

安装此覆盖后，此函数将覆盖 `Directory.systemTemp` 的行为。

### createFile()

```dart
File createFile(String path)
```

为给定的 [path] 创建一个新的 [File](https://www.yuque.com/thyname/dart.io/file) 对象。

安装此覆盖后，此函数将覆盖 `new File()` 和 `new File.fromUri()` 的行为。

### stat()

```dart
Future<FileStat> stat(String path)
```

异步返回 [path] 的 [FileStat](https://www.yuque.com/thyname/dart.io/filestat) 信息。

安装此覆盖后，此函数将覆盖 `FileStat.stat()` 的行为。

### statSync()

```dart
FileStat statSync(String path)
```

返回 [path] 的 [FileStat](https://www.yuque.com/thyname/dart.io/filestat) 信息。

安装此覆盖后，此函数将覆盖 `FileStat.statSync()` 的行为。

### fseIdentical()

```dart
Future<bool> fseIdentical(String path1, String path2)
```

如果 [path1] 和 [path2] 是指向同一文件系统对象的路径，则异步返回 `true`。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.identical` 的行为。

### fseIdenticalSync()

```dart
bool fseIdenticalSync(String path1, String path2)
```

如果 [path1] 和 [path2] 是指向同一文件系统对象的路径，则返回 `true`。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.identicalSync` 的行为。

### fseGetType()

```dart
Future<FileSystemEntityType> fseGetType(String path, bool followLinks)
```

异步返回 [path] 的 [FileSystemEntityType](https://www.yuque.com/thyname/dart.io/filesystementitytype)。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.type` 的行为。

### fseGetTypeSync()

```dart
FileSystemEntityType fseGetTypeSync(String path, bool followLinks)
```

返回 [path] 的 [FileSystemEntityType](https://www.yuque.com/thyname/dart.io/filesystementitytype)。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.typeSync` 的行为。

### fsWatch()

```dart
Stream<FileSystemEvent> fsWatch(String path, int events, bool recursive)
```

返回一个 [FileSystemEvent](https://www.yuque.com/thyname/dart.io/filesystemevent) 的 [Stream](https://www.yuque.com/thyname/dart.async/stream)。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.watch()` 的行为。

### fsWatchIsSupported()

```dart
bool fsWatchIsSupported()
```

当支持 [FileSystemEntity.watch] 时返回 `true`。

安装此覆盖后，此函数将覆盖 `FileSystemEntity.isWatchSupported` 的行为。

### createLink()

```dart
Link createLink(String path)
```

为给定的 [path] 返回一个新的 [Link](https://www.yuque.com/thyname/dart.io/link) 对象。

安装此覆盖后，此函数将覆盖 `new Link()` 和 `new Link.fromUri()` 的行为。

### socketConnect()

```dart
Future<Socket> socketConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

异步返回一个连接到给定主机和端口的 [Socket](https://www.yuque.com/thyname/dart.io/socket)。

安装此覆盖后，此函数将覆盖 `Socket.connect(...)` 的行为。

### socketStartConnect()

```dart
Future<ConnectionTask<Socket>> socketStartConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

异步返回一个 [ConnectionTask](https://www.yuque.com/thyname/dart.io/connectiontask)，成功时会连接到给定的主机和端口。

安装此覆盖后，此函数将覆盖 `Socket.startConnect(...)` 的行为。

### serverSocketBind()

```dart
Future<ServerSocket> serverSocketBind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

异步返回一个 [ServerSocket](https://www.yuque.com/thyname/dart.io/serversocket)，成功时会连接到给定的地址和端口。

安装此覆盖后，此函数将覆盖 `ServerSocket.bind(...)` 的行为。

### exit()

```dart
Never exit(int code)
```

立即以给定的退出代码退出 Dart 虚拟机进程。

安装此覆盖后，此函数将覆盖 `exit(...)` 的行为。

### stdin

```dart
Stdin get stdin
```

本程序读取数据的标准输入流。

安装此覆盖后，此获取器将覆盖顶层获取器 `stdin` 的行为。

### stdout

```dart
Stdout get stdout
```

本程序写入数据的标准输出流。

安装此覆盖后，此获取器将覆盖顶层获取器 `stdout` 的行为。

### stderr

```dart
Stdout get stderr
```

本程序写入错误信息的标准输出流。

安装此覆盖后，此获取器将覆盖顶层获取器 `stderr` 的行为。
