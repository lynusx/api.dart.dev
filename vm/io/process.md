# exit()

```dart
Never exit(int code)
```

Exit the Dart VM process immediately with the given exit code.

This does not wait for any asynchronous operations to terminate nor execute `finally` blocks. Using [exit] is therefore very likely to lose data.

Child processes are not explicitly terminated (but they may terminate themselves when they detect that their parent has exited).

While debugging, the VM will not respect the `--pause-isolates-on-exit` flag if [exit] is called as invoking this method causes the Dart VM process to shutdown immediately. To properly break on exit, consider calling [debugger] from `dart:developer` or [Isolate.pause] from `dart:isolate` on [Isolate.current] to pause the isolate before invoking [exit].

The handling of exit codes is platform specific.

On Linux and OS X an exit code for normal termination will always be in the range `[0..255]`. If an exit code outside this range is set the actual exit code will be the lower 8 bits masked off and treated as an unsigned value. E.g. using an exit code of -1 will result in an actual exit code of 255 being reported.

On Windows the exit code can be set to any 32-bit value. However some of these values are reserved for reporting system errors like crashes.

Besides this the Dart executable itself uses an exit code of `254` for reporting compile time errors and an exit code of `255` for reporting runtime error (unhandled exception).

Due to these facts it is recommended to only use exit codes in the range \[0..127\] for communicating the result of running a Dart program to the surrounding environment. This will avoid any cross-platform issues.

# exitCode

```dart
void set exitCode(int code)
```

Set the global exit code for the Dart VM.

The exit code is global for the Dart VM and the last assignment to exitCode from any isolate determines the exit code of the Dart VM on normal termination.

Default value is `0`.

See [exit] for more information on how to chose a value for the exit code.

# exitCode

```dart
int get exitCode
```

Get the global exit code for the Dart VM.

The exit code is global for the Dart VM and the last assignment to exitCode from any isolate determines the exit code of the Dart VM on normal termination.

See [exit] for more information on how to chose a value for the exit code.

# sleep()

```dart
void sleep(Duration duration)
```

Sleep for the duration specified in [duration].

Use this with care, as no asynchronous operations can be processed in a isolate while it is blocked in a [sleep] call.

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

Returns the PID of the current process.

# ProcessInfo

```dart
abstract final class ProcessInfo {}
```

Methods for retrieving information about the current process.

### currentRss

```dart
int get currentRss
```

The current resident set size of memory for the process, in bytes.

Note that the meaning of this field is platform dependent. For example, some memory accounted for here may be shared with other processes, or if the same page is mapped into a process's address space, it may be counted twice.

### maxRss

```dart
int get maxRss
```

The high-watermark in bytes for the resident set size of memory for the process.

Note that the meaning of this field is platform dependent. For example, some memory accounted for here may be shared with other processes, or if the same page is mapped into a process's address space, it may be counted twice.

# ProcessStartMode

```dart
final class ProcessStartMode {}
```

Modes for running a new process.

### normal

```dart
dynamic normal
```

Normal child process.

### inheritStdio

```dart
dynamic inheritStdio
```

Stdio handles are inherited by the child process.

### detached

```dart
dynamic detached
```

Detached child process with no open communication channel.

### detachedWithStdio

```dart
dynamic detachedWithStdio
```

Detached child process with stdin, stdout and stderr still open for communication with the child.

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

The means to execute a program.

Use the static [start] and [run] methods to start a new process. The run method executes the process non-interactively to completion. In contrast, the start method allows your code to interact with the running process.

## Start a process with the run method

The following code sample uses the run method to create a process that runs the UNIX command `ls`, which lists the contents of a directory. The run method completes with a [ProcessResult] object when the process terminates. This provides access to the output and exit code from the process. The run method does not return a `Process` object; this prevents your code from interacting with the running process.

```dart
import 'dart:io';

main() async {
  // List all files in the current directory in UNIX-like systems.
  var result = await Process.run('ls', ['-l']);
  print(result.stdout);
}
```

## Start a process with the start method

The following example uses start to create the process. The start method returns a [Future] for a `Process` object. When the future completes the process is started and your code can interact with the process: writing to stdin, listening to stdout, and so on.

The following sample starts the UNIX `cat` utility, which when given no command-line arguments, echos its input. The program writes to the process's standard input stream and prints data from its standard output stream.

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

