# FileMode

```dart
class FileMode {}
```

The modes in which a [File] can be opened.

### read

```dart
dynamic read
```

The mode for opening a file only for reading.

### write

```dart
dynamic write
```

Mode for opening a file for reading and writing. The file is overwritten if it already exists. The file is created if it does not already exist.

### append

```dart
dynamic append
```

Mode for opening a file for reading and writing to the end of it. The file is created if it does not already exist.

### writeOnly

```dart
dynamic writeOnly
```

Mode for opening a file for writing _only_. The file is overwritten if it already exists. The file is created if it does not already exist.

### writeOnlyAppend

```dart
dynamic writeOnlyAppend
```

Mode for opening a file for writing _only_ to the end of it. The file is created if it does not already exist.

# FileLock

```dart
class FileLock {}
```

Type of lock when requesting a lock on a file.

### shared

```dart
dynamic shared
```

Shared file lock.

### exclusive

```dart
dynamic exclusive
```

Exclusive file lock.

### blockingShared

```dart
dynamic blockingShared
```

Blocking shared file lock.

### blockingExclusive

```dart
dynamic blockingExclusive
```

Blocking exclusive file lock.

# File

```dart
abstract interface class File implements FileSystemEntity {}
```

A reference to a file on the file system.

A `File` holds a [path] on which operations can be performed. You can get the parent directory of the file using [parent], a property inherited from [FileSystemEntity].

Create a new `File` object with a pathname to access the specified file on the file system from your program.

```dart
var myFile = File('file.txt');
```

The `File` class contains methods for manipulating files and their contents. Using methods in this class, you can open and close files, read to and write from them, create and delete them, and check for their existence.

When reading or writing a file, you can use streams (with [openRead]), random access operations (with [open]), or convenience methods such as [readAsString],

Most methods in this class occur in synchronous and asynchronous pairs, for example, [readAsString] and [readAsStringSync]. Unless you have a specific reason for using the synchronous version of a method, prefer the asynchronous version to avoid blocking your program.

## If path is a link

If [path] is a symbolic link, rather than a file, then the methods of `File` operate on the ultimate target of the link, except for [delete] and [deleteSync], which operate on the link.

## Read from a file

The following code sample reads the entire contents from a file as a string using the asynchronous [readAsString] method:

```dart
import 'dart:async';
import 'dart:io';

void main() {
  File('file.txt').readAsString().then((String contents) {
    print(contents);
  });
}
```

A more flexible and useful way to read a file is with a [Stream]. Open the file with [openRead], which returns a stream that provides the data in the file as chunks of bytes. Read the stream to process the file contents when available. You can use various transformers in succession to manipulate the file content into the required format, or to prepare it for output.

You might want to use a stream to read large files, to manipulate the data with transformers, or for compatibility with another API, such as [WebSocket]s.

```dart
import 'dart:io';
import 'dart:convert';
import 'dart:async';

void main() async {
  final file = File('file.txt');
  Stream<String> lines = file.openRead()
    .transform(utf8.decoder)       // Decode bytes to UTF-8.
    .transform(LineSplitter());    // Convert stream to individual lines.
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

## Write to a file

To write a string to a file, use the [writeAsString] method:

```dart
import 'dart:io';

void main() async {
  final filename = 'file.txt';
  var file = await File(filename).writeAsString('some content');
  // Do something with the file.
}
```

You can also write to a file using a [Stream]. Open the file with [openWrite], which returns an [IOSink] to which you can write data. Be sure to close the sink with the [IOSink.close] method.

```dart
import 'dart:io';

void main() async {
  var file = File('file.txt');
  var sink = file.openWrite();
  sink.write('FILE ACCESSED ${DateTime.now()}\n');
  await sink.flush();

  // Close the IOSink to free system resources.
  await sink.close();
}
```

## The use of asynchronous methods

To avoid unintentional blocking of the program, several methods are asynchronous and return a [Future]. For example, the [length] method, which gets the length of a file, returns a [Future]. Wait for the future to get the result when it's ready.

```dart
import 'dart:io';

