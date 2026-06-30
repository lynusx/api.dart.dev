# FileSystemEntityType

```dart
final class FileSystemEntityType {}
```

The type of an entity on the file system, such as a file, directory, or link.

These constants are used by the [FileSystemEntity] class to indicate the object's type.

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

The result of calling the POSIX `stat()` function on a file system object.

This is an immutable object, representing the snapshotted values returned by the `stat()` call.

### changed

```dart
DateTime changed
```

The time of the last change to the data or metadata of the file system object.

On Windows platforms, this is instead the file creation time.

### modified

```dart
DateTime modified
```

The time of the last change to the data of the file system object.

### accessed

```dart
DateTime accessed
```

The time of the last access to the data of the file system object.

On Windows platforms, this may have 1 day granularity, and be out of date by an hour.

### type

```dart
FileSystemEntityType type
```

The type of the underlying file system object.

[FileSystemEntityType.notFound] if [stat] or [statSync] failed.

### mode

```dart
int mode
```

The mode of the file system object.

Permissions are encoded in the lower 16 bits of this number, and can be decoded using the [modeString] getter.

### size

```dart
int size
```

The size of the file system object.

### statSync()

```dart
FileStat statSync(String path)
```

Calls the operating system's `stat()` function (or equivalent) on [path].

If [path] is a symbolic link then it is resolved and results for the resulting file are returned.

Returns a [FileStat] object containing the data returned by `stat()`. If the call fails, returns a [FileStat] object with [FileStat.type] set to [FileSystemEntityType.notFound] and the other fields invalid.

### stat()

```dart
Future<FileStat> stat(String path)
```

Asynchronously calls the operating system's `stat()` function (or equivalent) on [path].

If [path] is a symbolic link then it is resolved and results for the resulting file are returned.

Returns a [Future] which completes with the same results as [statSync].

### toString()

```dart
String toString()
```

### modeString()

```dart
String modeString()
```

The mode value as a human-readable string.

The string is in the format "rwxrwxrwx", reflecting the user, group, and world permissions to read, write, and execute the file system object, with "-" replacing the letter for missing permissions. Extra permission bits may be represented by prepending "(suid)", "(guid)", and/or "(sticky)" to the mode string.

# FileSystemEntity

```dart
abstract class FileSystemEntity {}
```

The common superclass of [File], [Directory], and [Link].

[FileSystemEntity] objects are returned from directory listing operations. To determine whether a [FileSystemEntity] is a [File], a [Directory], or a [Link] perform a type check:

```dart
if (entity is File) (entity as File).readAsStringSync();
```

You can also use the [type] or [typeSync] methods to determine the type of a file system object.

Most methods in this class exist both in synchronous and asynchronous versions, for example, [exists] and [existsSync]. Unless you have a specific reason for using the synchronous version of a method, prefer the asynchronous version to avoid blocking your program.

Here's the exists method in action:

```dart
var isThere = await entity.exists();
print(isThere ? 'exists' : 'nonexistent');
```

## Other resources

