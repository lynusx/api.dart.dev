# Stream

```dart
abstract mixin class Stream<T> {}
```

异步数据事件的来源。

Stream 提供了一种接收事件序列的方式。每个事件要么是数据事件（也称为流的一个*元素*），要么是错误事件（表示发生了某种失败的通知）。当一个流已经发出了它的所有事件后，会有一个单独的“done”事件通知监听者已经到达末尾。

你可以通过调用 `async*` 函数来生成一个流，该函数会返回一个流。消费这个流会导致该函数持续发出事件直到结束，然后流关闭。你可以使用 `await for` 循环（仅在 `async` 或 `async*` 函数内部可用）来消费一个流，或者在 `async*` 函数内部使用 `yield*` 直接转发它的事件。示例：

```dart
Stream<T> optionalMap<T>(
    Stream<T> source , [T Function(T)? convert]) async* {
  if (convert == null) {
    yield* source;
  } else {
    await for (var event in source) {
      yield convert(event);
    }
  }
}
```

当调用此函数时，它会立即返回一个 `Stream<T>` 对象。此后不会发生任何事情，直到有人尝试消费这个流。此时，`async*` 函数体才开始运行。如果省略了 `convert` 函数，`yield*` 将监听 `source` 流，并将所有事件（数据和错误）转发到返回的流。当 `source` 流关闭时，`yield*` 完成，`optionalMap` 函数体也随之结束，这会关闭返回的流。如果提供了 `convert`，函数则改为监听 source 流并进入一个 `await for` 循环，该循环反复等待下一个数据事件。在数据事件到来时，它调用 `convert` 处理该值，并将结果发送到返回的流上。如果 `source` 流没有发出任何错误事件，循环会在 `source` 流结束时结束，然后 `optionalMap` 函数体完成，从而关闭返回的流。当 `source` 流发出错误事件时，`await for` 会重新抛出该错误，从而中断循环。由于该错误未被捕获，它会到达 `optionalMap` 函数体的末尾。这会使该错误在返回的流上发出，然后该流关闭。

`Stream` 类还提供了一些功能，允许你手动监听来自流的事件，或将一个流转换为另一个流或 Future。

[forEach] 函数对应于 `await for` 循环，正如 [Iterable.forEach] 对应于普通的 `for`/`in` 循环一样。与该循环一样，它会为每个数据事件调用一个函数，并在出现错误时中断。

更底层的 [listen] 方法是其他所有方法的基础。你在流上调用 `listen`，告诉它你想要接收事件，并注册用于接收这些事件的回调函数。调用 `listen` 时，你会得到一个 [StreamSubscription] 对象，它是提供事件的活动对象，可用于停止监听，或临时暂停来自该订阅的事件。

流有两种：“单订阅（single-subscription）”流和“广播（broadcast）”流。

_单订阅流_ 在其整个生命周期内只允许一个监听者。它在没有监听者之前不会开始产生事件，并且在监听者取消订阅后就会停止发送事件，即使事件源仍然可以提供更多事件。由 `async*` 函数创建的流是单订阅流，但每次调用该函数都会创建一个新的这样的流。

对单订阅流监听两次是不允许的，即使第一个订阅已经被取消也是如此。

单订阅流通常用于流式传输较大的连续数据块，例如文件 I/O。

_广播流_ 允许任意数量的监听者，并且它会在事件就绪时立即触发，无论当前是否有监听者。

广播流用于独立的事件/观察者场景。

如果多个监听者想要监听同一个单订阅流，可以使用 [asBroadcastStream] 在这个非广播流之上创建一个广播流。

无论是哪种流，像 [where] 和 [skip] 这样的流转换方法，都会返回与调用该方法的流相同类型的流，除非另有说明。

当一个事件被触发时，当时的监听者将会收到该事件。如果在事件触发过程中向广播流添加了一个监听者，该监听者不会收到当前正在触发的事件。如果某个监听者被取消，它会立即停止接收事件。可以将监听广播流视为监听一个新的流，该流只包含在调用 [listen] 时尚未发出的事件。例如，[first] 获取器会监听该流，然后返回该监听者收到的第一个事件。这不一定是流发出的第一个事件，而是广播流*剩余*事件中的第一个。

当“done”事件触发时，订阅者会在收到该事件之前被取消订阅。事件发送后，该流将不再有订阅者。此后仍然可以向广播流添加新的订阅者，但它们只会尽快收到一个新的“done”事件。

流订阅始终遵守“暂停”请求。如果有必要，它们需要缓冲输入；但通常情况下，且更推荐的做法是，它们可以直接请求其输入源也暂停。

[isBroadcast] 的默认实现返回 `false`。继承自 [Stream] 的广播流必须重写 [isBroadcast]，使其返回 `true`，以表明自己表现得像一个广播流。

## 构造函数

### Stream()

```dart
const Stream<T>()
```

### Stream.empty()

```dart
const Stream<T>.empty({
	@Since.new("3.2") bool broadcast,
})
```

创建一个空的广播流。

这是一个在被监听时，除了发送一个 done 事件之外什么都不做的流。

示例：

```dart
const stream = Stream.empty();
stream.listen(
  (value) {
    throw "Unreachable";
  },
  onDone: () {
    print('Done');
  },
);
```

该流默认是广播流，正如 [isBroadcast] 所报告的那样。可以通过将 [broadcast] 参数（默认值为 `true`）设为 `false` 来更改此值。

无论该流是否报告自身为广播流，都可以被监听多次。

### Stream.value()

```dart
Stream<T>.value( T value )
```

创建一个在关闭前发出单个数据事件的流。

该流发出一个值为 [value] 的数据事件，然后以一个 done 事件关闭。

示例：

```dart
Future<void> printThings(Stream<String> data) async {
  await for (var x in data) {
    print(x);
  }
}
printThings(Stream<String>.value('ok')); // 输出 "ok"。
```

返回的流实际上等价于由 `(() async* { yield value; } ())` 或 `Future<T>.value(value).asStream()` 创建的流。

### Stream.error()

```dart
Stream<T>.error(Object error, [StackTrace? stackTrace])
```

创建一个在完成前发出单个错误事件的流。

该流发出一个包含 [error] 和 [stackTrace] 的单个错误事件，然后以一个 done 事件完成。

示例：

```dart
Future<void> tryThings(Stream<int> data) async {
  try {
    await for (var x in data) {
      print('Data: $x');
    }
  } catch (e) {
    print(e);
  }
}
tryThings(Stream<int>.error('Error')); // 输出 "Error"。
```

返回的流实际上等价于由 `Future<T>.error(error, stackTrace).asStream()` 或 `(() async* { throw error; } ())` 创建的流，只是你可以自行控制堆栈跟踪信息。

### Stream.fromFuture()

```dart
Stream<T>.fromFuture(Future<T> future)
```

从给定的 future 创建一个新的单订阅流。

当该 future 完成时，流会触发一个事件（数据或错误），然后以一个 done 事件关闭。

示例：

```dart
Future<String> futureTask() async {
  await Future.delayed(const Duration(seconds: 5));
  return 'Future complete';
}

final stream = Stream<String>.fromFuture(futureTask());
stream.listen(print,
    onDone: () => print('Done'), onError: print);

// 输出：
// 'futureTask' 完成后输出 "Future complete"。
// 流完成后输出 "Done"。
```

### Stream.fromFutures()

```dart
Stream<T>.fromFutures(Iterable<Future<T>> futures)
```

从一组 future 创建一个单订阅流。

该流按照这些 future 完成的顺序在流上报告其结果。每个 future 根据其完成方式提供一个数据事件或一个错误事件。

如果在调用 `Stream.fromFutures` 时某些 future 已经完成，它们的结果将按未指定的顺序发出。

当所有 future 都已完成时，流关闭。

如果 [futures] 为空，流会尽快关闭。

示例：

```dart
Future<int> waitTask() async {
  await Future.delayed(const Duration(seconds: 2));
  return 10;
}

Future<String> doneTask() async {
  await Future.delayed(const Duration(seconds: 5));
  return 'Future complete';
}

final stream = Stream<Object>.fromFutures([doneTask(), waitTask()]);
stream.listen(print, onDone: () => print('Done'), onError: print);

// 输出：
// 'waitTask' 完成后输出 10。
// 'doneTask' 完成后输出 "Future complete"。
// 流完成后输出 "Done"。
```

