# FileMode

```dart
class FileMode {}
```

[File](https://www.yuque.com/thyname/dart.io/file) 可被打开的模式。

### read

```dart
dynamic read
```

仅以只读方式打开文件的模式。

### write

```dart
dynamic write
```

以可读写方式打开文件的模式。若文件已存在，将被覆盖；若文件不存在，则会被创建。

### append

```dart
dynamic append
```

以可读写方式打开文件、并在文件末尾进行写入的模式。若文件不存在，则会被创建。

### writeOnly

```dart
dynamic writeOnly
```

仅以*写入*方式打开文件的模式。若文件已存在，将被覆盖；若文件不存在，则会被创建。

### writeOnlyAppend

```dart
dynamic writeOnlyAppend
```

仅以*写入*方式打开文件、并在文件末尾进行写入的模式。若文件不存在，则会被创建。

# FileLock

```dart
class FileLock {}
```

请求对文件加锁时的锁类型。

### shared

```dart
dynamic shared
```

共享文件锁。

### exclusive

```dart
dynamic exclusive
```

独占文件锁。

### blockingShared

```dart
dynamic blockingShared
```

阻塞式共享文件锁。

### blockingExclusive

```dart
dynamic blockingExclusive
```

阻塞式独占文件锁。

# File

```dart
abstract interface class File implements FileSystemEntity {}
```

对文件系统上一个文件的引用。

`File` 持有一个 [path]，可对其执行各种操作。可以使用 [FileSystemEntity](https://www.yuque.com/thyname/dart.io/filesystementity) 继承而来的属性 [parent] 获取该文件的父目录。

使用一个路径名创建新的 `File` 对象，即可从程序中访问文件系统上指定的文件。

```dart
var myFile = File('file.txt');
```

`File` 类包含用于操作文件及其内容的方法。使用此类中的方法，可以打开和关闭文件，读写文件，创建和删除文件，以及检查文件是否存在。

在读写文件时，可以使用流（借助 [openRead]）、随机访问操作（借助 [open]），或使用诸如 [readAsString] 之类的便捷方法。

此类中的大多数方法都以同步/异步成对的形式出现，例如 [readAsString] 与 [readAsStringSync]。除非有特殊原因需要使用同步版本，否则应优先使用异步版本，以避免阻塞程序。

## 若 path 是一个链接

若 [path] 是一个符号链接而非文件，则 `File` 的方法将作用于该链接最终指向的目标，[delete] 和 [deleteSync] 除外，它们作用于链接本身。

## 从文件读取

以下代码示例使用异步方法 [readAsString] 将文件的全部内容作为字符串读取：

```dart
import 'dart:async';
import 'dart:io';

void main() {
  File('file.txt').readAsString().then((String contents) {
    print(contents);
  });
}
```

一种更灵活实用的读取文件方式是使用 [Stream](https://www.yuque.com/thyname/dart.async/stream)。使用 [openRead] 打开文件，会返回一个以字节块形式提供文件数据的流。读取该流即可在数据可用时处理文件内容。你可以依次使用各种转换器（transformer）将文件内容处理为所需格式，或为输出做准备。

在读取大文件、使用转换器处理数据，或需要与其他 API（例如 [WebSocket]）兼容时，可能会用到流。

```dart
import 'dart:io';
import 'dart:convert';
import 'dart:async';

void main() async {
  final file = File('file.txt');
  Stream<String> lines = file.openRead()
    .transform(utf8.decoder)       // 将字节解码为 UTF-8。
    .transform(LineSplitter());    // 将流转换为逐行内容。
  try {
    await for (var line in lines) {
      print('$line: ${line.length} characters');
    }
    print('File is now closed.');
  } catch (e) {
    print('Error: $e');
  }
}
```

## 写入文件

要将字符串写入文件，可使用 [writeAsString] 方法：

```dart
import 'dart:io';

void main() async {
  final filename = 'file.txt';
  var file = await File(filename).writeAsString('some content');
  // 对该文件执行某些操作。
}
```

也可以使用 [Stream](https://www.yuque.com/thyname/dart.async/stream) 写入文件。使用 [openWrite] 打开文件，会返回一个可向其写入数据的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink)。请务必使用 [IOSink.close] 方法关闭该 sink。

```dart
import 'dart:io';

void main() async {
  var file = File('file.txt');
  var sink = file.openWrite();
  sink.write('FILE ACCESSED ${DateTime.now()}\n');
  await sink.flush();

  // 关闭 IOSink 以释放系统资源。
  await sink.close();
}
```

## 异步方法的使用

为避免程序被意外阻塞，多个方法都是异步的，并返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)。例如，用于获取文件长度的 [length] 方法会返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)。等待该 future 完成即可获取结果。

```dart
import 'dart:io';

void main() async {
  final file = File('file.txt');

  var length = await file.length();
  print(length);
}
```

除 length 外，[exists]、[lastModified]、[stat] 等方法同样也是异步的。

## 特殊的 'nul' 文件

在 Linux 和 Mac 上，'/dev/null' 是一个特殊文件；在 Windows 上，'\\?\NUL' 是对应的特殊文件——写入该文件的所有内容都会被消耗并消失，所有的读取都会产生空输出。请注意，在 Windows 上，不带 '\\?\' 前缀的 'nul' 指的是当前目录下名为 'nul' 的普通文件。

## 其他资源

- Dart 库导览中的[文件与目录](https://dart.dev/guides/libraries/library-tour#files-and-directories)一节。

- [编写命令行应用](https://dart.dev/tutorials/server/cmdline)，一篇关于编写命令行应用的教程，其中包含文件与目录相关的信息。

### File()

```dart
File(String path)
```

创建一个 [File](https://www.yuque.com/thyname/dart.io/file) 对象。

若 [path] 为相对路径，使用时将相对于当前工作目录（参见 [Directory.current]）进行解析。

若 [path] 为绝对路径，则不受当前工作目录变化的影响。

### File.fromUri()

```dart
File.fromUri(Uri uri)
```

通过 URI 创建一个 [File](https://www.yuque.com/thyname/dart.io/file) 对象。

若 [uri] 无法引用一个文件，则会抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

### File.fromRawPath()

```dart
File.fromRawPath(Uint8List rawPath)
```

通过原始路径创建一个 [File](https://www.yuque.com/thyname/dart.io/file) 对象。

原始路径是操作系统用于表示路径的字节序列。

### create()

```dart
Future<File> create({bool recursive = false, bool exclusive = false})
```

创建该文件。

返回一个 `Future<File>`，其在文件创建完成后携带该文件完成。

若 [recursive] 为 `false`（默认值），仅当路径中的所有目录都已存在时，该文件才会被创建。若 [recursive] 为 `true`，则会先创建路径中所有尚不存在的父路径。

若 [exclusive] 为 `true` 且待创建的文件已存在，此操作将以 [PathExistsException](https://www.yuque.com/thyname/dart.io/pathexistsexception) 完成该 future。

若 [exclusive] 为 `false`，[create] 不会改动已存在的文件。若文件存在限制性权限，即便调用 [create] 也可能失败。

若操作失败，将以 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 完成该 future。

### createSync()

```dart
void createSync({bool recursive = false, bool exclusive = false})
```

同步地创建该文件。

若 [recursive] 为 `false`（默认值），仅当路径中的所有目录都已存在时，该文件才会被创建。若 [recursive] 为 `true`，则会先创建路径中所有尚不存在的父路径。

若 [exclusive] 为 `true` 且待创建的文件已存在，则抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

若 [exclusive] 为 `false`，[createSync] 不会改动已存在的文件。若文件存在限制性权限，即便调用 [createSync] 也可能失败。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### rename()

```dart
Future<File> rename(String newPath)
```

重命名此文件。

返回一个 `Future<File>`，其在完成时携带重命名后文件对应的 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [newPath] 是相对路径，会相对于当前工作目录（[Directory.current]）进行解析。这意味着，若仅想更改文件名而保持文件所在目录不变，需要构造一个以新名称结尾的完整新路径。示例：

```dart
Future<File> changeFileNameOnly(File file, String newFileName) {
  var path = file.path;
  var lastSeparator = path.lastIndexOf(Platform.pathSeparator);
  var newPath = path.substring(0, lastSeparator + 1) + newFileName;
  return file.rename(newPath);
}
```

在某些平台上，重命名操作无法将文件移动到不同的文件系统。若是这种情况，请改为使用 [copy] 将文件复制到新位置，然后删除原文件。

若 [newPath] 指向一个已存在的文件或链接，该实体会被先移除。若 [newPath] 指向一个已存在的目录，操作将失败，future 将以 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 完成。

### renameSync()

```dart
File renameSync(String newPath)
```

同步地重命名此文件。

返回重命名后文件对应的 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [newPath] 是相对路径，会相对于当前工作目录（[Directory.current]）进行解析。这意味着，若仅想更改文件名而保持文件所在目录不变，需要构造一个以新名称结尾的完整新路径。示例：

```dart
File changeFileNameOnlySync(File file, String newFileName) {
  var path = file.path;
  var lastSeparator = path.lastIndexOf(Platform.pathSeparator);
  var newPath = path.substring(0, lastSeparator + 1) + newFileName;
  return file.renameSync(newPath);
}
```

在某些平台上，重命名操作无法将文件移动到不同的文件系统。若是这种情况，请改为使用 [copySync] 将文件复制到新位置，然后 [deleteSync] 原文件。

若 [newPath] 指向一个已存在的文件或链接，该实体会被先移除。若 [newPath] 指向一个已存在的目录，则抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

删除此 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [recursive] 为 `false`：

- 若 [path] 对应一个常规文件、命名管道或套接字，则删除该路径。若 [path] 对应一个链接，且该链接解析指向一个文件，则会删除 [path] 处的链接。其他所有情况下，[delete] 都将以 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 完成。

若 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity](https://www.yuque.com/thyname/dart.io/filesystementity) 将被删除，无论其类型如何。若 [path] 对应一个文件或链接，则删除该文件或链接。若 [path] 对应一个目录，则该目录及其下所有子目录与文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这一行为使得 [delete] 可用于无条件删除任意文件系统对象。

若无法删除此 [File](https://www.yuque.com/thyname/dart.io/file)，则 [delete] 将以 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 完成。

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

同步地删除此 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [recursive] 为 `false`：

- 若 [path] 对应一个常规文件、命名管道或套接字，则删除该路径。若 [path] 对应一个链接，且该链接解析指向一个文件，则会删除 [path] 处的链接。其他所有情况下，[delete] 都将抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

若 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity](https://www.yuque.com/thyname/dart.io/filesystementity) 将被删除，无论其类型如何。若 [path] 对应一个文件或链接，则删除该文件或链接。若 [path] 对应一个目录，则该目录及其下所有子目录与文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这一行为使得 [delete] 可用于无条件删除任意文件系统对象。

若无法删除此 [File](https://www.yuque.com/thyname/dart.io/file)，则 [delete] 将抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### copy()

```dart
Future<File> copy(String newPath)
```

复制此文件。

若 [newPath] 是相对路径，会相对于当前工作目录（[Directory.current]）进行解析。

返回一个 `Future<File>`，其在完成时携带复制后文件对应的 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [newPath] 指向一个已存在的文件，该文件会被先移除。若 [newPath] 指向一个已存在的目录，操作将失败，future 将以异常完成。

### copySync()

```dart
File copySync(String newPath)
```

同步地复制此文件。

若 [newPath] 是相对路径，会相对于当前工作目录（[Directory.current]）进行解析。

返回复制后文件对应的 [File](https://www.yuque.com/thyname/dart.io/file)。

若 [newPath] 指向一个已存在的文件，该文件会被先移除。若 [newPath] 指向一个已存在的目录，操作将失败并抛出异常。

### length()

```dart
Future<int> length()
```

该文件的长度。

返回一个 `Future<int>`，其完成时携带以字节为单位的长度。

### lengthSync()

```dart
int lengthSync()
```

同步获取该文件的长度。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### absolute

```dart
File get absolute
```

一个路径为 [path] 绝对路径的 [File](https://www.yuque.com/thyname/dart.io/file)。

绝对路径的计算方式为：对相对路径添加当前工作目录作为前缀，或原样返回已是绝对路径的路径。

### lastAccessed()

```dart
Future<DateTime> lastAccessed()
```

该文件的最后访问时间。

返回一个 `Future<DateTime>`，其在该信息可用时，携带该文件最后一次被访问的日期与时间完成。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### lastAccessedSync()

```dart
DateTime lastAccessedSync()
```

该文件的最后访问时间。

在该信息可用时，返回该文件最后一次被访问的日期与时间。会阻塞，直到该信息可返回，或确定该信息不可用为止。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### setLastAccessed()

```dart
Future setLastAccessed(DateTime time)
```

修改该文件最后被访问的时间。

返回一个在该操作完成后即完成的 [Future](https://www.yuque.com/thyname/dart.async/future)。

若无法设置该时间，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### setLastAccessedSync()

```dart
void setLastAccessedSync(DateTime time)
```

同步地修改该文件最后被访问的时间。

若无法设置该时间，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### lastModified()

```dart
Future<DateTime> lastModified()
```

获取该文件的最后修改时间。

返回一个 `Future<DateTime>`，其在该信息可用时，携带该文件最后一次被修改的日期与时间完成。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### lastModifiedSync()

```dart
DateTime lastModifiedSync()
```

获取该文件的最后修改时间。

在该信息可用时，返回该文件最后一次被修改的日期与时间。会阻塞，直到该信息可返回，或确定该信息不可用为止。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### setLastModified()

```dart
Future setLastModified(DateTime time)
```

修改该文件最后被修改的时间。

返回一个在该操作完成后即完成的 [Future](https://www.yuque.com/thyname/dart.async/future)。

若无法设置该时间，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### setLastModifiedSync()

```dart
void setLastModifiedSync(DateTime time)
```

同步地修改该文件最后被修改的时间。

若无法设置属性，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### open()

```dart
Future<RandomAccessFile> open({FileMode mode = FileMode.read})
```

打开该文件以进行随机访问操作。

返回一个 `Future<RandomAccessFile>`，其在完成时携带已打开的随机访问文件。[RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile) 必须使用 [RandomAccessFile.close] 方法关闭。

文件可以三种模式打开：

- [FileMode.read]：以只读方式打开文件。

- [FileMode.write]：以可读写方式打开文件，并将文件截断为长度零。若文件不存在，则会创建该文件。

- [FileMode.append]：与 [FileMode.write] 相同，但不会截断文件。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### openSync()

```dart
RandomAccessFile openSync({FileMode mode = FileMode.read})
```

同步地打开该文件以进行随机访问操作。

其结果是一个可执行随机访问操作的 [RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile)。已打开的 [RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile) 必须使用 [RandomAccessFile.close] 方法关闭。

有关 [mode] 参数的信息，参见 [open]。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### openRead()

```dart
Stream<List<int>> openRead([int? start, int? end])
```

为此文件的内容创建一个新的独立 [Stream](https://www.yuque.com/thyname/dart.async/stream)。

若提供了 [start]，将从字节偏移量 [start] 处开始读取文件；否则从开头（索引 0）开始读取。

若提供了 [end]，则只读取到字节索引 [end] 为止；否则读取到文件末尾。

为确保系统资源被正确释放，必须将该流读取到底，或取消该流上的订阅。

若 [File](https://www.yuque.com/thyname/dart.io/file) 是一个[命名管道](https://en.wikipedia.org/wiki/Named_pipe)，则返回的 [Stream](https://www.yuque.com/thyname/dart.async/stream) 会等待管道的写入端被关闭后才发出 "done" 信号。若在管道被打开时没有任何写入者与之相连，则 [Stream.listen] 会一直等待，直到有写入者打开该管道。

打开或读取文件时发生的错误，将在返回的 [Stream](https://www.yuque.com/thyname/dart.async/stream) 上作为 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 错误事件出现，之后该 [Stream](https://www.yuque.com/thyname/dart.async/stream) 会被关闭。例如：

```dart
// 此示例将打印 "Error reading file" 消息，
// 且 `await for` 循环会正常结束，
// 不会收到任何数据事件。
final stream = File('does-not-exist')
    .openRead()
    .handleError((e) => print('Error reading file: $e'));
await for (final data in stream) {
  print(data);
}
```

### openWrite()

```dart
IOSink openWrite({FileMode mode = FileMode.write, Encoding encoding = utf8})
```

为该文件创建一个新的独立 [IOSink](https://www.yuque.com/thyname/dart.io/iosink)。

该 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 在不再使用时必须关闭，以释放系统资源。

文件对应的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 可以两种模式打开：

- [FileMode.write]：将文件截断为长度零。
- [FileMode.append]：将初始写入位置设为文件末尾。

通过返回的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 写入字符串时，将使用 [encoding] 指定的编码。返回的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 具有一个 `encoding` 属性，可在创建 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 后修改。

返回的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 不会将换行符（`"\n"`）转换为平台约定的行结束符（例如 Windows 上的 `"\r\n"`）。如需使用与平台相关的行结束符，请写入一个 [Platform.lineTerminator]。

若在打开或写入文件时发生错误，[IOSink.done]、[IOSink.flush] 与 [IOSink.close] 对应的 future 都将以 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception) 完成。必须处理来自 [IOSink.done] future 的错误，否则该错误将不会被捕获。

例如，可以对 [IOSink.done] 的错误调用 [FutureExtensions.ignore]，并记得在 `try`/`catch` 中 `await` [IOSink.flush] 和 [IOSink.close] 调用：

```dart
final sink = File('/tmp').openWrite(); // 无法写入 /tmp
sink.done.ignore();
sink.write("This is a test");
try {
  // 若未对以下调用之一使用 await，错误将被静默忽略！
  await sink.flush();
  await sink.close();
} on FileSystemException catch (e) {
  print('Error writing file: $e');
}
```

若要在 [IOSink.flush] 和 [IOSink.close] 的上下文之外异步处理错误，可以对 [IOSink.done] 使用 [Future.catchError]。

```dart
final sink = File('/tmp').openWrite(); // 无法写入 /tmp
sink.done.catchError((e) {
 // 处理该错误。
});
```

### readAsBytes()

```dart
Future<Uint8List> readAsBytes()
```

将文件的全部内容作为字节列表读取。

返回一个 `Future<Uint8List>`，其完成时携带作为文件内容的字节列表。

### readAsBytesSync()

```dart
Uint8List readAsBytesSync()
```

同步地将文件的全部内容作为字节列表读取。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### readAsString()

```dart
Future<String> readAsString({Encoding encoding = utf8})
```

使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 将文件的全部内容作为字符串读取。

返回一个 `Future<String>`，其在文件内容读取完成后携带该字符串完成。

### readAsStringSync()

```dart
String readAsStringSync({Encoding encoding = utf8})
```

同步地使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 将文件的全部内容作为字符串读取。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### readAsLines()

```dart
Future<List<String>> readAsLines({Encoding encoding = utf8})
```

使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 将文件的全部内容按行读取为文本。

返回一个 `Future<List<String>>`，其在文件内容读取完成后携带各行内容完成。

### readAsLinesSync()

```dart
List<String> readAsLinesSync({Encoding encoding = utf8})
```

同步地使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 将文件的全部内容按行读取为文本。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### writeAsBytes()

```dart
Future<File> writeAsBytes(List<int> bytes, {FileMode mode = FileMode.write, bool flush = false})
```

将字节列表写入文件。

打开文件，将字节列表写入其中，然后关闭该文件。返回一个 `Future<File>`，其在整个操作完成后携带此 [File](https://www.yuque.com/thyname/dart.io/file) 对象完成。

默认情况下，[writeAsBytes] 会以写入方式创建文件，并在文件已存在时将其截断。若要将字节追加到已有文件末尾，可传入可选参数 mode 为 [FileMode.append]。

[bytes] 中的元素应为 0 到 255 范围内的整数。任何不在该范围内的整数，在写入前都会被转换为一个字节。该转换等同于执行 `value.toUnsigned(8)`。

若参数 [flush] 设为 `true`，写入的数据会在返回的 future 完成之前被刷新到文件系统。

### writeAsBytesSync()

```dart
void writeAsBytesSync(List<int> bytes, {FileMode mode = FileMode.write, bool flush = false})
```

同步地将字节列表写入文件。

打开文件，将字节列表写入其中，然后关闭该文件。

默认情况下，[writeAsBytesSync] 会以写入方式创建文件，并在文件已存在时将其截断。若要将字节追加到已有文件末尾，可传入可选参数 mode 为 [FileMode.append]。

[bytes] 中的元素应为 0 到 255 范围内的整数。任何不在该范围内的整数，在写入前都会被转换为一个字节。该转换等同于执行 `value.toUnsigned(8)`。

若参数 [flush] 设为 `true`，写入的数据会在返回之前被刷新到文件系统。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### writeAsString()

```dart
Future<File> writeAsString(String contents, {FileMode mode = FileMode.write, Encoding encoding = utf8, bool flush = false})
```

将字符串写入文件。

打开文件，以给定编码写入该字符串，然后关闭该文件。返回一个 `Future<File>`，其在整个操作完成后携带此 [File](https://www.yuque.com/thyname/dart.io/file) 对象完成。

默认情况下，[writeAsString] 会以写入方式创建文件，并在文件已存在时将其截断。若要将字节追加到已有文件末尾，可传入可选参数 mode 为 [FileMode.append]。

若参数 [flush] 设为 `true`，写入的数据会在返回的 future 完成之前被刷新到文件系统。

此方法不会将换行符（`"\n"`）转换为平台约定的行结束符（例如 Windows 上的 `"\r\n"`）。如需在 [contents] 中使用与平台相关的行结束符分隔各行，请使用 [Platform.lineTerminator]。

### writeAsStringSync()

```dart
void writeAsStringSync(String contents, {FileMode mode = FileMode.write, Encoding encoding = utf8, bool flush = false})
```

同步地将字符串写入文件。

打开文件，以给定编码写入该字符串，然后关闭该文件。

默认情况下，[writeAsStringSync] 会以写入方式创建文件，并在文件已存在时将其截断。若要将字节追加到已有文件末尾，可传入可选参数 mode 为 [FileMode.append]。

若参数 [flush] 设为 `true`，写入的数据会在返回之前被刷新到文件系统。

此方法不会将换行符（`"\n"`）转换为平台约定的行结束符（例如 Windows 上的 `"\r\n"`）。如需在 [contents] 中使用与平台相关的行结束符分隔各行，请使用 [Platform.lineTerminator]。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### path

```dart
String get path
```

获取该文件的路径。

# RandomAccessFile

```dart
abstract interface class RandomAccessFile {}
```

对文件中数据的随机访问。

`RandomAccessFile` 对象通过在 [File](https://www.yuque.com/thyname/dart.io/file) 对象上调用 `open` 方法获得。

`RandomAccessFile` 同时具有异步和同步两种方法。所有异步方法都返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，而同步方法则会直接返回结果，并阻塞当前 isolate 直到结果就绪。

对于给定的 `RandomAccessFile` 实例，同一时刻最多只能有一个待处理的异步方法。若在某个异步方法仍在进行时调用了另一个异步方法，将抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

若有异步方法正在挂起，此时也无法调用任何同步方法，同样会抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### close()

```dart
Future<void> close()
```

关闭该文件。

返回一个在文件关闭后完成的 [Future](https://www.yuque.com/thyname/dart.async/future)。

### closeSync()

```dart
void closeSync()
```

同步地关闭该文件。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### readByte()

```dart
Future<int> readByte()
```

从文件中读取一个字节。

返回一个 `Future<int>`，其完成时携带该字节；若已到达文件末尾，则携带 -1。

### readByteSync()

```dart
int readByteSync()
```

同步地从文件中读取单个字节。

若已到达文件末尾，返回 -1。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### read()

```dart
Future<Uint8List> read(int count)
```

从文件中最多读取 [count] 个字节。

实际读取的字节数可能少于 [count]。例如在读取超出文件末尾，或从当前不包含额外数据的管道中读取时，就可能出现这种情况。

只有在读取超出文件末尾，或 [count] 为 `0` 时，才会返回一个空的 [Uint8List](https://www.yuque.com/thyname/dart.typed_data/uint8list)。

### readSync()

```dart
Uint8List readSync(int count)
```

同步地从文件中最多读取 [count] 个字节。

实际读取的字节数可能少于 [count]。例如在读取超出文件末尾，或从当前不包含额外数据的管道中读取时，就可能出现这种情况。

只有在读取超出文件末尾，或 [count] 为 `0` 时，才会返回一个空的 [Uint8List](https://www.yuque.com/thyname/dart.typed_data/uint8list)。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### readInto()

```dart
Future<int> readInto(List<int> buffer, [int start = 0, int? end])
```

将数据读入一个已有的 [buffer]。

读取字节并将其写入 [buffer] 中从 [start] 到 [end] 的范围内。[start] 必须非负，且不大于 [buffer].length。若省略 [end]，其默认值为 [buffer].length；否则 [end] 必须不小于 [start] 且不大于 [buffer].length。

返回读取的字节数。若文件中没有 `end - start` 那么多字节可供读取，返回值可能小于该数值。

### readIntoSync()

```dart
int readIntoSync(List<int> buffer, [int start = 0, int? end])
```

同步地读入一个已有的 [buffer]。

读取字节并将其写入 [buffer] 中从 [start] 到 [end] 的范围内。[start] 必须非负，且不大于 [buffer].length。若省略 [end]，其默认值为 [buffer].length；否则 [end] 必须不小于 [start] 且不大于 [buffer].length。

返回读取的字节数。若文件中没有 `end - start` 那么多字节可供读取，返回值可能小于该数值。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### writeByte()

```dart
Future<RandomAccessFile> writeByte(int value)
```

向文件写入单个字节。

返回一个 `Future<RandomAccessFile>`，其在写入完成时携带此随机访问文件完成。

### writeByteSync()

```dart
int writeByteSync(int value)
```

同步地向文件写入单个字节。

成功时返回 1。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### writeFrom()

```dart
Future<RandomAccessFile> writeFrom(List<int> buffer, [int start = 0, int? end])
```

将一个 [buffer] 中的内容写入文件。

会读取 [buffer] 中从索引 [start] 到索引 [end] 的内容。[start] 必须非负，且不大于 [buffer].length。若省略 [end]，其默认值为 [buffer].length；否则 [end] 必须不小于 [start] 且不大于 [buffer].length。

返回一个 `Future<RandomAccessFile>`，其在写入完成时携带此 [RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile) 完成。

### writeFromSync()

```dart
void writeFromSync(List<int> buffer, [int start = 0, int? end])
```

同步地将一个 [buffer] 中的内容写入文件。

会读取 [buffer] 中从索引 [start] 到索引 [end] 的内容。[start] 必须非负，且不大于 [buffer].length。若省略 [end]，其默认值为 [buffer].length；否则 [end] 必须不小于 [start] 且不大于 [buffer].length。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### writeString()

```dart
Future<RandomAccessFile> writeString(String string, {Encoding encoding = utf8})
```

使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 将字符串写入文件。

返回一个 `Future<RandomAccessFile>`，其在写入完成时携带此随机访问文件完成。

### writeStringSync()

```dart
void writeStringSync(String string, {Encoding encoding = utf8})
```

使用给定的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 同步地将单个字符串写入文件。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### position()

```dart
Future<int> position()
```

获取文件中当前的字节位置。

返回一个 `Future<int>`，其完成时携带该位置。

### positionSync()

```dart
int positionSync()
```

同步获取文件中当前的字节位置。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### setPosition()

```dart
Future<RandomAccessFile> setPosition(int position)
```

设置文件中的字节位置。

返回一个 `Future<RandomAccessFile>`，其在该位置设置完成后携带此随机访问文件完成。

### setPositionSync()

```dart
void setPositionSync(int position)
```

同步地设置文件中的字节位置。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### truncate()

```dart
Future<RandomAccessFile> truncate(int length)
```

将文件截断（或扩展）为 [length] 字节。

返回一个 `Future<RandomAccessFile>`，其在截断操作执行完成后携带此随机访问文件完成。

### truncateSync()

```dart
void truncateSync(int length)
```

同步地将文件截断（或扩展）为 [length] 字节。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### length()

```dart
Future<int> length()
```

获取文件的长度。

返回一个 `Future<int>`，其完成时携带以字节为单位的长度。

### lengthSync()

```dart
int lengthSync()
```

同步获取文件的长度。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### flush()

```dart
Future<RandomAccessFile> flush()
```

将文件内容刷新到磁盘。

返回一个 `Future<RandomAccessFile>`，其在刷新操作完成时携带此随机访问文件完成。

### flushSync()

```dart
void flushSync()
```

同步地将文件内容刷新到磁盘。

若操作失败，抛出 [FileSystemException](https://www.yuque.com/thyname/dart.io/filesystemexception)。

### lock()

```dart
Future<RandomAccessFile> lock([FileLock mode = FileLock.exclusive, int start = 0, int end = -1])
```

对文件或文件的一部分加锁。

默认会获取独占锁，但可以通过 [mode] 参数覆盖此行为。

对文件从 [start] 到 [end] 的字节范围加锁，其中位置 `end` 处的字节不包含在内。若未指定任何参数，则对整个文件加锁；若仅指定了 `start`，则文件从字节位置 `start` 到文件末尾（无论其后续增长多大）都会被加锁。也可以显式指定一个超过文件当前长度的 `end` 值。

要获取文件的独占锁，该文件必须以写入方式打开。

若 [mode] 为 [FileLock.exclusive] 或 [FileLock.shared]，当无法获取锁时会报错。若 [mode] 为 [FileLock.blockingExclusive] 或 [FileLock.blockingShared]，则只有在获取到锁之后，返回的 [Future](https://www.yuque.com/thyname/dart.async/future) 才会被解析。

_注意_ 文件加锁在不同平台上的行为略有差异：

在 Linux 和 OS X 上，此功能使用的是建议性锁（advisory lock），其特殊语义是：当进程关闭该文件*任意*一个文件描述符时，与该文件关联的所有锁都会被移除。请注意，这实际上并不会真正锁定文件的访问。另外还需注意，建议性锁是进程级别的，这意味着同一进程内的多个 isolate 可以同时对同一文件获得独占锁。

在 Windows 上，加锁与解锁所使用的区域必须一致，否则解锁将导致操作系统错误 "The segment is already unlocked"。

在 Windows 上，对文件加锁会将该锁与获取该锁时所用的具体文件句柄相关联。若同一文件以不同的句柄再次被打开（即便在同一进程内），使用新句柄尝试写入被锁定的区域将会失败。为确保加锁后写入成功，应使用获取该锁的同一个 [RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile) 对象进行后续的写入操作。

### lockSync()

```dart
void lockSync([FileLock mode = FileLock.exclusive, int start = 0, int end = -1])
```

同步地对文件或文件的一部分加锁。

默认会获取独占锁，但可以通过 [mode] 参数覆盖此行为。

对文件从 [start] 到 [end] 的字节范围加锁，其中位置 `end` 处的字节不包含在内。若未指定任何参数，则对整个文件加锁；若仅指定了 `start`，则文件从字节位置 `start` 到文件末尾（无论其后续增长多大）都会被加锁。也可以显式指定一个超过文件当前长度的 `end` 值。

要获取文件的独占锁，该文件必须以写入方式打开。

若 [mode] 为 [FileLock.exclusive] 或 [FileLock.shared]，当无法获取锁时会抛出异常。若 [mode] 为 [FileLock.blockingExclusive] 或 [FileLock.blockingShared]，则只有在获取到锁之后，该调用才会返回。

_注意_ 文件加锁在不同平台上的行为略有差异：

在 Linux 和 OS X 上，此功能使用的是建议性锁（advisory lock），其特殊语义是：当进程关闭该文件*任意*一个文件描述符时，与该文件关联的所有锁都会被移除。请注意，这实际上并不会真正锁定文件的访问。另外还需注意，建议性锁是进程级别的，这意味着同一进程内的多个 isolate 可以同时对同一文件获得独占锁。

在 Windows 上，加锁与解锁所使用的区域必须一致，否则解锁将导致操作系统错误 "The segment is already unlocked"。

在 Windows 上，对文件加锁会将该锁与获取该锁时所用的具体文件句柄相关联。若同一文件以不同的句柄再次被打开（即便在同一进程内），使用新句柄尝试写入被锁定的区域将会失败。为确保加锁后写入成功，应使用获取该锁的同一个 [RandomAccessFile](https://www.yuque.com/thyname/dart.io/randomaccessfile) 对象进行后续的写入操作。

### unlock()

```dart
Future<RandomAccessFile> unlock([int start = 0, int end = -1])
```

解锁文件或文件的一部分。

解锁文件从 [start] 到 [end] 的字节范围，其中位置 `end` 处的字节不包含在内。若未指定任何参数，则解锁整个文件；若仅指定了 `start`，则文件从字节位置 `start` 到文件末尾都会被解锁。

_注意_ 文件加锁在不同平台上的行为略有差异：

详情参见 [lock]。

### unlockSync()

```dart
void unlockSync([int start = 0, int end = -1])
```

同步地解锁文件或文件的一部分。

解锁文件从 [start] 到 [end] 的字节范围，其中位置 `end` 处的字节不包含在内。若未指定任何参数，则解锁整个文件；若仅指定了 `start`，则文件从字节位置 `start` 到文件末尾都会被解锁。

_注意_ 文件加锁在不同平台上的行为略有差异：

详情参见 [lockSync]。

### toString()

```dart
String toString()
```

返回此随机访问文件的可读字符串表示形式。

### path

```dart
String get path
```

此随机访问文件底层对应的文件路径。

# FileSystemException

```dart
class FileSystemException implements IOException {}
```

文件操作失败时抛出的异常。

### message

```dart
String message
```

描述该错误的消息。

该消息不包含来自底层操作系统错误的详细信息。有关这方面的信息，请查看 [osError]。

### path

```dart
String? path
```

发生该错误所在的文件系统路径。

若该异常与某个文件系统路径没有直接关联，可为 `null`。

### osError

```dart
OSError? osError
```

底层操作系统错误。

若该异常并非由操作系统错误引起，可为 `null`。

### FileSystemException()

```dart
FileSystemException([String message = "", String? path = "", OSError? osError])
```

使用可选的各部分创建一个新的文件系统异常。

创建一个异常，其 [FileSystemException.message]、[FileSystemException.path] 和 [FileSystemException.osError] 值取自同名可选参数。

若省略，[message] 和 [path] 默认为空字符串，[osError] 默认为 `null`。

### toString()

```dart
String toString()
```

# PathAccessException

```dart
class PathAccessException extends FileSystemException {}
```

当文件操作因缺少必要的访问权限而失败时抛出的异常。

### PathAccessException()

```dart
PathAccessException(String path, OSError osError, [String message = ""])
```

### toString()

```dart
String toString()
```

# PathExistsException

```dart
class PathExistsException extends FileSystemException {}
```

当文件操作因目标路径已存在而失败时抛出的异常。

### PathExistsException()

```dart
PathExistsException(String path, OSError osError, [String message = ""])
```

### toString()

```dart
String toString()
```

# PathNotFoundException

```dart
class PathNotFoundException extends FileSystemException {}
```

当文件操作因某个文件或目录不存在而失败时抛出的异常。

### PathNotFoundException()

```dart
PathNotFoundException(String path, OSError osError, [String message = ""])
```

### toString()

```dart
String toString()
```

# ReadPipe

```dart
abstract interface class ReadPipe implements Stream<List<int>> {}
```

由 [Pipe.create] 创建的 [Pipe](https://www.yuque.com/thyname/dart.io/pipe) 中的"读取"端。

只要 [Pipe](https://www.yuque.com/thyname/dart.io/pipe) 的"写入"端（即 [Pipe.write]）尚未关闭，读取流就会持续监听。

```dart
final pipe = await Pipe.create();
pipe.read.transform(utf8.decoder).listen((data) {
  print(data);
}, onDone: () => print('Done'));
```

# WritePipe

```dart
abstract interface class WritePipe implements IOSink {}
```

由 [Pipe.create] 创建的 [Pipe](https://www.yuque.com/thyname/dart.io/pipe) 中的"写入"端。

```dart
final pipe = await Pipe.create();
pipe.write.add("Hello World!".codeUnits);
await pipe.write.flush();
await pipe.write.close();
```

# Pipe

```dart
abstract interface class Pipe {}
```

一个用于单向发送数据的匿名管道，即写入 [write] 的数据可以通过 [read] 读取。

在 macOS 和 Linux（Android 除外）上，管道的 [read] 或 [write] 部分可以被传递给另一个进程，用于实现进程间通信。

例如：

```dart
final pipe = await Pipe.create();
final socket = await RawSocket.connect(address, 0);
socket.sendMessage(<SocketControlMessage>[
SocketControlMessage.fromHandles(
    <ResourceHandle>[ResourceHandle.fromReadPipe(pipe.read)])
], 'Hello'.codeUnits);
pipe.write.add('Hello over pipe!'.codeUnits);
await pipe.write.flush();
await pipe.write.close();
```

### read

```dart
ReadPipe get read
```

[Pipe](https://www.yuque.com/thyname/dart.io/pipe) 的读取端。

### write

```dart
WritePipe get write
```

[Pipe](https://www.yuque.com/thyname/dart.io/pipe) 的写入端。

### create()

```dart
Future<Pipe> create()
```

### Pipe.createSync()

```dart
Pipe.createSync()
```

同步地创建一个匿名管道。