void main() async {
  final file = File('file.txt');

  var length = await file.length();
  print(length);
}
```

In addition to length, the [exists], [lastModified], [stat], and other methods, are asynchronous.

## Special 'nul' file

On Linux and Mac '/dev/null' and on Windows '\\?\NUL' refer to a special file, such that all writes to it get consumed and disappear, and all reads produce empty output. Note that on Windows 'nul'(without '\\?\'-prefix) refers to a regular file named 'nul' in current directory.

## Other resources

- The [Files and directories](https://dart.dev/guides/libraries/library-tour#files-and-directories) section of the library tour.

- [Write Command-Line Apps](https://dart.dev/tutorials/server/cmdline), a tutorial about writing command-line apps, includes information about files and directories.

### File()

```dart
File(String path)
```

Creates a [File] object.

If [path] is a relative path, it will be interpreted relative to the current working directory (see [Directory.current]), when used.

If [path] is an absolute path, it will be immune to changes to the current working directory.

### File.fromUri()

```dart
File.fromUri(Uri uri)
```

Create a [File] object from a URI.

If [uri] cannot reference a file this throws [UnsupportedError].

### File.fromRawPath()

```dart
File.fromRawPath(Uint8List rawPath)
```

Creates a [File] object from a raw path.

A raw path is a sequence of bytes, as paths are represented by the OS.

### create()

```dart
Future<File> create({bool recursive = false, bool exclusive = false})
```

Creates the file.

Returns a `Future<File>` that completes with the file when it has been created.

If [recursive] is `false`, the default, the file is created only if all directories in its path already exist. If [recursive] is `true`, any non-existing parent paths are created first.

If [exclusive] is `true` and to-be-created file already exists, this operation completes the future with a [PathExistsException].

If [exclusive] is `false`, existing files are left untouched by [create]. Calling [create] on an existing file still might fail if there are restrictive permissions on the file.

Completes the future with a [FileSystemException] if the operation fails.

### createSync()

```dart
void createSync({bool recursive = false, bool exclusive = false})
```

Synchronously creates the file.

If [recursive] is `false`, the default, the file is created only if all directories in its path already exist. If [recursive] is `true`, all non-existing parent paths are created first.

If [exclusive] is `true` and to-be-created file already exists, then a [FileSystemException] is thrown.

If [exclusive] is `false`, existing files are left untouched by [createSync]. Calling [createSync] on an existing file still might fail if there are restrictive permissions on the file.

Throws a [FileSystemException] if the operation fails.

### rename()

```dart
Future<File> rename(String newPath)
```

Renames this file.

Returns a `Future<File>` that completes with a [File] for the renamed file.

If [newPath] is a relative path, it is resolved against the current working directory ([Directory.current]). This means that simply changing the name of a file, but keeping it the original directory, requires creating a new complete path with the new name at the end. Example:

```dart
Future<File> changeFileNameOnly(File file, String newFileName) {
  var path = file.path;
  var lastSeparator = path.lastIndexOf(Platform.pathSeparator);
  var newPath = path.substring(0, lastSeparator + 1) + newFileName;
  return file.rename(newPath);
}
```

On some platforms, a rename operation cannot move a file between different file systems. If that is the case, instead [copy] the file to the new location and then remove the original.

If [newPath] identifies an existing file or link, that entity is removed first. If [newPath] identifies an existing directory, the operation fails and the future completes with a [FileSystemException].

### renameSync()

```dart
File renameSync(String newPath)
```

Synchronously renames this file.

Returns a [File] for the renamed file.

If [newPath] is a relative path, it is resolved against the current working directory ([Directory.current]). This means that simply changing the name of a file, but keeping it the original directory, requires creating a new complete path with the new name at the end. Example:

```dart
File changeFileNameOnlySync(File file, String newFileName) {
  var path = file.path;
  var lastSeparator = path.lastIndexOf(Platform.pathSeparator);
  var newPath = path.substring(0, lastSeparator + 1) + newFileName;
  return file.renameSync(newPath);
}
```

On some platforms, a rename operation cannot move a file between different file systems. If that is the case, instead [copySync] the file to the new location and then [deleteSync] the original.

If [newPath] identifies an existing file or link, that entity is removed first. If [newPath] identifies an existing directory the operation throws a [FileSystemException].

### delete()

```dart
Future<FileSystemEntity> delete({bool recursive = false})
```

Deletes this [File].

If [recursive] is `false`:

- If [path] corresponds to a regular file, named pipe or socket, then that path is deleted. If [path] corresponds to a link, and that link resolves to a file, then the link at [path] will be deleted. In all other cases, [delete] completes with a [FileSystemException].

If [recursive] is `true`:

- The [FileSystemEntity] at [path] is deleted regardless of type. If [path] corresponds to a file or link, then that file or link is deleted. If [path] corresponds to a directory, then it and all sub-directories and files in those directories are deleted. Links are not followed when deleting recursively. Only the link is deleted, not its target. This behavior allows [delete] to be used to unconditionally delete any file system object.

If this [File] cannot be deleted, then [delete] completes with a [FileSystemException].

### deleteSync()

```dart
void deleteSync({bool recursive = false})
```

Synchronously deletes this [File].

If [recursive] is `false`:

- If [path] corresponds to a regular file, named pipe or socket, then that path is deleted. If [path] corresponds to a link, and that link resolves to a file, then the link at [path] will be deleted. In all other cases, [delete] throws a [FileSystemException].

If [recursive] is `true`:

- The [FileSystemEntity] at [path] is deleted regardless of type. If [path] corresponds to a file or link, then that file or link is deleted. If [path] corresponds to a directory, then it and all sub-directories and files in those directories are deleted. Links are not followed when deleting recursively. Only the link is deleted, not its target. This behavior allows [delete] to be used to unconditionally delete any file system object.

If this [File] cannot be deleted, then [delete] throws a [FileSystemException].

### copy()

```dart
Future<File> copy(String newPath)
```

Copies this file.

If [newPath] is a relative path, it is resolved against the current working directory ([Directory.current]).

Returns a `Future<File>` that completes with a [File] for the copied file.

If [newPath] identifies an existing file, that file is removed first. If [newPath] identifies an existing directory, the operation fails and the future completes with an exception.

### copySync()

```dart
File copySync(String newPath)
```

Synchronously copies this file.

If [newPath] is a relative path, it is resolved against the current working directory ([Directory.current]).

Returns a [File] for the copied file.

If [newPath] identifies an existing file, that file is removed first. If [newPath] identifies an existing directory the operation fails and an exception is thrown.

### length()

```dart
Future<int> length()
```

The length of the file.

Returns a `Future<int>` that completes with the length in bytes.

### lengthSync()

```dart
int lengthSync()
```

The length of the file provided synchronously.

Throws a [FileSystemException] if the operation fails.

### absolute

```dart
File get absolute
```

A [File] with the absolute path of [path].

The absolute path is computed by prefixing a relative path with the current working directory, or returning an absolute path unchanged.

### lastAccessed()

```dart
Future<DateTime> lastAccessed()
```

The last-accessed time of the file.

Returns a `Future<DateTime>` that completes with the date and time when the file was last accessed, if the information is available.

Throws a [FileSystemException] if the operation fails.

### lastAccessedSync()

```dart
DateTime lastAccessedSync()
```

The last-accessed time of the file.

Returns the date and time when the file was last accessed, if the information is available. Blocks until the information can be returned or it is determined that the information is not available.

Throws a [FileSystemException] if the operation fails.

### setLastAccessed()

```dart
Future setLastAccessed(DateTime time)
```

Modifies the time the file was last accessed.

Returns a [Future] that completes once the operation has completed.

Throws a [FileSystemException] if the time cannot be set.

### setLastAccessedSync()

```dart
void setLastAccessedSync(DateTime time)
```

Synchronously modifies the time the file was last accessed.

Throws a [FileSystemException] if the time cannot be set.

### lastModified()

```dart
Future<DateTime> lastModified()
```

Get the last-modified time of the file.

Returns a `Future<DateTime>` that completes with the date and time when the file was last modified, if the information is available.

Throws a [FileSystemException] if the operation fails.

### lastModifiedSync()

```dart
DateTime lastModifiedSync()
```

Get the last-modified time of the file.

Returns the date and time when the file was last modified, if the information is available. Blocks until the information can be returned or it is determined that the information is not available.

Throws a [FileSystemException] if the operation fails.

### setLastModified()

```dart
Future setLastModified(DateTime time)
```

Modifies the time the file was last modified.

Returns a [Future] that completes once the operation has completed.

Throws a [FileSystemException] if the time cannot be set.

### setLastModifiedSync()

```dart
void setLastModifiedSync(DateTime time)
```

Synchronously modifies the time the file was last modified.

If the attributes cannot be set, throws a [FileSystemException].

### open()

```dart
Future<RandomAccessFile> open({FileMode mode = FileMode.read})
```

Opens the file for random access operations.

Returns a `Future<RandomAccessFile>` that completes with the opened random access file. [RandomAccessFile]s must be closed using the [RandomAccessFile.close] method.

Files can be opened in three modes:

- [FileMode.read]: open the file for reading.

- [FileMode.write]: open the file for both reading and writing and truncate the file to length zero. If the file does not exist the file is created.

- [FileMode.append]: same as [FileMode.write] except that the file is not truncated.

Throws a [FileSystemException] if the operation fails.

### openSync()

```dart
RandomAccessFile openSync({FileMode mode = FileMode.read})
```

Synchronously opens the file for random access operations.

The result is a [RandomAccessFile] on which random access operations can be performed. Opened [RandomAccessFile]s must be closed using the [RandomAccessFile.close] method.

See [open] for information on the [mode] argument.

Throws a [FileSystemException] if the operation fails.

### openRead()

```dart
Stream<List<int>> openRead([int? start, int? end])
```

Creates a new independent [Stream] for the contents of this file.

If [start] is present, the file will be read from byte-offset [start]. Otherwise from the beginning (index 0).

If [end] is present, only bytes up to byte-index [end] will be read. Otherwise, until end of file.

In order to make sure that system resources are freed, the stream must be read to completion or the subscription on the stream must be cancelled.

If [File] is a [named pipe](https://en.wikipedia.org/wiki/Named_pipe) then the returned [Stream] will wait until the write side of the pipe is closed before signaling "done". If there are no writers attached to the pipe when it is opened, then [Stream.listen] will wait until a writer opens the pipe.

An error opening or reading the file will appear as a [FileSystemException] error event on the returned [Stream], after which the [Stream] is closed. For example:

```dart
// This example will print the "Error reading file" message and the
// `await for` loop will complete normally, without seeing any data
// events.
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