### Stream.fromIterable()

```dart
Stream<T>.fromIterable(Iterable<T> elements)
```

创建一个从 [elements] 获取数据的流。

该可迭代对象在流收到监听者时开始迭代，并在监听者取消订阅时，或者 [Iterator.moveNext] 方法返回 `false` 或抛出异常时，停止迭代。当流订阅被暂停时，迭代也会暂停。

如果在 `elements.iterator` 上调用 [Iterator.moveNext] 时抛出异常，流会发出该错误然后关闭。如果在 `elements.iterator` 上读取 [Iterator.current] 时抛出异常，流会发出该错误，但会继续迭代。

可以被监听多次。每个监听者都独立地迭代 [elements]。

示例：

```dart
final numbers = [1, 2, 3, 5, 6, 7];
final stream = Stream.fromIterable(numbers);
```

### Stream.multi()

```dart
Stream<T>.multi(
  void Function(MultiStreamController<T>) onListen, {
  bool isBroadcast = false
})
```

创建一个多订阅流。

每次监听所创建的流时，都会以一个新的 [MultiStreamController] 调用 [onListen] 回调，该控制器将事件转发给该次 [listen] 调用所返回的 [StreamSubscription]。

这使得每个监听者都可以被当作一个独立的流来处理。

[MultiStreamController] 不支持读取其 [StreamController.stream]。设置其 [StreamController.onListen] 没有任何效果，因为会改为调用 [onListen] 回调，且之后不会再调用 [StreamController.onListen]。该控制器的行为类似于异步控制器，但提供了额外的方法用于同步地传递事件。

如果 [isBroadcast] 设置为 `true`，则返回的流的 [Stream.isBroadcast] 将为 `true`。这不会影响流的行为，是否表现得像广播流取决于 [onListen] 函数。

多订阅流可以表现得像任何其他流。如果 [onListen] 回调在第一次调用之后的每次调用都抛出异常，该流的表现就像单订阅流。如果流向所有当前监听者发出相同的事件，则表现得像广播流。

它也可以选择向不同的监听者发出不同的事件。例如，一个向新监听者重复发送最近一次非 `null` 事件的流，可以按如下示例实现：

```dart
extension StreamRepeatLatestExtension<T extends Object> on Stream<T> {
  Stream<T> repeatLatest() {
    var done = false;
    T? latest = null;
    var currentListeners = <MultiStreamController<T>>{};
    this.listen((event) {
      latest = event;
      for (var listener in [...currentListeners]) listener.addSync(event);
    }, onError: (Object error, StackTrace stack) {
      for (var listener in [...currentListeners]) listener.addErrorSync(error, stack);
    }, onDone: () {
      done = true;
      latest = null;
      for (var listener in currentListeners) listener.closeSync();
      currentListeners.clear();
    });
    return Stream.multi((controller) {
      if (done) {
        controller.close();
        return;
      }
      currentListeners.add(controller);
      var latestValue = latest;
      if (latestValue != null) controller.add(latestValue);
      controller.onCancel = () {
        currentListeners.remove(controller);
      };
    });
  }
}
```

### Stream.periodic()

```dart
Stream<T>.periodic(
  Duration period, [
  T Function(int)? computation
])
```

创建一个每隔 [period] 时间间隔重复发出事件的流。

事件值通过调用 [computation] 来计算。该回调的参数是一个整数，从 0 开始，每次事件递增。

[period] 必须是一个非负的 [Duration]。

如果省略 [computation]，事件值将全部为 `null`。

如果事件类型 [T] 不允许 `null` 作为值，则不能省略 [computation]。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(
        seconds: 1), (count) => count * count).take(5);

stream.forEach(print); // 输出事件值 0,1,4,9,16。
```

### Stream.eventTransformed()

```dart
Stream<T>.eventTransformed(
  Stream<dynamic> source,
  EventSink<dynamic> Function(EventSink<T>) mapSink
)
```

创建一个流，其中现有流的所有事件都经过一个 sink 转换处理。

当返回的流被监听时，会调用给定的 [mapSink] 闭包。来自 [source] 的所有事件都被添加到该次调用返回的事件 sink 中。转换过程会将所有转换后的事件放入 [mapSink] 闭包在调用时收到的 sink 中。从概念上讲，[mapSink] 创建了一个转换管道，输入 sink 是返回的 [EventSink]，输出 sink 是它接收到的那个 sink。

该构造函数常用于构建转换器（transformer）。

用于重复（duplicating）转换器的示例用法：

```dart
class DuplicationSink implements EventSink<String> {
  final EventSink<String> _outputSink;
  DuplicationSink(this._outputSink);

  void add(String data) {
    _outputSink.add(data);
    _outputSink.add(data);
  }

  void addError(e, [st]) { _outputSink.addError(e, st); }
  void close() { _outputSink.close(); }
}

class DuplicationTransformer extends StreamTransformerBase<String, String> {
  // 为简洁起见省略了部分泛型类型。
  Stream bind(Stream stream) => Stream<String>.eventTransformed(
      stream,
      (EventSink sink) => DuplicationSink(sink));
}

stringStream.transform(DuplicationTransformer());
```

如果 [source] 是广播流，则返回的流也是广播流。

## 静态方法

### castFrom()

```dart
Stream<T> castFrom<S, T>(Stream<S> source)
```

将 [source] 适配为 `Stream<T>`。

这使得 [source] 可以以新类型使用，但在运行时它必须同时满足新类型和原始类型两方面的要求。

source 流创建的数据事件也必须是 [T] 的实例。

## 属性

### isBroadcast

```dart
bool get isBroadcast
```

该流是否为广播流。

### length

```dart
Future<int> get length
```

该流中的元素数量。

等待该流的所有元素。当该流结束时，返回的 future 将以元素数量完成。

如果该流发出错误，返回的 future 将以该错误完成，处理随之停止。

此操作会监听该流，非广播流在获取其长度后不能被重复使用。

### isEmpty

```dart
Future<bool> get isEmpty
```

该流是否包含任何元素。

等待该流的第一个元素，然后以 `false` 完成返回的 future。如果该流在未发出任何元素的情况下结束，返回的 future 将以 `true` 完成。

如果第一个事件是错误，返回的 future 将以该错误完成。

此操作会监听该流，非广播流在检查是否为空后不能被重复使用。

### first

```dart
Future<T> get first
```

该流的第一个元素。

在收到第一个元素后停止监听该流。

在内部，该方法会在第一个元素之后取消其订阅。这意味着单订阅（非广播）流在调用此获取器后会被关闭，不能再被重用。

如果在第一个数据事件之前发生错误事件，返回的 future 将以该错误完成。

如果该流为空（在第一个数据事件之前发生了 done 事件），返回的 future 将以错误完成。

除了错误的类型不同外，该方法等价于 `this.elementAt(0)`。

### last

```dart
Future<T> get last
```

该流的最后一个元素。

如果该流发出错误事件，返回的 future 将以该错误完成，处理随之停止。

如果该流为空（done 事件是第一个事件），返回的 future 将以错误完成。

### single

```dart
Future<T> get single
```

该流的唯一元素。

如果该流发出错误事件，返回的 future 将以该错误完成，处理随之停止。

如果该 [Stream] 为空或包含多个元素，返回的 future 将以错误完成。

## 方法

### asBroadcastStream()

```dart
Stream<T> asBroadcastStream({
  void Function(StreamSubscription<T>)? onListen,
  void Function(StreamSubscription<T>)? onCancel
})
```

返回一个产生与此流相同事件的多订阅流。

返回的流会在其第一个订阅者被添加时订阅该流，并保持订阅状态，直到该流结束，或某个回调取消该订阅。

如果提供了 [onListen]，会以一个代表底层订阅的类似订阅对象来调用它。在调用 [onListen] 期间，可以暂停、恢复或取消该订阅。但无法更改事件处理程序，包括使用 [StreamSubscription.asFuture]。

如果提供了 [onCancel]，当返回的流不再有监听者时，会以类似的方式调用它，就像调用 [onListen] 一样。如果之后又获得了新的监听者，[onListen] 函数会再次被调用。

例如，可以使用这些回调，在没有订阅者时暂停底层订阅以防止丢失事件，或者在没有监听者时取消订阅。

取消操作旨在于当前没有订阅者时使用。如果传递给 `onListen` 或 `onCancel` 的订阅被取消，那么返回的广播流上的当前订阅将不会再发出任何进一步的事件，甚至连 done 事件也不会发出。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(seconds: 1), (count) => count)
        .take(10);

final broadcastStream = stream.asBroadcastStream(
  onCancel: (controller) {
    print('Stream paused');
    controller.pause();
  },
  onListen: (controller) async {
    if (controller.isPaused) {
      print('Stream resumed');
      controller.resume();
    }
  },
);

final oddNumberStream = broadcastStream.where((event) => event.isOdd);
final oddNumberListener = oddNumberStream.listen(
      (event) {
    print('Odd: $event');
  },
  onDone: () => print('Done'),
);

final evenNumberStream = broadcastStream.where((event) => event.isEven);
final evenNumberListener = evenNumberStream.listen((event) {
  print('Even: $event');
}, onDone: () => print('Done'));

await Future.delayed(const Duration(milliseconds: 3500)); // 3.5 秒
// 输出：
// Even: 0
// Odd: 1
// Even: 2
oddNumberListener.cancel(); // 不输出任何内容。
evenNumberListener.cancel(); // "Stream paused"
await Future.delayed(const Duration(seconds: 2));
print(await broadcastStream.first); // "Stream resumed"
// 输出：
// 3
```

