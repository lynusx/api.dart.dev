# exit()

```dart
Never exit(int code)
```

立即以给定的退出码退出 Dart VM 进程。

此操作不会等待任何异步操作终止，也不会执行 `finally` 代码块。因此使用 [exit] 很可能会导致数据丢失。

子进程不会被显式终止（但当它们检测到父进程已退出时，可能会自行终止）。

在调试时，如果调用 [exit]，VM 将不会遵循 `--pause-isolates-on-exit` 标志，因为调用此方法会导致 Dart VM 进程立即关闭。要在退出时正确中断，可考虑在调用 [exit] 之前，从 `dart:developer` 调用 [debugger]，或在当前 isolate（[Isolate.current]）上调用 `dart:isolate` 中的 [Isolate.pause] 来暂停该 isolate。

退出码的处理方式因平台而异。

在 Linux 和 OS X 上，正常终止的退出码始终在 `[0..255]` 范围内。如果设置的退出码超出此范围，实际退出码将是屏蔽掉高位后取低 8 位作为无符号值。例如，使用退出码 -1 将得到实际退出码 255。

在 Windows 上，退出码可以设置为任意 32 位值。但其中一些值被保留用于报告系统错误，如崩溃。

除此之外，Dart 可执行文件本身使用退出码 `254` 报告编译时错误，使用退出码 `255` 报告运行时错误（未处理的异常）。

由于这些原因，建议仅使用 \[0..127\] 范围内的退出码来向外部环境传达 Dart 程序的运行结果，这样可以避免任何跨平台问题。

# exitCode

```dart
void set exitCode(int code)
```

设置 Dart VM 的全局退出码。

退出码是 Dart VM 全局的，任何 isolate 对 exitCode 的最后一次赋值将决定 Dart VM 正常终止时的退出码。

默认值为 `0`。

有关如何选择退出码值的更多信息，请参见 [exit]。

# exitCode

```dart
int get exitCode
```

获取 Dart VM 的全局退出码。

退出码是 Dart VM 全局的，任何 isolate 对 exitCode 的最后一次赋值将决定 Dart VM 正常终止时的退出码。

有关退出码值的更多信息，请参见 [exit]。

# sleep()

```dart
void sleep(Duration duration)
```

按 [duration] 指定的时长休眠。

请谨慎使用此方法，因为在 isolate 因调用 [sleep] 而阻塞期间，无法处理任何异步操作。

```dart
var duration = const Duration(seconds: 5);
print('Start sleeping');
sleep(duration);
print('5 seconds has passed');
```

# pid

```dart
int get pid
```

返回当前进程的 PID。

# ProcessInfo

```dart
abstract final class ProcessInfo {}
```

用于获取当前进程信息的方法。

### currentRss

```dart
int get currentRss
```

进程当前的常驻内存集大小（resident set size），单位为字节。

请注意，此字段的含义因平台而异。例如，此处统计的部分内存可能与其他进程共享，或者如果同一页面被映射到某个进程的地址空间中，可能会被重复计算。

### maxRss

```dart
int get maxRss
```

进程常驻内存集大小的历史最高水位线，单位为字节。

请注意，此字段的含义因平台而异。例如，此处统计的部分内存可能与其他进程共享，或者如果同一页面被映射到某个进程的地址空间中，可能会被重复计算。

# ProcessStartMode

```dart
final class ProcessStartMode {}
```

运行新进程的模式。

### normal

```dart
dynamic normal
```

普通子进程。

### inheritStdio

```dart
dynamic inheritStdio
```

子进程继承标准输入输出句柄。

### detached

```dart
dynamic detached
```

分离的子进程，没有开放的通信通道。

### detachedWithStdio

```dart
dynamic detachedWithStdio
```

分离的子进程，其 stdin、stdout 和 stderr 仍保持开放以便通信。

### values

```dart
List<ProcessStartMode> get values
```

### toString()

```dart
String toString()
```

# Process

```dart
abstract interface class Process {}
```

用于执行程序的手段。

使用静态方法 [start] 和 [run] 启动新进程。run 方法非交互式地运行进程直至完成。而 start 方法允许你的代码与正在运行的进程进行交互。

## 使用 run 方法启动进程

以下示例代码使用 run 方法创建一个进程来运行 UNIX 命令 `ls`，用于列出目录内容。当进程终止时，run 方法会以一个 [ProcessResult] 对象完成。该对象提供了对进程输出和退出码的访问。run 方法不会返回 `Process` 对象；这可以防止代码与正在运行的进程进行交互。

