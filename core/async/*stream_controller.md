# ControllerCallback

```dart
typedef ControllerCallback = void Function()
```

流控制器的 `onListen`、`onPause` 和 `onResume` 回调的类型。

---

# ControllerCancelCallback

```dart
typedef ControllerCancelCallback = FutureOr<void> Function()
```

流控制器 `onCancel` 回调的类型。

---

# StreamController

```dart
abstract interface class StreamController<T> implements StreamSink<T> {}
```

一个控制器，包含它所控制的流。

此控制器允许在其 [stream] 上发送数据、错误和完成（done）事件。

这个类可用于创建一个简单的流，供其他对象监听，并可向该流推送事件。

可以检查该流是否处于暂停状态，是否有订阅者，以及在这些状态发生变化时获取回调通知。

示例：

```dart
final streamController = StreamController(
  onPause: () => print('Paused'),
  onResume: () => print('Resumed'),
  onCancel: () => print('Cancelled'),
  onListen: () => print('Listens'),
);

streamController.stream.listen(
  (event) => print('Event: $event'),
  onDone: () => print('Done'),
  onError: (error) => print(error),
);
```

要检查流是否有订阅者，请使用 [hasListener]。

```dart continued
var hasListener = streamController.hasListener; // true
```

要向流发送数据事件，请使用 [add] 或 [addStream]。

```dart continued
streamController.add(999);
final stream = Stream<int>.periodic(
    const Duration(milliseconds: 200), (count) => count * count).take(4);
await streamController.addStream(stream);
```

要向流发送错误事件，请使用 [addError] 或 [addStream]。

```dart continued
streamController.addError(Exception('Issue 101'));
await streamController.addStream(Stream.error(Exception('Issue 404')));
```

要检查流是否已关闭，请使用 [isClosed]。

```dart continued
var isClosed = streamController.isClosed; // false
```

要关闭流，请使用 [close]。

```dart continued
await streamController.close();
isClosed = streamController.isClosed; // true
```

## 构造函数

### StreamController()

```dart
StreamController({
  void onListen(),
  void onPause(),
  void onResume(),
  FutureOr<void> onCancel(),
  bool sync = false
})
```

一个仅支持单个订阅者的 [stream] 控制器。

如果 [sync] 为 true，返回的流控制器将是一个 [SynchronousStreamController]，使用时必须格外小心，以免破坏 [Stream] 的约定。如果有疑问，请使用非 sync 版本。

使用异步控制器永远不会产生错误的行为，但错误地使用同步控制器可能会导致原本正确的程序出现问题。

同步控制器仅用于在一个异步事件立即触发另一个事件时优化事件传播。除非能保证对 [add] 或 [addError] 的调用发生在不会破坏 `Stream` 约定的位置，否则不应使用它。

在有订阅者开始监听流之前（通过 [onListen] 回调开始产生事件），流应保持“惰性（inert）”状态。当没有任何用户监听流时，流不应泄漏资源（如 websocket）。

在有订阅者注册之前，控制器会缓冲所有传入的事件，但此功能应仅在极少数情况下使用。

当流变为暂停状态时，会调用 [onPause] 函数；当流恢复时，会调用 [onResume]。

当流接收到其监听器时，会调用 [onListen] 回调；当监听器结束其订阅时，会调用 [onCancel]。如果 [onCancel] 需要执行异步操作，[onCancel] 应返回一个在取消操作完成时完成的 future。

如果在控制器需要数据之前流被取消，[onResume] 调用可能不会被执行。

### StreamController.broadcast()

```dart
StreamController.broadcast({
  void onListen(),
  void onCancel(),
  bool sync = false
})
```

一个 [stream] 可被多次监听的控制器。

此处 [stream] 返回的 [Stream] 是一个广播流（broadcast stream）。它可以被多次监听。

在有订阅者开始监听流之前（通过 [onListen] 回调开始产生事件），流应保持惰性状态。当没有任何用户监听流时，流不应泄漏资源（如 websocket）。

广播流在没有监听器时不会缓冲事件。

控制器会在调用 [add]、[addError] 或 [close] 时，将事件分发给当时所有已订阅的监听器。不允许在前一次调用尚未返回之前调用 `add`、`addError` 或 `close`。控制器不会维护任何内部事件队列，如果在添加事件或错误时没有监听器，该事件会被直接丢弃。

每个监听器订阅都被独立处理，如果其中一个暂停，只有该暂停的监听器会受到影响。暂停的监听器会在内部缓冲事件，直到恢复或取消。

如果 [sync] 为 true，事件可能会在 [add]、[addError] 或 [close] 调用期间由流的订阅直接触发。返回的流控制器将是一个 [SynchronousStreamController]，使用时必须格外小心，以免破坏 [Stream] 的约定。有关何时可以使用同步分发的说明，请参见 [Completer.sync]。如果有疑问，请保持控制器为非 sync 模式。

如果 [sync] 为 false，事件将始终在稍后的时间触发，即添加该事件的代码执行完毕之后。在这种情况下，无法保证多个监听器获取事件的时间顺序，只能保证每个监听器都会以正确的顺序获得所有事件。每个订阅都会单独处理事件。如果在一个异步控制器上向两个监听器发送两个事件，其中一个监听器可能会在另一个监听器获得任何事件之前就获得这两个事件。监听器必须在事件发起时（即调用 [add] 时）以及事件后续被传递时都处于订阅状态，才能接收到该事件。

[onListen] 回调会在第一个监听器订阅时被调用，[onCancel] 会在不再有任何活跃监听器时被调用。如果在 [onCancel] 被调用之后又添加了监听器，[onListen] 会被再次调用。

## 属性

### stream

```dart
Stream<T> get stream
```

此控制器所控制的流。

### onListen

```dart
void Function()? onListen
```

当流被监听时调用的回调。

可以设置为 `null`，此时不会发生任何回调。

### onPause

```dart
void Function()? onPause
```

当流暂停时调用的回调。

可以设置为 `null`，此时不会发生任何回调。

暂停相关的回调在广播流控制器上不受支持。

### onResume

```dart
void Function()? onResume
```

当流恢复时调用的回调。

可以设置为 `null`，此时不会发生任何回调。

暂停相关的回调在广播流控制器上不受支持。

### onCancel

```dart
FutureOr<void> Function()? onCancel
```

当流被取消时调用的回调。

可以设置为 `null`，此时不会发生任何回调。

### sink

```dart
StreamSink<T> get sink
```

返回此对象的一个视图，该视图仅暴露 [StreamSink] 接口。

### isClosed

```dart
bool get isClosed
```

流控制器是否已关闭，无法再添加更多事件。

调用 [close] 方法后，控制器将变为关闭状态。此后无法再通过 [add] 或 [addError] 向已关闭的控制器添加新事件。

如果控制器已关闭，“done”事件可能尚未被传递，但已被安排好，此时再添加事件已经太迟了。

### isPaused

```dart
bool get isPaused
```

订阅是否需要缓冲事件。

如果控制器的流有监听器且该监听器处于暂停状态，或者该流尚未收到监听器，则属于这种情况。在这些情况下，控制器也被视为处于暂停状态。

广播流控制器永远不会被视为处于暂停状态。它总是将事件转发给所有未取消的订阅（如果有的话），并让各个订阅自行处理其暂停和缓冲。

### hasListener

```dart
bool get hasListener
```

[Stream] 上是否有订阅者。

### done

```dart
Future get done
```

当流控制器完成事件发送时完成的 future。

这种情况发生在 done 事件已发送，或者单订阅流的订阅者被取消时。

在 done 事件发送时存在的所有监听器都停止监听之前，流控制器不会完成返回的 future。如果监听器被取消，或者它已经处理了 done 事件，则该监听器就会停止监听。暂停的监听器在恢复之前不会处理 done 事件，因此返回的 Future 的完成会被延迟，直到所有暂停的监听器都被恢复或取消为止。

如果非广播流上没有监听器，或者监听器暂停后从未恢复，done 事件将不会被发送，此 future 也将永远不会完成。

## 方法

### add()

```dart
void add(T event)
```

发送一个数据事件 [event]。

监听器会在稍后的一个微任务中收到此事件。

请注意，同步控制器（通过向 `StreamController` 构造函数的 `sync` 参数传入 true 创建）会立即传递事件。由于这种行为违反了此处提到的约定，同步控制器只应按照文档中的说明使用，以确保所传递的事件*看起来*总是像是在一个单独的微任务中传递的。

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

发送或排队一个错误事件。

监听器会在稍后的一个微任务中收到此事件。使用 `sync` 控制器可以覆盖这一行为。但请注意，同步控制器必须满足其构造函数文档中提到的前提条件。

### close()

```dart
Future close()
```

关闭流。

关闭后的流不能再添加任何事件。

返回的 future 与 [done] 提供的 future 相同。当流的监听器完成事件发送时，它就会完成。这种情况发生在 done 事件已发送，或者单订阅流的订阅者被取消时。

在 done 事件发送时存在的所有监听器都停止监听之前，流控制器不会完成返回的 future。如果监听器被取消，或者它已经处理了 done 事件，则该监听器就会停止监听。暂停的监听器在恢复之前不会处理 done 事件，因此返回的 Future 的完成会被延迟，直到所有暂停的监听器都被恢复或取消为止。

如果没有人监听非广播流，或者监听器暂停后从未恢复，done 事件将不会被发送，此 future 也将永远不会完成。

### addStream()

```dart
Future addStream(
  Stream<T> source, {
  bool? cancelOnError
})
```

接收来自 [source] 的事件，并将其放入此控制器的流中。

返回一个 future，当源流结束时完成。

在返回的 future 完成之前，不得直接使用 [add]、[addError]、[close] 或 [addStream] 向此控制器添加事件。

数据事件和错误事件会被转发到此控制器的流中。source 上的 done 事件会结束 `addStream` 操作，并完成返回的 future。

如果 [cancelOnError] 为 `true`，则只有 [source] 上的第一个错误会被转发到控制器的流中，之后 `addStream` 就会结束。如果 [cancelOnError] 为 false，则所有错误都会被转发，只有 done 事件才会结束 `addStream`。如果省略 [cancelOnError] 或其为 `null`，默认为 `false`。

---

# SynchronousStreamController

```dart
abstract interface class SynchronousStreamController<T> implements StreamController<T> {}
```

一个以同步方式传递事件的流控制器。

同步流控制器适用于已经异步的事件触发流上的事件这种情形。

事件不会在稍后的一个微任务中被添加到流中（这会造成额外的延迟），而是由同步流控制器立即触发，就好像该流事件是当前事件或微任务的一部分。

同步流控制器可用于打破 [Stream] 的约定，因此必须谨慎使用，以避免真正破坏该约定。

相比普通的 [StreamController]，使用 [SynchronousStreamController] 的唯一优势是改善了延迟。只有在这种改善很显著、且使用是安全的情况下，才应使用同步版本。否则应使用普通的流控制器，它对于 [Stream] 而言总能表现出正确的行为，也不会意外破坏其他代码。

向同步控制器添加事件应仅作为原始事件处理过程的最后一部分进行。此时，向流中添加事件等价于返回事件循环并在下一个微任务中添加该事件。

每个监听器回调都会被当作顶层事件或微任务来运行。这意味着如果它抛出异常，该错误会尽快被报告为未捕获错误。这也是为什么应将事件的添加放在原始事件处理器最后执行的一个原因——在添加事件之后执行的任何操作都会延迟对监听器回调中错误的报告。

如果在并非已知为另一个事件的情境中添加了一个事件，可能会导致流的监听器在尚未准备好处理该事件时就收到它。我们承诺，在调用 [Stream.listen] 之后，在执行该监听调用的代码执行完毕之前，你不会收到任何事件。在来源不明的函数调用中响应式地调用 [add] 可能会破坏这一承诺。

来自控制器的 [onListen] 回调*不是*一个异步事件，在 `onListen` 回调中向控制器添加事件总是错误的。这些事件会在监听器甚至还未收到订阅之前就被传递。

同步广播流控制器还有一个普通流控制器所没有的限制：在事件正在被传递期间，不得调用 [add]、[addError]、[close] 和 [addStream] 方法。也就是说，如果控制器流上某个订阅的回调导致调用了上述任何一个函数，该调用将会失败。一个广播流可能有多个监听器，如果在另一个事件也正在被添加的过程中同步地添加了一个事件，后一个事件可能会先于前一个事件到达某些监听器。为防止这种情况，在前一个事件正在被添加时不能添加新的事件。这保证了当第一个 [add]、[addError] 或 [close] 返回时，该事件已经被完整地传递，后续事件也将按正确的顺序被传递。

这仅仅保证事件被传递到订阅。如果订阅处于暂停状态，实际的回调可能仍会延后发生，此时事件会被订阅缓冲。除了暂停的情形，以及随之而来的尚未传递的已缓冲事件之外，添加事件时回调通常会被同步调用。

在另一个事件正在处理过程中，向同步的单订阅流控制器添加事件，可能会导致第二个事件被延迟，无法同步传递，直到该事件被传递之前，控制器都不会以同步方式运作。

## 方法

### add()

```dart
void add(T data)
```

将事件添加到控制器的流中。

与 [StreamController.add] 相同，但不得在 [add]、[addError] 或 [close] 正在添加事件的过程中调用。

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

将错误添加到控制器的流中。

与 [StreamController.addError] 相同，但不得在 [add]、[addError] 或 [close] 正在添加事件的过程中调用。

### close()

```dart
Future close()
```

关闭控制器的流。

与 [StreamController.close] 相同，但不得在 [add]、[addError] 或 [close] 正在添加事件的过程中调用。
