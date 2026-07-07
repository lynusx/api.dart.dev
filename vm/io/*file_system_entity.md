# FileSystemEntityType

```dart
final class FileSystemEntityType {}
```

文件系统上某个实体的类型，例如文件、目录或链接。

这些常量由 [FileSystemEntity] 类用于指示对象的类型。

### file

```dart
dynamic file
```

### directory

```dart
dynamic directory
```

### link

```dart
dynamic link
```

### unixDomainSock

```dart
dynamic unixDomainSock
```

### pipe

```dart
dynamic pipe
```

### notFound

```dart
dynamic notFound
```

### NOT_FOUND

```dart
dynamic NOT_FOUND
```

### toString()

```dart
String toString()
```

# FileStat

```dart
interface class FileStat {}
```

对文件系统对象调用 POSIX `stat()` 函数的结果。

这是一个不可变对象，表示 `stat()` 调用时的快照值。

### changed

```dart
DateTime changed
```

该文件系统对象的数据或元数据最后一次发生变化的时间。

在 Windows 平台上，此值为文件的创建时间。

### modified

```dart
DateTime modified
```

该文件系统对象的数据最后一次发生变化的时间。

### accessed

```dart
DateTime accessed
```

该文件系统对象的数据最后一次被访问的时间。

在 Windows 平台上，此值的精度可能仅为 1 天，且可能有长达一小时的滞后。

### type

```dart
FileSystemEntityType type
```

底层文件系统对象的类型。

若 [stat] 或 [statSync] 调用失败，则为 [FileSystemEntityType.notFound]。

### mode

```dart
int mode
```

文件系统对象的模式。

权限编码在该数值的低 16 位中，可通过 [modeString] 获取器解码。

### size

```dart
int size
```

文件系统对象的大小。

### statSync()

```dart
FileStat statSync(String path)
```

对 [path] 调用操作系统的 `stat()` 函数（或等效函数）。

若 [path] 是一个符号链接，则会先解析该链接，并返回解析结果所指文件的相关信息。

返回一个包含 `stat()` 返回数据的 [FileStat] 对象。若调用失败，则返回一个 [FileStat.type] 为 [FileSystemEntityType.notFound] 且其余字段无效的 [FileStat] 对象。

### stat()

```dart
Future<FileStat> stat(String path)
```

异步地对 [path] 调用操作系统的 `stat()` 函数（或等效函数）。

返回一个 [Future]，其完成结果与 [statSync] 相同。

### toString()

```dart
String toString()
```

### modeString()

```dart
String modeString()
```

以人类可读的字符串形式表示模式值。

该字符串格式为 "rwxrwxrwx"，分别体现用户、组和其他用户对该文件系统对象的读、写、执行权限，缺失的权限用 "-" 表示。额外的权限位可能会以在模式字符串前添加 "(suid)"、"(guid)" 和/或 "(sticky)" 的方式表示。

# FileSystemEntity

```dart
abstract class FileSystemEntity {}
```

[File]、[Directory] 和 [Link] 的公共超类。

目录列出操作会返回 [FileSystemEntity] 对象。要判断某个 [FileSystemEntity] 是 [File]、[Directory] 还是 [Link]，可执行类型检查：

```dart
if (entity is File) (entity as File).readAsStringSync();
```

也可以使用 [type] 或 [typeSync] 方法来判断某个文件系统对象的类型。

此类中的大多数方法都同时存在同步和异步两种版本，例如 [exists] 和 [existsSync]。除非有特殊原因需要使用同步版本，否则应优先使用异步版本，以避免阻塞程序。

以下是 exists 方法的实际用法：

```dart
var isThere = await entity.exists();
print(isThere ? 'exists' : 'nonexistent');
```

## 其他资源

- Dart 库导览中的[文件与目录](https://dart.dev/guides/libraries/library-tour#files-and-directories)一节。

- [编写命令行应用](https://dart.dev/tutorials/server/cmdline)，一篇关于编写命令行应用的教程，其中包含文件与目录相关的信息。

### path

```dart
String get path
```

### uri

```dart
Uri get uri
```

表示该文件系统实体位置的 [Uri]。

若该实体的 [path] 为绝对路径，则返回的 URI 的 scheme 始终为 "file"；否则 scheme 为空且该 URI 为相对路径。

### exists()

```dart
Future<bool> exists()
```

检查此路径对应的文件系统实体是否存在。

返回一个 `Future<bool>`，其完成结果即为检查结果。

由于 [FileSystemEntity] 是抽象类，每个 [FileSystemEntity] 对象实际上都是其子类 [File]、[Directory] 或 [Link] 之一的实例。对这些子类的实例调用 [exists] 会检查该文件系统对象是否存在*并且*属于正确的类型（文件、目录或链接）。若要检查某个路径是否指向文件系统上的某个对象而不论其类型，请使用静态方法 [type]。

### existsSync()

```dart
bool existsSync()
```

同步地检查此路径对应的文件系统实体是否存在。

由于 [FileSystemEntity] 是抽象类，每个 [FileSystemEntity] 对象实际上都是其子类 [File]、[Directory] 或 [Link] 之一的实例。对这些子类的实例调用 [existsSync] 会检查该文件系统对象是否存在并且属于正确的类型（文件、目录或链接）。若要检查某个路径是否指向文件系统上的某个对象而不论其类型，请使用静态方法 [typeSync]。

### rename()

```dart
Future<FileSystemEntity> rename(String newPath)
```

重命名此文件系统实体。

返回一个 `Future<FileSystemEntity>`，其在完成时携带重命名后文件系统实体对应的 [FileSystemEntity] 实例。

### renameSync()

```dart
FileSystemEntity renameSync(String newPath)
```

同步地重命名此文件系统实体。

返回重命名后实体对应的 [FileSystemEntity] 实例。

### resolveSymbolicLinks()

```dart
Future<String> resolveSymbolicLinks()
```

相对于当前工作目录解析某个文件系统对象的路径。

解析该路径上所有的符号链接，并解析所有的 `..` 与 `.` 路径片段。

[resolveSymbolicLinks] 使用操作系统原生的文件系统 API 来解析路径，在 Linux 和 OS X 上使用 `realpath` 函数，在 Windows 上使用 `GetFinalPathNameByHandle` 函数。若该路径未指向一个已存在的文件系统对象，`resolveSymbolicLinks` 将抛出 `FileSystemException`。

在 Windows 上，`..` 片段会在解析符号链接*之前*被解析；而在其他平台上，符号链接会先被*解析为其目标*，然后再应用其后的 `..`。

为确保在所有平台上行为一致，应在调用 `resolveSymbolicLinks` 之前先解析 `..` 片段。一种实现方式是使用 [Uri] 类：

```dart
var path = Uri.parse('.').resolveUri(Uri.file(input)).toFilePath();
if (path == '') path = '.';
var resolved = await File(path).resolveSymbolicLinks();
print(resolved);
```

因为 `Uri.resolve` 会移除 `..` 片段，这样便可得到与 Windows 一致的行为。

在 Windows 上，若尝试解析的符号链接的类型与其解析目标文件系统对象的类型不一致，Future 将以 [PathAccessException] 错误完成。

### resolveSymbolicLinksSync()

```dart
String resolveSymbolicLinksSync()
```

相对于当前工作目录解析某个文件系统对象的路径。

解析该路径上所有的符号链接，并解析所有的 `..` 与 `.` 路径片段。

[resolveSymbolicLinksSync] 使用操作系统原生的文件系统 API 来解析路径，在 Linux 和 OS X 上使用 `realpath` 函数，在 Windows 上使用 `GetFinalPathNameByHandle` 函数。若该路径未指向一个已存在的文件系统对象，`resolveSymbolicLinksSync` 将抛出 `FileSystemException`。

在 Windows 上，`..` 片段会在解析符号链接*之前*被解析；而在其他平台上，符号链接会先被*解析为其目标*，然后再应用其后的 `..`。

为确保在所有平台上行为一致，应在调用 `resolveSymbolicLinksSync` 之前先解析 `..` 片段。一种实现方式是使用 [Uri] 类：

```dart
var path = Uri.parse('.').resolveUri(Uri.file(input)).toFilePath();
if (path == '') path = '.';
var resolved = File(path).resolveSymbolicLinksSync();
print(resolved);
```

因为 `Uri.resolve` 会移除 `..` 片段，这样便可得到与 Windows 一致的行为。

在 Windows 上，符号链接被创建为文件链接或目录链接。解析这样的符号链接时，链接类型必须与其所指向的文件系统对象的类型一致，否则将抛出 [PathAccessException]。

### stat()

```dart
Future<FileStat> stat()
```

对 [path] 调用操作系统的 `stat()` 函数。

等同于 `FileStat.stat(this.path)`。

返回一个包含 `stat()` 返回数据的 `Future<FileStat>` 对象。

若 [path] 是一个符号链接，则会先解析该链接，并返回解析结果所指文件的相关信息。

若调用失败，则以 `.type` 为 [FileSystemEntityType.notFound] 且其余字段无效的 [FileStat] 对象完成该 future。

### statSync()

```dart
FileStat statSync()
```

同步地对 [path] 调用操作系统的 `stat()` 函数。

等同于 `FileStat.statSync(this.path)`。

返回一个包含 `stat()` 返回数据的 [FileStat] 对象。

若 [path] 是一个符号链接，则会先解析该链接，并返回解析结果所指文件的相关信息。

若调用失败，则返回一个 `.type` 为 [FileSystemEntityType.notFound] 且其余字段无效的 [FileStat] 对象。

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

删除此 [FileSystemEntity]。

具体细节因 [FileSystemEntity] 的类型而异：

- [Directory.delete]
- [File.delete]
- [Link.delete]

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

同步地删除此 [FileSystemEntity]。

具体细节因 [FileSystemEntity] 的类型而异：

- [Directory.deleteSync]
- [File.deleteSync]
- [Link.deleteSync]

### watch()

```dart
Stream<FileSystemEvent> watch({int events = FileSystemEvent.all, bool recursive = false})
```

开始监视此 [FileSystemEntity] 的变化。

具体实现使用与平台相关的、基于事件的 API 来接收文件系统通知，因此其行为取决于所在平台。

- `Windows`：使用 `ReadDirectoryChangesW`。此实现仅支持监视目录。支持递归监视。
- `Linux`：使用 `inotify`。此实现同时支持监视文件与目录。不支持递归监视。注意：直接监视文件时，删除事件可能不会按预期发生。
- `OS X`：使用 [File System Events API](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/FSEvents_ProgGuide/TechnologyOverview/TechnologyOverview.html)。此实现同时支持监视文件与目录，且支持递归监视。该 API 存在若干限制：

- 在调用 [watch] 方法*之前*不久发生的变化，仍可能出现在 [Stream] 中。_ 短时间内发生的多个变化可能会乱序到达。_ 在单个目录中进行的多次变更，可能会被合并为单个 `FileSystemEvent`。

系统会在返回的 [Stream] 开始被监听时才启动事件监听，而非在调用 [watch] 时。

返回值是一个无穷尽的广播 [Stream]，仅在以下情况之一发生时才会停止：

- 该 [Stream] 被取消，例如通过在 [StreamSubscription] 上调用 `cancel`。
- 被监视的 [FileSystemEntity] 被删除。
- 系统监视器意外退出。例如在 `Windows` 上，当接收 `ReadDirectoryChangesW` 事件的缓冲区溢出时会发生这种情况。

使用 `events` 指定要监听的事件类型。可以通过对 [FileSystemEvent] 中的常量进行"或"运算来混合多种事件。默认值为 [FileSystemEvent.all]。

一次移动事件可能会被报告为独立的删除事件和创建事件。

### identical()

```dart
Future<bool> identical(String path1, String path2)
```

检查两个路径是否指向文件系统中的同一个对象。

返回一个 `Future<bool>`，其完成结果即为检查结果。

将链接与其目标进行比较会返回 `false`，将两个指向同一目标的链接进行比较同样会返回 `false`。若要检查链接的目标，请显式使用 Link.target 来获取该目标。不过，路径中出现的目录链接会被跟随解析，以找到对应的文件系统对象。

若其中一个路径指向的对象不存在，返回的 Future 将以错误完成。

### isAbsolute

```dart
bool get isAbsolute
```

此对象的路径是否为绝对路径。

绝对路径不依赖于当前工作目录（[Directory.current]）。非绝对路径则必须相对于当前工作目录进行解释。

在 Windows 上，若路径以 `\\`（两个反斜杠，表示 UNC 路径）开头，或以 `a` 到 `z`（大写或小写）之间的驱动器字母后跟 `:\` 或 `:/` 开头，则该路径为绝对路径。因此，例如 `\file.ext` 并非绝对路径，因为它依赖于当前的驱动器字母。

在非 Windows 系统上，以 `/` 开头的路径为绝对路径。

若路径不是绝对路径，可使用 [absolute] 获取一个引用文件系统中同一对象、且（若可能）路径为绝对路径的实体。

### absolute

```dart
FileSystemEntity get absolute
```

一个路径为 [path] 绝对路径的 [FileSystemEntity]。

返回实例的类型与此实体的类型相同。

若某文件系统实体的路径已经是绝对路径（如 [isAbsolute] 所示），则直接返回该实体。对于非绝对路径，返回的实体会*在可能的情况下*是绝对路径（[isAbsolute]），但仍指向同一个文件系统对象。

### identicalSync()

```dart
bool identicalSync(String path1, String path2)
```

同步地检查两个路径是否指向文件系统中的同一个对象。

将链接与其目标进行比较会返回 `false`，将两个指向同一目标的链接进行比较同样会返回 `false`。若要检查链接的目标，请显式使用 Link.target 来获取该目标。不过，路径中出现的目录链接会被跟随解析，以找到对应的文件系统对象。

若其中一个路径指向的对象不存在，将抛出错误。

### isWatchSupported

```dart
bool get isWatchSupported
```

测试当前系统是否支持 [watch]。

OS X 10.6 及以下版本不受支持。

### type()

```dart
Future<FileSystemEntityType> type(String path, {bool followLinks = true})
```

查找某个路径所指向的文件系统对象的类型。

返回一个 `Future<FileSystemEntityType>`，其完成结果与 [typeSync] 相同。

### typeSync()

```dart
FileSystemEntityType typeSync(String path, {bool followLinks = true})
```

同步地查找某个路径所指向的文件系统对象的类型。

若 [path] 未指向任何文件系统对象，或在查找该路径过程中发生了任何其他错误，则返回 [FileSystemEntityType.notFound]。

若 [path] 指向一个链接且 [followLinks] 为 `true`，则结果为该链接所指向的文件系统对象的类型。若该对象不存在，则结果为 [FileSystemEntityType.notFound]。若 [path] 指向一个链接且 [followLinks] 为 `false`，则结果为 [FileSystemEntityType.link]。

### isLink()

```dart
Future<bool> isLink(String path)
```

判断 [path] 是否指向一个链接。

检查 `type(path, followLinks: false)` 是否返回 [FileSystemEntityType.link]。

### isFile()

```dart
Future<bool> isFile(String path)
```

判断 [path] 是否指向一个文件。

检查 `type(path)` 是否返回 [FileSystemEntityType.file]。

### isDirectory()

```dart
Future<bool> isDirectory(String path)
```

判断 [path] 是否指向一个目录。

检查 `type(path)` 是否返回 [FileSystemEntityType.directory]。

### isLinkSync()

```dart
bool isLinkSync(String path)
```

同步地检查 [path] 是否指向一个链接。

检查 `typeSync(path, followLinks: false)` 是否返回 [FileSystemEntityType.link]。

### isFileSync()

```dart
bool isFileSync(String path)
```

同步地检查 [path] 是否指向一个文件。

检查 `typeSync(path)` 是否返回 [FileSystemEntityType.file]。

### isDirectorySync()

```dart
bool isDirectorySync(String path)
```

同步地检查 [path] 是否指向一个目录。

检查 `typeSync(path)` 是否返回 [FileSystemEntityType.directory]。

### parentOf()

```dart
String parentOf(String path)
```

某个路径的父路径。

使用平台的路径分隔符对路径进行分割，找到路径的最后一个组成部分，并返回该部分之前的前缀。

不会移除 Windows 路径中的根组成部分，例如 "C:\\" 或 "\\\\server_name\\"。[path] 最后一部分包含末尾的路径分隔符，但结果不含末尾的路径分隔符。

### parent

```dart
Directory get parent
```

此实体的父目录。

# FileSystemEvent

```dart
sealed class FileSystemEvent {}
```

由 [FileSystemEntity.watch] 发出事件的基类。

### create

```dart
int create
```

供 [FileSystemEntity.watch] 使用的位字段，用于启用 [FileSystemCreateEvent] 事件。

### modify

```dart
int modify
```

供 [FileSystemEntity.watch] 使用的位字段，用于启用 [FileSystemModifyEvent] 事件。

### delete

```dart
int delete
```

供 [FileSystemEntity.watch] 使用的位字段，用于启用 [FileSystemDeleteEvent] 事件。

### move

```dart
int move
```

供 [FileSystemEntity.watch] 使用的位字段，用于启用 [FileSystemMoveEvent] 事件。

### all

```dart
int all
```

供 [FileSystemEntity.watch] 使用的位字段，用于同时启用 [create]、[modify]、[delete] 与 [move]。

### type

```dart
int type
```

事件的类型。参见 [FileSystemEvent] 获取事件列表。

### path

```dart
String path
```

触发该事件的路径。

根据平台与 [FileSystemEntity] 的不同，该路径可能是相对路径。

### isDirectory

```dart
bool isDirectory
```

事件的目标是否为目录。

对于 [FileSystemDeleteEvent]，该值始终为 `false`。

在 Windows 上，`isDirectory` 是通过在事件发生后检查文件系统实体类型来计算的，因此其结果可能不准确。对于目录的创建、修改或移动事件，若该目录在事件发生后不久即被删除，该值可能被错误地判定为 `false`；而对于新创建的指向目录的链接，该值可能被错误地判定为 `true`。

# FileSystemCreateEvent

```dart
final class FileSystemCreateEvent extends FileSystemEvent {}
```

用于新创建的文件系统对象的文件系统事件。

### FileSystemCreateEvent()

```dart
FileSystemCreateEvent(String path, bool isDirectory)
```

构造一个新的 [FileSystemCreateEvent]。

### toString()

```dart
String toString()
```

# FileSystemModifyEvent

```dart
final class FileSystemModifyEvent extends FileSystemEvent {}
```

用于文件系统对象被修改的文件系统事件。

### contentChanged

```dart
bool contentChanged
```

若发生变化的是内容而不仅仅是属性，则 [contentChanged] 为 `true`。

### FileSystemModifyEvent()

```dart
FileSystemModifyEvent(String path, bool isDirectory, bool contentChanged)
```

构造一个新的 [FileSystemModifyEvent]。

### toString()

```dart
String toString()
```

# FileSystemDeleteEvent

```dart
final class FileSystemDeleteEvent extends FileSystemEvent {}
```

用于文件系统对象被删除的文件系统事件。

### FileSystemDeleteEvent()

```dart
FileSystemDeleteEvent(String path, bool isDirectory)
```

构造一个新的 [FileSystemDeleteEvent]。

### toString()

```dart
String toString()
```

### isDirectory

```dart
bool get isDirectory
```

该文件系统对象此前是否为目录。

对于 [FileSystemDeleteEvent]，该值始终为 `false`。

# FileSystemMoveEvent

```dart
final class FileSystemMoveEvent extends FileSystemEvent {}
```

用于文件系统对象被移动的文件系统事件。

### destination

```dart
String? destination
```

被移动文件的目标路径。

若底层实现无法识别被移动文件的目标位置，则该 destination 为 `null`。

源路径可通过 [path] 获取。

### FileSystemMoveEvent()

```dart
FileSystemMoveEvent(String path, bool isDirectory, String? destination)
```

构造一个新的 [FileSystemMoveEvent]。

### toString()

```dart
String toString()
```