```dart
import 'dart:io';

main() async {
  // 在类 UNIX 系统上列出当前目录中的所有文件。
  var result = await Process.run('ls', ['-l']);
  print(result.stdout);
}
```

## 使用 start 方法启动进程

以下示例使用 start 来创建进程。start 方法返回一个 `Process` 对象的 [Future]。当该 Future 完成时，进程已启动，代码可以与该进程进行交互：写入 stdin、监听 stdout 等。

以下示例启动 UNIX 的 `cat` 工具，该工具在不带命令行参数的情况下，会回显其输入。程序向进程的标准输入流写入数据，并打印来自其标准输出流的数据。

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  var process = await Process.start('cat', []);
  process.stdout
      .transform(utf8.decoder)
      .forEach(print);
  process.stdin.writeln('Hello, world!');
  process.stdin.writeln('Hello, galaxy!');
  process.stdin.writeln('Hello, universe!');
}
```

## 标准 I/O 流

如前面的示例代码所示，可以通过 getter [stdout] 与 `Process` 的标准输出流进行交互，通过 getter [stdin] 与 `Process` 的标准输入流进行交互。此外，`Process` 还提供了 getter [stderr]，用于使用 `Process` 的标准错误流。

`Process` 的流与当前程序的顶层流是不同的。

**注意：** `stdin`、`stdout` 和 `stderr` 是通过父进程与生成的子进程之间的管道实现的。这些管道的容量是有限的。如果子进程向 stderr 或 stdout 写入的数据超过该限制而未被读取，子进程会阻塞，等待管道缓冲区接受更多数据。例如：

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // 以下 await 语句永远不会完成，因为子进程
  // 永远不会退出，因为它被阻塞在等待其 stdout
  // 被读取。
  await process.stderr.forEach(print);
}
```

## 退出码

调用 [exitCode] 方法获取进程的退出码。退出码表示程序是成功终止（通常用退出码 0 表示）还是出错终止。

如果使用 start 方法，则可以通过 `Process` 对象上的 Future 获取 [exitCode]（如下例所示）。如果使用 run 方法，则可以通过 [ProcessResult] 实例上的 getter 获取 [exitCode]。

```dart
import 'dart:io';

main() async {
  var process = await Process.start('ls', ['-l']);
  var exitCode = await process.exitCode;
  print('exit code: $exitCode');
}
```

### exitCode

```dart
Future<int> get exitCode
```

一个 `Future`，当进程结束时以进程的退出码完成。

对于以 [ProcessStartMode.detached] 或 [ProcessStartMode.detachedWithStdio] 模式运行的进程，退出码不可用，使用该 getter 将抛出 [StateError]。

退出码的处理方式因平台而异。

在 Linux 和 OS X 上，正常退出码为 `[0..255]` 范围内的正值。如果进程因信号而终止，退出码将是 `[-255..-1]` 范围内的负值，其中退出码的绝对值即为信号编号。例如，如果进程因段错误而崩溃，退出码将为 -11，因为 SIGSEGV 信号的编号为 11。

在 Windows 上，进程可以报告任意 32 位值作为退出码。返回退出码时，该退出码会被转换为有符号值。一些特殊值用于报告因系统事件而终止的情况。例如，如果进程因访问违规而崩溃，32 位退出码为 `0xc0000005`，将作为负数 `-1073741819` 返回。要获得原始的 32 位值，可使用 `(0x100000000 + exitCode) & 0xffffffff`。

无法保证在返回的 Future 完成时，[stdout] 和 [stderr] 已完成报告进程的缓冲输出。要确保捕获所有输出，请等待流上的 done 事件。

### start()

```dart
Future<Process> start(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, ProcessStartMode mode = ProcessStartMode.normal})
```

使用指定的 [arguments] 启动运行 [executable] 的进程。

返回一个 `Future<Process>`，当进程成功启动时以 [Process] 实例完成。该 [Process] 对象可用于与进程交互。如果进程无法启动，返回的 [Future] 将以异常完成。

建议为 [executable] 使用绝对路径，因为解析 [executable] 路径的方式是特定于平台的。在 Windows 上，出于解析 [executable] 路径的目的，[environment] 映射参数中设置的任何 `PATH` 以及 [workingDirectory] 参数中设置的路径都会被忽略。