## Standard I/O streams

As seen in the previous code sample, you can interact with the `Process`'s standard output stream through the getter [stdout], and you can interact with the `Process`'s standard input stream through the getter [stdin]. In addition, `Process` provides a getter [stderr] for using the `Process`'s standard error stream.

A `Process`'s streams are distinct from the top-level streams for the current program.

**NOTE:** `stdin`, `stdout`, and `stderr` are implemented using pipes between the parent process and the spawned subprocess. These pipes have limited capacity. If the subprocess writes to stderr or stdout in excess of that limit without the output being read, the subprocess blocks waiting for the pipe buffer to accept more data. For example:

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // The following await statement will never complete because the
  // subprocess never exits since it is blocked waiting for its
  // stdout to be read.
  await process.stderr.forEach(print);
}
```

## Exit codes

Call the [exitCode] method to get the exit code of the process. The exit code indicates whether the program terminated successfully (usually indicated with an exit code of 0) or with an error.

If the start method is used, the [exitCode] is available through a future on the `Process` object (as shown in the example below). If the run method is used, the [exitCode] is available through a getter on the [ProcessResult] instance.

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

A `Future` which completes with the exit code of the process when the process completes.

The exit code is not available for processes running with [ProcessStartMode.detached] or [ProcessStartMode.detachedWithStdio] and the getter will throw [StateError] if it is used.

The handling of exit codes is platform specific.

On Linux and OS X a normal exit code will be a positive value in the range `[0..255]`. If the process was terminated due to a signal the exit code will be a negative value in the range `[-255..-1]`, where the absolute value of the exit code is the signal number. For example, if a process crashes due to a segmentation violation the exit code will be -11, as the signal SIGSEGV has the number 11.

On Windows a process can report any 32-bit value as an exit code. When returning the exit code this exit code is turned into a signed value. Some special values are used to report termination due to some system event. E.g. if a process crashes due to an access violation the 32-bit exit code is `0xc0000005`, which will be returned as the negative number `-1073741819`. To get the original 32-bit value use `(0x100000000 + exitCode) & 0xffffffff`.

There is no guarantee that [stdout] and [stderr] have finished reporting the buffered output of the process when the returned future completes. To be sure that all output is captured, wait for the done event on the streams.

### start()

```dart
Future<Process> start(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, ProcessStartMode mode = ProcessStartMode.normal})
```

Starts a process running the [executable] with the specified [arguments].

Returns a `Future<Process>` that completes with a [Process] instance when the process has been successfully started. That [Process] object can be used to interact with the process. If the process cannot be started the returned [Future] completes with an exception.

Using an absolute path for [executable] is recommended since resolving the [executable] path is platform-specific. On Windows, both any `PATH` set in the [environment] map parameter and the path set in [workingDirectory] parameter are ignored for the purposes of resolving the [executable] path.

Use [workingDirectory] to set the working directory for the process. Note that the change of directory occurs before executing the process on some platforms, which may have impact when using relative paths for the executable and the arguments.

Use [environment] to set the environment variables for the process. If not set the environment of the parent process is inherited. Currently, only US-ASCII environment variables are supported and errors are likely to occur if an environment variable with code-points outside the US-ASCII range is passed in.

If [includeParentEnvironment] is `true`, the process's environment will include the parent process's environment, with [environment] taking precedence. Default is `true`.

If [runInShell] is `true`, the process will be spawned through a system shell. On Linux and OS X, `/bin/sh` is used, while `%WINDIR%\system32\cmd.exe` is used on Windows.

**NOTE**: On Windows, if [executable] is a batch file ('_.bat' or '_.cmd'), it may be launched by the operating system in a system shell regardless of the value of [runInShell]. This could result in arguments being parsed according to shell rules. For example:

```
void main() async {
  // Will launch notepad.
  Process.start('test.bat', ['test&notepad.exe']);
}
```

Users must read all data coming on the [stdout] and [stderr] streams of processes started with `Process.start`. If the user does not read all data on the streams the underlying system resources will not be released since there is still pending data.

The following code uses `Process.start` to grep for `main` in the file `test.dart` on Linux.

```dart
var process = await Process.start('grep', ['-i', 'main', 'test.dart']);
stdout.addStream(process.stdout);
stderr.addStream(process.stderr);
```

If [mode] is [ProcessStartMode.normal] (the default) a child process will be started with `stdin`, `stdout` and `stderr` connected to its parent. The parent process will not exit so long as the child is running, unless [exit] is called by the parent. If [exit] is called by the parent then the parent will be terminated but the child will continue running.

If `mode` is [ProcessStartMode.detached] a detached process will be created. A detached process has no connection to its parent, and can keep running on its own when the parent dies. The only information available from a detached process is its `pid`. There is no connection to its `stdin`, `stdout` or `stderr`, nor will the process' exit code become available when it terminates.

If `mode` is [ProcessStartMode.detachedWithStdio] a detached process will be created where the `stdin`, `stdout` and `stderr` are connected. The creator can communicate with the child through these. The detached process will keep running even if these communication channels are closed or the parent dies. The process' exit code will not become available when it terminated.

The default value for `mode` is `ProcessStartMode.normal`.

### run()

```dart
Future<ProcessResult> run(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, Encoding? stdoutEncoding = systemEncoding, Encoding? stderrEncoding = systemEncoding})
```

Starts a process and runs it non-interactively to completion. The process run is [executable] with the specified [arguments].

Using an absolute path for [executable] is recommended since resolving the [executable] path is platform-specific. On Windows, both any `PATH` set in the [environment] map parameter and the path set in [workingDirectory] parameter are ignored for the purposes of resolving the [executable] path.

Use [workingDirectory] to set the working directory for the process. Note that the change of directory occurs before executing the process on some platforms, which may have impact when using relative paths for the executable and the arguments.

Use [environment] to set the environment variables for the process. If not set the environment of the parent process is inherited. Currently, only US-ASCII environment variables are supported and errors are likely to occur if an environment variable with code-points outside the US-ASCII range is passed in.

If [includeParentEnvironment] is `true`, the process's environment will include the parent process's environment, with [environment] taking precedence. Default is `true`.

If [runInShell] is true, the process will be spawned through a system shell. On Linux and OS X, `/bin/sh` is used, while `%WINDIR%\system32\cmd.exe` is used on Windows.

**NOTE**: On Windows, if [executable] is a batch file ('_.bat' or '_.cmd'), it may be launched by the operating system in a system shell regardless of the value of [runInShell]. This could result in arguments being parsed according to shell rules. For example:

```
void main() async {
  // Will launch notepad.
  await Process.run('test.bat', ['test&notepad.exe']);
}
```

The encoding used for decoding `stdout` and `stderr` into text is controlled through [stdoutEncoding] and [stderrEncoding]. The default encoding is [systemEncoding]. If `null` is used no decoding will happen and the [ProcessResult] will hold binary data.

Returns a `Future<ProcessResult>` that completes with the result of running the process, i.e., exit code, standard out and standard error.

The following code uses `Process.run` to grep for `main` in the file `test.dart` on Linux.

```dart
var result = await Process.run('grep', ['-i', 'main', 'test.dart']);
stdout.write(result.stdout);
stderr.write(result.stderr);
```

### runSync()

```dart
ProcessResult runSync(String executable, List<String> arguments, {String? workingDirectory, Map<String, String>? environment, bool includeParentEnvironment = true, bool runInShell = false, Encoding? stdoutEncoding = systemEncoding, Encoding? stderrEncoding = systemEncoding})
```

Starts a process and runs it to completion. This is a synchronous call and will block until the child process terminates.

The arguments are the same as for [Process.run].

Returns a [ProcessResult] with the result of running the process, i.e., exit code, standard out and standard error.

### killPid()

```dart
bool killPid(int pid, [ProcessSignal signal = ProcessSignal.sigterm])
```

Kills the process with id [pid].

Where possible, sends the [signal] to the process with id [pid]. This includes Linux and OS X. The default signal is [ProcessSignal.sigterm] which will normally terminate the process.

On platforms without signal support, including Windows, the call just terminates the process with id [pid] in a platform specific way, and the [signal] parameter is ignored.

Returns `true` if the signal is successfully delivered to the process. Otherwise the signal could not be sent, usually meaning that the process is already dead.

### stdout

```dart
Stream<List<int>> get stdout
```

The standard output stream of the process as a `Stream`.

**NOTE:** `stdin`, `stdout`, and `stderr` are implemented using pipes between the parent process and the spawned subprocess. These pipes have limited capacity. If the subprocess writes to stderr or stdout in excess of that limit without the output being read, the subprocess blocks waiting for the pipe buffer to accept more data. For example:

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // The following await statement will never complete because the
  // subprocess never exits since it is blocked waiting for its
  // stdout to be read.
  await process.stderr.forEach(print);
}
```