Creates a new independent [IOSink] for the file.

The [IOSink] must be closed when no longer used, to free system resources.

An [IOSink] for a file can be opened in two modes:

- [FileMode.write]: truncates the file to length zero.
- [FileMode.append]: sets the initial write position to the end of the file.

When writing strings through the returned [IOSink] the encoding specified using [encoding] will be used. The returned [IOSink] has an `encoding` property which can be changed after the [IOSink] has been created.

The returned [IOSink] does not transform newline characters (`"\n"`) to the platform's conventional line ending (e.g. `"\r\n"` on Windows). Write a [Platform.lineTerminator] if a platform-specific line ending is needed.

If an error occurs while opening or writing to the file, the [IOSink.done] [IOSink.flush], and [IOSink.close] futures will all complete with a [FileSystemException]. You must handle errors from the [IOSink.done] future or the error will be uncaught.

For example, [FutureExtensions.ignore] the [IOSink.done] error and remember to `await` the [IOSink.flush] and [IOSink.close] calls within a `try`/`catch`:

```dart
final sink = File('/tmp').openWrite(); // Can't write to /tmp
sink.done.ignore();
sink.write("This is a test");
try {
  // If one of these isn't awaited, then errors will pass silently!
  await sink.flush();
  await sink.close();
} on FileSystemException catch (e) {
  print('Error writing file: $e');
}
```

