与调试器、检查器等开发者工具进行交互。

这是一个专用库，用于以编程方式与 Dart 运行时交互，进行调试和检查。示例用途包括高级调试和创建开发者工具。

该库针对 Dart Web 和 Dart Native（虚拟机）分别提供了平台特定的实现。特定平台可能并不支持所有操作。

此库提供的功能通常仅在以开发模式运行的 Dart 代码中可用，例如 `dart run`，而在生产模式（例如 `dart compile exe` 的输出）中不可用。

## 调试

[debugger] 函数可用于像触发断点一样停止程序运行。断点将被设置在调用 `debugger` 之后。此功能可用于根据代码中的逻辑触发断点。

示例：

```dart template:main
var counter = 0;
final someInterestingValue = 1000;
while (true) {
  if (counter == someInterestingValue) {
    // 在调试器中触发一个断点。
    debugger();
  }
  counter++;
}
```

使用 `dart run --observe` 执行并在 DevTools 中打开时，调试器将在 `counter` 的值为 `1000` 时停止。

## 检查

连接到运行时系统的开发者工具（例如 Dart DevTools）可以在“时间线”（timeline）视图中检查执行时序。[Timeline] 的静态方法可以向该视图添加额外的信息和计时事件。

示例：

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

使用 `dart run --observe` 执行并在 DevTools 中打开时，“Performance”（性能）选项卡将显示包含传递给 `timeSync` 的注释的时间线。

## 开发者工具

开发者工具（如 `dart` 命令内置的调试器）可以使用 [Service] 和 [ServiceProtocolInfo] 类访问运行时系统暴露的正在运行的应用程序信息。

{@category Core}

# debugger()

```dart
bool debugger({bool when = true, String? message})
```

如果 [when] 为 true，则在下一条语句处像命中断点一样停止程序。

返回 [when] 的值。部分调试器可能会显示 [message]。

注意：调用时，isolate 将不会返回，直到调试器继续执行。在 Dart VM 中运行时，无论是否连接了调试器，行为都是相同的。编译为 JavaScript 时，此功能使用 “debugger” 语句，其行为与该语句完全一致。

# inspect()

```dart
Object? inspect(Object? object)
```

将 [object] 的引用发送给任何已连接的调试器。

调试器可能会为该对象打开一个检查器。返回传入的参数。

# log()

```dart
void log(String message, {DateTime? time, int? sequenceNumber, int level = 0, String name = '', Zone? zone, Object? error, StackTrace? stackTrace})
```