使用 [workingDirectory] 设置进程的工作目录。请注意，在某些平台上，目录切换发生在执行进程之前，这可能会影响可执行文件和参数使用相对路径时的行为。

使用 [environment] 设置进程的环境变量。如果未设置，则继承父进程的环境。目前仅支持 US-ASCII 环境变量，如果传入的环境变量含有 US-ASCII 范围之外的码点，很可能会出错。

如果 [includeParentEnvironment] 为 `true`，进程的环境将包含父进程的环境，[environment] 优先。默认值为 `true`。

如果 [runInShell] 为 `true`，进程将通过系统 shell 生成。在 Linux 和 OS X 上使用 `/bin/sh`，在 Windows 上使用 `%WINDIR%\system32\cmd.exe`。

**注意**：在 Windows 上，如果 [executable] 是批处理文件（`*.bat` 或 `*.cmd`），无论 [runInShell] 的值如何，操作系统都可能会在系统 shell 中启动它。这可能导致参数按照 shell 规则被解析。例如：

```
void main() async {
  // 将启动记事本。
  Process.start('test.bat', ['test&notepad.exe']);
}
```

使用 `Process.start` 启动的进程，用户必须读取其 [stdout] 和 [stderr] 流上的所有数据。如果用户没有读取流上的所有数据，由于仍有待处理数据，底层系统资源将不会被释放。

以下代码使用 `Process.start` 在 Linux 上的 `test.dart` 文件中查找 `main`。

```dart
var process = await Process.start('grep', ['-i', 'main', 'test.dart']);
stdout.addStream(process.stdout);
stderr.addStream(process.stderr);
```

如果 [mode] 为 [ProcessStartMode.normal]（默认值），将启动一个子进程，其 `stdin`、`stdout` 和 `stderr` 与其父进程相连。只要子进程在运行，父进程就不会退出，除非父进程调用了 [exit]。如果父进程调用了 [exit]，父进程将被终止，但子进程会继续运行。

如果 `mode` 为 [ProcessStartMode.detached]，将创建一个分离的进程。分离的进程与其父进程没有连接，即使父进程终止，它也可以继续独立运行。分离进程唯一可获取的信息是其 `pid`。它的 `stdin`、`stdout` 或 `stderr` 均无连接，其退出码在终止时也不可用。

如果 `mode` 为 [ProcessStartMode.detachedWithStdio]，将创建一个分离的进程，其 `stdin`、`stdout` 和 `stderr` 保持连接。创建者可以通过这些通道与子进程通信。即使这些通信通道关闭或父进程终止，该分离进程仍将继续运行。该进程终止时其退出码不可用。

`mode` 的默认值为 `ProcessStartMode.normal`。

### run()

```dart
Future<ProcessResult> run(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, Encoding? stdoutEncoding = systemEncoding, Encoding? stderrEncoding = systemEncoding})
```

启动一个进程并非交互式地运行至完成。运行的进程为使用指定 [arguments] 的 [executable]。

建议为 [executable] 使用绝对路径，因为解析 [executable] 路径的方式是特定于平台的。在 Windows 上，出于解析 [executable] 路径的目的，[environment] 映射参数中设置的任何 `PATH` 以及 [workingDirectory] 参数中设置的路径都会被忽略。

使用 [workingDirectory] 设置进程的工作目录。请注意，在某些平台上，目录切换发生在执行进程之前，这可能会影响可执行文件和参数使用相对路径时的行为。

使用 [environment] 设置进程的环境变量。如果未设置，则继承父进程的环境。目前仅支持 US-ASCII 环境变量，如果传入的环境变量含有 US-ASCII 范围之外的码点，很可能会出错。

如果 [includeParentEnvironment] 为 `true`，进程的环境将包含父进程的环境，[environment] 优先。默认值为 `true`。

如果 [runInShell] 为 true，进程将通过系统 shell 生成。在 Linux 和 OS X 上使用 `/bin/sh`，在 Windows 上使用 `%WINDIR%\system32\cmd.exe`。

**注意**：在 Windows 上，如果 [executable] 是批处理文件（`*.bat` 或 `*.cmd`），无论 [runInShell] 的值如何，操作系统都可能会在系统 shell 中启动它。这可能导致参数按照 shell 规则被解析。例如：

```
void main() async {
  // 将启动记事本。
  await Process.run('test.bat', ['test&notepad.exe']);
}
```