To handle errors asynchronously outside of the context of [IOSink.flush] and [IOSink.close], you can [Future.catchError] the [IOSink.done].

```dart
final sink = File('/tmp').openWrite(); // Can't write to /tmp
sink.done.catchError((e) {
 // Handle the error.
});
```

### readAsBytes()

```dart
Future<Uint8List> readAsBytes()
```

Reads the entire file contents as a list of bytes.

Returns a `Future<Uint8List>` that completes with the list of bytes that is the contents of the file.

### readAsBytesSync()

```dart
Uint8List readAsBytesSync()
```

Synchronously reads the entire file contents as a list of bytes.

Throws a [FileSystemException] if the operation fails.

### readAsString()

```dart
Future<String> readAsString({Encoding encoding = utf8})
```

Reads the entire file contents as a string using the given [Encoding].

Returns a `Future<String>` that completes with the string once the file contents has been read.

### readAsStringSync()

```dart
String readAsStringSync({Encoding encoding = utf8})
```

Synchronously reads the entire file contents as a string using the given [Encoding].

Throws a [FileSystemException] if the operation fails.

### readAsLines()

```dart
Future<List<String>> readAsLines({Encoding encoding = utf8})
```

Reads the entire file contents as lines of text using the given [Encoding].

Returns a `Future<List<String>>` that completes with the lines once the file contents has been read.

