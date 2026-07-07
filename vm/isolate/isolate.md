使用 _isolate_ 进行并发编程：isolate 是独立的工作单元，类似于线程，但不共享内存，仅通过消息进行通信。

_注意_：`dart:isolate` 库目前仅受 [Dart Native](https://dart.dev/overview#platform) 平台支持。

在代码中使用该库：

```dart
import 'dart:isolate';
```

{@category VM}

# IsolateSpawnException

```dart
class IsolateSpawnException implements Exception {}
```

当无法创建 isolate 时抛出。

### message

```dart
String message
```

spawn 操作报告的错误消息。

### IsolateSpawnException()

```dart
IsolateSpawnException(String message)
```

### toString()

```dart
String toString()
```

# Isolate

```dart
final class Isolate {}
```

一个隔离的 Dart 执行上下文。

所有 Dart 代码都运行在某个 isolate 中，代码只能访问同一 isolate 内的类和值。不同的 isolate 可以通过端口（port）发送值来进行通信（参见 [ReceivePort]、[SendPort]）。

`Isolate` 对象是对某个 isolate 的引用，通常不同于当前 isolate。它代表另一个 isolate，并可用于控制该 isolate。

创建新 isolate 时，如果 spawn 操作成功，发起方 isolate 会收到一个代表新 isolate 的 `Isolate` 对象。

每个 isolate 都在自己的事件循环（event loop）中运行代码，每个事件还可能在嵌套的微任务队列（microtask queue）中运行更小的任务。

`Isolate` 对象允许其他 isolate 控制它所代表的 isolate 的事件循环，并对该 isolate 进行检查，例如暂停该 isolate，或在其发生未捕获错误时获取事件通知。

[controlPort] 用于标识 isolate 并提供对其的控制权限，[pauseCapability] 和 [terminateCapability] 则用于保护部分控制操作的访问权限。例如，对一个未提供 [pauseCapability] 的 `Isolate` 对象调用 [pause] 不会产生任何效果。

由 spawn 操作提供的 `Isolate` 对象将拥有控制该 isolate 所需的控制端口和各项 capability。如有需要，也可以使用 [Isolate.new] 构造函数创建缺少部分 capability 的新 `Isolate` 对象。

`Isolate` 对象本身不能通过 `SendPort` 发送，但其控制端口和各项 capability 是可以发送的，接收端口所在的 isolate 可以利用它们创建出一个功能正常的新 `Isolate` 对象。

### immediate

```dart
int immediate
```

`ping` 和 `kill` 的参数：请求立即执行操作。

### beforeNextEvent

```dart
int beforeNextEvent
```

`ping` 和 `kill` 的参数：请求在下一个事件之前执行操作。

### controlPort

```dart
SendPort controlPort
```

用于向 isolate 发送控制消息的控制端口。

控制端口用于标识该 isolate。

`Isolate` 对象允许通过控制端口发送控制消息。

部分控制消息要求随消息一起传递特定的 capability（参见 [pauseCapability] 和 [terminateCapability]），否则该消息会被 isolate 忽略。

### pauseCapability

```dart
Capability? pauseCapability
```

授予暂停该 isolate 能力的 capability。

[pause] 需要该 capability。如果该 capability 为 `null`，或者不是 [controlPort] 所标识 isolate 的正确暂停 capability，那么调用 [pause] 将不会产生任何效果。

如果该 isolate 是以暂停状态创建的，可将此 capability 作为参数传递给 [resume] 方法，以恢复该已暂停的 isolate。

### terminateCapability

```dart
Capability? terminateCapability
```

授予终止该 isolate 能力的 capability。

[kill] 和 [setErrorsFatal] 需要该 capability。如果该 capability 为 `null`，或者不是 [controlPort] 所标识 isolate 的正确终止 capability，那么调用这些方法将不会产生任何效果。

### debugName

```dart
String? get debugName
```

为调试目的而显示的 [Isolate] 名称。

可通过 [spawn] 和 [spawnUri] 中的 `debugName` 参数进行设置。

该名称并不能唯一标识某个 isolate。同一进程中的多个 isolate 可能拥有相同的 `debugName`。

对于给定的 isolate，该值与 C 嵌入式 API 中 `Dart_DebugName` 返回的值，以及 [IsolateMirror] 中的 `debugName` 属性值相同。

### Isolate()

```dart
Isolate(SendPort controlPort, {Capability? pauseCapability, Capability? terminateCapability})
```

创建一个具有受限 capability 集合的新 [Isolate] 对象。

该端口应为某个 isolate 的控制端口，取自另一个 `Isolate` 对象。

这些 capability 应为原始 isolate 所拥有 capability 的子集。isolate 的 capability 与该 isolate 绑定，在其他任何地方均不产生效果，因此这些 capability 应与控制端口来自同一个 isolate。

也可用于根据通过 [SendPort] 发送过来的控制端口及可用的 capability 创建一个 [Isolate] 对象。

示例：

```dart
Isolate isolate = findSomeIsolate();
Isolate restrictedIsolate = Isolate(isolate.controlPort);
untrustedCode(restrictedIsolate);
```

此示例创建了一个无法用于暂停或终止该 isolate 的新 `Isolate` 对象。不受信任的代码所能做的，仅仅是检查该 isolate、查看未捕获的错误，或者获知其何时终止。

### run()

```dart
Future<R> run<R>(FutureOr<R> computation(), {String? debugName})
```

在新的 isolate 中运行 [computation] 并返回结果。

```dart
int slowFib(int n) =>
    n <= 1 ? 1 : slowFib(n - 1) + slowFib(n - 2);

// Compute without blocking current isolate.
var fib40 = await Isolate.run(() => slowFib(40));
```

如果 [computation] 是异步的（返回 `Future<R>`），那么会在新 isolate 中等待该 future 完成整个异步计算，然后再返回结果。

```dart
int slowFib(int n) =>
    n <= 1 ? 1 : slowFib(n - 1) + slowFib(n - 2);
Stream<int> fibStream() async* {
  for (var i = 0;; i++) yield slowFib(i);
}

// Returns `Future<int>`.
var fib40 = await Isolate.run(() => fibStream().elementAt(40));
```

如果 [computation] 抛出异常，该 isolate 将被终止，此函数会抛出同样的错误。

```dart import:convert
Future<int> eventualError() async {
  await Future.delayed(const Duration(seconds: 1));
  throw StateError("In a bad state!");
}

try {
  await Isolate.run(eventualError);
} on StateError catch (e, s) {
  print(e.message); // In a bad state!
  print(LineSplitter.split("$s").first); // Contains "eventualError"
}
```

任何未捕获的异步错误同样会终止计算，但会以 [RemoteError] 的形式报告，因为 [addErrorListener] 不会提供原始的错误对象。

结果通过 [exit] 发送，这意味着它会在不经复制的情况下发送到当前 isolate。

[computation] 函数及其结果（或错误）必须能够在 isolate 之间发送。不可发送的对象包括已打开的文件和套接字（详见 [SendPort.send]）。

如果 [computation] 是一个闭包，由于 Dart 实现的限制，它可能会隐式地向该 isolate 发送意外的状态。这可能导致性能问题、内存占用增加（参见 http://dartbug.com/36983），或者如果该状态中包含无法在 isolate 之间传递的对象，则会导致运行时失败。

```dart import:convert import:io

void serializeAndWrite(File f, Object o) async {
  final openFile = await f.open(mode: FileMode.append);
  Future writeNew() async {
    // Will fail with:
    // "Invalid argument(s): Illegal argument in isolate message"
    // because `openFile` is captured.
    final encoded = await Isolate.run(() => jsonEncode(o));
    await openFile.writeString(encoded);
    await openFile.flush();
    await openFile.close();
  }

  if (await openFile.position() == 0) {
    await writeNew();
  }
}
```

遇到这种情况时，可以创建一个新函数来调用 [Isolate.run]，并将所有所需状态作为参数传入。

```dart import:convert import:io

void serializeAndWrite(File f, Object o) async {
  final openFile = await f.open(mode: FileMode.append);
  Future writeNew() async {
    Future<String> encode(o) => Isolate.run(() => jsonEncode(o));
    final encoded = await encode(o);
    await openFile.writeString(encoded);
    await openFile.flush();
    await openFile.close();
  }

  if (await openFile.position() == 0) {
    await writeNew();
  }
}
```

[debugName] 仅用于为新 isolate 命名，以便调试。

### current

```dart
Isolate get current
```

代表当前 isolate 的 [Isolate] 对象。

对于使用 [current] 的代码而言，当前 isolate 即为运行该代码的 isolate。

该 isolate 对象提供了检查、暂停或终止该 isolate 所需的 capability，并允许将这些 capability 授予其他 isolate。

可以暂停当前 isolate，但如果在暂停之前 _没有_ 先将恢复其运行的能力传递给另一个 isolate，程序必然会因此挂起。

### packageConfig

```dart
Future<Uri?> get packageConfig
```

当前 isolate 的包配置文件所在位置。

如果创建该 isolate 时未指定其包配置文件，则返回值为 `null`。

否则，返回值是一个绝对 URI，指明该 isolate 包配置文件的位置。

包配置文件通常命名为 `package_config.json`，可使用 [`package:package_config`](https://pub.dev/documentation/package_config/latest/) 对其进行读取和解析。

### packageConfigSync

```dart
Uri? get packageConfigSync
```

当前 isolate 的包配置文件所在位置。

如果创建该 isolate 时未指定其包配置文件，则返回值为 `null`。

否则，返回值是一个绝对 URI，指明该 isolate 包配置文件的位置。

包配置文件通常命名为 `package_config.json`，可使用 [`package:package_config`](https://pub.dev/documentation/package_config/latest/) 对其进行读取和解析。

### resolvePackageUri()

```dart
Future<Uri?> resolvePackageUri(Uri packageUri)
```

将 `package:` URI 解析为其实际位置。

返回 [packageUri] 这个 `package:` URI 所指定文件或目录的实际位置。

如果 [packageUri] 不是 `package:` URI，则按原样返回。

如果 [packageUri] 是 `package:` URI，但当前包配置中没有该 URI 对应包名的配置，或者该 URI 无效（不以 `package:valid_package_name/` 开头），则返回 `null`。

`package:` URI 会根据包解析配置（参见 [packageConfig]）被解析为其实际位置，该配置指明了如何找到 `package:` URI 所指向文件或目录的实际位置。

`package:` URI 对应的实际位置始终是非 `package:` 的 URI，通常为 `file:` URI，也可能是 `http:` URI。

程序可能在源文件不可用的情况下运行，此时返回的 URI 可能与实际文件或目录不对应，或者为 `null`。

### resolvePackageUriSync()

```dart
Uri? resolvePackageUriSync(Uri packageUri)
```

将 `package:` URI 解析为其实际位置。

返回 [packageUri] 这个 `package:` URI 所指定文件或目录的实际位置。

如果 [packageUri] 不是 `package:` URI，则按原样返回。

如果 [packageUri] 是 `package:` URI，但当前包配置中没有该 URI 对应包名的配置，或者该 URI 无效（不以 `package:valid_package_name/` 开头），则返回 `null`。

`package:` URI 会根据包解析配置（参见 [packageConfig]）被解析为其实际位置，该配置指明了如何找到 `package:` URI 所指向文件或目录的实际位置。

`package:` URI 对应的实际位置始终是非 `package:` 的 URI，通常为 `file:` URI，也可能是 `http:` URI。

程序可能在源文件不可用的情况下运行，此时返回的 URI 可能与实际文件或目录不对应，或者为 `null`。

### spawn()

```dart
Future<Isolate> spawn<T>(void entryPoint(T message), T message, {bool paused = false, bool errorsAreFatal = true, SendPort? onExit, SendPort? onError, String? debugName})
```

创建一个与当前 isolate 共享相同代码的 isolate。

参数 [entryPoint] 指定了在新创建的 isolate 中要调用的初始函数。该入口函数会在新 isolate 中被调用，并以 [message] 作为唯一参数。

[entryPoint] 函数必须能够以单个参数进行调用，也就是说，该函数至少接受一个位置参数，且最多只有一个必需的位置参数。只要 _可以_ 仅用单个参数调用，该函数可以接受任意数量的可选参数。如果 [entryPoint] 是一个闭包，由于 Dart 实现的限制，它可能会隐式地向该 isolate 发送意外的状态。这可能导致性能问题、内存占用增加（参见 http://dartbug.com/36983），或者如果该状态中包含无法在 isolate 之间传递的对象，则会导致运行时失败。示例参见 [run]。

[message] 必须能够在 isolate 之间发送。不可发送的对象包括已打开的文件和套接字（详见 [SendPort.send]）。初始 [message] 通常包含一个 [SendPort]，以便发起方与被创建方之间能够相互通信。

如果 [paused] 参数设为 `true`，该 isolate 将在以 [message] 调用 [entryPoint] 函数之前，以暂停状态启动，如同初始时调用了 `isolate.pause(isolate.pauseCapability)` 一样。要恢复该 isolate，需调用 `isolate.resume(isolate.pauseCapability)`。

如果提供了 [errorsAreFatal]、[onExit] 和/或 [onError] 参数，该 isolate 的行为将分别如同在其开始运行之前，已使用相应参数调用并处理了 [setErrorsFatal]、[addOnExitListener] 和 [addErrorListener]。

如果提供了 [debugName]，新创建的 [Isolate] 将可以在调试器和日志中通过该名称进行识别。

如果省略 [errorsAreFatal]，平台可能会选择默认行为，或继承当前 isolate 的行为。

也可以在返回的 isolate 上调用 [setErrorsFatal]、[addOnExitListener] 和 [addErrorListener] 方法，但除非该 isolate 是以 [paused] 状态启动的，否则在这些方法执行完成之前，它可能已经终止。

如果创建成功，返回的 future 将以一个 [Isolate] 实例完成；否则将以错误完成。

一个 isolate 的基础内存开销大约在 30 kb 量级。

### spawnUri()

```dart
Future<Isolate> spawnUri(Uri uri, List<String> args, dynamic message, {bool paused = false, SendPort? onExit, SendPort? onError, bool errorsAreFatal = true, bool? checked, Map<String, String>? environment, Uri? packageRoot, Uri? packageConfig, bool automaticPackageResolution = false, String? debugName})
```

创建一个运行 [uri] 所指定脚本文件的 isolate。

创建并启动一个新 isolate，运行以 [uri] 文件作为入口点（提供 `main` 方法）的 Dart 程序。新 isolate 属于一个新的 [isolate group][](isolate 组)，与发起方 isolate 所属的 isolate 组不同。

[isolate group]: https://dart.dev/language/concurrency#performance-and-isolate-groups

该 isolate 开始执行具有给定 URI 的库中的顶层 `main` 函数。

目标 `main` 必须能够以零个、一个或两个参数调用。示例：

- `main()`
- `main(args)`
- `main(args, message)`

如果存在参数 `args`，会将其设为提供的 [args] 列表。如果存在参数 `message`，会将其设为初始 [message]。如果 [message] 参数的运行时类型无法赋值给 `main` 方法的第二个参数，将发生运行时错误。

如果 [paused] 参数设为 `true`，该 isolate 将以暂停状态启动，如同初始时调用了 `isolate.pause(isolate.pauseCapability)` 一样。要恢复该 isolate，需调用 `isolate.resume(isolate.pauseCapability)`。

如果提供了 [errorsAreFatal]、[onExit] 和/或 [onError] 参数，该 isolate 的行为将分别如同在其开始运行之前，已使用相应参数调用并处理了 [setErrorsFatal]、[addOnExitListener] 和 [addErrorListener]。

也可以在返回的 isolate 上调用 [setErrorsFatal]、[addOnExitListener] 和 [addErrorListener] 方法，但除非该 isolate 是以 [paused] 状态启动的，否则在这些方法执行完成之前，它可能已经终止。

[checked] 参数用于控制是否在新创建的 isolate 中启用断言（assert）。在生产模式（production-mode）程序中，该参数会被忽略，所有 isolate 中的断言始终处于禁用状态。否则，如果提供了 `true` 或 `false` 值，则在新创建的 isolate 中相应地启用或禁用断言。如果提供 `null` 或未提供该参数，则新创建的 isolate 中启用断言当且仅当发起方 isolate 中也启用了断言。

`checked` 参数并非总能得到遵循。如果 isolate 代码已被预编译，则可能无法动态更改 checked 模式的设置，此时 `checked` 参数会被忽略。

如果提供了 [packageConfig] 参数，则会使用它来查找新创建 isolate 的包解析配置文件的位置。

如果提供了 [automaticPackageResolution] 参数，则会自动确定新创建 isolate 中包源代码的位置。

[environment] 是一个字符串到字符串的映射，新创建的 isolate 在查找 [String.fromEnvironment] 值时会使用它。系统也可能向该 environment 中添加自己的条目。如果省略 `environment`，则新创建的 isolate 将拥有与发起方 isolate 相同的环境声明。

警告：[environment] 参数尚未在所有平台上实现。

如果提供了 [debugName]，新创建的 [Isolate] 将可以在调试器和日志中通过该名称进行识别。

如果创建成功，返回的 future 将以一个 [Isolate] 实例完成；否则将以错误完成。

### pause()

```dart
Capability pause([Capability? resumeCapability])
```

请求该 isolate 暂停。

当该 isolate 收到暂停命令后，会停止处理事件循环队列中的事件。它可能仍会因定时器或接收端口消息等而向队列中添加新事件。当该 isolate 恢复运行后，会开始处理已入队的事件。

暂停请求通过该 isolate 的命令端口发送，绕过接收方 isolate 的事件循环。暂停会在收到请求时立即生效，将事件循环暂停在收到请求那一刻的状态。

[resumeCapability] 用于标识此次暂停，之后必须使用同一个 capability 通过 [resume] 结束该次暂停。如果省略 [resumeCapability]，则会创建一个新的 capability 对象并使用它。

如果使用同一个 capability 对某个 isolate 多次暂停，则只需用该 capability 调用一次 resume 即可结束暂停。

如果某个 isolate 是使用多个不同的 capability 分别暂停的，则必须逐一结束每次暂停，isolate 才会恢复运行。

返回必须用于结束此次暂停的 capability。该值要么是 [resumeCapability]，要么是在省略 [resumeCapability] 时创建的新 capability。

如果 [pauseCapability] 为 `null`，或者不是 [controlPort] 所标识 isolate 的暂停 capability，接收方 isolate 将忽略该暂停请求。

### resume()

```dart
void resume(Capability resumeCapability)
```

恢复一个已暂停的 isolate。

向某个 isolate 发送消息，请求其结束此前请求的暂停。

当所有有效的暂停请求都被取消后，该 isolate 将继续处理事件并处理正常的消息。

如果 [resumeCapability] 不是此前用于暂停该 isolate 的 capability，或者它已被用于从该次暂停中恢复，则此次 resume 调用不会产生任何效果。

### addOnExitListener()

```dart
void addOnExitListener(SendPort responsePort, {Object? response})
```

请求在该 isolate 终止时向 [responsePort] 发送一条退出消息。

该 isolate 在终止之前的最后一步，会将 [response] 作为消息发送到 [responsePort]。消息发送完毕后，将不再运行任何代码。

多次添加同一个端口，只会使其收到一条退出消息，且使用的是最后一次添加的 response 值；同时只需调用一次 [removeOnExitListener] 即可将其移除。

如果该 isolate 在能够收到此请求之前就已终止，则不会发送任何退出消息。

[response] 对象必须遵循与 [SendPort.send] 在向另一个 isolate 组中的 isolate 发送消息时相同的限制；只允许使用可发送给所有 isolate 的简单值，例如 `null`、布尔值、数字或字符串。

由于各 isolate 是并发运行的，该 isolate 有可能在退出监听器建立之前就已退出，此时不会向 [responsePort] 发送任何响应。为避免此情况，可以使用 spawn 函数对应的参数，或者以暂停状态启动该 isolate、添加监听器后再将其恢复运行。

### removeOnExitListener()

```dart
void removeOnExitListener(SendPort responsePort)
```

停止监听来自该 isolate 的退出消息。

请求该 isolate 不再向 [responsePort] 发送退出消息。如果该 isolate 原本就不会向 [responsePort] 发送退出消息——因为该端口未通过 [addOnExitListener] 添加，或已被移除——则此请求会被忽略。

如果同一个端口曾多次通过 [addOnExitListener] 传入，只需调用一次 `removeOnExitListener` 即可使其停止接收退出消息。

关闭与 [responsePort] 关联的接收端口，并不会阻止该 isolate 发送未捕获的错误，这些错误只是会因此丢失。

如果该 isolate 在此请求被接收和处理之前就已终止，仍可能会发送退出消息。

### setErrorsFatal()

```dart
void setErrorsFatal(bool errorsAreFatal)
```

设置未捕获的错误是否会终止该 isolate。

如果错误被设置为致命的，任何未捕获的错误都会终止该 isolate 的事件循环，并关闭该 isolate。

此调用需要该 isolate 的 [terminateCapability]。如果该 capability 缺失或不正确，则不会做出任何更改。

由于各 isolate 是并发运行的，接收方 isolate 有可能在通过此方法发出的请求被接收和处理之前，就因错误而退出。为避免此情况，可以使用 spawn 函数对应的参数，或者以暂停状态启动该 isolate、将错误设为非致命后再将其恢复运行。

### kill()

```dart
void kill({int priority = beforeNextEvent})
```

请求该 isolate 关闭。

请求该 isolate 自行终止。[priority] 参数指定此操作必须在何时发生。

如果提供了 [priority]，其值必须为 [immediate] 或 [beforeNextEvent]（默认值）之一。关闭操作会根据优先级在不同的时刻执行：

- `immediate`：该 isolate 会尽快关闭。控制消息按顺序处理，因此此前从该 isolate 发送的所有控制事件都会先被处理完毕。关闭时间不会晚于使用 `beforeNextEvent` 时的关闭时间；如果系统能够在更早的时刻（甚至在执行另一个事件的过程中）进行干净的关闭，关闭也可能提前发生。
- `beforeNextEvent`：关闭操作会被安排在控制权下一次返回到接收方 isolate 的事件循环时执行，即在当前事件以及所有已排定的控制事件都执行完毕之后。

如果 [terminateCapability] 为 `null`，或者不是 [controlPort] 所标识 isolate 的终止 capability，接收方 isolate 将忽略该 kill 请求。

### ping()

```dart
void ping(SendPort responsePort, {Object? response, int priority = immediate})
```

请求该 isolate 将 [response] 发送到 [responsePort]。

[response] 对象必须遵循与 [SendPort.send] 在向另一个 isolate 组中的 isolate 发送消息时相同的限制；只允许使用可发送给所有 isolate 的简单值，例如 `null`、布尔值、数字或字符串。

如果该 isolate 处于存活状态，它最终会在响应端口上发送 `response`（默认为 `null`）。

[priority] 必须为 [immediate] 或 [beforeNextEvent] 之一。响应的发送时机取决于 ping 的类型：

- `immediate`：该 isolate 在收到控制消息后立即响应。这发生在同一 isolate 之前的任何控制消息都已被接收和处理之后，但可能是在执行另一个事件的过程中发生。
- `beforeNextEvent`：响应会被安排在控制权下一次返回到接收方 isolate 的事件循环时发送，即在当前事件以及所有已排定的控制事件都执行完毕之后。

### addErrorListener()

```dart
void addErrorListener(SendPort port)
```

请求将该 isolate 的未捕获错误发送回 [port]。

错误会以包含两个元素的列表形式发送回来。第一个元素是该错误的 `String` 表示形式，通常通过对该错误调用 `toString` 得到。第二个元素是随附堆栈跟踪的 `String` 表示形式，如果未提供堆栈跟踪则为 `null`。若需将其转换回 [StackTrace] 对象，可使用 [StackTrace.fromString]。

使用同一端口多次进行监听不会产生任何效果。一个端口对每个错误只会收到一次，也只需调用一次 [removeErrorListener] 即可将其移除。

关闭与该端口关联的接收端口，并不会阻止该 isolate 发送未捕获的错误，这些错误只是会因此丢失。若要停止在 [port] 上接收错误，应使用 [removeErrorListener]。

由于各 isolate 是并发运行的，该 isolate 有可能在错误监听器建立之前就已退出。为避免此情况，可以以暂停状态启动该 isolate、添加监听器后再将其恢复运行。

### removeErrorListener()

```dart
void removeErrorListener(SendPort port)
```

停止监听来自该 isolate 的未捕获错误。

请求该 isolate 不再向 [port] 发送未捕获的错误。如果该 isolate 原本就不会向 [port] 发送未捕获的错误——因为该端口未通过 [addErrorListener] 添加，或已被移除——则此请求会被忽略。

如果同一个端口曾多次通过 [addErrorListener] 传入，只需调用一次 `removeErrorListener` 即可使其停止接收未捕获的错误。

在此请求被接收和处理之前，该 isolate 仍可能发送未捕获错误消息。

### errors

```dart
Stream get errors
```

返回一个广播流（broadcast stream），包含来自该 isolate 的未捕获错误。

每个错误都会作为流上的一个错误事件提供。

实际的错误对象和堆栈跟踪不一定与该 isolate 中的实际对象类型相同，但它们的 [Object.toString] 结果始终一致。

此流基于 [addErrorListener] 和 [removeErrorListener] 实现。

### exit()

```dart
Never exit([SendPort? finalMessagePort, Object? message])
```

同步终止当前 isolate。

此操作具有潜在危险性，应谨慎使用。该 isolate 会 _立即_ 停止运行。如果可选参数 [message] 不符合 isolate 之间可发送内容的限制（详见 [SendPort.send]），则会抛出异常。如果 [finalMessagePort] 关联的是通过 [spawnUri] 在当前 isolate 组之外创建的 isolate，同样会抛出异常。关于 isolate 组的更多详情，请参见 [isolate group](https://dart.dev/language/concurrency#performance-and-isolate-groups)。

如果成功，此方法的调用将不会返回。待执行的 `finally` 块不会被执行，控制流不会回到事件循环，已排定的异步任务永远不会运行，甚至待处理的 isolate 控制命令也可能被忽略。（该 isolate 仍会向通过 [Isolate.addOnExitListener] 注册的端口发送消息，但不会再运行任何 Dart 代码。）

如果提供了 [finalMessagePort]，且 [message] 能够通过该端口发送（详见 [SendPort.send]），则该消息会作为当前 isolate 的最后一步操作通过该端口发送。该 [SendPort.send] 调用返回后，该 isolate 立即终止。

如果该端口是原生端口——即由 [ReceivePort.sendPort] 或 [RawReceivePort.sendPort] 提供的端口——系统可能会以比正常存活 isolate 间端口通信更高效的方式发送这条最终消息。在这种情况下，这条最终消息的对象图会在不经复制的情况下被重新分配给接收方 isolate。此外，在大多数情况下，接收方 isolate 能够以常数时间接收该消息。

### runSync()

```dart
R runSync<R>(R Function() f)
```

在给定 isolate 的上下文中执行给定的函数。

如果目标 isolate 正在运行，此函数会抛出异常。

如果目标 isolate 已被固定（pin）到另一个线程，从而无法从当前线程进入，会抛出错误。参见 [pinToCurrentThread] 和 [isPinnedToCurrentThread]。

如果目标 isolate 属于另一个 isolate 组，会抛出错误。

如果 [f] 不是深度不可变（deeply immutable）的，会抛出错误。

如果 [f] 返回的结果不是深度不可变的，会抛出错误。

### create()

```dart
Isolate create({String? debugName})
```

在当前 isolate 组中创建一个新的 isolate。

类似于 Dart VM C API 中的 `Dart_CreateIsolateInGroup`。

该 isolate 已被创建，但其事件循环尚未运行。

要开始处理该 isolate 的消息：

- 通过调用 [Isolate.runEventLoopSync]，在当前线程上同步启动该 isolate 的事件循环；
- 通过注册事件回调（[Isolate.onEvent]）将事件通知转发给外部事件循环，并从该外部事件循环中处理待处理事件（[Isolate.handleEvent]），从而将该 isolate 的事件循环与外部事件循环集成。

### shutdownSync()

```dart
void shutdownSync()
```

关闭目标 isolate。

关闭该 isolate 会在不处理任何待处理消息的情况下停止其事件循环，并关闭该 isolate 拥有的所有已打开的接收端口。

此函数会阻塞，直到获得对目标 isolate 的独占访问权限。只有当没有其他线程正在目标 isolate 中执行代码时，才能在其事件循环的两次轮转之间进入该 isolate 进行同步执行。

### pinToCurrentThread()

```dart
bool pinToCurrentThread()
```

将当前 isolate 固定到当前操作系统线程。

一旦某个 isolate 被固定到某个操作系统线程，其他任何操作系统线程都无法进入该 isolate。从另一个线程尝试获取对其的独占访问权限将会失败并报错。

等价于 Dart VM C API 中的 `Dart_SetCurrentThreadOwnsIsolate`。

成功时返回 `true`，否则返回 `false`（例如目标 isolate 已被固定到另一个线程时）。

### isPinnedToCurrentThread

```dart
bool get isPinnedToCurrentThread
```

该 isolate 是否已被固定到当前操作系统线程。

等价于 Dart VM C API 中的 `Dart_GetCurrentThreadOwnsIsolate`。

### runEventLoopSync()

```dart
void runEventLoopSync()
```

在当前线程上同步运行目标 isolate 的事件循环。

此函数会阻塞，直到获得对目标 isolate 的独占访问权限。只有当没有其他线程正在目标 isolate 中执行代码时，才能在其事件循环的两次轮转之间进入该 isolate 进行同步执行。

一旦该 isolate 没有任何保持存活（keep-alive）的已打开接收端口，此函数将返回。

该 isolate 将被标记为固定到当前线程。

类似于 Dart VM C API 中的 `Dart_RunLoop`，但与 `Dart_RunLoop` 不同的是，此函数会在当前线程上执行该 isolate 的事件循环，而不是将其委托给线程池。

如果目标 isolate 已被固定到另一个线程，或已有事件循环在运行，会抛出错误。

### onEvent

```dart
void set onEvent(void Function(Isolate) callback)
```

该 isolate 的事件通知回调。

每当该 isolate 需要响应一个新事件时，提供的回调都会被调用一次。之后可以通过调用 [Isolate.handleEvent] 来处理待处理的事件。

提供的 [callback] 必须是深度不可变的，且会在任意线程上被调用，未必处于任何 isolate 之内。参见 [NativeCallable.isolateGroupBound]。

重要提示：_不得_ 在 `callback` 中调用 [Isolate.handleEvent]，否则会导致 Dart 执行环境发生死锁。

类似于 Dart VM C API 中的 `Dart_SetMessageNotifyCallback`。

### handleEvent()

```dart
void handleEvent()
```

最多处理该 isolate 的一个待处理事件。

如果没有待处理事件，此函数不执行任何操作。

此函数会阻塞，直到获得对目标 isolate 的独占访问权限。只有当没有其他线程正在目标 isolate 中执行代码时，才能在其事件循环的两次轮转之间进入该 isolate 进行同步执行。

类似于 Dart VM C API 中的 `Dart_HandleMessage`。

# SendPort

```dart
abstract interface class SendPort implements Capability {}
```

向其对应的 [ReceivePort] 发送消息。

[SendPort] 由 [ReceivePort] 创建。通过某个 [SendPort] 发送的任何消息都会传递给其对应的 [ReceivePort]。同一个 [ReceivePort] 可能对应多个 [SendPort]。

[SendPort] 可以被传递给其他 isolate，并且在发送过程中保持相等性。

### send()

```dart
void send(Object? message)
```

通过此发送端口，向其对应的 [ReceivePort] 发送一条异步 [message]。

如果发送方和接收方 isolate 不共享相同的代码（使用 [Isolate.spawnUri] 创建的 isolate 不与创建它的 isolate 共享代码），则 [message] 的传递闭包对象图 **只能** 包含以下几类对象：

- `null`
- `true` 和 `false`
- [int]、[double]、[String] 的实例
- 通过列表、映射和集合字面量创建的实例
- 由以下构造函数创建的实例：[List]、[Map]、[LinkedHashMap]、[Set] 和 [LinkedHashSet]；[TransferableTypedData]；[Capability]
- 来自 [ReceivePort.sendPort] 或 [RawReceivePort.sendPort] 的 [SendPort] 实例，其中相应的接收端口是通过这些类的构造函数创建的
- 表示上述类型之一、`Object`、`dynamic`、`void` 和 `Never` 的 [Type] 实例，以及所有这些类型的可空变体。对于泛型类型，其类型参数必须是可发送的类型，整个类型才可发送。

如果发送方和接收方 isolate 共享相同的代码（例如通过 [Isolate.spawn] 创建的 isolate），[message] 的传递闭包对象图可以包含任意对象，但以下情况除外：

- 具有原生资源的对象（例如 `NativeFieldWrapperClass1` 的子类）。例如，[Socket] 对象内部引用了附带原生资源的对象，因此无法被发送。
- [ReceivePort]
- [DynamicLibrary]
- [Finalizable]
- [Finalizer]
- [NativeFinalizer]
- [UserTag]
- `MirrorReference`

凡是本身标注了 `@pragma('vm:isolate-unsendable')` 的类，或继承、实现了此类类的类，其实例都无法通过端口发送。

除上述例外情况外，任何对象都可以发送。被判定为不可变的对象（例如字符串）会被共享，而其他所有对象都会被复制。

发送操作会立即进行，复制传递闭包对象图可能会产生与其大小呈线性关系的时间开销。发送操作本身不会阻塞（即不会等待接收方收到消息）。只要对应接收端口所在 isolate 的事件循环准备好投递消息，该端口便可以接收到消息，这与发送方 isolate 当时在做什么无关。

注意：由于 Dart VM 在闭包表示已捕获状态方面所做的实现选择，闭包目前可能会捕获比实际所需更多的状态，这可能导致传递闭包比所需的更大。相关未解决问题参见：http://dartbug.com/36983

### operator ==()

```dart
bool operator ==(dynamic other)
```

测试 [other] 是否为指向与当前对象相同 [ReceivePort] 的 [SendPort]。

### hashCode

```dart
int get hashCode
```

该发送端口的哈希码，与 `==` 运算符保持一致。

# ReceivePort

```dart
abstract interface class ReceivePort implements Stream<dynamic> {}
```

与 [SendPort] 一起，构成 isolate 之间通信的唯一方式。

[ReceivePort] 具有一个 `sendPort` 获取器，返回一个 [SendPort]。通过该 [SendPort] 发送的任何消息都会传递给创建它的那个 [ReceivePort]，并在那里被分派给该 `ReceivePort` 的监听器。

[ReceivePort] 是一个非广播流（non-broadcast stream）。这意味着在注册监听器之前，它会缓冲收到的消息。只能有一个监听器接收消息。若要将该端口转换为广播流，请参见 [Stream.asBroadcastStream]。

一个 [ReceivePort] 可以对应多个 [SendPort]。

### ReceivePort()

```dart
ReceivePort([String debugName = ''])
```

打开一个长期存在的端口用于接收消息。

[ReceivePort] 是一个非广播流。这意味着在注册监听器之前，它会缓冲收到的消息。只能有一个监听器接收消息。若要将该端口转换为广播流，请参见 [Stream.asBroadcastStream]。

可设置可选的 `debugName` 参数，为该端口关联一个可在工具中显示的名称。

通过取消其订阅来关闭一个接收端口。

### ReceivePort.fromRawReceivePort()

```dart
ReceivePort.fromRawReceivePort(RawReceivePort rawPort)
```

根据一个 [RawReceivePort] 创建一个 [ReceivePort]。

在构造结果对象的过程中，给定 [rawPort] 的处理器（handler）会被覆盖。

### listen()

```dart
StreamSubscription<dynamic> listen(void onData(dynamic message), {Function? onError, void onDone(), bool? cancelOnError})
```

监听来自 [Stream] 的事件。

参见 [Stream.listen]。

请注意，由于 [ReceivePort] 永远不会收到错误，[onError] 和 [cancelOnError] 会被忽略。

流关闭时会调用 [onDone] 处理器。调用 [close] 时流会关闭。

### close()

```dart
void close()
```

关闭该接收端口。

该接收端口将不再接收任何事件，也不会再作为流事件发出。

如果已调用 [listen] 且 [StreamSubscription] 尚未被取消，该订阅将随一个 "done" 事件一起关闭。

如果该流已被取消，此方法不会产生任何效果。

### sendPort

```dart
SendPort get sendPort
```

一个向该接收端口发送消息的 [SendPort]。

# RawReceivePort

```dart
abstract interface class RawReceivePort {}
```

一个低级别的异步消息接收器。

[RawReceivePort] 是一个低级别特性，不感知 [Zone]。[handler] 始终会在 [Zone.root] 区域（zone）中被调用。

该端口无法被暂停。数据处理器必须在收到第一条消息之前设置好，否则该消息会丢失。

可以使用 [sendPort] 向该端口发送消息。

### RawReceivePort()

```dart
RawReceivePort([Function? handler, String debugName = ''])
```

打开一个长期存在的端口用于接收消息。

[RawReceivePort] 是低级别的，不支持 [Zone]。它无法被暂停。数据处理器必须在收到第一条消息之前设置好，否则该消息会丢失。

如果提供了 [handler]，会将其设为 [RawReceivePort.handler]。

可设置可选的 `debugName` 参数，为该端口关联一个可在工具中显示的名称。

### handler

```dart
void set handler(Function? newHandler)
```

设置每条收到的消息都会调用的处理器。

该处理器会在 [Zone.root] 区域中被调用。如果需要在当前区域中调用该处理器，可执行以下操作：

```dart import:async
rawPort.handler = Zone.current.bindCallback(actualHandler);
```

该处理器必须是一个能够接受一个参数的函数，该参数类型与发送到此端口的消息类型一致。这意味着，如果已知所有消息都将是 [String] 类型，则可以使用类型为 `void Function(String)` 的处理器。该函数会以实际消息动态调用，如果此次调用失败，该错误会成为 [Zone.root] 区域中的顶层未捕获错误。

### close()

```dart
void close()
```

关闭该端口。

调用此方法后，任何传入的消息都会被静默丢弃，[handler] 也不会再被调用。

### sendPort

```dart
SendPort get sendPort
```

返回一个向此原始接收端口发送消息的 [SendPort]。

### keepIsolateAlive

```dart
bool keepIsolateAlive
```

该 [RawReceivePort] 是否使其所属的 [Isolate] 保持存活。

默认情况下，接收端口会使创建它们的 [Isolate] 保持存活，直到调用 [close] 为止。如果 [keepIsolateAlive] 设为 `false`，即使该端口仍处于打开状态，该 isolate 也可能关闭。当该 isolate 关闭时，该端口也会关闭，此后通过 [sendPort] 发送的消息都会被忽略。

# RemoteError

```dart
final class RemoteError implements Error {}
```

来自另一个 isolate 的错误描述。

此错误在 `toString()` 和 `stackTrace.toString()` 方面与原始错误行为一致，但不具备原始错误的其他特性。

### stackTrace

```dart
StackTrace stackTrace
```

### RemoteError()

```dart
RemoteError(String description, String stackDescription)
```

### toString()

```dart
String toString()
```

# TransferableTypedData

```dart
abstract final class TransferableTypedData {}
```

一个可高效传输的字节值序列。

[TransferableTypedData] 由若干字节创建而成，此过程所需时间与字节数成正比。

[TransferableTypedData] 可以在 isolate 之间移动，因此通过发送端口发送它只需要常数时间。

以这种方式发送后，本地的可传输对象将无法再被具体化（materialize），此后只能通过接收到的对象来具体化该数据。

### TransferableTypedData.fromList()

```dart
TransferableTypedData.fromList(List<TypedData> list)
```

创建一个新的 [TransferableTypedData]，其中包含 [list] 的字节内容。

必须能够创建出一个包含这些字节的单一 [Uint8List]；因此，如果字节数超过了平台对单一 [Uint8List] 所允许的上限，则创建会失败。

### materialize()

```dart
ByteBuffer materialize()
```

创建一个新的 [ByteBuffer]，其中包含存储在此 [TransferableTypedData] 中的字节。

[TransferableTypedData] 是一种跨 isolate 的一次性资源。即使调用发生在不同的 isolate 中，也不得对同一底层可传输字节多次调用此方法。
