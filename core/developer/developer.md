Interact with developer tools such as the debugger and inspector.

This is a specialized library intended for interacting with the Dart runtime programmatically for debugging and inspection. Sample uses include advanced debugging, and creating developer tools.

This library has platform specific implementations for Dart web and Dart Native (VM). A specific platform may not support all operations.

The functionality provided by this library is generally only available to Dart code run in development mode, e.g., `dart run`, and not in production mode, e.g., the output of `dart compile exe`.

## Debugging

The [debugger] function can be used to stop the program as if a breakpoint was hit. The breakpoint will be placed right after the call to `debugger`. This functionality can be useful for triggering breakpoints based on logic in the code.

Example:

```dart template:main
var counter = 0;
final someInterestingValue = 1000;
while (true) {
  if (counter == someInterestingValue) {
    // Trigger a breakpoint in the debugger.
    debugger();
  }
  counter++;
}
```

When executed with `dart run --observe`, and opened in DevTools, the debugger will be stopped with `counter` at the value `1000`.

## Inspection

Developer tools, such as Dart DevTools, connected to the runtime system may allow for inspecting execution timing in a "timeline" view. The static methods of [Timeline] can add extra information and timing events to this view.

Example:

```dart
void main() {
  Timeline.timeSync('Calculation loop', () {
    for (var i = 30; i < 50; i++) {
      Timeline.timeSync('fib($i)', () {
        fibonacci(i);
      });
    }
  });
}

int fibonacci(int n) => (n < 2) ? n : fibonacci(n - 2) + fibonacci(n - 1);
```

When executed with `dart run --observe`, and opened in DevTools, the Performance tab will display a timeline containing the annotations passed to `timeSync`.

## Developer tools

A developer tool, like the debugger built into the `dart` command, may access information about the running application exposed by the runtime system, using the [Service] and [ServiceProtocolInfo] classes.

{@category Core}

# debugger()

```dart
bool debugger({bool when = true, String? message})
```

If [when] is true, stop the program as if a breakpoint were hit at the following statement.

Returns the value of [when]. Some debuggers may display [message].

NOTE: When invoked, the isolate will not return until a debugger continues execution. When running in the Dart VM, the behaviour is the same regardless of whether or not a debugger is connected. When compiled to JavaScript, this uses the "debugger" statement, and behaves exactly as that does.

# inspect()

```dart
Object? inspect(Object? object)
```

Send a reference to [object] to any attached debuggers.

Debuggers may open an inspector on the object. Returns the argument.

# log()

```dart
void log(String message, {DateTime? time, int? sequenceNumber, int level = 0, String name = '', Zone? zone, Object? error, StackTrace? stackTrace})
```

Emit a log event, which can can viewed using the DevTools [Logging view](https://docs.flutter.dev/tools/devtools/logging).

This function was designed to map closely to the logging information collected by `package:logging`.

- [message] is the log message
- [time] (optional) is the timestamp
- [sequenceNumber] (optional) is a monotonically increasing sequence number
- [level] (optional) is the severity level (a value between 0 and 2000); see the `package:logging` `Level` class for an overview of the possible values
- [name] (optional) is the name of the source of the log message
- [zone] (optional) the zone where the log was emitted
- [error] (optional) an error object associated with this log event
- [stackTrace] (optional) a stack trace associated with this log event

# reachabilityBarrier

```dart
int get reachabilityBarrier
```

Current reachability barrier state.

A reachability barrier state that provides a way to synchronize on reachability. At value 'x', any object that became unreachable during 'value' < 'x' has been collected and any associated finalizers have been scheduled for execution, i.e. the non-execution of a finalizer reliably indicates the object is still reachable in the previous barrier state.

Objects that became unreachable in the current barrier state may have not yet been collected or finalized.

NOTE: There are no guarantees of forward progress. An implementation may return the same value forever for this barrier state.

# TimelineRecorder

```dart
enum TimelineRecorder {}
```

Types of timeline recorders supported by the VM.

[Perfetto](https://ui.perfetto.dev)'s protobuf based format.

Supports both profiling and tracing data.

Scheme is available in [Perfetto docs](https://perfetto.dev/docs/reference/trace-packet-proto).

Chrome's JSON based format viewable by [Catapult](chrome://tracing).

Supports only tracing data.

Scheme is described in [here](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview?tab=t.0).

Emits platform specific timeline events.

- On Linux and Android this means writing events into [ftrace](https://docs.kernel.org/trace/ftrace.html) buffer.
- On Mac OS X this uses [signposts](https://developer.apple.com/documentation/os/recording-performance-data).
- On Fuchsia it uses [Fuchsia tracing system](https://fuchsia.dev/fuchsia-src/concepts/kernel/tracing-system).
- Not supported on Windows.

# TimelineStream

```dart
enum TimelineStream {}
```

Specific sets of events whose recording can be enabled separately.

Calls to `Dart_*` VM C API functions.

Events related to compilation to machine code.

Detailed timing information about compiler phases.

Events created via [Timeline] APIs.

Events related to debugger.

Events created by `Dart_RecordTimelineEvent`.

Events related to garbage collection and/or heap iteration.

Isolate and isolate group lifecycle events such as startup and shutdown.

Events representing `dart:async` microtasks. VM will only populate this stream with events if it is started with `--profile-microtasks`.

VM lifecycle events such as startup and shutdown.

# NativeRuntime

```dart
abstract final class NativeRuntime {}
```

Functionality available on the native runtime.

### buildId

```dart
String? get buildId
```

The build ID for the running application.

The build ID of an application is a string containing a hexadecimal representation of an arbitrarily sized sequence of bytes. This string can be used to match a specific ahead-of-time compiled version of an application, for example, to determine which debugging artifacts emitted during compilation should be used to translate crash and error reports.

The build ID is only available for ahead-of-time compiled programs. If a build ID is not available, the value is `null`.

### writeHeapSnapshotToFile()

```dart
void writeHeapSnapshotToFile(String filepath)
```

Writes a snapshot of the heap to [filepath].

The [filepath] should be a native file path that can be opened for writing. Relative paths will be relative to the current working directory. If the file already exists it will be overwritten.

**WARNING**: Only works on a native runtime in certain configurations. An [UnsupportedError] error is thrown if this functionality is not available (e.g. in product mode, in non-standalone VM, ...)

NOTE: This is an experimental function. We reserve the right to change or remove it in the future.

### streamTimelineTo()

```dart
void streamTimelineTo(TimelineRecorder recorder, {String? path, List<TimelineStream> streams = const [TimelineStream.dart, TimelineStream.gc], bool enableProfiler = false, Duration samplingInterval = const Duration(microseconds: 1000)})
```

Tells runtime to write timeline data using [recorder].

Timeline recording is enabled for the whole runtime and not for any specific isolate or isolate group.

Once started timeline recording will continue until it is stopped by [stopStreamingTimeline].

Some recorders write into a specific file (specified by [path]), while others write to system wide recording buffer.

The [streams] specifies which timeline streams to enable. Only [TimelineStream.dart] and [TimelineStream.gc] are enabled by default.

If [recorder] supports profiling data then setting [enableProfiler] to `true` will turn on sampling profiler, which will collect profiling samples with frequency specified by [samplingInterval]. These samples will then written into the timeline.

Throws [ArgumentError] iff:

- [path] is specified but [recorder] writes to a fixed location.
- [path] is not specified and [recorder] requires it.
- [enableProfiler] is `true` and [recorder] does not support writing out profiling data.
- [samplingInterval] is too small.

### stopStreamingTimeline()

```dart
void stopStreamingTimeline()
```

Finishes capturing of timeline data started by [streamTimelineTo].