### stderr

```dart
Stream<List<int>> get stderr
```

The standard error stream of the process as a `Stream`.

**NOTE:** `stdin`, `stdout`, and `stderr` are implemented using pipes between the parent process and the spawned subprocess. These pipes have limited capacity. If the subprocess writes to stderr or stdout in excess of that limit without the output being read, the subprocess blocks waiting for the pipe buffer to accept more data. For example:

```dart
import 'dart:io';

main() async {
  var process = await Process.start('cat', ['largefile.txt']);
  // The following await statement will never complete because the
  // subprocess never exits since it is blocked waiting for its
  // stdout to be read.
  await process.stderr.forEach(print);
}
```

### stdin

```dart
IOSink get stdin
```

The standard input stream of the process as an [IOSink].

### pid

```dart
int get pid
```

The process id of the process.

### kill()

```dart
bool kill([ProcessSignal signal = ProcessSignal.sigterm])
```

Kills the process.

Where possible, sends the [signal] to the process. This includes Linux and OS X. The default signal is [ProcessSignal.sigterm] which will normally terminate the process.

On platforms without signal support, including Windows, the call just terminates the process in a platform specific way, and the [signal] parameter is ignored.

Returns `true` if the signal is successfully delivered to the process. Otherwise the signal could not be sent, usually meaning that the process is already dead.

