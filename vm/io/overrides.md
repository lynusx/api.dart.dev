# IOOverrides

```dart
abstract base class IOOverrides {}
```

Facilities for overriding various APIs of `dart:io` with mock implementations.

This abstract base class should be extended with overrides for the operations needed to construct mocks. The implementations in this base class default to the actual `dart:io` implementation. For example:

```dart
class MyDirectory implements Directory {
  ...
  // An implementation of the Directory interface
  ...
}

void main() {
  IOOverrides.runZoned(() {
    ...
    // Operations will use MyDirectory instead of dart:io's Directory
    // implementation whenever Directory is used.
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

The [IOOverrides] to use in the root [Zone].

These are the [IOOverrides] that will be used in the root [Zone], and in [Zone]'s that do not set [IOOverrides] and whose ancestors up to the root [Zone] also do not set [IOOverrides].

### runZoned()

```dart
R runZoned<R>(R body(), {Directory Function(String)? createDirectory, Directory Function()? getCurrentDirectory, void Function(String)? setCurrentDirectory, Directory Function()? getSystemTempDirectory, File Function(String)? createFile, Future<FileStat> Function(String)? stat, FileStat Function(String)? statSync, Future<bool> Function(String, String)? fseIdentical, bool Function(String, String)? fseIdenticalSync, Future<FileSystemEntityType> Function(String, bool)? fseGetType, FileSystemEntityType Function(String, bool)? fseGetTypeSync, Stream<FileSystemEvent> Function(String, int, bool)? fsWatch, bool Function()? fsWatchIsSupported, Link Function(String)? createLink, Future<Socket> Function(dynamic, int, {dynamic sourceAddress, int sourcePort, Duration? timeout})? socketConnect, Future<ConnectionTask<Socket>> Function(dynamic, int, {dynamic sourceAddress, int sourcePort})? socketStartConnect, Future<ServerSocket> Function(dynamic, int, {int backlog, bool v6Only, bool shared})? serverSocketBind, Stdin Function()? stdin, Stdout Function()? stdout, Stdout Function()? stderr, Never Function(int)? exit})
```

Runs [body] in a fresh [Zone] using the provided overrides.

See the documentation on the corresponding methods of [IOOverrides] for information about what the optional arguments do.

### runWithIOOverrides()

```dart
R runWithIOOverrides<R>(R body(), IOOverrides overrides)
```

Runs [body] in a fresh [Zone] using the overrides found in [overrides].

Note that [overrides] should be an instance of a class that extends [IOOverrides].

### createDirectory()

```dart
Directory createDirectory(String path)
```

Creates a new [Directory] object for the given [path].

When this override is installed, this function overrides the behavior of `new Directory()` and `new Directory.fromUri()`.

### getCurrentDirectory()

```dart
Directory getCurrentDirectory()
```

Returns the current working directory.

When this override is installed, this function overrides the behavior of the static getter `Directory.current`

### setCurrentDirectory()

```dart
void setCurrentDirectory(String path)
```

Sets the current working directory to be [path].

When this override is installed, this function overrides the behavior of the setter `Directory.current`.

### getSystemTempDirectory()

```dart
Directory getSystemTempDirectory()
```

Returns the system temporary directory.

When this override is installed, this function overrides the behavior of `Directory.systemTemp`.

### createFile()

```dart
File createFile(String path)
```

Creates a new [File] object for the given [path].

When this override is installed, this function overrides the behavior of `new File()` and `new File.fromUri()`.

### stat()

```dart
Future<FileStat> stat(String path)
```

Asynchronously returns [FileStat] information for [path].

When this override is installed, this function overrides the behavior of `FileStat.stat()`.

### statSync()

```dart
FileStat statSync(String path)
```

Returns [FileStat] information for [path].

When this override is installed, this function overrides the behavior of `FileStat.statSync()`.

### fseIdentical()

```dart
Future<bool> fseIdentical(String path1, String path2)
```

Asynchronously returns `true` if [path1] and [path2] are paths to the same file system object.

When this override is installed, this function overrides the behavior of `FileSystemEntity.identical`.

### fseIdenticalSync()

```dart
bool fseIdenticalSync(String path1, String path2)
```

Returns `true` if [path1] and [path2] are paths to the same file system object.

When this override is installed, this function overrides the behavior of `FileSystemEntity.identicalSync`.

### fseGetType()

```dart
Future<FileSystemEntityType> fseGetType(String path, bool followLinks)
```

Asynchronously returns the [FileSystemEntityType] for [path].

When this override is installed, this function overrides the behavior of `FileSystemEntity.type`.

### fseGetTypeSync()

```dart
FileSystemEntityType fseGetTypeSync(String path, bool followLinks)
```

Returns the [FileSystemEntityType] for [path].

When this override is installed, this function overrides the behavior of `FileSystemEntity.typeSync`.

### fsWatch()

```dart
Stream<FileSystemEvent> fsWatch(String path, int events, bool recursive)
```

Returns a [Stream] of [FileSystemEvent]s.

When this override is installed, this function overrides the behavior of `FileSystemEntity.watch()`.

### fsWatchIsSupported()

```dart
bool fsWatchIsSupported()
```

Returns `true` when [FileSystemEntity.watch] is supported.

When this override is installed, this function overrides the behavior of `FileSystemEntity.isWatchSupported`.

### createLink()

```dart
Link createLink(String path)
```

Returns a new [Link] object for the given [path].

When this override is installed, this function overrides the behavior of `new Link()` and `new Link.fromUri()`.

### socketConnect()

```dart
Future<Socket> socketConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0, Duration? timeout})
```

Asynchronously returns a [Socket] connected to the given host and port.

When this override is installed, this function overrides the behavior of `Socket.connect(...)`.

### socketStartConnect()

```dart
Future<ConnectionTask<Socket>> socketStartConnect(dynamic host, int port, {dynamic sourceAddress, int sourcePort = 0})
```

Asynchronously returns a [ConnectionTask] that connects to the given host and port when successful.

When this override is installed, this function overrides the behavior of `Socket.startConnect(...)`.

### serverSocketBind()

```dart
Future<ServerSocket> serverSocketBind(dynamic address, int port, {int backlog = 0, bool v6Only = false, bool shared = false})
```

Asynchronously returns a [ServerSocket] that connects to the given address and port when successful.

When this override is installed, this function overrides the behavior of `ServerSocket.bind(...)`.

### exit()

```dart
Never exit(int code)
```

Exit the Dart VM process immediately with the given exit code.

When this override is installed, this function overrides the behavior of `exit(...)`.

### stdin

```dart
Stdin get stdin
```

The standard input stream of data read by this program.

When this override is installed, this getter overrides the behavior of the top-level `stdin` getter.

### stdout

```dart
Stdout get stdout
```

The standard output stream of data written by this program.

When this override is installed, this getter overrides the behavior of the top-level `stdout` getter.

### stderr

```dart
Stdout get stderr
```

The standard output stream of errors written by this program.

When this override is installed, this getter overrides the behavior of the top-level `stderr` getter.