用于将 `stdout` 和 `stderr` 解码为文本的编码方式由 [stdoutEncoding] 和 [stderrEncoding] 控制。默认编码为 [systemEncoding]。如果使用 `null`，则不进行解码，[ProcessResult] 将持有二进制数据。

返回一个 `Future<ProcessResult>`，以运行该进程的结果（即退出码、标准输出和标准错误）完成。

以下代码使用 `Process.run` 在 Linux 上的 `test.dart` 文件中查找 `main`。

```dart
var result = await Process.run('grep', ['-i', 'main', 'test.dart']);
stdout.write(result.stdout);
stderr.write(result.stderr);
```

### runSync()

```dart
ProcessResult runSync(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, Encoding? stdoutEncoding = systemEncoding, Encoding? stderrEncoding = systemEncoding})
```

启动一个进程并运行至完成。这是一个同步调用，将阻塞直到子进程终止。

参数与 [Process.run] 相同。

返回一个 [ProcessResult]，包含运行该进程的结果（即退出码、标准输出和标准错误）。

### killPid()

```dart
bool killPid(int pid, [ProcessSignal signal = ProcessSignal.sigterm])
```

终止 id 为 [pid] 的进程。

在可能的情况下，将 [signal] 发送给 id 为 [pid] 的进程，包括在 Linux 和 OS X 上。默认信号为 [ProcessSignal.sigterm]，通常会终止进程。

在不支持信号的平台（包括 Windows）上，此调用只会以特定于平台的方式终止 id 为 [pid] 的进程，[signal] 参数将被忽略。

如果信号成功发送到进程，则返回 `true`。否则信号无法发送，通常意味着进程已经终止。

### stdout

```dart
Stream<List<int>> get stdout
```

进程的标准输出流，以 `Stream` 形式提供。

**注意：** `stdin`、`stdout` 和 `stderr` 是通过父进程与生成的子进程之间的管道实现的。这些管道的容量是有限的。如果子进程向 stderr 或 stdout 写入的数据超过该限制而未被读取，子进程会阻塞，等待管道缓冲区接受更多数据。例如：

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // 以下 await 语句永远不会完成，因为子进程
  // 永远不会退出，因为它被阻塞在等待其 stdout
  // 被读取。
  await process.stderr.forEach(print);
}
```

### stderr

```dart
Stream<List<int>> get stderr
```

进程的标准错误流，以 `Stream` 形式提供。

**注意：** `stdin`、`stdout` 和 `stderr` 是通过父进程与生成的子进程之间的管道实现的。这些管道的容量是有限的。如果子进程向 stderr 或 stdout 写入的数据超过该限制而未被读取，子进程会阻塞，等待管道缓冲区接受更多数据。例如：

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // 以下 await 语句永远不会完成，因为子进程
  // 永远不会退出，因为它被阻塞在等待其 stdout
  // 被读取。
  await process.stderr.forEach(print);
}
```

### stdin

```dart
IOSink get stdin
```

进程的标准输入流，以 [IOSink] 形式提供。

### pid

```dart
int get pid
```

进程的进程 id。

### kill()

```dart
bool kill([ProcessSignal signal = ProcessSignal.sigterm])
```

终止该进程。

在可能的情况下，将 [signal] 发送给该进程，包括在 Linux 和 OS X 上。默认信号为 [ProcessSignal.sigterm]，通常会终止进程。

在不支持信号的平台（包括 Windows）上，此调用只会以特定于平台的方式终止进程，[signal] 参数将被忽略。

如果信号成功发送到进程，则返回 `true`。否则信号无法发送，通常意味着进程已经终止。

# ProcessResult

```dart
final class ProcessResult {}
```

使用 [Process.run] 或 [Process.runSync] 启动的非交互式进程的运行结果。

### exitCode

```dart
int exitCode
```

进程的退出码。

有关退出码值的更多信息，请参见 [Process.exitCode]。

### stdout

```dart
dynamic stdout
```

进程的标准输出。

传给 [Process.run] 的 `stdoutEncoding` 参数的值决定了该值的类型。如果使用了 `null`，此值为 [Uint8List] 类型，否则为 `String` 类型。

### stderr

```dart
dynamic stderr
```

进程的标准错误。

传给 [Process.run] 的 `stderrEncoding` 参数的值决定了该值的类型。如果使用了 `null`，此值为 [Uint8List] 类型，否则为 [String] 类型。