发出一个日志事件，可使用 DevTools 的[日志视图](https://docs.flutter.dev/tools/devtools/logging)查看。

此函数的设计旨在紧密对应 `package:logging` 收集的日志信息。

- [message] 为日志消息
- [time]（可选）为时间戳
- [sequenceNumber]（可选）为单调递增的序列号
- [level]（可选）为严重级别（0 到 2000 之间的值）；可参阅 `package:logging` 中的 `Level` 类了解可能取值的概览
- [name]（可选）为日志消息来源的名称
- [zone]（可选）为发出日志的 zone
- [error]（可选）为与此日志事件关联的错误对象
- [stackTrace]（可选）为与此日志事件关联的堆栈跟踪

# reachabilityBarrier

```dart
int get reachabilityBarrier
```

当前的可达性屏障（reachability barrier）状态。

可达性屏障状态提供了一种基于可达性进行同步的方式。当状态为 'x' 时，任何在 'value' < 'x' 期间变为不可达的对象都已被回收，且其关联的终结器（finalizer）已被调度执行，也就是说，终结器未执行可以可靠地表明该对象在之前的屏障状态下仍然可达。

在当前屏障状态下变为不可达的对象可能尚未被回收或终结。

注意：不保证有前进进度（forward progress）。具体实现可能会永远返回相同的屏障状态值。

# TimelineRecorder

```dart
enum TimelineRecorder {}
```

VM 支持的时间线记录器类型。

[Perfetto](https://ui.perfetto.dev) 基于 protobuf 的格式。

同时支持性能分析（profiling）和追踪（tracing）数据。

相关格式定义参见 [Perfetto 文档](https://perfetto.dev/docs/reference/trace-packet-proto)。

Chrome 基于 JSON 的格式，可通过 [Catapult](chrome://tracing) 查看。

仅支持追踪数据。

格式说明参见[此处](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview?tab=t.0)。

生成平台特定的时间线事件。

- 在 Linux 和 Android 上，这意味着将事件写入 [ftrace](https://docs.kernel.org/trace/ftrace.html) 缓冲区。
- 在 Mac OS X 上使用 [signposts](https://developer.apple.com/documentation/os/recording-performance-data)。
- 在 Fuchsia 上使用 [Fuchsia 追踪系统](https://fuchsia.dev/fuchsia-src/concepts/kernel/tracing-system)。
- Windows 不支持。

# TimelineStream

```dart
enum TimelineStream {}
```

可单独启用记录的特定事件集合。

对 `Dart_*` VM C API 函数的调用。

与编译为机器码相关的事件。

关于编译器各阶段的详细计时信息。

通过 [Timeline] API 创建的事件。

与调试器相关的事件。

通过 `Dart_RecordTimelineEvent` 创建的事件。

与垃圾回收和/或堆遍历相关的事件。

Isolate 和 isolate 组的生命周期事件，例如启动和关闭。

表示 `dart:async` 微任务（microtask）的事件。仅当 VM 以 `--profile-microtasks` 启动时，才会向此流填充事件。

VM 生命周期事件，例如启动和关闭。

# NativeRuntime

```dart
abstract final class NativeRuntime {}
```

原生运行时（native runtime）上可用的功能。

### buildId

```dart
String? get buildId
```

正在运行的应用程序的构建 ID（build ID）。

应用程序的构建 ID 是一个字符串，包含任意大小字节序列的十六进制表示。此字符串可用于匹配特定的提前编译（AOT）版本的应用程序，例如，用于确定在编译期间生成的哪些调试产物应当用于转换崩溃和错误报告。

构建 ID 仅在提前编译的程序中可用。如果构建 ID 不可用，则该值为 `null`。

### writeHeapSnapshotToFile()

```dart
void writeHeapSnapshotToFile(String filepath)
```

将堆的快照写入 [filepath]。

[filepath] 应为可打开写入的原生文件路径。相对路径将相对于当前工作目录解析。如果文件已存在，将被覆盖。

**警告**：仅在特定配置下的原生运行时上可用。如果此功能不可用（例如在 product 模式下、在非独立虚拟机中等），将抛出 [UnsupportedError] 错误。

注意：这是一个实验性函数。我们保留在未来更改或移除它的权利。

### streamTimelineTo()

```dart
void streamTimelineTo(TimelineRecorder recorder, {String? path, List<TimelineStream> streams = const [TimelineStream.dart, TimelineStream.gc], bool enableProfiler = false, Duration samplingInterval = const Duration(microseconds: 1000)})
```

通知运行时使用 [recorder] 写入时间线数据。

时间线记录会针对整个运行时启用，而非针对特定的 isolate 或 isolate 组。

一旦启动，时间线记录将持续进行，直到被 [stopStreamingTimeline] 停止。

部分记录器会写入指定文件（由 [path] 指定），而其他记录器则写入系统级的记录缓冲区。

[streams] 指定要启用的时间线流。默认仅启用 [TimelineStream.dart] 和 [TimelineStream.gc]。

如果 [recorder] 支持性能分析数据，将 [enableProfiler] 设置为 `true` 将启用采样分析器（sampling profiler），该分析器将以 [samplingInterval] 指定的频率收集性能分析样本。这些样本随后将被写入时间线。

在以下情况下将抛出 [ArgumentError]：

- 指定了 [path]，但 [recorder] 写入固定位置。
- 未指定 [path]，但 [recorder] 需要该参数。
- [enableProfiler] 为 `true`，但 [recorder] 不支持写出性能分析数据。
- [samplingInterval] 过小。

### stopStreamingTimeline()

```dart
void stopStreamingTimeline()
```

结束由 [streamTimelineTo] 启动的时间线数据捕获。