### readAsLinesSync()

```dart
List<String> readAsLinesSync({Encoding encoding = utf8})
```

Synchronously reads the entire file contents as lines of text using the given [Encoding].

Throws a [FileSystemException] if the operation fails.

### writeAsBytes()

```dart
Future<File> writeAsBytes(List<int> bytes, {FileMode mode = FileMode.write, bool flush = false})
```

Writes a list of bytes to a file.

Opens the file, writes the list of bytes to it, and closes the file. Returns a `Future<File>` that completes with this [File] object once the entire operation has completed.

By default [writeAsBytes] creates the file for writing and truncates the file if it already exists. In order to append the bytes to an existing file, pass [FileMode.append] as the optional mode parameter.

The elements of [bytes] should be integers in the range 0 to 255. Any integer, which is not in that range, is converted to a byte before being written. The conversion is equivalent to doing `value.toUnsigned(8)`.

If the argument [flush] is set to `true`, the data written will be flushed to the file system before the returned future completes.

### writeAsBytesSync()

```dart
void writeAsBytesSync(List<int> bytes, {FileMode mode = FileMode.write, bool flush = false})
```

Synchronously writes a list of bytes to a file.

Opens the file, writes the list of bytes to it and closes the file.

By default [writeAsBytesSync] creates the file for writing and truncates the file if it already exists. In order to append the bytes to an existing file, pass [FileMode.append] as the optional mode parameter.

The elements of [bytes] should be integers in the range 0 to 255. Any integer, which is not in that range, is converted to a byte before being written. The conversion is equivalent to doing `value.toUnsigned(8)`.

If the [flush] argument is set to `true` data written will be flushed to the file system before returning.

Throws a [FileSystemException] if the operation fails.

### writeAsString()

```dart
Future<File> writeAsString(String contents, {FileMode mode = FileMode.write, Encoding encoding = utf8, bool flush = false})
```

Writes a string to a file.

Opens the file, writes the string in the given encoding, and closes the file. Returns a `Future<File>` that completes with this [File] object once the entire operation has completed.

By default [writeAsString] creates the file for writing and truncates the file if it already exists. In order to append the bytes to an existing file, pass [FileMode.append] as the optional mode parameter.

If the argument [flush] is set to `true`, the data written will be flushed to the file system before the returned future completes.

This method does not transform newline characters (`"\n"`) to the platform conventional line ending (e.g. `"\r\n"` on Windows). Use [Platform.lineTerminator] to separate lines in [contents] if platform conventional line endings are needed.

### writeAsStringSync()

```dart
void writeAsStringSync(String contents, {FileMode mode = FileMode.write, Encoding encoding = utf8, bool flush = false})
```

Synchronously writes a string to a file.

Opens the file, writes the string in the given encoding, and closes the file.

By default [writeAsStringSync] creates the file for writing and truncates the file if it already exists. In order to append the bytes to an existing file, pass [FileMode.append] as the optional mode parameter.

If the [flush] argument is set to `true`, data written will be flushed to the file system before returning.

This method does not transform newline characters (`"\n"`) to the platform conventional line ending (e.g. `"\r\n"` on Windows). Use [Platform.lineTerminator] to separate lines in [contents] if platform conventional line endings are needed.

Throws a [FileSystemException] if the operation fails.

### path

```dart
String get path
```