### listen()

```dart
StreamSubscription<T> listen(
  void onData(T event), {
  Function? onError,
  void onDone(),
  bool? cancelOnError
})
```

向该流添加一个订阅。

返回一个 [StreamSubscription]，它使用提供的 [onData]、[onError] 和 [onDone] 处理程序处理来自该流的事件。这些处理程序可以在订阅上更改，但它们最初就是提供的这些函数。

对于该流的每个数据事件，会调用订阅者的 [onData] 处理程序。如果 [onData] 为 `null`，则什么都不会发生。

对于来自该流的错误，会以错误对象（可能还有堆栈跟踪）调用 [onError] 处理程序。

[onError] 回调的类型必须是 `void Function(Object error)` 或 `void Function(Object error, StackTrace)`。该函数的类型决定了是否以堆栈跟踪参数调用 [onError]。如果该流接收到的错误没有堆栈跟踪，堆栈跟踪参数可能是 [StackTrace.empty]。

否则，它会仅以错误对象作为参数被调用。如果省略了 [onError]，该流上的任何错误都会被视为未处理，并传递给当前 [Zone] 的错误处理程序。默认情况下，未处理的异步错误会被当作未捕获的顶级错误处理。

如果该流关闭并发送一个 done 事件，会调用 [onDone] 处理程序。如果 [onDone] 为 `null`，则什么都不会发生。

如果 [cancelOnError] 为 `true`，当第一个错误事件被传递时，该订阅会被自动取消。默认值为 `false`。

当某个订阅被暂停或已被取消时，该订阅不会接收事件，也不会调用任何事件处理函数。

### where()

```dart
Stream<T> where(bool test(T event))
```

从该流创建一个丢弃部分元素的新流。

新流发送与该流相同的错误和 done 事件，但只发送满足 [test] 的数据事件。

如果 [test] 函数抛出异常，该数据事件会被丢弃，并改为在返回的流上发出该错误。

如果该流是广播流，返回的流也是广播流。如果一个广播流被监听多次，每个订阅都会各自单独执行 `test`。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(seconds: 1), (count) => count)
        .take(10);

final customStream = stream.where((event) => event > 3 && event <= 6);
customStream.listen(print); // 输出事件值：4,5,6。
```

### map()

```dart
Stream<S> map<S>(S convert(T event))
```

将该流的每个元素转换为一个新的流事件。

创建一个新的流，使用 [convert] 函数将该流的每个元素转换为一个新值，并发出该结果。

对于该流中的每个数据事件 `o`，返回的流提供一个值为 `convert(o)` 的数据事件。如果 [convert] 抛出异常，返回的流会改为将其报告为错误事件。

错误和 done 事件不加改变地传递给返回的流。

如果该流是广播流，返回的流也是广播流。[convert] 函数对每个监听者的每个数据事件调用一次。如果一个广播流被监听多次，每个订阅都会各自单独对每个数据事件调用 [convert]。

与 [transform] 不同，该方法不会将流当作单个值的分块来处理。而是每个事件都独立于前后事件被转换，这可能并不总是正确的。例如，如果代理对（surrogate pair）或多字节 UTF-8 编码被拆分到不同的事件中，并且这些事件被独立地编码或解码，UTF-8 编码或解码就会得到错误的结果。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(seconds: 1), (count) => count)
        .take(5);

final calculationStream =
    stream.map<String>((event) => 'Square: ${event * event}');
calculationStream.forEach(print);
// Square: 0
// Square: 1
// Square: 4
// Square: 9
// Square: 16
```

### asyncMap()

```dart
Stream<E> asyncMap<E>(FutureOr<E> convert(T event))
```

创建一个新流，将该流的每个数据事件异步地映射为一个新事件。

其行为类似于 [map]，[convert] 函数对每个数据事件调用一次，但此处 [convert] 可以是异步的，并返回一个 [Future]。如果发生这种情况，该流会在继续处理后续事件之前等待该 future 完成。

如果该流是广播流，返回的流也是广播流。

### asyncExpand()

```dart
Stream<E> asyncExpand<E>(Stream<E>? convert(T event))
```

将每个元素转换为一系列异步事件。

返回一个新流，对于该流的每个事件执行以下操作：

- 如果该事件是错误事件或 done 事件，则由返回的流直接发出。
- 否则它是一个元素。此时会以该元素为参数调用 [convert] 函数，为该元素生成一个转换流。
- 如果该调用抛出异常，该错误会在返回的流上发出。
- 如果该调用返回 `null`，则不对该元素采取进一步的操作。
- 否则，该流会被暂停，并监听转换流。转换流的每个数据和错误事件都会按照产生的顺序在返回的流上发出。当转换流结束时，该流恢复。

如果该流是广播流，返回的流也是广播流。

### handleError()

```dart
Stream<T> handleError(
  Function onError, {
  bool test(dynamic error)
})
```

创建一个包装流，拦截该流的部分错误。

如果该流发送的错误与 [test] 匹配，则会被 [onError] 函数拦截。

[onError] 回调的类型必须是 `void Function(Object error)` 或 `void Function(Object error, StackTrace)`。该函数的类型决定了是否以堆栈跟踪参数调用 [onError]。如果该流接收到的错误没有堆栈跟踪，堆栈跟踪参数可能是 [StackTrace.empty]。

如果 `test(error)` 返回 true，则认为异步错误 `error` 与该测试函数匹配。如果省略 [test]，则认为所有错误都匹配。

如果该错误被拦截，[onError] 函数可以决定如何处理它。如果想要引发一个新的（或相同的）错误，可以抛出异常；如果只是想让该流忽略该错误，则直接返回即可。如果 [onError] 函数再次抛出接收到的 `error` 值，其行为就像 `rethrow` 一样，它会连同其原始堆栈跟踪一起被发出，而不是 [onError] 内部 `throw` 时的堆栈跟踪。

如果你需要将一个错误转换为数据事件，请使用更通用的 [Stream.transform]，通过向输出 sink 写入数据事件来处理该事件。

如果该流是广播流，返回的流也是广播流。如果一个广播流被监听多次，每个订阅都会各自单独执行 `test` 并处理错误。

示例：

```dart
Stream.periodic(const Duration(seconds: 1), (count) {
  if (count == 2) {
    throw Exception('Exceptional event');
  }
  return count;
}).take(4).handleError(print).forEach(print);

// 输出：
// 0
// 1
// Exception: Exceptional event
// 3
// 4
```

### expand()

```dart
Stream<S> expand<S>(Iterable<S> convert(T element))
```

将该流的每个元素转换为一系列元素。

返回一个新流，该流中该流的每个元素都被替换为零个或多个数据事件。事件值通过以该元素为参数调用 [convert] 得到，返回的是一个 [Iterable]，其元素按迭代顺序发出。如果调用 [convert] 抛出异常，或者对返回值的迭代抛出异常，该错误会在返回的流上发出，并且该流中该元素的迭代随之结束。

该流的错误事件和 done 事件会直接转发到返回的流。

如果该流是广播流，返回的流也是广播流。如果一个广播流被监听多次，每个订阅都会各自单独调用 `convert` 并展开事件。

### pipe()