- The [Files and directories](https://dart.dev/guides/libraries/library-tour#files-and-directories) section of the library tour.

- [Write Command-Line Apps](https://dart.dev/tutorials/server/cmdline), a tutorial about writing command-line apps, includes information about files and directories.

### path

```dart
String get path
```

### uri

```dart
Uri get uri
```

A [Uri] representing the file system entity's location.

The returned URI's scheme is always "file" if the entity's [path] is absolute, otherwise the scheme will be empty and the URI relative.

### exists()

```dart
Future<bool> exists()
```

Checks whether the file system entity with this path exists.

Returns a `Future<bool>` that completes with the result.

Since [FileSystemEntity] is abstract, every [FileSystemEntity] object is actually an instance of one of the subclasses [File], [Directory], and [Link]. Calling [exists] on an instance of one of these subclasses checks whether the object exists in the file system object exists _and_ is of the correct type (file, directory, or link). To check whether a path points to an object on the file system, regardless of the object's type, use the [type] static method.

### existsSync()

```dart
bool existsSync()
```

Synchronously checks whether the file system entity with this path exists.

Since [FileSystemEntity] is abstract, every [FileSystemEntity] object is actually an instance of one of the subclasses [File], [Directory], and [Link]. Calling [existsSync] on an instance of one of these subclasses checks whether the object exists in the file system object exists and is of the correct type (file, directory, or link). To check whether a path points to an object on the file system, regardless of the object's type, use the [typeSync] static method.

### rename()

```dart
Future<FileSystemEntity> rename(String newPath)
```

Renames this file system entity.

Returns a `Future<FileSystemEntity>` that completes with a [FileSystemEntity] instance for the renamed file system entity.

### renameSync()

```dart
FileSystemEntity renameSync(String newPath)
```

Synchronously renames this file system entity.

Returns a [FileSystemEntity] instance for the renamed entity.

### resolveSymbolicLinks()

```dart
Future<String> resolveSymbolicLinks()
```

Resolves the path of a file system object relative to the current working directory.

Resolves all symbolic links on the path and resolves all `..` and `.` path segments.

[resolveSymbolicLinks] uses the operating system's native file system API to resolve the path, using the `realpath` function on Linux and OS X, and the `GetFinalPathNameByHandle` function on Windows. If the path does not point to an existing file system object, `resolveSymbolicLinks` throws a `FileSystemException`.

On Windows the `..` segments are resolved _before_ resolving the symbolic link, and on other platforms the symbolic links are _resolved to their target_ before applying a `..` that follows.

To ensure the same behavior on all platforms resolve `..` segments before calling `resolveSymbolicLinks`. One way of doing this is with the [Uri] class:

```dart
var path = Uri.parse('.').resolveUri(Uri.file(input)).toFilePath();
if (path == '') path = '.';
var resolved = await File(path).resolveSymbolicLinks();
print(resolved);
```

since `Uri.resolve` removes `..` segments. This will result in the Windows behavior.

On Windows, attempting to resolve a symbolic link where the link type does not match the type of the resolved file system object will cause the Future to complete with a [PathAccessException] error.

### resolveSymbolicLinksSync()

```dart
String resolveSymbolicLinksSync()
```

Resolves the path of a file system object relative to the current working directory.

Resolves all symbolic links on the path and resolves all `..` and `.` path segments.

[resolveSymbolicLinksSync] uses the operating system's native file system API to resolve the path, using the `realpath` function on linux and OS X, and the `GetFinalPathNameByHandle` function on Windows. If the path does not point to an existing file system object, `resolveSymbolicLinksSync` throws a `FileSystemException`.

On Windows the `..` segments are resolved _before_ resolving the symbolic link, and on other platforms the symbolic links are _resolved to their target_ before applying a `..` that follows.

To ensure the same behavior on all platforms resolve `..` segments before calling `resolveSymbolicLinksSync`. One way of doing this is with the [Uri] class:

```dart
var path = Uri.parse('.').resolveUri(Uri.file(input)).toFilePath();
if (path == '') path = '.';
var resolved = File(path).resolveSymbolicLinksSync();
print(resolved);
```

since `Uri.resolve` removes `..` segments. This will result in the Windows behavior.

On Windows, a symbolic link is created as either a file link or a directory link. Attempting to resolve such a symbolic link requires the link type to match the type of the file system object that it points to, otherwise it throws a [PathAccessException].

### stat()

```dart
Future<FileStat> stat()
```

Calls the operating system's `stat()` function on [path].

Identical to `FileStat.stat(this.path)`.

Returns a `Future<FileStat>` object containing the data returned by `stat()`.

If [path] is a symbolic link then it is resolved and results for the resulting file are returned.

If the call fails, completes the future with a [FileStat] object with `.type` set to [FileSystemEntityType.notFound] and the other fields invalid.

### statSync()

```dart
FileStat statSync()
```

Synchronously calls the operating system's `stat()` function on [path].

Identical to `FileStat.statSync(this.path)`.

Returns a [FileStat] object containing the data returned by `stat()`.

If [path] is a symbolic link then it is resolved and results for the resulting file are returned.

If the call fails, returns a [FileStat] object with `.type` set to [FileSystemEntityType.notFound] and the other fields invalid.

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

Deletes this [FileSystemEntity].

The exact details vary according to the [FileSystemEntity]:

- [Directory.delete]
- [File.delete]
- [Link.delete]

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

Synchronously deletes this [FileSystemEntity].

The exact details vary according to the [FileSystemEntity]:

- [Directory.deleteSync]
- [File.deleteSync]
- [Link.deleteSync]

### watch()

```dart
Stream<FileSystemEvent> watch({int events = FileSystemEvent.all, bool recursive = false})
```

Start watching the [FileSystemEntity] for changes.

The implementation uses platform-dependent event-based APIs for receiving file-system notifications, thus behavior depends on the platform.

- `Windows`: Uses `ReadDirectoryChangesW`. The implementation only supports watching directories. Recursive watching is supported.
- `Linux`: Uses `inotify`. The implementation supports watching both files and directories. Recursive watching is not supported. Note: When watching files directly, delete events might not happen as expected.
- `OS X`: Uses the [File System Events API](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/FSEvents_ProgGuide/TechnologyOverview/TechnologyOverview.html). The implementation supports watching both files and directories. Recursive watching is supported. This API has several limitations:

- Changes that occurred shortly _before_ the [watch] method was called may still appear in the [Stream]. _ Changes that occur in a short period of time may arrive out-of-order. _ Multiple changes made in a single directory may be coalesced into a single `FileSystemEvent`.

The system will start listening for events once the returned [Stream] is being listened to, not when the call to [watch] is issued.

The returned value is an endless broadcast [Stream], that only stops when one of the following happens:

- The [Stream] is canceled, e.g. by calling `cancel` on the [StreamSubscription].
- The [FileSystemEntity] being watched is deleted.
- System Watcher exits unexpectedly. e.g. On `Windows` this happens when buffer that receive events from `ReadDirectoryChangesW` overflows.

Use `events` to specify what events to listen for. The constants in [FileSystemEvent] can be or'ed together to mix events. Default is [FileSystemEvent.all].

A move event may be reported as separate delete and create events.

### identical()

```dart
Future<bool> identical(String path1, String path2)
```

Checks whether two paths refer to the same object in the file system.

Returns a `Future<bool>` that completes with the result.

Comparing a link to its target returns `false`, as does comparing two links that point to the same target. To check the target of a link, use Link.target explicitly to fetch it. Directory links appearing inside a path are followed, though, to find the file system object.

Completes the returned Future with an error if one of the paths points to an object that does not exist.

### isAbsolute

```dart
bool get isAbsolute
```

Whether this object's path is absolute.

An absolute path is independent of the current working directory ([Directory.current]). A non-absolute path must be interpreted relative to the current working directory.

On Windows, a path is absolute if it starts with `\\` (two backslashes or representing a UNC path) or with a drive letter between `a` and `z` (upper or lower case) followed by `:\` or `:/`. This makes, for example, `\file.ext` a non-absolute path because it depends on the current drive letter.

On non-Windows, a path is absolute if it starts with `/`.

If the path is not absolute, use [absolute] to get an entity with an absolute path referencing the same object in the file system, if possible.

### absolute

```dart
FileSystemEntity get absolute
```

A [FileSystemEntity] whose path is the absolute path of [path].

The type of the returned instance is the same as the type of this entity.

A file system entity with an already absolute path (as reported by [isAbsolute]) is returned directly. For a non-absolute path, the returned entity is absolute ([isAbsolute]) _if possible_, but still refers to the same file system object.

### identicalSync()

```dart
bool identicalSync(String path1, String path2)
```

Synchronously checks whether two paths refer to the same object in the file system.

Comparing a link to its target returns `false`, as does comparing two links that point to the same target. To check the target of a link, use Link.target explicitly to fetch it. Directory links appearing inside a path are followed, though, to find the file system object.

Throws an error if one of the paths points to an object that does not exist.

### isWatchSupported

```dart
bool get isWatchSupported
```

Test if [watch] is supported on the current system.

OS X 10.6 and below is not supported.

### type()

```dart
Future<FileSystemEntityType> type(String path, {bool followLinks = true})
```

Finds the type of file system object that a path points to.

Returns a `Future<FileSystemEntityType>` that completes with the same results as [typeSync].

### typeSync()

```dart
FileSystemEntityType typeSync(String path, {bool followLinks = true})
```

Synchronously finds the type of file system object that a path points to.

Returns [FileSystemEntityType.notFound] if [path] does not point to a file system object or if any other error occurs in looking up the path.

If [path] points to a link and [followLinks] is `true` then the result will be for the file system object that the link points to. If that object does not exist then the result will be [FileSystemEntityType.notFound]. If [path] points to a link and [followLinks] is `false` then the result will be [FileSystemEntityType.link].

### isLink()

```dart
Future<bool> isLink(String path)
```

Whether [path] refers to a link.

Checks whether `type(path, followLinks: false)` returns [FileSystemEntityType.link].

### isFile()

```dart
Future<bool> isFile(String path)
```

Whether [path] refers to a file.

Checks whether `type(path)` returns [FileSystemEntityType.file].

### isDirectory()

```dart
Future<bool> isDirectory(String path)
```

Whether [path] refers to a directory.

Checks whether `type(path)` returns [FileSystemEntityType.directory].

### isLinkSync()

```dart
bool isLinkSync(String path)
```

Synchronously checks whether [path] refers to a link.

Checks whether `typeSync(path, followLinks: false)` returns [FileSystemEntityType.link].

### isFileSync()

```dart
bool isFileSync(String path)
```

Synchronously checks whether [path] refers to a file.

Checks whether `typeSync(path)` returns [FileSystemEntityType.file].

### isDirectorySync()

```dart
bool isDirectorySync(String path)
```

Synchronously checks whether [path] refers to a directory.

Checks whether `typeSync(path)` returns [FileSystemEntityType.directory].

### parentOf()

```dart
String parentOf(String path)
```

The parent path of a path.

Finds the final path component of a path, using the platform's path separator to split the path, and returns the prefix up to that part.

Will not remove the root component of a Windows path, like "C:\\" or "\\\\server_name\\". Includes a trailing path separator in the last part of [path], and leaves no trailing path separator.

### parent

```dart
Directory get parent
```

The parent directory of this entity.

# FileSystemEvent

```dart
sealed class FileSystemEvent {}
```

Base event class emitted by [FileSystemEntity.watch].

### create

```dart
int create
```

Bitfield for [FileSystemEntity.watch], to enable [FileSystemCreateEvent]s.

### modify

```dart
int modify
```

Bitfield for [FileSystemEntity.watch], to enable [FileSystemModifyEvent]s.

### delete

```dart
int delete
```

Bitfield for [FileSystemEntity.watch], to enable [FileSystemDeleteEvent]s.

### move

```dart
int move
```

Bitfield for [FileSystemEntity.watch], to enable [FileSystemMoveEvent]s.

### all

```dart
int all
```

Bitfield for [FileSystemEntity.watch], for enabling all of [create], [modify], [delete] and [move].

### type

```dart
int type
```

The type of event. See [FileSystemEvent] for a list of events.

### path

```dart
String path
```

The path that triggered the event.

Depending on the platform and the [FileSystemEntity], the path may be relative.

### isDirectory

```dart
bool isDirectory
```

Whether the event target is a directory.

The value will always be `false` for [FileSystemDeleteEvent].

On Windows `isDirectory` is computed by checking the file system entity type after the event, which means it can be incorrect. It can be incorrectly `false` for a directory create, modify or move event if that directory was deleted soon after the event occurred. And, it can be incorrectly `true` for a newly created link to a directory.

# FileSystemCreateEvent

```dart
final class FileSystemCreateEvent extends FileSystemEvent {}
```

File system event for newly created file system objects.

### FileSystemCreateEvent()

```dart
FileSystemCreateEvent(String path, bool isDirectory)
```

Constructs a new [FileSystemCreateEvent].

### toString()

```dart
String toString()
```

# FileSystemModifyEvent

```dart
final class FileSystemModifyEvent extends FileSystemEvent {}
```

File system event for modifications of file system objects.

### contentChanged

```dart
bool contentChanged
```

If the content was changed and not only the attributes, [contentChanged] is `true`.

### FileSystemModifyEvent()

```dart
FileSystemModifyEvent(String path, bool isDirectory, bool contentChanged)
```

Constructs a new [FileSystemModifyEvent].

### toString()

```dart
String toString()
```

# FileSystemDeleteEvent

```dart
final class FileSystemDeleteEvent extends FileSystemEvent {}
```

File system event for deletion of file system objects.

### FileSystemDeleteEvent()

```dart
FileSystemDeleteEvent(String path, bool isDirectory)
```

Constructs a new [FileSystemDeleteEvent].

### toString()

```dart
String toString()
```

### isDirectory

```dart
bool get isDirectory
```

Whether the file system object was a directory.

The value will always be `false` for [FileSystemDeleteEvent].

# FileSystemMoveEvent

```dart
final class FileSystemMoveEvent extends FileSystemEvent {}
```

File system event for moving of file system objects.

### destination

```dart
String? destination
```

The destination path of the file being moved.

The destination is `null` if the underlying implementation is unable to identify the destination of the moved file.

The source path is available as [path].

### FileSystemMoveEvent()

```dart
FileSystemMoveEvent(String path, bool isDirectory, String? destination)
```

Constructs a new [FileSystemMoveEvent].

### toString()

```dart
String toString()
```