Get the path of the file.

# RandomAccessFile

```dart
abstract interface class RandomAccessFile {}
```

Random access to the data in a file.

`RandomAccessFile` objects are obtained by calling the `open` method on a [File] object.

A `RandomAccessFile` has both asynchronous and synchronous methods. The asynchronous methods all return a [Future] whereas the synchronous methods will return the result directly, and block the current isolate until the result is ready.

At most one asynchronous method can be pending on a given `RandomAccessFile` instance at the time. If another asynchronous method is called when one is already in progress, a [FileSystemException] is thrown.

If an asynchronous method is pending, it is also not possible to call any synchronous methods. This will also throw a [FileSystemException].

### close()

```dart
Future<void> close()
```

Closes the file.

Returns a [Future] that completes when it has been closed.

### closeSync()

```dart
void closeSync()
```

Synchronously closes the file.

Throws a [FileSystemException] if the operation fails.

### readByte()

```dart
Future<int> readByte()
```

Reads a byte from the file.

Returns a `Future<int>` that completes with the byte, or with -1 if end-of-file has been reached.

### readByteSync()

```dart
int readByteSync()
```

Synchronously reads a single byte from the file.

If end-of-file has been reached -1 is returned.

Throws a [FileSystemException] if the operation fails.

### read()

```dart
Future<Uint8List> read(int count)
```

Reads up to [count] bytes from a file.

May return fewer than [count] bytes. This can happen, for example, when reading past the end of a file or when reading from a pipe that does not currently contain additional data.

An empty [Uint8List] will only be returned when reading past the end of the file or when [count] is `0`.

### readSync()

```dart
Uint8List readSync(int count)
```

Synchronously reads up to [count] bytes from a file

May return fewer than [count] bytes. This can happen, for example, when reading past the end of a file or when reading from a pipe that does not currently contain additional data.

An empty [Uint8List] will only be returned when reading past the end of the file or when [count] is `0`.

Throws a [FileSystemException] if the operation fails.

### readInto()

```dart
Future<int> readInto(List<int> buffer, [int start = 0, int? end])
```

Reads bytes into an existing [buffer].

Reads bytes and writes them into the range of [buffer] from [start] to [end]. The [start] must be non-negative and no greater than [buffer].length. If [end] is omitted, it defaults to [buffer].length. Otherwise [end] must be no less than [start] and no greater than [buffer].length.

Returns the number of bytes read. This maybe be less than `end - start` if the file doesn't have that many bytes to read.

### readIntoSync()

```dart
int readIntoSync(List<int> buffer, [int start = 0, int? end])
```

Synchronously reads into an existing [buffer].

Reads bytes and writes them into the range of [buffer] from [start] to [end]. The [start] must be non-negative and no greater than [buffer].length. If [end] is omitted, it defaults to [buffer].length. Otherwise [end] must be no less than [start] and no greater than [buffer].length.

Returns the number of bytes read. This maybe be less than `end - start` if the file doesn't have that many bytes to read.

Throws a [FileSystemException] if the operation fails.

### writeByte()

```dart
Future<RandomAccessFile> writeByte(int value)
```

Writes a single byte to the file.

Returns a `Future<RandomAccessFile>` that completes with this random access file when the write completes.

### writeByteSync()

```dart
int writeByteSync(int value)
```

Synchronously writes a single byte to the file.

Returns 1 on success.

Throws a [FileSystemException] if the operation fails.

### writeFrom()

```dart
Future<RandomAccessFile> writeFrom(List<int> buffer, [int start = 0, int? end])
```

Writes from a [buffer] to the file.

Will read the buffer from index [start] to index [end]. The [start] must be non-negative and no greater than [buffer].length. If [end] is omitted, it defaults to [buffer].length. Otherwise [end] must be no less than [start] and no greater than [buffer].length.

Returns a `Future<RandomAccessFile>` that completes with this [RandomAccessFile] when the write completes.

### writeFromSync()

```dart
void writeFromSync(List<int> buffer, [int start = 0, int? end])
```

Synchronously writes from a [buffer] to the file.

Will read the buffer from index [start] to index [end]. The [start] must be non-negative and no greater than [buffer].length. If [end] is omitted, it defaults to [buffer].length. Otherwise [end] must be no less than [start] and no greater than [buffer].length.

