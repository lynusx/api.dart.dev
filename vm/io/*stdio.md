# Stdin

```dart
class Stdin extends _StdStream implements Stream<List<int>> {}
```

进程的标准输入流。

支持对标准输入流进行同步和异步读取。

同时进行同步和异步读取的行为是未定义的。

### readLineSync()

```dart
String? readLineSync({Encoding encoding = systemEncoding, bool retainNewlines = false})
```

从 stdin 读取一行。

阻塞直到获得完整的一行。

行可以由 `<CR><LF>` 或 `<LF>` 终止。在 Windows 上，当 stdin 的 [stdioType] 为 [StdioType.terminal] 时，终止符也可以是单个 `<CR>`。

输入字节通过 [encoding] 转换为字符串。如果省略 [encoding]，默认使用 [systemEncoding]。

如果 [retainNewlines] 为 `false`，返回的字符串将不包含末尾的行终止符；如果为 `true`，返回的字符串将包含行终止符。默认值为 `false`。

如果在从 stdin 读取了部分字节之后到达文件末尾，将返回这部分数据，且不带行终止符。如果在输入结束之前未读取到任何字节，则返回 `null`。

### echoMode

```dart
bool get echoMode
```

是否在 [stdin] 上启用了回显模式。

如果禁用，来自控制台的输入将不会被回显。

默认值取决于父进程，但通常是启用的。

在 POSIX 系统上，此模式对应 `echo` 本地终端模式。在 Dart 2.18 之前，它还控制 `echonl` 模式，该模式现在由 [echoNewlineMode] 控制。

在 Windows 上，只有同时启用 [lineMode] 时，此模式才能启用。

### echoMode

```dart
set echoMode(bool echoMode)
```

### echoNewlineMode

```dart
bool get echoNewlineMode
```

是否在 [stdin] 上启用了回显换行模式。

如果启用，即使常规的 [echoMode] 被禁用，终端的换行符仍会被回显。此模式可能需要同时开启 `lineMode` 才能生效。

默认值取决于父进程，但通常是禁用的。

在 POSIX 系统上，此模式对应 `echonl` 本地终端模式。

在 Windows 上，此模式无法设置。

### echoNewlineMode

```dart
set echoNewlineMode(bool echoNewlineMode)
```

### lineMode

```dart
bool get lineMode
```

是否在 [stdin] 上启用了行模式。

如果启用，字符会被延迟保留，直到输入换行符为止。如果禁用，字符将在输入时立即可用。

默认值取决于父进程，但通常是启用的。

在 POSIX 系统上，此模式对应 `icanon` 本地终端模式。

在 Windows 上，只有同时禁用 [echoMode] 时，此模式才能禁用。

### lineMode

```dart
set lineMode(bool lineMode)
```

### supportsAnsiEscapes

```dart
bool get supportsAnsiEscapes
```

是否连接到支持 ANSI 转义序列的终端。

并非所有终端都能被识别，也并非所有可识别的终端都能报告其是否支持 ANSI 转义序列，因此该值只是对支持情况的最佳猜测。

实际的转义序列支持程度因终端而异，有些终端支持的转义序列比其他终端更多，甚至某些终端对同一转义序列的处理方式也可能不同。

ANSI 颜色选择通常都受支持。

目前，如果 `TERM` 环境变量包含字符串 `xterm`、`screen`、`rxvt` 或 `tmux`，则视为支持 ANSI 转义序列的证据。在 Windows 上，只有 Windows 10 v1511（"TH2"，操作系统内部版本 10586）之后的版本才会被检测为支持 ANSI 转义序列的输出，只有 v1607（"周年更新"，操作系统内部版本 14393）之后的版本才会被检测为支持 ANSI 转义序列的输入。

### readByteSync()

```dart
int readByteSync()
```

从 stdin 同步读取一个字节。

此调用将阻塞，直到有字节可用为止。

如果已到达文件末尾，返回 -1。

### hasTerminal

```dart
bool get hasTerminal
```

stdin 是否连接了终端。

# Stdout

```dart
class Stdout extends _StdSink implements IOSink {}
```

连接到进程标准输出或标准错误的 [IOSink]。

提供一个*阻塞式*的 `IOSink`，因此使用它写入时会阻塞，直到输出完成。

在某些场景下，这种阻塞行为并不理想，因为它不具备 `dart:io` 通常提供的非阻塞行为。可使用 [nonBlocking] 属性获取具有非阻塞行为的 [IOSink]。

此类还可用于检查 `stdout` 或 `stderr` 是否连接到终端，并查询部分终端属性。

[addError] API 继承自 [StreamSink]，调用它将导致未处理的异步错误，除非在 [done] 上设置了错误处理程序。

[lineTerminator] 字段被 [write]、[writeln]、[writeAll] 和 [writeCharCode] 方法用于转换 `"\n"`。默认情况下，`"\n"` 会被原样输出。

### hasTerminal

```dart
bool get hasTerminal
```

stdout 是否连接了终端。

### terminalColumns

```dart
int get terminalColumns
```

终端的列数。

如果 stdout 未连接终端，将抛出 [StdoutException]。更多信息参见 [hasTerminal]。

### terminalLines

```dart
int get terminalLines
```

终端的行数。

如果 stdout 未连接终端，将抛出 [StdoutException]。更多信息参见 [hasTerminal]。

### supportsAnsiEscapes

```dart
bool get supportsAnsiEscapes
```

是否连接到支持 ANSI 转义序列的终端。

并非所有终端都能被识别，也并非所有可识别的终端都能报告其是否支持 ANSI 转义序列，因此该值只是对支持情况的最佳猜测。

实际的转义序列支持程度因终端而异，有些终端支持的转义序列比其他终端更多，甚至某些终端对同一转义序列的处理方式也可能不同。

ANSI 颜色选择通常都受支持。

目前，如果 `TERM` 环境变量包含字符串 `xterm`，则视为支持 ANSI 转义序列的证据。在 Windows 上，只有 Windows 10 v1511（"TH2"，操作系统内部版本 10586）之后的版本才会被检测为支持 ANSI 转义序列的输出，只有 v1607（"周年更新"，操作系统内部版本 14393）之后的版本才会被检测为支持 ANSI 转义序列的输入。

### nonBlocking

```dart
IOSink get nonBlocking
```

同一输出对应的非阻塞式 `IOSink`。

返回的 `IOSink` 的 [encoding] 将初始化为 UTF-8，且不会进行行终止符转换。

# StdoutException

```dart
class StdoutException implements IOException {}
```

由 [Stdout] 的某些操作抛出的异常。

### message

```dart
String message
```

描述异常原因的消息。

### osError

```dart
OSError? osError
```

底层的操作系统错误（如果可用）。

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

由 [Stdin] 的某些操作抛出的异常。

### message

```dart
String message
```

描述异常原因的消息。

### osError

```dart
OSError? osError
```

底层的操作系统错误（如果可用）。

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

标准 IO 流可连接到的对象类型。

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

本程序读取数据所使用的标准输入流。

# stdout

```dart
Stdout get stdout
```

本程序写入数据所使用的标准输出流。

`addError` API 继承自 `StreamSink`，调用它将导致未处理的异步错误，除非在 `done` 上设置了错误处理程序。

# stderr

```dart
Stdout get stderr
```

本程序写入错误信息所使用的标准输出流。

`addError` API 继承自 `StreamSink`，调用它将导致未处理的异步错误，除非在 `done` 上设置了错误处理程序。

# stdioType()

```dart
StdioType stdioType(dynamic object)
```

判断某个流连接到的是文件、管道、终端，还是其他类型。