### pid

```dart
int pid
```

进程的进程 id。

### ProcessResult()

```dart
ProcessResult(int pid, int exitCode, dynamic stdout, dynamic stderr)
```

# ProcessSignal

```dart
interface class ProcessSignal {}
```

在 Posix 系统上，[ProcessSignal] 用于向子进程发送特定信号，参见 `Process.kill`。

某些 [ProcessSignal] 也可以被监听，作为拦截默认信号处理程序并实现另一个处理程序的方式。有关更多信息，请参见 [ProcessSignal.watch]。

### sighup

```dart
ProcessSignal sighup
```

### sigint

```dart
ProcessSignal sigint
```

### sigquit

```dart
ProcessSignal sigquit
```

### sigill

```dart
ProcessSignal sigill
```

### sigtrap

```dart
ProcessSignal sigtrap
```

### sigabrt

```dart
ProcessSignal sigabrt
```

### sigbus

```dart
ProcessSignal sigbus
```

### sigfpe

```dart
ProcessSignal sigfpe
```

### sigkill

```dart
ProcessSignal sigkill
```

### sigusr1

```dart
ProcessSignal sigusr1
```

### sigsegv

```dart
ProcessSignal sigsegv
```

### sigusr2

```dart
ProcessSignal sigusr2
```

### sigpipe

```dart
ProcessSignal sigpipe
```

### sigalrm

```dart
ProcessSignal sigalrm
```

### sigterm

```dart
ProcessSignal sigterm
```

### sigchld

```dart
ProcessSignal sigchld
```

### sigcont

```dart
ProcessSignal sigcont
```

### sigstop

```dart
ProcessSignal sigstop
```

### sigtstp

```dart
ProcessSignal sigtstp
```

### sigttin

```dart
ProcessSignal sigttin
```

### sigttou

```dart
ProcessSignal sigttou
```

### sigurg

```dart
ProcessSignal sigurg
```

### sigxcpu

```dart
ProcessSignal sigxcpu
```

### sigxfsz

```dart
ProcessSignal sigxfsz
```

### sigvtalrm

```dart
ProcessSignal sigvtalrm
```

### sigprof

```dart
ProcessSignal sigprof
```

### sigwinch

```dart
ProcessSignal sigwinch
```

### sigpoll

```dart
ProcessSignal sigpoll
```

### sigsys

```dart
ProcessSignal sigsys
```

### signalNumber

```dart
int signalNumber
```

信号的数字常量，例如在大多数平台上，[ProcessSignal.sighup] 对应的 [ProcessSignal.signalNumber] 为 1。

### name

```dart
String name
```

信号的 POSIX 标准名称，例如 [ProcessSignal.sighup] 对应的 [ProcessSignal.name] 为 "SIGHUP"。

### toString()

```dart
String toString()
```

### watch()

```dart
Stream<ProcessSignal> watch()
```

监听进程信号。

可以监听以下 [ProcessSignal]：

- [ProcessSignal.sighup]。
- [ProcessSignal.sigint]。例如由 CTRL-C 发送的信号。
- [ProcessSignal.sigterm]。在 Windows 上不可用。
- [ProcessSignal.sigusr1]。在 Windows 上不可用。
- [ProcessSignal.sigusr2]。在 Windows 上不可用。
- [ProcessSignal.sigwinch]。在 Windows 上不可用。

不允许监听其他信号，因为它们可能被 VM 使用。

一个信号可以被多次监听，来自多个 isolate，所有回调都会以不确定的顺序被调用。

# SignalException

```dart
class SignalException implements IOException {}
```

### message

```dart
String message
```

### osError

```dart
dynamic osError
```

### SignalException()

```dart
SignalException(String message, [dynamic osError])
```

### toString()

```dart
String toString()
```

# ProcessException

```dart
class ProcessException implements IOException {}
```

### executable

```dart
String executable
```

为该进程提供的可执行文件。

### arguments

```dart
List<String> arguments
```

为该进程提供的参数。

### message

```dart
String message
```

该进程异常的系统消息（如果有）。

如果没有可用的消息，则为空字符串。

### errorCode

```dart
int errorCode
```

该进程异常的操作系统错误码（如果有）。

如果没有可用的操作系统错误码，该值为零。

### ProcessException()

```dart
ProcessException(String executable, List<String> arguments, [String message = "", int errorCode = 0])
```

### toString()

```dart
String toString()
```