Throws a [FileSystemException] if the operation fails.

### writeString()

```dart
Future<RandomAccessFile> writeString(String string, {Encoding encoding = utf8})
```

Writes a string to the file using the given [Encoding].

Returns a `Future<RandomAccessFile>` that completes with this random access file when the write completes.

### writeStringSync()

```dart
void writeStringSync(String string, {Encoding encoding = utf8})
```

Synchronously writes a single string to the file using the given [Encoding].

Throws a [FileSystemException] if the operation fails.

### position()

```dart
Future<int> position()
```

Gets the current byte position in the file.

Returns a `Future<int>` that completes with the position.

### positionSync()

```dart
int positionSync()
```

Synchronously gets the current byte position in the file.

Throws a [FileSystemException] if the operation fails.

### setPosition()

```dart
Future<RandomAccessFile> setPosition(int position)
```

Sets the byte position in the file.

Returns a `Future<RandomAccessFile>` that completes with this random access file when the position has been set.

### setPositionSync()

```dart
void setPositionSync(int position)
```

Synchronously sets the byte position in the file.

Throws a [FileSystemException] if the operation fails.

### truncate()

```dart
Future<RandomAccessFile> truncate(int length)
```

Truncates (or extends) the file to [length] bytes.

Returns a `Future<RandomAccessFile>` that completes with this random access file when the truncation has been performed.

### truncateSync()

```dart
void truncateSync(int length)
```

Synchronously truncates (or extends) the file to [length] bytes.

Throws a [FileSystemException] if the operation fails.

### length()

```dart
Future<int> length()
```

Gets the length of the file.

Returns a `Future<int>` that completes with the length in bytes.

### lengthSync()

```dart
int lengthSync()
```

Synchronously gets the length of the file.

Throws a [FileSystemException] if the operation fails.

### flush()

```dart
Future<RandomAccessFile> flush()
```

Flushes the contents of the file to disk.

Returns a `Future<RandomAccessFile>` that completes with this random access file when the flush operation completes.

### flushSync()

```dart
void flushSync()
```

Synchronously flushes the contents of the file to disk.

Throws a [FileSystemException] if the operation fails.

### lock()

```dart
Future<RandomAccessFile> lock([FileLock mode = FileLock.exclusive, int start = 0, int end = -1])
```

Locks the file or part of the file.

By default an exclusive lock will be obtained, but that can be overridden by the [mode] argument.

Locks the byte range from [start] to [end] of the file, with the byte at position `end` not included. If no arguments are specified, the full file is locked, If only `start` is specified the file is locked from byte position `start` to the end of the file, no matter how large it grows. It is possible to specify an explicit value of `end` which is past the current length of the file.

To obtain an exclusive lock on a file, it must be opened for writing.

If [mode] is [FileLock.exclusive] or [FileLock.shared], an error is signaled if the lock cannot be obtained. If [mode] is [FileLock.blockingExclusive] or [FileLock.blockingShared], the returned [Future] is resolved only when the lock has been obtained.

_NOTE_ file locking does have slight differences in behavior across platforms:

On Linux and OS X this uses advisory locks, which have the surprising semantics that all locks associated with a given file are removed when _any_ file descriptor for that file is closed by the process. Note that this does not actually lock the file for access. Also note that advisory locks are on a process level. This means that several isolates in the same process can obtain an exclusive lock on the same file.

On Windows the regions used for lock and unlock needs to match. If that is not the case unlocking will result in the OS error "The segment is already unlocked".

On Windows, locking a file associates the lock with the specific file handle used to acquire the lock. If the same file is opened again with a different handle (even within the same process), attempts to write to the locked region using the new handle will fail. To ensure successful writes after locking, use the same [RandomAccessFile] object that acquired the lock for subsequent write operations.

### lockSync()

```dart
void lockSync([FileLock mode = FileLock.exclusive, int start = 0, int end = -1])
```

Synchronously locks the file or part of the file.

By default an exclusive lock will be obtained, but that can be overridden by the [mode] argument.

