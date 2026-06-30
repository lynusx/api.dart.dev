# Link

```dart
abstract interface class Link implements FileSystemEntity {}
```

References to filesystem links.

### Link()

```dart
Link(String path)
```

Creates a Link object.

### Link.fromRawPath()

```dart
Link.fromRawPath(Uint8List rawPath)
```

### Link.fromUri()

```dart
Link.fromUri(Uri uri)
```

Creates a [Link] object.

If [path] is a relative path, it will be interpreted relative to the current working directory (see [Directory.current]), when used.

If [path] is an absolute path, it will be immune to changes to the current working directory.

### create()

```dart
Future<Link> create(String target, {bool recursive = false})
```

Creates a symbolic link in the file system.

The created link will point to the path at [target], whether that path exists or not.

Returns a `Future<Link>` that completes with the link when it has been created. If the link path already exists, the future will complete with an error.

If [recursive] is `false`, the default, the link is created only if all directories in its path exist. If [recursive] is `true`, all non-existing parent paths are created first. The directories in the path of [target] are not affected, unless they are also in [path].

On the Windows platform, this call will create a true symbolic link instead of a junction. Windows treats links to files and links to directories as different and non-interchangable kinds of links. Each link is either a file-link or a directory-link, and the type is chosen when the link is created, and the link then counts as either a file or directory for most purposes. Different Win32 API calls are used to manipulate each. For example, the `DeleteFile` function is used to delete links to files, and `RemoveDirectory` must be used to delete links to directories.

The created Windows symbolic link will match the type of the [target], if [target] exists, otherwise a file-link is created. The type of the created link will not change if [target] is later replaced by something of a different type, but then the link will not be resolvable by [resolveSymbolicLinks].

In order to create a symbolic link on Windows, Dart must be run in Administrator mode or the system must have Developer Mode enabled, otherwise a [FileSystemException] will be raised with `ERROR_PRIVILEGE_NOT_HELD` set as the errno when this call is made.

On other platforms, the POSIX `symlink()` call is used to make a symbolic link containing the string [target]. If [target] is a relative path, it will be interpreted relative to the directory containing the link.

### createSync()

```dart
void createSync(String target, {bool recursive = false})
```

Creates a symbolic link in the file system.

The created link will point to the path at [target], whether that path exists or not.

If the link path already exists, an exception will be thrown.

If [recursive] is `false`, the default, the link is created only if all directories in its path exist. If [recursive] is `true`, all non-existing parent paths are created first. The directories in the path of [target] are not affected, unless they are also in [path].

On the Windows platform, this call will create a true symbolic link instead of a junction. Windows treats links to files and links to directories as different and non-interchangable kinds of links. Each link is either a file-link or a directory-link, and the type is chosen when the link is created, and the link then counts as either a file or directory for most purposes. Different Win32 API calls are used to manipulate each. For example, the `DeleteFile` function is used to delete links to files, and `RemoveDirectory` must be used to delete links to directories.

The created Windows symbolic link will match the type of the [target], if [target] exists, otherwise a file-link is created. The type of the created link will not change if [target] is later replaced by something of a different type, but then the link will not be resolvable by [resolveSymbolicLinks].

In order to create a symbolic link on Windows, Dart must be run in Administrator mode or the system must have Developer Mode enabled, otherwise a [FileSystemException] will be raised with `ERROR_PRIVILEGE_NOT_HELD` set as the errno when this call is made.

On other platforms, the POSIX `symlink()` call is used to make a symbolic link containing the string [target]. If [target] is a relative path, it will be interpreted relative to the directory containing the link.

### updateSync()

```dart
void updateSync(String target)
```

Synchronously updates an existing link.

Deletes the existing link at [path] and uses [createSync] to create a new link to [target]. Throws [PathNotFoundException] if the original link does not exist or any [FileSystemException] that [deleteSync] or [createSync] can throw.

### update()

```dart
Future<Link> update(String target)
```

Updates an existing link.

Deletes the existing link at [path] and creates a new link to [target], using [create].

Returns a future which completes with this `Link` if successful, and with a [PathNotFoundException] if there is no existing link at [path], or with any [FileSystemException] that [delete] or [create] can throw.

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

Renames this link.

Returns a `Future<Link>` that completes with a [Link] for the renamed link.

If [newPath] identifies an existing file or link, that entity is removed first. If [newPath] identifies an existing directory then the future completes with a [FileSystemException].

### renameSync()

```dart
Link renameSync(String newPath)
```

Synchronously renames this link.

Returns a [Link] instance for the renamed link.

If [newPath] identifies an existing file or link, that entity is removed first. If [newPath] identifies an existing directory then [FileSystemException] is thrown.

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

Deletes this [Link].

If [recursive] is `false`:

- If [path] corresponds to a link then that path is deleted. Otherwise, [delete] completes with a [FileSystemException].

If [recursive] is `true`:

- The [FileSystemEntity] at [path] is deleted regardless of type. If [path] corresponds to a file or link, then that file or link is deleted. If [path] corresponds to a directory, then it and all sub-directories and files in those directories are deleted. Links are not followed when deleting recursively. Only the link is deleted, not its target. This behavior allows [delete] to be used to unconditionally delete any file system object.

If this [Link] cannot be deleted, then [delete] completes with a [FileSystemException].

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

Synchronously deletes this [Link].

If [recursive] is `false`:

- If [path] corresponds to a link then that path is deleted. Otherwise, [delete] throws a [FileSystemException].

If [recursive] is `true`:

- The [FileSystemEntity] at [path] is deleted regardless of type. If [path] corresponds to a file or link, then that file or link is deleted. If [path] corresponds to a directory, then it and all sub-directories and files in those directories are deleted. Links are not followed when deleting recursively. Only the link is deleted, not its target. This behavior allows [delete] to be used to unconditionally delete any file system object.

If this [Link] cannot be deleted, then [delete] throws a [FileSystemException].

### absolute

```dart
Link get absolute
```

A [Link] instance whose path is the absolute path to this [Link].

The absolute path is computed by prefixing a relative path with the current working directory, or returning an absolute path unchanged.

### target()

```dart
Future<String> target()
```

Gets the target of the link.

Returns a future that completes with the path to the target.

If the returned target is a relative path, it is relative to the directory containing the link.

If the link does not exist, or is not a link, the future completes with a [FileSystemException].

### targetSync()

```dart
String targetSync()
```

Synchronously gets the target of the link.

Returns the path to the target.

If the returned target is a relative path, it is relative to the directory containing the link.

If the link does not exist, or is not a link, throws a [FileSystemException].