```dart
Future pipe(StreamConsumer<T> streamConsumer)
```

将该流的事件导入 [streamConsumer]。

该流的所有事件都通过 [StreamConsumer.addStream] 添加到 `streamConsumer` 中。当该流已成功添加到 `streamConsumer` 中——即 `addStream` 返回的 future 无错误完成时——`streamConsumer` 会被关闭。

返回一个 future，当该流被消费完毕且消费者已被关闭时完成。

返回的 future 以与 [StreamConsumer.close] 返回的 future 相同的结果完成。如果对 [StreamConsumer.addStream] 的调用以某种方式失败，该方法也会以同样的方式失败。

### transform()

```dart
Stream<S> transform<S>(StreamTransformer<T, S> streamTransformer)
```

将 [streamTransformer] 应用于该流。

返回转换后的流，即 `streamTransformer.bind(this)` 的结果。该方法只是允许以链式的方式调用 `streamTransformer.bind`，例如

```dart
stream.map(mapping).transform(transformation).toList()
```

这可能比直接调用 `bind` 更方便。

[streamTransformer] 可以返回任意流。返回的流是否为广播流，以及它将包含哪些元素，完全取决于该转换本身。

该方法应始终用于将整个流视为代表单个值（该值也许已被拆分成若干部分以便传输）的转换，例如从磁盘读取的文件或通过网络获取的数据。此类转换会产生一个新的流，该流以增量方式转换流的值（也许使用 [Converter.startChunkedConversion]）。所生成的流可能同样是结果的分块，但不一定与源字符串中的具体事件相对应。

### reduce()

```dart
Future<T> reduce(T combine(T previous, T element))
```

通过反复应用 [combine] 来组合一系列值。

与 [Iterable.reduce] 类似，该函数维护一个值，起始值为该流的第一个元素，并针对该流的每个后续元素进行更新。对于第一个元素之后的每个元素，该值都会更新为以先前的值和该元素调用 [combine] 的结果。

当该流结束时，返回的 future 会以此时的值完成。

如果该流为空，返回的 future 将以错误完成。如果该流发出错误，或对 [combine] 的调用抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result = await Stream.fromIterable([2, 6, 10, 8, 2])
    .reduce((previous, element) => previous + element);
print(result); // 28
```

### fold()

```dart
Future<S> fold<S>(
  S initialValue,
  S combine(S previous, T element)
)
```

通过反复应用 [combine] 来组合一系列值。

与 [Iterable.fold] 类似，该函数维护一个值，起始值为 [initialValue]，并针对该流的每个元素进行更新。对于每个元素，该值都会更新为以先前的值和该元素调用 [combine] 的结果。

当该流结束时，返回的 future 会以此时的值完成。对于空流，future 以 [initialValue] 完成。

如果该流发出错误，或对 [combine] 的调用抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result = await Stream.fromIterable([2, 6, 10, 8, 2])
    .fold<int>(10, (previous, element) => previous + element);
print(result); // 38
```

### join()

```dart
Future<String> join([String separator = ""])
```

将元素的字符串表示形式组合成一个字符串。

每个元素都使用其 [Object.toString] 方法转换为字符串。如果提供了 [separator]，会将其插入到各元素的字符串表示形式之间。

当该流结束时，返回的 future 会以组合后的字符串完成。

如果该流发出错误，或对 [Object.toString] 的调用抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result = await Stream.fromIterable(['Mars', 'Venus', 'Earth'])
    .join('--');
print(result); // 'Mars--Venus--Earth'
```

### contains()

```dart
Future<bool> contains(Object? needle)
```

返回该流提供的元素中是否出现 [needle]。

使用 [Object.==] 将该流的每个元素与 [needle] 进行比较。如果找到相等的元素，返回的 future 将以 `true` 完成。如果该流结束时未找到匹配项，future 将以 `false` 完成。

如果该流发出错误，或对 [Object.==] 的调用抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result = await Stream.fromIterable(['Year', 2021, 12, 24, 'Dart'])
    .contains('Dart');
print(result); // true
```

### forEach()

```dart
Future<void> forEach(void action(T element))
```

对该流的每个元素执行 [action]。

当该流的所有元素都已处理完毕时，返回的 [Future] 完成。

如果该流发出错误，或者调用 [action] 抛出异常，返回的 future 将以该错误完成，处理随之停止。

### every()

```dart
Future<bool> every(bool test(T element))
```

检查 [test] 是否接受该流提供的所有元素。

对该流的每个元素调用 [test]。如果调用返回 `false`，返回的 future 将以 `false` 完成，处理随之停止。

如果该流结束时未找到 [test] 拒绝的元素，返回的 future 将以 `true` 完成。

如果该流发出错误，或者调用 [test] 抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result =
    await Stream.periodic(const Duration(seconds: 1), (count) => count)
        .take(15)
        .every((x) => x <= 5);
print(result); // false
```

### any()

```dart
Future<bool> any(bool test(T element))
```

检查 [test] 是否接受该流提供的任意元素。

对该流的每个元素调用 [test]。如果调用返回 `true`，返回的 future 将以 `true` 完成，处理随之停止。

如果该流结束时未找到 [test] 接受的元素，返回的 future 将以 `false` 完成。

如果该流发出错误，或者调用 [test] 抛出异常，返回的 future 将以该错误完成，处理随之停止。

示例：

```dart
final result =
    await Stream.periodic(const Duration(seconds: 1), (count) => count)
        .take(15)
        .any((element) => element >= 5);

print(result); // true
```

### cast()

```dart
Stream<R> cast<R>()
```

将该流适配为 `Stream<R>`。

该流被包装为 `Stream<R>`，在运行时会检查该流发出的每个数据事件是否也是 [R] 的实例。

### toList()

```dart
Future<List<T>> toList()
```

将该流的所有元素收集到一个 [List] 中。

创建一个 `List<T>`，并按照到达顺序将该流的所有元素添加到该列表中。当该流结束时，返回的 future 会以该列表完成。

如果该流发出错误，返回的 future 将以该错误完成，处理随之停止。

### toSet()

```dart
Future<Set<T>> toSet()
```

将该流的数据收集到一个 [Set] 中。

创建一个 `Set<T>`，并按照到达顺序将该流的所有元素添加到该集合中。当该流结束时，返回的 future 会以该集合完成。

返回的集合与由 `<T>{}` 创建的类型相同。如果需要另一种类型的集合，可以使用 [forEach] 将每个元素添加到该集合中，或使用 `toList().then((list) => new SomeOtherSet.from(list))` 来创建该集合。

如果该流发出错误，返回的 future 将以该错误完成，处理随之停止。

### drain()

```dart
Future<E> drain<E>([E? futureValue])
```

丢弃该流上的所有数据，但会在其结束或发生错误时发出信号。

使用 [drain] 订阅时，cancelOnError 将为 true。这意味着该 future 将以该流上的第一个错误完成，然后取消订阅。

如果该流发出错误，返回的 future 将以该错误完成，处理随之停止。

在出现 `done` 事件的情况下，future 以给定的 [futureValue] 完成。

如果 `null` 不能赋值给 [E]，则不能省略 [futureValue]。

示例：

```dart
final result = await Stream.fromIterable([1, 2, 3]).drain(100);
print(result); // 输出：100。
```

### take()

```dart
Stream<T> take(int count)
```

提供该流最多前 [count] 个数据事件。

返回一个流，如果同时监听该流，它会发出相同的事件，直到该流结束，或者已经发出了 [count] 个数据事件，此时返回的流结束。

如果该流在结束前产生的数据事件少于 [count] 个，返回的流也会如此。

当返回的流被监听时开始监听该流，并在收到前 [count] 个数据事件后停止监听。

这意味着，如果这是一个单订阅（非广播）流，在返回的流被监听之后就不能再被重用。

如果这是一个广播流，返回的流也是广播流。在这种情况下，事件只从返回的流被监听的时刻开始计数。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(seconds: 1), (i) => i)
        .take(60);
stream.forEach(print); // 输出事件：0, ... 59。
```

### takeWhile()

```dart
Stream<T> takeWhile(bool test(T element))
```

在 [test] 成功时转发数据事件。

返回一个流，提供与该流相同的事件，直到 [test] 对某个数据事件失败为止。当该流结束，或者该流首次发出未通过 [test] 的数据事件时，返回的流结束。