Locks the byte range from [start] to [end] of the file ,with the byte at position `end` not included. If no arguments are specified, the full file is locked, If only `start` is specified the file is locked from byte position `start` to the end of the file, no matter how large it grows. It is possible to specify an explicit value of `end` which is past the current length of the file.

To obtain an exclusive lock on a file it must be opened for writing.

If [mode] is [FileLock.exclusive] or [FileLock.shared], an exception is thrown if the lock cannot be obtained. If [mode] is [FileLock.blockingExclusive] or [FileLock.blockingShared], the call returns only after the lock has been obtained.

_NOTE_ file locking does have slight differences in behavior across platforms:

On Linux and OS X this uses advisory locks, which have the surprising semantics that all locks associated with a given file are removed when _any_ file descriptor for that file is closed by the process. Note that this does not actually lock the file for access. Also note that advisory locks are on a process level. This means that several isolates in the same process can obtain an exclusive lock on the same file.

On Windows the regions used for lock and unlock needs to match. If that is not the case unlocking will result in the OS error "The segment is already unlocked".

On Windows, locking a file associates the lock with the specific file handle used to acquire the lock. If the same file is opened again with a different handle (even within the same process), attempts to write to the locked region using the new handle will fail. To ensure successful writes after locking, use the same [RandomAccessFile] object that acquired the lock for subsequent write operations.

### unlock()

```dart
Future<RandomAccessFile> unlock([int start = 0, int end = -1])
```

Unlocks the file or part of the file.

Unlocks the byte range from [start] to [end] of the file, with the byte at position `end` not included. If no arguments are specified, the full file is unlocked, If only `start` is specified the file is unlocked from byte position `start` to the end of the file.

_NOTE_ file locking does have slight differences in behavior across platforms:

See [lock] for more details.

### unlockSync()

```dart
void unlockSync([int start = 0, int end = -1])
```

Synchronously unlocks the file or part of the file.

Unlocks the byte range from [start] to [end] of the file, with the byte at position `end` not included. If no arguments are specified, the full file is unlocked, If only `start` is specified the file is unlocked from byte position `start` to the end of the file.

_NOTE_ file locking does have slight differences in behavior across platforms:

See [lockSync] for more details.

### toString()

```dart
String toString()
```

Returns a human-readable string for this random access file.

### path

```dart
String get path
```

The path of the file underlying this random access file.

# FileSystemException

```dart
class FileSystemException implements IOException {}
```

Exception thrown when a file operation fails.

### message

```dart
String message
```

Message describing the error.

The message does not include any detailed information from the underlying OS error. Check [osError] for that information.

### path

```dart
String? path
```

The file system path on which the error occurred.

Can be `null` if the exception does not relate directly to a file system path.

### osError

```dart
OSError? osError
```

The underlying OS error.

Can be `null` if the exception is not raised due to an OS error.

### FileSystemException()

```dart
FileSystemException([String message = "", String? path = "", OSError? osError])
```

Creates a new file system exception with optional parts.

Creates an exception with [FileSystemException.message], [FileSystemException.path] and [FileSystemException.osError] values take from the optional parameters of the same name.

The [message] and [path] path defaults to empty strings if omitted, and [osError] defaults to `null`.

### toString()

```dart
String toString()
```

# PathAccessException

```dart
class PathAccessException extends FileSystemException {}
```

Exception thrown when a file operation fails because the necessary access rights are not available.

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

Exception thrown when a file operation fails because the target path already exists.

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

Exception thrown when a file operation fails because a file or directory does not exist.

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

The "read" end of an [Pipe] created by [Pipe.create].

The read stream will continue to listen until the "write" end of the pipe (i.e. [Pipe.write]) is closed.

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

The "write" end of an [Pipe] created by [Pipe.create].

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

An anonymous pipe that can be used to send data in a single direction i.e. data written to [write] can be read using [read].

On macOS and Linux (excluding Android), either the [read] or [write] portion of the pipe can be transmitted to another process and used for interprocess communication.

For example:

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

The read end of the [Pipe].

### write

```dart
WritePipe get write
```

The write end of the [Pipe].

### create()

```dart
Future<Pipe> create()
```

### Pipe.createSync()

```dart
Pipe.createSync()
```

Synchronously creates an anonymous pipe.
