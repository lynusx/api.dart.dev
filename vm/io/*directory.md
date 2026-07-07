# Directory

```dart
abstract interface class Directory implements FileSystemEntity {}
```

对文件系统上一个目录（或称*文件夹*）的引用。

[Directory] 是一个持有 [path] 的对象，可对其执行各种操作。目录的路径既可以是[绝对路径][absolute]，也可以是相对路径。由于它是一个 [FileSystemEntity]，因此可以访问其[父目录][parent]。

[Directory] 还提供了对系统临时文件目录（[systemTemp]）的静态访问，以及访问和修改[当前目录][current]的能力。

创建一个新的 [Directory] 以访问指定路径的目录：

```dart
var myDir = Directory('myDir');
```

[Directory] 的大多数实例方法都同时存在同步和异步两种版本，例如 [create] 和 [createSync]。除非有特殊原因需要使用同步版本，否则应优先使用异步版本，以避免阻塞程序。

## 创建目录

以下代码示例使用 [create] 方法创建一个目录。通过将 `recursive` 参数设为 true，可以创建指定目录及其所有尚不存在的父目录。

```dart
import 'dart:io';

void main() async {
  // 创建 dir/ 和 dir/subdir/。
  var directory = await Directory('dir/subdir').create(recursive: true);
  print(directory.path);
}
```

## 列出目录的条目

使用 [list] 或 [listSync] 方法获取某个目录中包含的文件与目录。将 `recursive` 设为 true 可递归列出所有子目录。将 `followLinks` 设为 true 可跟随符号链接。[list] 方法返回一个 [FileSystemEntity] 对象的 [Stream]。监听该流即可在找到每个对象时进行访问：

```dart
import 'dart:io';

void main() async {
  // 获取系统临时目录。
  var systemTempDir = Directory.systemTemp;

  // 列出目录内容，递归进入子目录，
  // 但不跟随符号链接。
  await for (var entity in
      systemTempDir.list(recursive: true, followLinks: false)) {
    print(entity.path);
  }
}
```

## 异步方法的使用

I/O 操作在等待完成期间可能会阻塞程序一段时间。为避免这种情况，所有涉及 I/O 的方法都提供了返回 [Future] 的异步版本。该 future 会在 I/O 操作完成时完成。在 I/O 操作进行期间，Dart 程序不会被阻塞，可以执行其他操作。

例如，用于判断目录是否存在的 [exists] 方法，会通过 [Future] 异步返回一个布尔值。

```dart
import 'dart:io';

void main() async {
  final myDir = Directory('dir');
  var isThere = await myDir.exists();
  print(isThere ? 'exists' : 'nonexistent');
}
```

除 [exists] 外，[stat]、[rename] 等方法同样也是异步的。

## 其他资源

- Dart 库导览中的[文件与目录](https://dart.dev/guides/libraries/library-tour#files-and-directories)一节。

- [编写命令行应用](https://dart.dev/tutorials/server/cmdline)，一篇关于编写命令行应用的教程，其中包含文件与目录相关的信息。

### path

```dart
String get path
```

获取此目录的路径。

### Directory()

```dart
Directory(String path)
```

创建一个 [Directory] 对象。

若 [path] 为相对路径，使用时将相对于当前工作目录（参见 [Directory.current]）进行解析。

若 [path] 为绝对路径，则不受当前工作目录变化的影响。

### Directory.fromRawPath()

```dart
Directory.fromRawPath(Uint8List path)
```

### Directory.fromUri()

```dart
Directory.fromUri(Uri uri)
```

通过 URI 创建一个 [Directory]。

若 [uri] 无法引用一个目录，则会抛出 [UnsupportedError]。

### current

```dart
Directory get current
```

创建一个指向当前工作目录的目录对象。

### uri

```dart
Uri get uri
```

表示该目录位置的 [Uri]。

若该实体的 [path] 为绝对路径，则该 URI 的 scheme 始终为 "file"；否则 scheme 为空且该 URI 为相对路径。该 URI 的路径始终以斜杠（'/'）结尾。

### current

```dart
void set current(dynamic path)
```

设置 Dart 进程的当前工作目录。

此设置会影响所有正在运行的 isolate。新值可以是一个 [Directory] 或一个字符串。

新值会原样传递给操作系统的系统调用，因此若传入的是相对路径，将由操作系统进行解析。

请注意，设置当前工作目录是一个同步操作，并且会更改*所有* isolate 的工作目录。

请谨慎使用此功能——尤其是在存在待处理的异步操作或其他 isolate 正在操作文件系统时。在异步操作挂起期间更改工作目录，可能导致意外结果。

### create()

```dart
Future<Directory> create({bool recursive = false})
```

若目录不存在，则创建该目录。

若 [recursive] 为 false，则只创建路径中的最后一级目录。若 [recursive] 为 true，则会创建路径中所有尚不存在的组成部分。若目录已存在，则不执行任何操作。

返回一个 `Future<Directory>`，该目录创建完成后即以此目录完成。若目录无法创建，该 future 将以异常完成。

### createSync()

```dart
void createSync({bool recursive = false})
```

同步地在目录不存在时创建该目录。

若 [recursive] 为 false，则只创建路径中的最后一级目录。若 [recursive] 为 true，则会创建路径中所有尚不存在的组成部分。若目录已存在，则不执行任何操作。

若目录无法创建，则抛出异常。

### systemTemp

```dart
Directory get systemTemp
```

系统临时目录。

这是操作系统提供的用于创建临时文件与目录的目录。系统临时目录的位置因平台而异，在某些平台上可通过环境变量控制。

### createTemp()

```dart
Future<Directory> createTemp([String? prefix])
```

在此目录中创建一个临时目录。

会在 [prefix] 后追加随机字符以生成唯一的目录名。若未提供 [prefix] 或其为 null，则使用空字符串作为 [prefix]。

返回一个 `Future<Directory>`，其在完成时携带新创建的临时目录。

### createTempSync()

```dart
Directory createTempSync([String? prefix])
```

同步地在此目录中创建一个临时目录。

会在 [prefix] 后追加随机字符以生成唯一的目录名。若未提供 [prefix] 或其为 null，则使用空字符串作为 [prefix]。

返回新创建的临时目录。

### resolveSymbolicLinks()

```dart
Future<String> resolveSymbolicLinks()
```

### resolveSymbolicLinksSync()

```dart
String resolveSymbolicLinksSync()
```

### rename()

```dart
Future<Directory> rename(String newPath)
```

重命名此目录。

返回一个 `Future<Directory>`，其在完成时携带重命名后目录对应的 [Directory]。

若 [newPath] 指向一个已存在的目录，其行为因平台而异。在所有平台上，若该已存在的目录非空，future 都将以 [FileSystemException] 完成。在 POSIX 系统上，若 [newPath] 指向一个已存在的空目录，该目录会先被删除，然后再执行本目录的重命名。

若 [newPath] 指向一个已存在的文件或链接，操作将失败，future 将以 [FileSystemException] 完成。

### renameSync()

```dart
Directory renameSync(String newPath)
```

同步地重命名此目录。

返回重命名后目录对应的 [Directory]。

若 [newPath] 指向一个已存在的目录，其行为因平台而异。在所有平台上，若该已存在的目录非空，都将抛出 [FileSystemException]。在 POSIX 系统上，若 [newPath] 指向一个已存在的空目录，该目录会先被删除，然后再执行本目录的重命名。

若 [newPath] 指向一个已存在的文件或链接，操作将失败，并抛出 [FileSystemException]。

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

删除此 [Directory]。

若 [recursive] 为 `false`：

- 若 [path] 对应一个空目录，则删除该目录。若 [path] 对应一个链接，且该链接解析指向一个目录，则会删除 [path] 处的链接。其他所有情况下，[delete] 都将以 [FileSystemException] 完成。

若 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity] 将被删除，无论其类型如何。若 [path] 对应一个文件或链接，则删除该文件或链接。若 [path] 对应一个目录，则该目录及其下所有子目录与文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这一行为使得 [delete] 可用于无条件删除任意文件系统对象。

若无法删除此 [Directory]，则 [delete] 将以 [FileSystemException] 完成。

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

同步地删除此 [Directory]。

若 [recursive] 为 `false`：

- 若 [path] 对应一个空目录，则删除该目录。若 [path] 对应一个链接，且该链接解析指向一个目录，则会删除 [path] 处的链接。其他所有情况下，[delete] 都将抛出 [FileSystemException]。

若 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity] 将被删除，无论其类型如何。若 [path] 对应一个文件或链接，则删除该文件或链接。若 [path] 对应一个目录，则该目录及其下所有子目录与文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这一行为使得 [delete] 可用于无条件删除任意文件系统对象。

若无法删除此 [Directory]，则 [delete] 将抛出 [FileSystemException]。

### absolute

```dart
Directory get absolute
```

一个路径为此 [Directory] 绝对路径的 [Directory]。

绝对路径的计算方式为：对相对路径添加当前工作目录作为前缀，或原样返回已是绝对路径的路径。

### list()

```dart
Stream<FileSystemEntity> list({bool recursive = false, bool followLinks = true})
```

列出此 [Directory] 的子目录与文件。

可选择递归进入子目录。

若 [followLinks] 为 `false`，则任何找到的符号链接都将作为 [Link] 对象报告，而不是作为目录或文件报告，且不会递归进入这些链接。

若 [followLinks] 为 `true`，则有效的链接会根据其指向的内容报告为目录或文件，若 [recursive] 为 `true`，指向目录的链接也会被递归进入。

失效的链接会作为 [Link] 对象报告。

若符号链接在文件系统中形成了循环，则递归列出时不会在同一次递归遍历中两次跟随同一链接，但在第二次遇到该链接时会将其作为 [Link] 报告。

结果是一个由目录、文件与链接对应的 [FileSystemEntity] 对象组成的 [Stream]。该 [Stream] 中条目的顺序是任意的，且不包含特殊条目 `'.'` 和 `'..'`。

### listSync()

```dart
List<FileSystemEntity> listSync({bool recursive = false, bool followLinks = true})
```

列出此 [Directory] 的子目录与文件。可选择递归进入子目录。

若 [followLinks] 为 `false`，则任何找到的符号链接都将作为 [Link] 对象报告，而不是作为目录或文件报告，且不会递归进入这些链接。

若 [followLinks] 为 `true`，则有效的链接会根据其指向的内容报告为目录或文件，若 [recursive] 为 `true`，指向目录的链接也会被递归进入。

失效的链接会作为 [Link] 对象报告。

若链接在文件系统中形成了循环，则递归列出时不会在同一次递归遍历中两次跟随同一链接，但在第二次遇到该链接时会将其作为 [Link] 报告。

返回一个由目录、文件与链接对应的 [FileSystemEntity] 对象组成的 [List]。该 [List] 中条目的顺序是任意的，且不包含特殊条目 `'.'` 和 `'..'`。

### toString()

```dart
String toString()
```

返回此 [Directory] 的可读字符串表示形式。