如果 `test` 调用返回非 `true` 的值，或者抛出异常，则认为该调用失败。如果 `test` 调用抛出异常，该错误会作为返回流上的最后一个事件发出。

在接受的元素之后停止监听该流。

在内部，该方法会在这些元素之后取消其订阅。这意味着单订阅（非广播）流在调用此方法后会被关闭，不能再被重用。

如果该流是广播流，返回的流也是广播流。对于广播流，事件只从返回的流被监听的时刻开始测试。

示例：

```dart
final stream = Stream<int>.periodic(const Duration(seconds: 1), (i) => i)
    .takeWhile((event) => event < 6);
stream.forEach(print); // 输出事件：0, ..., 5。
```

### skip()

```dart
Stream<T> skip(int count)
```

跳过该流的前 [count] 个数据事件。

返回一个流，如果同时监听该流，它会发出与该流相同的事件，只是前 [count] 个数据事件不会被发出。当该流结束时，返回的流也结束。

如果该流在结束前发出的数据事件少于 [count] 个，返回的流不会发出任何数据事件。

如果该流是广播流，返回的流也是广播流。对于广播流，事件只从返回的流被监听的时刻开始计数。

示例：

```dart
final stream =
    Stream<int>.periodic(const Duration(seconds: 1), (i) => i).skip(7);
stream.forEach(print); // 跳过事件 0, ..., 6。输出事件：7, ...
```

### skipWhile()

```dart
Stream<T> skipWhile(bool test(T element))
```

跳过该流中被 [test] 匹配的数据事件。

返回一个流，提供与该流相同的事件，只是在某个数据事件未通过 `test` 之前，不会发出数据事件。当以某个数据事件调用 `test` 时，如果它返回非 `true` 的值，或者对 `test` 的调用抛出异常，则认为该测试失败。如果该调用抛出异常，该错误会作为返回流上的一个错误事件发出，而不是该数据事件；否则，使 `test` 返回非 true 的那个事件会作为第一个数据事件发出。

错误事件和该流的 done 事件会不加修改地由返回的流提供。

如果该流是广播流，返回的流也是广播流。对于广播流，事件只从返回的流被监听的时刻开始测试。

示例：

```dart
final stream = Stream<int>.periodic(const Duration(seconds: 1), (i) => i)
    .take(10)
    .skipWhile((x) => x < 5);
stream.forEach(print); // 输出事件：5, ..., 9。
```

### distinct()

```dart
Stream<T> distinct([bool equals(T previous, T next)])
```

如果数据事件与前一个数据事件相等，则跳过该数据事件。

返回的流提供与该流相同的事件，只是它绝不会提供两个连续且相等的数据事件。也就是说，错误会被传递给返回的流，而数据事件只有在与最近发出的数据事件不同时才会被传递。

相等性由提供的 [equals] 方法决定。如果省略该方法，则使用最后提供的数据元素上的 '==' 运算符。

如果 [equals] 抛出异常，该数据事件会被替换为一个包含所抛出错误的错误事件。其行为等价于原始流发出该错误事件，并且不会改变最近发出的数据事件是什么。

如果该流是广播流，返回的流也是广播流。如果一个广播流被监听多次，每个订阅都会各自单独执行 `equals` 测试。

示例：

```dart
final stream = Stream.fromIterable([2, 6, 6, 8, 12, 8, 8, 2]).distinct();
stream.forEach(print); // 输出事件：2,6,8,12,8,2。
```

### firstWhere()

```dart
Future<T> firstWhere(
  bool test(T element), {
  T orElse()
})
```

查找该流中第一个匹配 [test] 的元素。

返回一个 future，以该流中第一个使 [test] 返回 `true` 的元素完成。

如果在该流结束之前未找到这样的元素，且提供了 [orElse] 函数，则调用 [orElse] 的结果将成为该 future 的值。如果 [orElse] 抛出异常，返回的 future 将以该错误完成。

如果该流在第一个匹配元素之前发出错误，返回的 future 将以该错误完成，处理随之停止。

在第一个匹配元素或错误到达后停止监听该流。

在内部，该方法会在第一个匹配该谓词的元素之后取消其订阅。这意味着单订阅（非广播）流在调用此方法后会被关闭，不能再被重用。

如果发生错误，或者该流结束时未找到匹配项且未提供 [orElse] 函数，返回的 future 将以错误完成。

示例：

```dart
var result = await Stream.fromIterable([1, 3, 4, 9, 12])
    .firstWhere((element) => element % 6 == 0, orElse: () => -1);
print(result); // 12

result = await Stream.fromIterable([1, 2, 3, 4, 5])
    .firstWhere((element) => element % 6 == 0, orElse: () => -1);
print(result); // -1
```

### lastWhere()

```dart
Future<T> lastWhere(
  bool test(T element), {
  T orElse()
})
```

查找该流中最后一个匹配 [test] 的元素。

返回一个 future，以该流中最后一个使 [test] 返回 `true` 的元素完成。

如果在该流结束之前未找到这样的元素，且提供了 `orElse` 函数，则调用 `orElse` 的结果将成为该 future 的值。如果 `orElse` 抛出异常，返回的 future 将以该错误完成。

如果该流在任意时刻发出错误，返回的 future 将以该错误完成，并且该订阅会被取消。

在该流结束之前，不能提供非错误的结果。

与 [firstWhere] 类似，只是查找的是最后一个匹配的元素，而不是第一个。

示例：

```dart
var result = await Stream.fromIterable([1, 3, 4, 7, 12, 24, 32])
    .lastWhere((element) => element % 6 == 0, orElse: () => -1);
print(result); // 24

result = await Stream.fromIterable([1, 3, 4, 7, 12, 24, 32])
    .lastWhere((element) => element % 10 == 0, orElse: () => -1);
print(result); // -1
```

### singleWhere()

```dart
Future<T> singleWhere(
  bool test(T element), {
  T orElse()
})
```

查找该流中匹配 [test] 的唯一元素。

返回一个 future，以该流中使 [test] 返回 `true` 的唯一元素完成。

如果在该流结束之前未找到这样的元素，且提供了 `orElse` 函数，则调用 `orElse` 的结果将成为该 future 的值。如果 `orElse` 抛出异常，返回的 future 将以该错误完成。

只能有一个元素匹配。如果找到多个匹配的元素，无论是否传入了 [orElse]，都会抛出错误。

如果该流在任意时刻发出错误，返回的 future 将以该错误完成，并且该订阅会被取消。

在该流结束之前，不能提供非错误的结果。

与 [lastWhere] 类似，只是如果该流中出现多个匹配元素，则视为错误。

示例：

```dart
var result = await Stream.fromIterable([1, 2, 3, 6, 9, 12])
    .singleWhere((element) => element % 4 == 0, orElse: () => -1);
print(result); // 12

result = await Stream.fromIterable([2, 6, 8, 12, 24, 32])
    .singleWhere((element) => element % 9 == 0, orElse: () => -1);
print(result); // -1

result = await Stream.fromIterable([2, 6, 8, 12, 24, 32])
    .singleWhere((element) => element % 6 == 0, orElse: () => -1);
// 抛出异常。
```

### elementAt()

```dart
Future<T> elementAt(int index)
```

返回该流第 [index] 个数据事件的值。

在收到第 [index] 个数据事件后停止监听该流。

在内部，该方法会在这些元素之后取消其订阅。这意味着单订阅（非广播）流在调用此方法后会被关闭，不能再被重用。

如果在找到该值之前发生错误事件，future 将以该错误完成。

如果在找到该值之前发生 done 事件，future 将以一个 [RangeError] 完成。

### timeout()

```dart
Stream<T> timeout(
  Duration timeLimit, {
  void onTimeout(EventSink<T> sink)
})
```

创建一个具有与该流相同事件的新流。

当有人正在监听返回的流，并且超过 [timeLimit] 的时间内该流没有发出任何事件时，会调用 [onTimeout] 函数，随后它可以在返回的流上发出进一步的事件。

倒计时从返回的流被监听时开始，并在该流发出事件时，或在返回的流的监听被暂停并恢复时重新开始。当返回的流的监听被暂停或取消时，倒计时会停止。当一次倒计时完成并调用 [onTimeout] 函数时，即使发出了事件，也不会启动新的倒计时。如果该流的事件之间的间隔是 [timeLimit] 的数倍，事件之间最多会发生一次超时。

