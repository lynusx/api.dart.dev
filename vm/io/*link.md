# Link

```dart
abstract interface class Link implements FileSystemEntity {}
```

对文件系统链接的引用。

### Link()

```dart
Link(String path)
```

创建一个 Link 对象。

### Link.fromRawPath()

```dart
Link.fromRawPath(Uint8List rawPath)
```

### Link.fromUri()

```dart
Link.fromUri(Uri uri)
```

创建一个 [Link] 对象。

如果 [path] 是相对路径，使用时将相对于当前工作目录（参见 [Directory.current]）进行解释。

如果 [path] 是绝对路径，它将不受当前工作目录变化的影响。

### create()

```dart
Future<Link> create(String target, {bool recursive = false})
```

在文件系统中创建一个符号链接。

创建的链接将指向 [target] 处的路径，无论该路径是否存在。

返回一个 `Future<Link>`，当链接创建完成时以该链接完成。如果链接路径已经存在，Future 将以错误完成。

如果 [recursive] 为 `false`（默认值），仅当其路径中的所有目录都存在时才会创建链接。如果 [recursive] 为 `true`，将首先创建所有不存在的父路径。[target] 路径中的目录不受影响，除非它们也在 [path] 中。

在 Windows 平台上，此调用将创建真正的符号链接，而不是联接点（junction）。Windows 将指向文件的链接和指向目录的链接视为不同且不可互换的链接类型。每个链接要么是文件链接，要么是目录链接，其类型在创建链接时确定，此后该链接在大多数场合下都算作文件或目录。不同的 Win32 API 调用用于操作这两种类型。例如，`DeleteFile` 函数用于删除指向文件的链接，而删除指向目录的链接必须使用 `RemoveDirectory`。

创建的 Windows 符号链接的类型将与 [target] 的类型一致（如果 [target] 存在）；否则将创建文件链接。如果之后 [target] 被替换为不同类型的对象，已创建链接的类型不会改变，但此时该链接将无法被 [resolveSymbolicLinks] 解析。

要在 Windows 上创建符号链接，Dart 必须以管理员模式运行，或者系统必须启用开发者模式，否则进行此调用时将抛出 [FileSystemException]，并将 `ERROR_PRIVILEGE_NOT_HELD` 设置为其 errno。

在其他平台上，使用 POSIX 的 `symlink()` 调用来创建包含字符串 [target] 的符号链接。如果 [target] 是相对路径，它将相对于包含该链接的目录进行解释。

### createSync()

```dart
void createSync(String target, {bool recursive = false})
```

在文件系统中创建一个符号链接。

创建的链接将指向 [target] 处的路径，无论该路径是否存在。

如果链接路径已经存在，将抛出异常。

如果 [recursive] 为 `false`（默认值），仅当其路径中的所有目录都存在时才会创建链接。如果 [recursive] 为 `true`，将首先创建所有不存在的父路径。[target] 路径中的目录不受影响，除非它们也在 [path] 中。

在 Windows 平台上，此调用将创建真正的符号链接，而不是联接点（junction）。Windows 将指向文件的链接和指向目录的链接视为不同且不可互换的链接类型。每个链接要么是文件链接，要么是目录链接，其类型在创建链接时确定，此后该链接在大多数场合下都算作文件或目录。不同的 Win32 API 调用用于操作这两种类型。例如，`DeleteFile` 函数用于删除指向文件的链接，而删除指向目录的链接必须使用 `RemoveDirectory`。

创建的 Windows 符号链接的类型将与 [target] 的类型一致（如果 [target] 存在）；否则将创建文件链接。如果之后 [target] 被替换为不同类型的对象，已创建链接的类型不会改变，但此时该链接将无法被 [resolveSymbolicLinks] 解析。

要在 Windows 上创建符号链接，Dart 必须以管理员模式运行，或者系统必须启用开发者模式，否则进行此调用时将抛出 [FileSystemException]，并将 `ERROR_PRIVILEGE_NOT_HELD` 设置为其 errno。

在其他平台上，使用 POSIX 的 `symlink()` 调用来创建包含字符串 [target] 的符号链接。如果 [target] 是相对路径，它将相对于包含该链接的目录进行解释。

### updateSync()

```dart
void updateSync(String target)
```

同步更新一个已存在的链接。

删除 [path] 处已存在的链接，并使用 [createSync] 创建指向 [target] 的新链接。如果原链接不存在，将抛出 [PathNotFoundException]；也可能抛出 [deleteSync] 或 [createSync] 可能抛出的任何 [FileSystemException]。

### update()

```dart
Future<Link> update(String target)
```

更新一个已存在的链接。

删除 [path] 处已存在的链接，并使用 [create] 创建指向 [target] 的新链接。

返回一个 Future，成功时以该 `Link` 完成；如果 [path] 处不存在链接，则以 [PathNotFoundException] 完成；也可能以 [delete] 或 [create] 可能抛出的任何 [FileSystemException] 完成。

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
Future<Link> rename(String newPath)
```

重命名此链接。

返回一个 `Future<Link>`，以重命名后链接的 [Link] 完成。

如果 [newPath] 指向一个已存在的文件或链接，该实体将先被删除。如果 [newPath] 指向一个已存在的目录，则该 Future 将以 [FileSystemException] 完成。

### renameSync()

```dart
Link renameSync(String newPath)
```

同步重命名此链接。

返回重命名后链接的 [Link] 实例。

如果 [newPath] 指向一个已存在的文件或链接，该实体将先被删除。如果 [newPath] 指向一个已存在的目录，将抛出 [FileSystemException]。

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

删除此 [Link]。

如果 [recursive] 为 `false`：

- 如果 [path] 对应一个链接，该路径将被删除。否则，[delete] 将以 [FileSystemException] 完成。

如果 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity] 将被删除，无论其类型如何。如果 [path] 对应文件或链接，该文件或链接将被删除。如果 [path] 对应目录，该目录及其所有子目录和文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这种行为使得 [delete] 可用于无条件删除任何文件系统对象。

如果此 [Link] 无法删除，[delete] 将以 [FileSystemException] 完成。

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

同步删除此 [Link]。

如果 [recursive] 为 `false`：

- 如果 [path] 对应一个链接，该路径将被删除。否则，[delete] 将抛出 [FileSystemException]。

如果 [recursive] 为 `true`：

- [path] 处的 [FileSystemEntity] 将被删除，无论其类型如何。如果 [path] 对应文件或链接，该文件或链接将被删除。如果 [path] 对应目录，该目录及其所有子目录和文件都将被删除。递归删除时不会跟随链接，只会删除链接本身，而不会删除其目标。这种行为使得 [delete] 可用于无条件删除任何文件系统对象。

如果此 [Link] 无法删除，[delete] 将抛出 [FileSystemException]。

### absolute

```dart
Link get absolute
```

路径为此 [Link] 绝对路径的 [Link] 实例。

绝对路径的计算方式是：为相对路径添加当前工作目录前缀，或者原样返回绝对路径。

### target()

```dart
Future<String> target()
```

获取链接的目标。

返回一个 Future，以指向目标的路径完成。

如果返回的目标是相对路径，则它是相对于包含该链接的目录而言的。

如果链接不存在，或者不是一个链接，Future 将以 [FileSystemException] 完成。

### targetSync()

```dart
String targetSync()
```

同步获取链接的目标。

返回指向目标的路径。

如果返回的目标是相对路径，则它是相对于包含该链接的目录而言的。

如果链接不存在，或者不是一个链接，将抛出 [FileSystemException]。