# ProcessResult

```dart
final class ProcessResult {}
```

The result of running a non-interactive process started with [Process.run] or [Process.runSync].

### exitCode

```dart
int exitCode
```

Exit code for the process.

See [Process.exitCode] for more information in the exit code value.

### stdout

```dart
dynamic stdout
```

Standard output from the process.

The value used for the `stdoutEncoding` argument to [Process.run] determines the type. If `null` was used, this value is of type [Uint8List] otherwise it is of type `String`.

### stderr

```dart
dynamic stderr
```

Standard error from the process.

The value used for the `stderrEncoding` argument to [Process.run] determines the type. If `null` was used, this value is of type [Uint8List] otherwise it is of type [String].

### pid

```dart
int pid
```

Process id of the process.

### ProcessResult()

```dart
ProcessResult(int pid, int exitCode, dynamic stdout, dynamic stderr)
```

# ProcessSignal

```dart
interface class ProcessSignal {}
```

On Posix systems, [ProcessSignal] is used to send a specific signal to a child process, see `Process.kill`.

Some [ProcessSignal]s can also be watched, as a way to intercept the default signal handler and implement another. See [ProcessSignal.watch] for more information.

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

The numeric constant for the signal e.g. [ProcessSignal.signalNumber] will be 1 for [ProcessSignal.sighup] on most platforms.

### name

```dart
String name
```

The POSIX-standardized name of the signal e.g. [ProcessSignal.name] will be "SIGHUP" for [ProcessSignal.sighup].

### toString()

```dart
String toString()
```

### watch()

```dart
Stream<ProcessSignal> watch()
```

Watch for process signals.

The following [ProcessSignal]s can be listened to:

- [ProcessSignal.sighup].
- [ProcessSignal.sigint]. Signal sent by e.g. CTRL-C.
- [ProcessSignal.sigterm]. Not available on Windows.
- [ProcessSignal.sigusr1]. Not available on Windows.
- [ProcessSignal.sigusr2]. Not available on Windows.
- [ProcessSignal.sigwinch]. Not available on Windows.

Other signals are disallowed, as they may be used by the VM.

A signal can be watched multiple times, from multiple isolates, where all callbacks are invoked when signaled, in no specific order.

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

The executable provided for the process.

### arguments

```dart
List<String> arguments
```

The arguments provided for the process.

### message

```dart
String message
```

The system message for the process exception, if any.

The empty string if no message was available.

### errorCode

```dart
int errorCode
```

The OS error code for the process exception, if any.

The value is zero if no OS error code was available.

### ProcessException()

```dart
ProcessException(String executable, List<String> arguments, [String message = "", int errorCode = 0])
```

### toString()

```dart
String toString()
```