调用 [onTimeout] 函数时会带有一个参数：一个允许向返回的流中放入事件的 [EventSink]。这个 `EventSink` 仅在 [onTimeout] 调用期间有效。在传递给 [onTimeout] 的 sink 上调用 [EventSink.close] 会关闭返回的流，并且不再处理进一步的事件。

如果省略了 [onTimeout]，超时会在返回的流的错误通道中发出一个 [TimeoutException]。如果对 [onTimeout] 的调用抛出异常，该错误会作为返回流上的一个错误发出。

如果该流是广播流，返回的流也是广播流。如果一个广播流被监听多次，每个订阅都会拥有各自独立的计时器，该计时器在监听开始时启动计数，且各订阅的计时器可以分别暂停。

示例：

```dart
Future<String> waitTask() async {
  return await Future.delayed(
      const Duration(seconds: 4), () => 'Complete');
}
final stream = Stream<String>.fromFuture(waitTask())
    .timeout(const Duration(seconds: 2), onTimeout: (controller) {
  print('TimeOut occurred');
  controller.close();
});

stream.listen(print, onDone: () => print('Done'));

// 输出：
// TimeOut occurred
// Done
```

---

# StreamSubscription

```dart
abstract interface class StreamSubscription<T> {}
```

对来自 [Stream] 的事件的订阅。

当你使用 [Stream.listen] 监听一个 [Stream] 时，会返回一个 [StreamSubscription] 对象。

该订阅向监听者提供事件，并持有用于处理这些事件的回调函数。该订阅还可用于取消订阅事件，或临时暂停来自流的事件。

示例：

```dart
final stream = Stream.periodic(const Duration(seconds: 1), (i) => i * i)
    .take(10);

final subscription = stream.listen(print); // 一个 StreamSubscription<int>。
```

要暂停订阅，使用 [pause]。

```dart
// 做一些工作。
subscription.pause();
print(subscription.isPaused); // true
```

要在暂停后恢复，使用 [resume]。

```dart
// 做一些工作。
subscription.resume();
print(subscription.isPaused); // false
```

要取消订阅，使用 [cancel]。

```dart
// 做一些工作。
subscription.cancel();
```

## 属性

### isPaused

```dart
bool get isPaused
```

该 [StreamSubscription] 当前是否处于暂停状态。

如果在该流订阅上调用 [pause] 的次数多于调用 [resume] 的次数，则该订阅处于暂停状态，此获取器返回 `true`。

如果该流当前可以发出事件，或者该订阅已完成或已被取消，则返回 `false`。

## 方法

### cancel()

```dart
Future<void> cancel()
```

取消该订阅。

此调用之后，该订阅将不再接收事件。

该流可能需要关闭事件源，并在订阅取消后进行清理。

返回一个 future，在流完成其清理工作后完成。

通常，当流需要释放资源时会进行清理。例如，一个流可能需要关闭一个打开的文件（这是一个异步操作）。如果监听者想在取消订阅后删除该文件，就必须等待该清理 future 完成。

如果清理过程抛出异常（这实际上不应该发生），返回的 future 将以该错误完成。

### onData()

```dart
void onData(void handleData(T data))
```

替换该订阅的数据事件处理程序。

在调用此函数之后，该流的每个数据事件都会调用 [handleData] 函数。如果 [handleData] 为 `null`，则数据事件会被忽略。

该方法会替换由调用 [Stream.listen] 或之前调用 [onData] 所设置的当前处理程序。

### onError()

```dart
void onError(Function? handleError)
```

替换该订阅的错误事件处理程序。

[handleError] 函数必须能够以一个位置参数调用，或者以两个位置参数调用（其中第二个参数始终是 [StackTrace]）。

[handleError] 参数可以为 `null`，此时进一步的错误事件将被视为*未处理*，并会报告给 [Zone.handleUncaughtError]。

对于来自该流订阅的所有错误事件，都会调用所提供的函数。

该方法会替换由调用 [Stream.listen]、调用 [asFuture]，或之前调用 [onError] 所设置的当前处理程序。

### onDone()

```dart
void onDone(void handleDone())
```

替换该订阅的 done 事件处理程序。

当流关闭时，会调用 [handleDone] 函数。该值可以为 `null`，此时不会调用任何函数。

该方法会替换由调用 [Stream.listen]、调用 [asFuture]，或之前调用 [onDone] 所设置的当前处理程序。

### pause()

```dart
void pause([Future<void>? resumeSignal])
```

请求该流暂停发出事件，直到另行通知。

在暂停期间，该订阅不会触发任何事件。如果它从事件源接收到事件，这些事件会被缓冲，直到该订阅恢复。对于非广播流，底层事件源通常会被告知该暂停，以便它可以停止产生事件，直到该订阅恢复。

为了避免在广播流上缓冲事件，更好的做法是取消该订阅，并在需要事件时重新开始监听（如果中间的事件并不重要的话）。

如果提供了 [resumeSignal]，当该 future 完成时，该流订阅将撤销此次暂停，就像调用了 [resume] 一样。如果该 future 以错误完成，该流仍会恢复，但该错误将被视为未处理，并传递给 [Zone.handleUncaughtError]。

调用 [resume] 也会撤销一次暂停。

如果该订阅被暂停多次，则必须进行相同次数的恢复才能使该流恢复。调用 [resume] 与一个 [resumeSignal] 的完成是可以互换的——传入了 [resumeSignal] 的那次 [pause] 可以通过调用 [resume] 来结束，而完成某个 [resumeSignal] 也可以结束另一次不同的 [pause]。

即使在该订阅未处于暂停状态时调用 [resume] 或完成 [resumeSignal] 也是安全的，此时恢复操作不会产生任何效果。

### resume()

```dart
void resume()
```

在暂停后恢复。

这会撤销之前的一次 [pause] 调用。当之前所有的 [pause] 调用都已被相应的 [resume] 调用（可能是通过传递给 [pause] 的 `resumeSignal`）匹配后，该流订阅才可能再次发出事件。

即使在该订阅未处于暂停状态时调用 [resume] 也是安全的，此时恢复操作不会产生任何效果。

### asFuture()

```dart
Future<E> asFuture<E>([E? futureValue])
```

返回一个处理 [onDone] 和 [onError] 回调的 future。

该方法会*覆盖*现有的 [onDone] 和 [onError] 回调，替换为完成返回的 future 的新回调。

在发生错误的情况下，该订阅将自动取消（即使它是以 `cancelOnError` 设为 `false` 来监听的）。

在出现 `done` 事件的情况下，future 以给定的 [futureValue] 完成。

如果省略了 [futureValue]，则默认使用值 `null as E`。如果 `E` 不可为空，调用 [asFuture] 时将立即抛出异常。

---

# EventSink

```dart
abstract interface class EventSink<T> implements Sink<T> {}
```

支持添加错误的 [Sink]。

这使得它适合用于捕获异步计算的结果，这些结果可以以一个值或一个错误完成。

[EventSink] 的设计目的是处理来自 [Stream] 的异步事件。例如参见 [Stream.eventTransformed]，它使用 `EventSink` 来转换事件。

## 方法

### add()

```dart
void add(T event)
```

向该 sink 添加一个数据 [event]。

不得在已关闭的 sink 上调用。

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

向该 sink 添加一个 [error]。

不得在已关闭的 sink 上调用。

### close()

```dart
void close()
```

关闭该 sink。

允许多次调用此方法，但不会产生任何效果。

在调用此方法之后，不得再调用 [add] 或 [addError]。

---

# StreamView

```dart
class StreamView<T> extends Stream<T> {}
```

只暴露 [Stream] 接口的 [Stream] 包装器。

### StreamView()

```dart
const StreamView<T>( Stream<T> stream )
```

- isBroadcast

- asBroadcastStream()
- listen()

---

# StreamConsumer

```dart
abstract interface class StreamConsumer<S> {}
```

用于接收多个完整流的“sink”的抽象接口。

消费者可以使用 [addStream] 接收若干个连续的流，当不再需要添加数据时，[close] 方法会告知消费者完成其工作并关闭。

[Stream.pipe] 接受一个 `StreamConsumer`，并会将该流传递给消费者的 [addStream] 方法。当该调用完成后，它会调用 [close]，然后完成其自身返回的 future。

## 方法

### addStream()

```dart
Future addStream(Stream<S> stream)
```

消费 [stream] 的元素。

监听 [stream]，并对每个事件执行某些操作。

