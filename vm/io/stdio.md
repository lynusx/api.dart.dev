# Stdin

```dart
class Stdin extends _StdStream implements Stream<List<int>> {}
```

The standard input stream of the process.

Allows both synchronous and asynchronous reads from the standard input stream.

Mixing synchronous and asynchronous reads is undefined.

### readLineSync()

```dart
String? readLineSync({Encoding encoding = systemEncoding, bool retainNewlines = false})
```

Reads a line from stdin.

Blocks until a full line is available.

Lines may be terminated by either `<CR><LF>` or `<LF>`. On Windows, in cases where the [stdioType] of stdin is [StdioType.terminal], the terminator may also be a single `<CR>`.

Input bytes are converted to a string by [encoding]. If [encoding] is omitted, it defaults to [systemEncoding].

If [retainNewlines] is `false`, the returned string will not include the final line terminator. If `true`, the returned string will include the line terminator. Default is `false`.

If end-of-file is reached after any bytes have been read from stdin, that data is returned without a line terminator. Returns `null` if no bytes preceded the end of input.

### echoMode

```dart
bool get echoMode
```

Whether echo mode is enabled on [stdin].

If disabled, input from the console will not be echoed.

Default depends on the parent process, but is usually enabled.

On POSIX systems this mode is the `echo` local terminal mode. Before Dart 2.18, it also controlled the `echonl` mode, which is now controlled by [echoNewlineMode].

On Windows this mode can only be enabled if [lineMode] is enabled as well.

### echoMode

```dart
set echoMode(bool echoMode)
```

### echoNewlineMode

```dart
bool get echoNewlineMode
```

Whether echo newline mode is enabled on [stdin].

If enabled, newlines from the terminal will be echoed even if the regular [echoMode] is disabled. This mode may require `lineMode` to be turned on to have an effect.

Default depends on the parent process, but is usually disabled.

On POSIX systems this mode is the `echonl` local terminal mode.

On Windows this mode cannot be set.

### echoNewlineMode

```dart
set echoNewlineMode(bool echoNewlineMode)
```

### lineMode

```dart
bool get lineMode
```

Whether line mode is enabled on [stdin].

If enabled, characters are delayed until a newline character is entered. If disabled, characters will be available as typed.

Default depends on the parent process, but is usually enabled.

On POSIX systems this mode is the `icanon` local terminal mode.

On Windows this mode can only be disabled if [echoMode] is disabled as well.

### lineMode

```dart
set lineMode(bool lineMode)
```

### supportsAnsiEscapes

```dart
bool get supportsAnsiEscapes
```

Whether connected to a terminal that supports ANSI escape sequences.

Not all terminals are recognized, and not all recognized terminals can report whether they support ANSI escape sequences, so this value is a best-effort attempt at detecting the support.

The actual escape sequence support may differ between terminals, with some terminals supporting more escape sequences than others, and some terminals even differing in behavior for the same escape sequence.

The ANSI color selection is generally supported.

Currently, a `TERM` environment variable containing the string `xterm`, `screen`, `rxvt`, or `tmux` will be taken as evidence that ANSI escape sequences are supported. On Windows, only versions of Windows 10 after v.1511 ("TH2", OS build 10586) will be detected as supporting the output of ANSI escape sequences, and only versions after v.1607 ("Anniversary Update", OS build 14393) will be detected as supporting the input of ANSI escape sequences.

### readByteSync()

```dart
int readByteSync()
```

Synchronously reads a byte from stdin.

This call will block until a byte is available.

If at end of file, -1 is returned.

### hasTerminal

```dart
bool get hasTerminal
```

Whether there is a terminal attached to stdin.

# Stdout

```dart
class Stdout extends _StdSink implements IOSink {}
```

An [IOSink] connected to either the standard out or error of the process.

Provides a _blocking_ `IOSink`, so using it to write will block until the output is written.

In some situations this blocking behavior is undesirable as it does not provide the same non-blocking behavior that `dart:io` in general exposes. Use the property [nonBlocking] to get an [IOSink] which has the non-blocking behavior.

This class can also be used to check whether `stdout` or `stderr` is connected to a terminal and query some terminal properties.

The [addError] API is inherited from [StreamSink] and calling it will result in an unhandled asynchronous error unless there is an error handler on [done].

The [lineTerminator] field is used by the [write], [writeln], [writeAll] and [writeCharCode] methods to translate `"\n"`. By default, `"\n"` is output literally.

### hasTerminal

```dart
bool get hasTerminal
```

Whether there is a terminal attached to stdout.

### terminalColumns

```dart
int get terminalColumns
```

The number of columns of the terminal.

If no terminal is attached to stdout, a [StdoutException] is thrown. See [hasTerminal] for more info.

### terminalLines

```dart
int get terminalLines
```

The number of lines of the terminal.

If no terminal is attached to stdout, a [StdoutException] is thrown. See [hasTerminal] for more info.

### supportsAnsiEscapes

```dart
bool get supportsAnsiEscapes
```

Whether connected to a terminal that supports ANSI escape sequences.

Not all terminals are recognized, and not all recognized terminals can report whether they support ANSI escape sequences, so this value is a best-effort attempt at detecting the support.

The actual escape sequence support may differ between terminals, with some terminals supporting more escape sequences than others, and some terminals even differing in behavior for the same escape sequence.

The ANSI color selection is generally supported.

Currently, a `TERM` environment variable containing the string `xterm` will be taken as evidence that ANSI escape sequences are supported. On Windows, only versions of Windows 10 after v.1511 ("TH2", OS build 10586) will be detected as supporting the output of ANSI escape sequences, and only versions after v.1607 ("Anniversary Update", OS build 14393) will be detected as supporting the input of ANSI escape sequences.

### nonBlocking

```dart
IOSink get nonBlocking
```

A non-blocking `IOSink` for the same output.

The returned `IOSink` will be initialized with an [encoding] of UTF-8 and will not do line ending conversion.

# StdoutException

```dart
class StdoutException implements IOException {}
```

Exception thrown by some operations of [Stdout]

### message

```dart
String message
```

Message describing cause of the exception.

### osError

```dart
OSError? osError
```

The underlying OS error, if available.

### StdoutException()

```dart
StdoutException(String message, [OSError? osError])
```

### toString()

```dart
String toString()
```

# StdinException

```dart
class StdinException implements IOException {}
```

Exception thrown by some operations of [Stdin]

### message

```dart
String message
```

Message describing cause of the exception.

### osError

```dart
OSError? osError
```

The underlying OS error, if available.

### StdinException()

```dart
StdinException(String message, [OSError? osError])
```

### toString()

```dart
String toString()
```

# StdioType

```dart
final class StdioType {}
```

The type of object a standard IO stream can be attached to.

### terminal

```dart
StdioType terminal
```

### pipe

```dart
StdioType pipe
```

### file

```dart
StdioType file
```

### other

```dart
StdioType other
```

### name

```dart
String name
```

### toString()

```dart
String toString()
```

# stdin

```dart
Stdin get stdin
```

The standard input stream of data read by this program.

# stdout

```dart
Stdout get stdout
```

The standard output stream of data written by this program.

The `addError` API is inherited from `StreamSink` and calling it will result in an unhandled asynchronous error unless there is an error handler on `done`.

# stderr

```dart
Stdout get stderr
```

The standard output stream of errors written by this program.

The `addError` API is inherited from `StreamSink` and calling it will result in an unhandled asynchronous error unless there is an error handler on `done`.

# stdioType()

```dart
StdioType stdioType(dynamic object)
```

Whether a stream is attached to a file, pipe, terminal, or something else.