返回一个 future，当该流添加完毕，且消费者已准备好接受新的流时完成。在该返回的 future 完成之前，不应再次调用 [addStream] 或 [close]。

消费者可能会在发生错误后停止监听该流，也可能会消费所有错误并仅在 done 事件时停止，或者如果接收方不想要任何进一步的事件，也可能提前被取消。

如果消费者因某个错误而停止监听，导致其无法继续，它可以在返回的 future 中报告该错误；否则，它只会以 `null` 完成该 future。

### close()

```dart
Future close()
```

告知该消费者不会再添加更多的流。

这使消费者能够完成任何剩余的工作，并释放不再需要的资源

返回一个 future，当消费者关闭完毕时完成。如果清理过程可能失败，该错误可能会在返回的 future 中报告；否则，它会以 `null` 完成。

---

# StreamSink

```dart
abstract interface class StreamSink<S> implements EventSink<S>, StreamConsumer<S> {}
```

一个既支持同步又支持异步接受流事件的对象。

[StreamSink] 结合了来自 [StreamConsumer] 和 [EventSink] 的方法。

在调用 [addStream] 期间，不能使用 [EventSink] 的方法。一旦 [addStream] 返回的 [Future] 以某个值完成，就可以再次使用 [EventSink] 的方法。

如果在任何 [EventSink] 方法之后调用 [addStream]，它将被延迟，直到底层系统消费完通过 [EventSink] 方法添加的数据。

当使用 [EventSink] 的方法时，可以使用 [done] [Future] 来捕获任何错误。

当调用 [close] 时，它将返回 [done] [Future]。

## 属性

### done

```dart
Future get done
```

返回一个 future，当该 [StreamSink] 完成时完成。

如果 `StreamSink` 在响应使用 [add]、[addError] 或 [close] 添加事件时发生错误而失败，[done] future 将以该错误完成。

否则，返回的 future 将在以下任一情况发生时完成：

- 所有事件都已处理完毕，且该 sink 已关闭；或者
- 该 sink 已因其他原因停止处理更多事件（例如取消了某个流订阅）。

## 方法

### close()

```dart
Future close()
```

告知该流 sink 不会再添加更多的流。

这使该流 sink 能够完成任何剩余的工作，并释放不再需要的资源

返回一个 future，当该流 sink 关闭完毕时完成。如果清理过程可能失败，该错误可能会在返回的 future 中报告；否则，它会以 `null` 完成。

返回与 [done] 相同的 future。

该流 sink 可能在调用 [close] 方法之前就已关闭，这可能是由于发生错误，也可能是因为它正在向已停止监听的对象提供事件。在这种情况下，[done] future 会先完成，随后调用 `close` 方法时会返回 `done` future。

统一了 [StreamConsumer.close] 和 [EventSink.close]，二者都用于标记该对象不再期望接收任何进一步的事件。

---

# StreamTransformer

```dart
abstract interface class StreamTransformer<S, T> {}
```

转换一个 Stream。

当在流上调用 [Stream.transform] 方法并传入一个 [StreamTransformer] 时，该流会在提供的转换器上调用 [bind] 方法。所产生的流随后由 [Stream.transform] 方法返回。

从概念上讲，转换器只是一个从 [Stream] 到 [Stream] 的函数，被封装成一个类。

编写可以多次使用的转换器是一种良好的实践。

`Stream` 上所有其他的转换方法，例如 [Stream.map]、[Stream.where] 或 [Stream.expand]，都可以使用 [Stream.transform] 来实现。因此，[StreamTransformer] 非常强大，但通常使用起来也稍微更复杂一些。

[StreamTransformer.fromHandlers] 构造函数允许传入单独的回调函数，以响应事件、错误和流的结束。[StreamTransformer.fromBind] 构造函数创建一个 `StreamTransformer`，其 [bind] 方法通过调用传递给该构造函数的函数来实现。

## 构造函数

### StreamTransformer()

```dart
StreamTransformer(
  StreamSubscription<T> onListen(Stream<S> stream, bool cancelOnError)
)
```

基于给定的 [onListen] 回调创建一个 [StreamTransformer]。

返回的流转换器在转换后的流被监听时使用提供的 [onListen] 回调。此时，该回调会收到输入流（传递给 [bind] 的那个流）以及一个布尔标志 `cancelOnError`，用于创建一个 [StreamSubscription]。

如果转换后的流是广播流，那么由该转换器的 [StreamTransformer.bind] 方法返回的流也是广播流。

如果多次监听转换后的流，[onListen] 回调会针对每次新的 [Stream.listen] 调用而再次被调用。无论该流是否为广播流，这一点都成立，但对于非广播流，该调用通常会失败。

[onListen] 回调*不会*收到传递给 [Stream.listen] 的处理程序。这些处理程序会在调用 [onListen] 回调之后自动被设置（使用 [StreamSubscription.onData]、[StreamSubscription.onError] 和 [StreamSubscription.onDone]）。

最常见的情况是，[onListen] 回调会先在提供的流上调用 [Stream.listen]（带有相应的 `cancelOnError` 标志），然后返回一个新的 [StreamSubscription]。

创建 StreamSubscription 有两种常见方式：

1.  分配一个 [StreamController]，并返回监听其流的结果。重要的是要转发暂停、恢复和取消事件（除非该转换器有意想改变此行为）。
2.  创建一个实现 [StreamSubscription] 的新类。请注意，该订阅应在流被监听时所在的 [Zone] 中运行回调（参见 [Zone] 和 [Zone.bindCallback]）。

示例：

```dart
/// 开始监听 [input]，并复制所有非错误事件。
StreamSubscription<int> _onListen(Stream<int> input, bool cancelOnError) {
  // 创建结果控制器。
  // 此处使用 `sync` 是正确的，因为只转发异步事件。
  var controller = StreamController<int>(sync: true);
  controller.onListen = () {
    var subscription = input.listen((data) {
      // 复制数据。
      controller.add(data);
      controller.add(data);
    },
        onError: controller.addError,
        onDone: controller.close,
        cancelOnError: cancelOnError);
    // 控制器转发暂停、恢复和取消事件。
    controller
      ..onPause = subscription.pause
      ..onResume = subscription.resume
      ..onCancel = subscription.cancel;
  };
  // 通过监听控制器的流，返回一个新的 [StreamSubscription]。
  return controller.stream.listen(null);
}

// 实例化一个转换器：
var duplicator = const StreamTransformer<int, int>(_onListen);

// 使用方式如下：
intStream.transform(duplicator);
```

### StreamTransformer.fromHandlers()

```dart
StreamTransformer<S, T>.fromHandlers({
  void Function(S, EventSink<T>)? handleData,
  void Function(Object, StackTrace, EventSink<T>)? handleError,
  void Function(EventSink<T>)? handleDone,
})
```

创建一个将事件委托给给定函数的 [StreamTransformer]。

用于重复（duplicating）转换器的示例用法：

```dart
stringStream.transform(StreamTransformer<String, String>.fromHandlers(
    handleData: (String value, EventSink<String> sink) {
      sink.add(value);
      sink.add(value);  // 复制传入的事件。
    }));
```

当监听由调用 [bind] 返回的转换后的流时，会监听源流，并针对源流的每个事件调用一个处理函数。

调用这些处理程序时会传入事件数据，以及一个可用于在转换后的流上发出事件的 sink。

[handleData] 处理程序会针对源流的数据事件被调用。如果省略 [handleData]，数据事件会被直接添加到所创建的流中，就像在 sink 上以该事件值调用 [EventSink.add] 一样。如果省略了 [handleData]，源流的事件类型 [S] 必须是转换后流事件类型 [T] 的子类型。

[handleError] 处理程序会针对源流的每个错误被调用。如果省略 [handleError]，错误会被直接转发到转换后的流，就像以该错误和堆栈跟踪调用 [EventSink.addError] 一样。

当源流关闭时（通过发送一个 done 事件表示），会调用 [handleDone] 处理程序。该 done 处理程序不接收事件值，但在调用 [EventSink.close] 之前仍可以发送其他事件。如果省略了 [handleDone]，源流上的 done 事件会关闭转换后的流。

如果任何处理程序在提供的 sink 上调用 [EventSink.close]，转换后的 sink 会关闭，并且源流订阅会被取消。该处理程序不能再向该 sink 添加任何进一步的事件，也不会再发生任何进一步的源流事件。

提供给事件处理程序的 sink 只能在该处理程序被调用期间使用。不得将其存储起来供稍后使用。

以这种方式创建的转换器应该是*无状态的*。它们不应在处理程序的多次调用之间保留状态，因为同一个转换器（从而是相同的处理程序）可能会用于多个流，或用于可以被多次监听的流。_要创建针对每个流的处理程序，可以使用 [StreamTransformer.fromBind] 为每个要转换的流创建一个新的 [StreamTransformer.fromHandlers]。_

```dart
var controller = StreamController<String>.broadcast();
controller.onListen = () {
  scheduleMicrotask(() {
    controller.addError("Bad");
    controller.addError("Worse");
    controller.addError("Worst");
  });
};
var sharedState = 0;
var transformedStream = controller.stream.transform(
    StreamTransformer<String>.fromHandlers(
        handleError: (error, stackTrace, sink) {
  sharedState++; // 递增共享的错误计数器。
  sink.add("Error $sharedState: $error");
}));

transformedStream.listen(print);
transformedStream.listen(print); // 监听两次。
// 对同一个流监听两次，会使该转换器共享同一个状态。这个程序不会
// 输出 "Error 1: Bad"、"Error 2: Worse"、"Error 3: Worst"（每个分别
// 针对两个独立的订阅各输出一次），而是会输出：
// Error 1: Bad
// Error 2: Bad
// Error 3: Worse
// Error 4: Worse
// Error 5: Worst
// Error 6: Worst
```

### StreamTransformer.fromBind()

```dart
StreamTransformer.fromBind(Stream<T> Function(Stream<S>) bind)
```

基于一个 [bind] 回调创建一个 [StreamTransformer]。

返回的流转换器使用 [bind] 参数来实现 [StreamTransformer.bind] API，可在该转换以流到流函数的形式可用时使用。

```dart import:convert
final splitDecoded = StreamTransformer<List<int>, String>.fromBind(
    (stream) => stream.transform(utf8.decoder).transform(LineSplitter()));
```

## 静态方法

### castFrom()

```dart
StreamTransformer<TS, TT> castFrom<SS, ST, TS, TT>(StreamTransformer<SS, ST> source)
```

将 [source] 适配为 `StreamTransformer<TS, TT>`。

这使得 [source] 可以以新类型使用，但在运行时它必须同时满足新类型和原始类型两方面的要求。

传入返回的转换器的数据事件也必须是 [SS] 的实例，并且 [source] 针对这些事件产生的数据事件也必须是 [TT] 的实例。

## 方法

### bind()

```dart
Stream<T> bind(Stream<S> stream)
```

转换提供的 [stream]。

返回一个新流，其事件是根据提供的 [stream] 的事件计算得出的。

[StreamTransformer] 接口是完全通用的，因此它无法说明子类具体做了什么。每个 [StreamTransformer] 都应（在访问该转换器的类或变量上）清楚地说明它是如何转换流的，以及与以下典型行为的任何差异：

- 当监听返回的流时，它开始监听输入 [stream]。
- 返回的流的订阅会（在合理的时间内）将 [StreamSubscription.pause] 调用转发给输入 [stream] 的订阅。
- 类似地，取消返回的流的一个订阅最终会（在合理的时间内）取消输入 [stream] 的订阅。

“合理的时间”取决于具体的转换器和流。有些转换器，比如“超时”转换器，可能会使这些操作依赖于某个时长。其他转换器可能根本不延迟这些操作，或者只延迟一个微任务的时间。

转换器可以自由地以任何方式处理错误。转换器的实现可以选择传播错误，或将其转换为其他事件，或完全忽略它们，但如果错误被忽略，应明确地记录这一点。

### cast()

```dart
StreamTransformer<RS, RT> cast<RS, RT>()
```

提供该流转换器的一个 `StreamTransformer<RS, RT>` 视图。

所产生的转换器将在运行时检查它所转换的流的所有数据事件是否确实是 [S] 的实例，并检查该转换器产生的所有数据事件是否确实是 [RT] 的实例。

---

# StreamTransformerBase

```dart
abstract class StreamTransformerBase<S, T> implements StreamTransformer<S, T> {}
```

用于实现 [StreamTransformer] 的基类。

包含除 [bind] 之外每个方法的默认实现。

### StreamTransformerBase()

```dart
StreamTransformerBase()
```

### cast()

```dart
StreamTransformer<RS, RT> cast<RS, RT>()
```

---

# StreamIterator

```dart
abstract interface class StreamIterator<T> {}
```

一个类似于 [Iterator] 的接口，用于 [Stream] 的值。

它包装了一个 [Stream] 及对该流的一个订阅。它监听该流，并在下一个值可用时完成 [moveNext] 返回的 future。

该流可以在多次 [moveNext] 调用之间被暂停。

[current] 值只能在 [moveNext] 返回的 future 以 `true` 完成之后使用，并且只能使用到下一次调用 [moveNext] 为止。

## 构造函数

### StreamIterator()

```dart
StreamIterator<T>(Stream<T> stream)
```

在 [stream] 上创建一个 [StreamIterator]。

## 属性

### current

```dart
T get current
```

该流的当前值。

当一次 [moveNext] 调用以 `true` 完成时，[current] 字段保存该流最近的事件，并保持该值不变，直到下一次调用 [moveNext]。该值只能在 [moveNext] 调用以 `true` 完成之后被读取，并且只能读取到 [moveNext] 再次被调用为止。

如果 StreamIterator 尚未移动到第一个元素（[moveNext] 尚未被调用并完成），或者 StreamIterator 已经移动到超过最后一个元素之后（[moveNext] 返回了 `false`），则 [current] 的值未指定。此时 [StreamIterator] 可以选择抛出异常，或返回一个特定于该迭代器实现的默认值。

## 方法

### moveNext()

```dart
Future<bool> moveNext()
```

等待流的下一个值可用。

返回一个 future，它将以 `true` 或 `false` 完成。以 `true` 完成表示又收到了一个事件，可以作为 [current] 读取。以 `false` 完成表示该流的迭代已经结束，不会再有进一步的事件可用。如果该流产生了错误（这也会结束迭代），该 future 也可能以该错误完成。

在上一次调用返回的 future 完成之前，不得再次调用该函数。

### cancel()

```dart
Future cancel()
```

提前取消该流迭代器（以及底层的流订阅）。

如果 [moveNext] 返回的 future 以 `false` 或错误完成，该流迭代器会被自动取消。

如果你需要在该流迭代器自动关闭之前停止监听值，必须调用 [cancel] 以确保该流被正确关闭。

如果在该迭代器被取消时 [moveNext] 已被调用，它返回的 future 将以值 `false` 完成，此后所有对 [moveNext] 的调用也是如此。

返回一个 future，当取消操作完成时完成。如果取消操作是同步发生的，这可以是一个已经完成的 future。

---

# MultiStreamController

```dart
abstract interface class MultiStreamController<T> implements StreamController<T> {}
```

由 [Stream.multi] 提供的增强流控制器。

其行为类似于普通的异步控制器，但也允许同步地添加事件。与任何同步事件传递一样，发送方应非常小心，不要在新监听者可能尚未准备好接收事件时同步地传递事件。这通常意味着只在响应其他异步事件时才同步传递事件，因为那才是可能发生异步事件的时刻。

## 方法

### addSync()

```dart
void addSync(T value)
```

添加并传递一个事件。

像 [add] 一样添加一个事件，并尝试立即传递它。如果其他先前添加的事件仍待传递，或者该订阅已暂停，或者该订阅尚未开始监听，传递可能会被延迟。

### addErrorSync()

```dart
void addErrorSync(Object error, [StackTrace? stackTrace])
```

添加并传递一个错误事件。

像 [addError] 一样添加一个错误，并尝试立即传递它。如果其他先前添加的事件仍待传递，或者该订阅已暂停，或者该订阅尚未开始监听，传递可能会被延迟。

### closeSync()

```dart
void closeSync()
```

关闭该控制器并传递一个 done 事件。

像 [close] 一样关闭该控制器，并尝试立即传递一个“done”事件。如果其他先前添加的事件仍待传递，或者该订阅已暂停，或者该订阅尚未开始监听，传递可能会被延迟。如果需要知道“done”事件是否已被传递，可以使用 [done] future，它会在事件传递完成时完成。
