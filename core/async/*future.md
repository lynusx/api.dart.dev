# FutureOr

```dart
abstract class FutureOr<T> {}
```

表示值为 `Future<T>` 或 `T` 的类型。

此类声明是一个内部 future-or-value 泛型类型的公开占位符，该内部类型本身并非类类型。对 `FutureOr` 的引用会被解析为该内部类型。

任何类对 `FutureOr` 进行继承（extend）、混入（mix in）或实现（implement）都是编译时错误。

### 示例

```dart
// The `Future<T>.then` function takes a callback [f] that returns either
// an `S` or a `Future<S>`.
Future<S> then<S>(FutureOr<S> f(T x), ...);

// `Completer<T>.complete` takes either a `T` or `Future<T>`.
void complete(FutureOr<T> value);
```

### 深入了解

`FutureOr<int>` 类型实际上是 `int` 和 `Future<int>` 两个类型的“类型联合（type union）”。这种类型联合的定义方式使得 `FutureOr<Object>` 既是 `Object` 的超类型，又是 `Object` 的子类型（是子类型，因为 `Object` 本身就是该联合中的一个类型；是超类型，因为 `Object` 是该联合中两个类型的超类型）。综合来看，这意味着 `FutureOr<Object>` 等价于 `Object`。

由此可以推出，`FutureOr<Object>` 等价于 `FutureOr<FutureOr<Object>>`，`FutureOr<Future<Object>>` 等价于 `Future<Object>`。

---

# Future

```dart
abstract interface class Future<T> {}
```

异步计算的结果。

*异步计算*不能像同步计算那样，在启动时立即通过返回值或抛出异常来提供结果。异步计算可能需要等待程序外部的某些操作（读取文件、查询数据库、获取网页），这需要花费时间。异步计算不会阻塞所有计算直到结果可用，而是立即返回一个 `Future`，该 `Future` 会在*将来某个时刻*以结果“完成（complete）”。

### 异步编程

要执行异步计算，可以使用 `async` 函数，该函数总是产生一个 future。在这样的异步函数内部，可以使用 `await` 操作来延迟执行，直到另一个异步计算得到结果。当等待中的函数执行被延迟时，程序不会被阻塞，可以继续执行其他操作。

示例：

```dart
import "dart:io";
Future<bool> fileContains(String path, String needle) async {
   var haystack = await File(path).readAsString();
   return haystack.contains(needle);
}
```

这里，来自 `dart:io` 的 `File.readAsString` 方法是一个返回 `Future<String>` 的异步函数。`fileContains` 函数在函数体前用 `async` 标记，这意味着可以在其内部使用 `await`，并且该函数必须返回一个 future。调用 `File(path).readAsString()` 会启动将文件内容读取为字符串的操作，并生成一个 `Future<String>`，该 future 最终会包含结果。`await` 会等待该 future 完成并返回一个字符串（如果读取文件失败，则返回一个错误）。在等待期间，程序可以执行其他操作。当 future 以字符串完成时，`fileContains` 函数会计算出一个布尔值并返回，这样就会完成它最初被调用时返回的那个 future。

如果 future 以*错误（error）*完成，等待该 future 将会（重新）抛出该错误。在这个示例中，我们可以加入错误处理：

```dart
import "dart:io";
Future<bool> fileContains(String path, String needle) async {
  try {
    var haystack = await File(path).readAsString();
    return haystack.contains(needle);
  } on FileSystemException catch (exception, stack) {
    _myLog.logError(exception, stack);
    return false;
  }
}
```

可以使用普通的 `try`/`catch` 来捕获所等待的异步计算的失败。

一般来说，在编写异步代码时，应始终在产生 future 时立即等待它，而不要等到另一个异步延迟之后再等待。这可以确保你能够及时接收 future 可能产生的任何错误，这一点很重要，因为没有人等待的异步错误是*未捕获*的错误，可能会导致正在运行的程序终止。

### 使用 `Future` API 进行编程

`Future` 类还提供了更直接、底层的功能，用于访问其完成时的结果。`async` 和 `await` 语言特性正是构建在这一功能之上的，有时直接使用它是有意义的。有些事情无法仅通过一次 `await` 一个 future 来完成。

对于一个 [Future](https://www.yuque.com/thyname/dart.async/future)，可以手动注册回调，以便在值或错误可用时对其进行处理。例如：

```dart
Future<int> future = getFuture();
future.then((value) => handleValue(value))
      .catchError((error) => handleError(error));
```

由于 [Future](https://www.yuque.com/thyname/dart.async/future) 可以以两种方式完成——以值完成（如果异步计算成功）或以错误完成（如果计算失败）——因此可以为其中一种或两种情况安装回调。

在某些情况下，我们说一个 future 是*被另一个 future 完成的*。这是一种简短的说法，表示该 future 将以与另一个 future 相同的方式完成，一旦那个 future 本身完成，就会得到相同的值或错误。平台库中大多数完成 future 的函数（例如 [Completer.complete] 或 [Future.value]）也接受另一个 future 作为参数，并自动将结果转发给被完成的 future。

注册回调的结果本身也是一个 `Future`，它会以调用相应回调处理原 future 的结果所得到的结果完成。如果被调用的回调抛出异常，新的 future 会以错误完成。例如：

```dart
Future<int> successor = future.then((int value) {
    // Invoked when the future is completed with a value.
    return 42;  // The successor is completed with the value 42.
  },
  onError: (e) {
    // Invoked when the future is completed with an error.
    if (canHandle(e)) {
      return 499;  // The successor is completed with the value 499.
    } else {
      throw e;  // The successor is completed with the error e.
    }
  });
```

如果一个 future 在完成时没有注册任何处理器来处理错误，它会将错误转发给“未捕获错误处理器（uncaught-error handler）”。这种行为确保不会有错误被静默丢弃。但这也意味着应尽早安装错误处理器，以便在 future 以错误完成时处理器已经就位。以下示例演示了这种潜在的 bug：

```dart
var future = getFuture();
Timer(const Duration(milliseconds: 5), () {
  // The error-handler is not attached until 5 ms after the future has
  // been received. If the future fails before that, the error is
  // forwarded to the global error-handler, even though there is code
  // (just below) to eventually handle the error.
  future.then((value) { useValue(value); },
              onError: (e) { handleError(e); });
});
```

在注册回调时，通常更易读的做法是分别注册两个回调：先使用带一个参数（值处理器）的 [then]，再使用第二个 [catchError] 处理错误。这两者中的每一个都会将自己不处理的结果转发给它们的后继者，两者结合起来就能同时处理值和错误结果。这样做的额外好处是 [catchError] 也能处理 [then] 中值回调所抛出的错误。使用顺序处理器而非并行处理器往往能让代码更易于理解。它还能使异步代码非常类似于同步代码：

```dart
// Synchronous code.
try {
  int value = foo();
  return bar(value);
} catch (e) {
  return 499;
}
```

等效的基于 future 的异步代码：

```dart
Future<int> asyncValue = Future(foo);  // Result of foo() as a future.
asyncValue.then((int value) {
  return bar(value);
}).catchError((e) {
  return 499;
});
```

与同步代码类似，错误处理器（通过 [catchError] 注册）会处理 `foo` 或 `bar` 抛出的任何错误。如果错误处理器是作为 `then` 调用的 `onError` 参数注册的，它将无法捕获来自 `bar` 调用的错误。

Future 可以注册多组回调对。每个后继者都被独立处理，就好像它是唯一的后继者一样。各个后继者完成的顺序是不确定的。

一个 future 也可能永远不会完成。这种情况通常应尽量避免，除非有非常清晰的文档说明。

## 构造函数

### Future()

```dart
Future<T>(FutureOr<T> computation())
```

创建一个包含异步调用 [computation] 结果的 future（通过 [Timer.run] 实现）。

如果执行 [computation] 时抛出异常，返回的 future 将以该错误完成。

如果返回值本身是一个 [Future](https://www.yuque.com/thyname/dart.async/future)，创建的 future 的完成将等待该返回的 future 完成，然后以相同的结果完成。

如果返回的是非 future 值，返回的 future 将以该值完成。

### Future.microtask()

```dart
Future<T>.microtask(FutureOr<T> computation())
```

创建一个包含异步调用 [computation] 结果的 future（通过 [scheduleMicrotask](https://www.yuque.com/thyname/dart.async/schedulemicrotask) 实现）。

如果执行 [computation] 时抛出异常，返回的 future 将以抛出的错误完成。

如果调用 [computation] 返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，创建的 future 的完成将等待该返回的 future 完成，然后以相同的结果完成。

如果调用 [computation] 返回非 future 值，返回的 future 将以该值完成。

### Future.sync()

```dart
Future<T>.sync(FutureOr<T> Function() computation)
```

将调用 [computation] 的结果作为一个 future。

主要用于处理返回 `FutureOr<T>` 的函数，或在非 `async` 的异步函数中使用非 async 函数，并希望将任何错误捕获到一个 future 中的场景。

如果调用 [computation] 抛出异常，返回的 future 将以该错误完成。

如果调用 [computation] 返回一个 `Future<T>`，则直接返回该 future。

如果调用 [computation] 返回一个非 `Future<T>` 的值，将创建一个已以该值完成的 future。

示例：

```dart
Future<int> asyncAdd(
  FutureOr<int> Function() a,
  FutureOr<int> Function() b,
) =>
  (Future.sync(a), Future.sync(b)).wait.then((ab) => ab.$1 + ab.$2);
```

要创建一个已知值的 future，请改用 [Future.syncValue]，例如 `Future.syncValue(12)`。

### Future.syncValue()

```dart
@Since.new("3.10")
Future<T>.syncValue( T value )
```

创建一个以 [value] 完成的 future。

该 future 保证不会带有错误。

如果同步计算可能抛出异常，与其使用 `Future.syncValue(computation())` 并将其包裹在 `try`/`catch` 中，不如使用 [Future.sync]，它会将错误捕获到其返回的 future 中，如 `Future.sync(() => computation())`。

### Future.value()

```dart
Future<T>.value([FutureOr<T>? value])
```

创建一个以 [value] 完成的 future。

如果 [value] 是一个 future，创建的 future 会等待 [value] 这个 future 完成，然后以相同的结果完成。由于 [value] 这个 future 可能以错误完成，因此 [Future.value] 创建的 future 也可能以错误完成，尽管其名称暗示的并非如此。

如果 [value] 不是 [Future](https://www.yuque.com/thyname/dart.async/future)，创建的 future 将以 [value] 的值完成，等价于 `new Future<T>.sync(() => value)`。

如果省略 [value] 或其为 `null`，则会通过 `value as FutureOr<T>` 将其转换为 `FutureOr<T>`。如果 `T` 不可为空，则必须提供非 `null` 的 [value]，否则构造函数会抛出异常。

使用 [Completer](https://www.yuque.com/thyname/dart.async/completer) 可以先创建一个 future，之后再完成它。

示例：

```dart
Future<int> getFuture() {
 return Future<int>.value(2021);
}

final result = await getFuture();
```

### Future.error()

```dart
Future<T>.error(Object error, [StackTrace? stackTrace])
```

创建一个以错误完成的 future。

创建的 future 会在稍后的一个微任务（microtask）中以错误完成。这样可以留出足够的时间，让人为该 future 添加错误处理器。如果在 future 完成之前没有添加错误处理器，该错误将被视为未处理的错误。

使用 [Completer](https://www.yuque.com/thyname/dart.async/completer) 可以创建一个 future 并稍后完成它。

示例：

```dart
Future<int> getFuture() {
 return Future.error(Exception('Issue'));
}

final error = await getFuture(); // Throws.
```

### Future.delayed()

```dart
Future<T>.delayed(Duration duration, [FutureOr<T> Function()? computation])
```

创建一个延迟一段时间后运行其计算的 future。

[computation] 将在给定的 [duration] 过去后执行，future 将以该计算的结果完成。

如果 [computation] 返回一个 future，此构造函数返回的 future 将以该 future 的值或错误完成，这可能要等到更晚的时候才能得到。

如果 duration 为 0 或更小，它最早也要等到下一次事件循环迭代（在所有微任务运行完毕之后）才会完成。（等待延迟时使用的是在当前 zone 中创建的 [Timer](https://www.yuque.com/thyname/dart.async/timer)。）

computation 不能省略，也不能为 null。如果只是想等待一段时间，请改用 [Future.pause]。在此反映到函数签名之前，如果省略 [computation]，则 [T] 必须是可空的。此时 computation 会被当作 `() => null` 处理，future 最终将以 `null` 值完成。

如果调用 [computation] 抛出异常，创建的 future 将以该错误完成。

另请参阅 [Completer](https://www.yuque.com/thyname/dart.async/completer)，了解如何在已知的固定延迟之后的某个时刻创建并完成一个 future。

示例：

```dart
var now = DateTime.now();
var later = await Future.delayed(const Duration(seconds: 1), () {
  return DateTime.timestamp();
});
print(now.difference(later)); // At least a second.
```

## 静态方法

### pause()

```dart
Future<void> pause([Duration duration = Duration.zero])
```

创建一个在 [duration] 之后完成且没有结果的 [Future](https://www.yuque.com/thyname/dart.async/future)。

类似于 [Future.delayed]，但不执行任何操作，也不会以错误完成。

### wait()

```dart
Future<List<T>> wait<T>(
  Iterable<Future<T>> futures, {
  bool eagerError = false,
  void cleanUp(T successValue)
})
```

等待多个 future 完成并收集它们的结果。

返回一个 future，该 future 会在所有提供的 future 都完成后完成，完成时携带它们的结果，或者如果其中任何一个提供的 future 失败，则携带错误完成。

返回的 future 的值将是一个列表，其中包含按照迭代 [futures] 时提供的顺序产生的所有值。

如果任何一个 future 以错误完成，则返回的 future 将以该错误完成。如果之后还有其他 future 也以错误完成，那些错误会被丢弃。

如果 `eagerError` 为 true，返回的 future 会在其中一个 future 首次出现错误时立即以该错误完成。否则，必须等待所有 future 都完成后返回的 future 才会完成（仍然以第一个错误完成；其余错误会被静默丢弃）。

在发生错误的情况下，如果提供了 [cleanUp]，会对任何成功完成的 future 的非 null 结果调用它。这样可以清理那些原本会丢失的资源（因为返回的 future 不会提供对这些值的访问）。如果没有发生错误，[cleanUp] 函数不会被使用。

对 [cleanUp] 的调用不应抛出异常。如果抛出异常，该错误将成为一个未捕获的异步错误。

示例：

```dart
void main() async {
  var value = await Future.wait([delayedNumber(), delayedString()]);
  print(value); // [2, result]
}

Future<int> delayedNumber() async {
  await Future.delayed(const Duration(seconds: 2));
  return 2;
}

Future<String> delayedString() async {
  await Future.delayed(const Duration(seconds: 2));
  return 'result';
}
```

### any()

```dart
Future<T> any<T>(Iterable<Future<T>> futures)
```

返回 [futures] 中第一个完成的 future 的结果。

返回的 future 会以 [futures] 中第一个报告完成的 future 的结果完成，无论是以值还是以错误完成。所有其他 future 的结果都会被丢弃。

如果 [futures] 为空，或者其中没有任何一个 future 完成，返回的 future 将永远不会完成。

示例：

```dart
void main() async {
  final result =
      await Future.any([slowInt(), delayedString(), fastInt()]);
  // The future of fastInt completes first, others are ignored.
  print(result); // 3
}
Future<int> slowInt() async {
  await Future.delayed(const Duration(seconds: 2));
  return 2;
}

Future<String> delayedString() async {
  await Future.delayed(const Duration(seconds: 2));
  throw TimeoutException('Time has passed');
}

Future<int> fastInt() async {
  await Future.delayed(const Duration(seconds: 1));
  return 3;
}
```

### forEach()

```dart
Future<void> forEach<T>(Iterable<T> elements, FutureOr action(T element))
```

依次对可迭代对象的每个元素执行一个操作。

[action] 可以是同步的，也可以是异步的。

按顺序对 [elements] 中的每个元素调用 [action]。如果对 [action] 的调用返回一个 `Future<T>`，迭代会等待该 future 完成后再继续处理下一个元素。

返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，当所有元素都处理完毕后以 `null` 完成。

非 [Future](https://www.yuque.com/thyname/dart.async/future) 的返回值，以及返回的 [Future](https://www.yuque.com/thyname/dart.async/future) 的完成值，都会被丢弃。

[action] 抛出的任何错误（无论是同步还是异步）都会中止迭代，并在返回的 [Future](https://www.yuque.com/thyname/dart.async/future) 中报告。

### doWhile()

```dart
Future<void> doWhile(FutureOr<bool> action())
```

重复执行一个操作，直到它返回 `false`。

操作 [action] 可以是同步的，也可以是异步的。

只要该操作返回布尔值 `true`，或返回一个最终以 `true` 完成的 `Future<bool>`，该操作就会被重复调用。

如果对 [action] 的某次调用返回 `false`，或返回的 [Future](https://www.yuque.com/thyname/dart.async/future) 最终以 `false` 完成，迭代结束，[doWhile] 返回的 future 将以 `null` 值完成。

如果对 [action] 的调用抛出异常，或 [action] 返回的 future 以错误完成，迭代结束，[doWhile] 返回的 future 将以相同的错误完成。

对 [action] 的调用可能在任何时间发生，包括在调用 `doWhile` 之后立即发生。唯一的限制是：在前一次调用返回之前，不会发生新的 [action] 调用；如果前一次调用返回的是 `Future<bool>`，则要等到该 future 完成之后才会发生新的调用。

示例：

```dart
void main() async {
  var value = 0;
  await Future.doWhile(() async {
    value++;
    await Future.delayed(const Duration(seconds: 1));
    if (value == 3) {
      print('Finished with $value');
      return false;
    }
    return true;
  });
}
// Outputs: 'Finished with 3'
```

## 方法

### then()

```dart
Future<R> then<R>(
  FutureOr<R> onValue( T value ), {
  Function? onError,
})
```

注册当此 future 完成时要调用的回调。

当此 future 以值完成时，将使用该值调用 [onValue] 回调。如果此 future 已经完成，回调不会立即被调用，而是会被安排在稍后的一个微任务中调用。

如果提供了 [onError]，并且此 future 以错误完成，`onError` 回调会被调用，并传入该错误及其堆栈跟踪。`onError` 回调必须接受一个参数，或者接受两个参数（其中第二个是 [StackTrace](https://www.yuque.com/thyname/dart.core/stacktrace)）。如果 `onError` 接受两个参数，调用时会同时传入错误和堆栈跟踪，否则只传入错误对象。`onError` 回调必须返回一个可用于完成返回的 future 的值或 future，因此它必须是可赋值给 `FutureOr<R>` 的内容。

返回一个新的 [Future](https://www.yuque.com/thyname/dart.async/future)，它将以调用 `onValue`（如果此 future 以值完成）或调用 `onError`（如果此 future 以错误完成）的结果完成。

如果被调用的回调抛出异常，返回的 future 将以抛出的错误及其堆栈跟踪完成。对于 `onError` 而言，如果抛出的异常与传给 `onError` 的错误参数是 `identical` 的，并且是*同步*抛出的，则该抛出会被视为一次重新抛出（rethrow），并使用原始的堆栈跟踪。要在异步回调中以相同的堆栈跟踪重新抛出，请使用 [Error.throwWithStackTrace]。

如果回调返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，`then` 返回的 future 将以该回调返回的 future 相同的结果完成。

如果未提供 [onError]，并且此 future 以错误完成，该错误会被直接转发给返回的 future。

在大多数情况下，单独使用 [catchError]（可能带有 `test` 参数）来分别处理值和错误，比在一次 [then] 调用中同时处理两者更具可读性。

请注意，future 不会延迟报告错误直到有监听器被添加。如果第一次调用 `then` 或 `catchError` 发生在此 future 已经以错误完成之后，那么该错误会被报告为未处理的错误。参见 [Future](https://www.yuque.com/thyname/dart.async/future) 的说明。

### catchError()

```dart
Future<T> catchError(
  Function onError, {
  bool test( Object error )?,
})
```

处理此 [Future](https://www.yuque.com/thyname/dart.async/future) 抛出的错误。

这是“catch”代码块的异步等价形式。

返回一个新的 [Future](https://www.yuque.com/thyname/dart.async/future)，它将以此 future 的结果，或调用 `onError` 回调的结果完成。

如果此 future 以值完成，返回的 future 将以相同的值完成。

如果此 future 以错误完成，则首先会用该错误值调用 [test]。

如果 `test` 返回 false，此异常不会被此 `catchError` 处理，返回的 future 将以与此 future 相同的错误和堆栈跟踪完成。

如果 `test` 返回 `true`，会用错误（可能还有堆栈跟踪）调用 [onError]，返回的 future 将以此次调用的结果完成，方式与 [then] 的 `onError` 完全相同。

如果省略 `test`，则默认为一个始终返回 true 的函数。`test` 函数不应抛出异常，但如果确实抛出了，会被当作 `onError` 函数抛出异常来处理。

请注意，future 不会延迟报告错误直到有监听器被添加。如果第一次调用 `catchError`（或 `then`）发生在此 future 已经以错误完成之后，那么该错误会被报告为未处理的错误。参见 [Future](https://www.yuque.com/thyname/dart.async/future) 的说明。

示例：

```dart
Future.delayed(
  const Duration(seconds: 1),
  () => throw 401,
).then((value) {
  throw 'Unreachable';
}).catchError((err) {
  print('Error: $err'); // Prints 401.
}, test: (error) {
  return error is int && error >= 400;
});
```

### whenComplete()

```dart
Future<T> whenComplete(FutureOr<void> action())
```

注册一个函数，在此 future 完成时调用。

无论此 future 是以值完成还是以错误完成，[action] 函数都会在其完成时被调用。

这是“finally”代码块的异步等价形式。

此调用返回的 future（记为 `f`）将以与此 future 相同的方式完成，除非在调用 [action] 时，或在 [action] 调用返回的 [Future](https://www.yuque.com/thyname/dart.async/future) 中发生了错误。如果对 [action] 的调用没有返回 future，其返回值会被忽略。

如果对 [action] 的调用抛出异常，则 `f` 将以抛出的错误完成。

如果对 [action] 的调用返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)（记为 `f2`），则 `f` 的完成会延迟，直到 `f2` 完成。如果 `f2` 以错误完成，该错误也将成为 `f` 的结果。`f2` 的值始终会被忽略。

此方法等价于：

```dart
Future<T> whenComplete(action()) {
  return this.then((v) {
    var f2 = action();
    if (f2 is Future) return f2.then((_) => v);
    return v;
  }, onError: (e) {
    var f2 = action();
    if (f2 is Future) return f2.then((_) { throw e; });
    throw e;
  });
}
```

示例：

```dart
void main() async {
  var value =
      await waitTask().whenComplete(() => print('do something here'));
  // Prints "do something here" after waitTask() completed.
  print(value); // Prints "done"
}

Future<String> waitTask() {
  Future.delayed(const Duration(seconds: 5));
  return Future.value('done');
}
// Outputs: 'do some work here' after waitTask is completed.
```

### asStream()

```dart
Stream<T> asStream()
```

创建一个包含此 future 结果的 [Stream](https://www.yuque.com/thyname/dart.async/stream)。

该流会产生一个单独的数据事件或错误事件，其中包含此 future 的完成结果，然后以一个 done 事件关闭。

如果此 future 永远不完成，该流将不会产生任何事件。

### timeout()

```dart
Future<T> timeout(
  Duration timeLimit, {
  FutureOr<T> onTimeout()?,
})
```

在经过 [timeLimit] 之后停止等待此 future。

创建一个新的*超时 future*，如果此 future（即*源 future*）能及时完成，超时 future 将以与其相同的结果完成。

如果源 future 未能在 [timeLimit] 之内完成，将执行 [onTimeout] 操作，并将其结果（无论是返回还是抛出）用作超时 future 的结果。[onTimeout] 函数必须返回一个 [T] 或 `Future<T>`。如果 [onTimeout] 返回一个 future（即*替代结果 future*），即使源 future 在替代结果 future 之前完成，超时 future 最终也将以替代结果 future 的结果完成。这里唯一重要的是源 future 没有及时完成。

如果省略 `onTimeout`，超时将导致返回的 future 以 [TimeoutException](https://www.yuque.com/thyname/dart.async/timeoutexception) 完成。

无论哪种情况，源 future 之后仍可以正常完成。只是除非它在时间限制内完成，否则不会被用作超时 future 的结果。即使源 future 以错误完成，如果该错误发生在 [timeLimit] 之后，该错误也会被忽略，就像值结果一样。

示例：

```dart
void main() async {
  var result =
      await waitTask("completed").timeout(const Duration(seconds: 10));
  print(result); // Prints "completed" after 5 seconds.

  result = await waitTask("completed")
      .timeout(const Duration(seconds: 1), onTimeout: () => "timeout");
  print(result); // Prints "timeout" after 1 second.

  result = await waitTask("first").timeout(const Duration(seconds: 2),
      onTimeout: () => waitTask("second"));
  print(result); // Prints "second" after 7 seconds.

  try {
    await waitTask("completed").timeout(const Duration(seconds: 2));
  } on TimeoutException {
    print("throws"); // Prints "throws" after 2 seconds.
  }

  var printFuture = waitPrint();
  await printFuture.timeout(const Duration(seconds: 2), onTimeout: () {
    print("timeout"); // Prints "timeout" after 2 seconds.
  });
  await printFuture; // Prints "printed" after additional 3 seconds.

  try {
    await waitThrow("error").timeout(const Duration(seconds: 2));
  } on TimeoutException {
    print("throws"); // Prints "throws" after 2 seconds.
  }
  // StateError is ignored
}

/// Returns [string] after five seconds.
Future<String> waitTask(String string) async {
  await Future.delayed(const Duration(seconds: 5));
  return string;
}

/// Prints "printed" after five seconds.
Future<void> waitPrint() async {
  await Future.delayed(const Duration(seconds: 5));
  print("printed");
}
/// Throws a [StateError] with [message] after five seconds.
Future<void> waitThrow(String message) async {
  await Future.delayed(const Duration(seconds: 5));
  throw Exception(message);
}
```

---

# unawaited()

```dart
@Since.new("2.15")
void unawaited( Future<void>? future )
```

明确忽略一个 future。

并非所有的 future 都需要被等待。Dart 语法检查工具（linter）有一个可选的 ["unawaited futures" lint](https://dart.dev/lints/unawaited_futures)，用于强制要求异步函数中潜在的 future（静态类型为 [Future](https://www.yuque.com/thyname/dart.async/future) 或 `Future?` 的表达式）必须以某种方式被处理。如果某个特定的 future 值不需要被等待，可以对它调用 `unawaited(...)`，这样可以避免触发该 lint，因为该表达式不再具有 [Future](https://www.yuque.com/thyname/dart.async/future) 类型。使用 `unawaited` 没有其他效果。应使用 `unawaited` 来表明*有意*不等待该 future 的意图。

如果该 future 以错误完成，很可能表明不等待它是一个错误的做法。该错误仍然会发生，并且会被视为未处理，除非在其他地方也等待（或以其他方式处理）了同一个 future。正因如此，`unawaited` 应仅用于*预期*会以值完成的 future。如果你还想忽略此 future 可能产生的错误，可以使用 [FutureExtensions.ignore]。

---

# FutureExtensions

```dart
extension FutureExtensions<T> on Future<T> {}
```

Future 上的便捷方法。

为 future 添加了功能，使编写类型良好的异步代码变得更容易。

## 方法

### onError()

```dart
Future<T> onError<E extends Object>(
  FutureOr<T> handleError( E error, StackTrace stackTrace ), {
  bool test( E error )?,
})
```

处理此 future 上的错误。

捕获此 future 可能以其完成的 [E] 类型的错误。如果提供了 [test]，则只捕获 [test] 返回 `true` 的 [E] 类型错误。如果 [E] 是 [Object](https://www.yuque.com/thyname/dart.core/object)，则所有错误都有可能被捕获，具体取决于所提供的 [test]。

如果错误被捕获，返回的 future 将以调用 [handleError]（传入错误和堆栈跟踪）的结果完成。此结果必须是此 future 原本可以完成的相同类型的值。例如，如果此 future 不能以 `null` 完成，那么 [handleError] 也不能返回 `null`。示例：

```dart
Future<T> retryOperation<T>(Future<T> operation(), T onFailure()) =>
    operation().onError<RetryException>((e, s) {
      if (e.canRetry) {
        return retryOperation(operation, onFailure);
      }
      return onFailure();
    });
```

如果 [handleError] 抛出异常，返回的 future 将以抛出的错误和堆栈跟踪完成，但如果它再次抛出*相同的*错误对象，则会被视为一次“重新抛出（rethrow）”，并保留原始的堆栈跟踪。这可以作为在 [test] 中跳过错误的一种替代方式。示例：

```dart
// Unwraps an exceptions cause, if it has one.
someFuture.onError<SomeException>((e, _) {
  throw e.cause ?? e;
});
// vs.
someFuture.onError<SomeException>((e, _) {
  throw e.cause!;
}, test: (e) => e.cause != null);
```

如果错误未被捕获，返回的 future 将以与此 future 相同的结果（值或错误）完成。

这个方法实际上是 [Future.catchError] 的一个类型更精确的版本。它使得捕获特定错误类型变得容易，并要求提供一个类型正确的错误处理函数，而不仅仅是 [Function](https://www.yuque.com/thyname/dart.core/function)。正因为如此，错误处理器必须接受堆栈跟踪参数。

### ignore()

```dart
@Since.new("2.14")
void ignore()
```

完全忽略此 future 及其结果。

并非所有的 future 都重要，即使它们包含错误也是如此，例如某个请求已经发出，但其响应已不再需要。仅仅忽略一个 future 可能会导致未捕获的异步错误。此方法则会处理（并忽略）来自此 future 的任何值或错误，从而可以安全地忽略该 future。

使用 `ignore` 来表明此 future 的结果对程序而言已不再重要，即使它是一个错误也是如此。如果你只是想消除 ["unawaited futures" lint](https://dart.dev/lints/unawaited_futures)，请改用 [unawaited](https://www.yuque.com/thyname/dart.async/unawaited) 函数，这样可以确保意外的错误仍会被报告。

---

# TimeoutException

```dart
class TimeoutException implements Exception {}
```

在等待异步结果时发生了计划中的超时，此时抛出该异常。

### message

```dart
String? message
```

描述超时原因的说明。

### duration

```dart
Duration? duration
```

被超出的时长。

### TimeoutException()

```dart
TimeoutException(String? message, [Duration? duration])
```

---

# Completer

```dart
abstract interface class Completer<T> {}
```

一种生成 Future 对象，并在稍后以值或错误完成它们的方式。

大多数情况下，创建一个 future 最简单的方式是使用 [Future](https://www.yuque.com/thyname/dart.async/future) 的某个构造函数来捕获单个异步计算的结果：

```dart
Future(() { doSomething(); return result; });
```

或者，如果该 future 表示一系列异步计算的结果，可以使用 [Future.then] 或 [Future](https://www.yuque.com/thyname/dart.async/future) 上的类似函数将它们串联起来：

```dart
Future doStuff(){
  return someAsyncOperation().then((result) {
    return someOtherAsyncOperation(result);
  });
}
```

如果你确实需要从零开始创建一个 Future——例如，在将基于回调的 API 转换为基于 Future 的 API 时——可以按如下方式使用 Completer：

```dart
class AsyncOperation {
  final Completer _completer = new Completer();

  Future<T> doOperation() {
    _startOperation();
    return _completer.future; // Send future object back to client.
  }

  // Something calls this when the value is ready.
  void _finishOperation(T result) {
    _completer.complete(result);
  }

  // If something goes wrong, call this.
  void _errorHappened(error) {
    _completer.completeError(error);
  }
}
```

## 构造函数

### Completer()

```dart
Completer<T>()
```

创建一个新的 completer。

创建新 future 的一般流程是：1）创建一个新的 completer；2）交出其 future；3）在稍后的某个时刻调用 [complete] 或 [completeError]。

completer 会异步完成 future。这意味着注册在该 future 上的回调不会在调用 [complete] 或 [completeError] 时立即被调用，而是会延迟到稍后的一个微任务中。

示例：

```dart
var completer = new Completer();
handOut(completer.future);
later: {
  completer.complete('completion value');
}
```

### Completer.sync()

```dart
Completer<T>.sync()
```

同步完成该 future。

除非该 future 的完成已知是另一个异步操作的最终结果，否则应避免使用此构造函数。如有疑问，请使用默认的 [Completer](https://www.yuque.com/thyname/dart.async/completer) 构造函数。

使用普通的异步 completer 永远不会产生错误的行为，但错误地使用同步 completer 可能会导致原本正确的程序出现问题。

同步 completer 仅用于在一个异步事件立即触发另一个事件时优化事件传播。除非能保证对 [complete] 和 [completeError] 的调用发生在不会破坏 `Future` 约定的位置，否则不应使用它。

同步完成意味着：在同步 completer 上调用 [complete] 或 [completeError] 方法时，该 completer 的 future 会立即完成，同时也会立即调用注册在该 future 上的所有回调。

同步完成绝不能违反这样一条规则：当你在一个 future 上添加回调时，该回调不能在添加它的代码执行完毕之前被调用。因此，同步完成只能发生在另一个同步事件的最末尾（“尾部位置”），因为此时立即完成该 future 等价于返回事件循环并在下一个微任务中完成该 future。

示例：

```dart
var completer = Completer.sync();
// The completion is the result of the asynchronous onDone event.
// No other operation is performed after the completion. It is safe
// to use the Completer.sync constructor.
stream.listen(print, onDone: () { completer.complete("done"); });
```

错误示例。请勿使用此代码。仅用于说明目的：

```dart
var completer = Completer.sync();
completer.future.then((_) { bar(); });
// The completion is the result of the asynchronous onDone event.
// However, there is still code executed after the completion. This
// operation is *not* safe.
stream.listen(print, onDone: () {
  completer.complete("done");
  foo();  // In this case, foo() runs after bar().
});
```

## 属性

### future

```dart
Future<T> get future
```

由此 completer 完成的 future。

当调用 [complete] 或 [completeError] 时会完成的 future。

### isCompleted

```dart
bool get isCompleted
```

[future] 是否已经完成。

反映是否已经调用了 [complete] 或 [completeError]。值为 `true` 并不一定意味着此 future 的监听器已经被调用，这可能是因为 completer 通常要等到稍后的一个微任务才会传播结果，也可能是因为调用 [complete] 时传入的 future 尚未完成。

当此值为 `true` 时，不得再次调用 [complete] 和 [completeError]。

## 方法

### complete()

```dart
void complete([FutureOr<T>? value])
```

以提供的值完成 [future]。

该值必须是 [T] 类型的值，或者是 `Future<T>` 类型的 future。如果省略该值或其为 `null`，且 `T` 不可为空，则调用 `complete` 会抛出异常。

如果该值本身是一个 future，completer 将等待该 future 完成，然后以相同的结果（无论是成功还是错误）完成。

调用 [complete] 或 [completeError] 最多只能进行一次。

future 的所有监听器都会被告知该值。

### completeError()

```dart
void completeError(Object error, [StackTrace? stackTrace])
```

以错误完成 [future]。

调用 [complete] 或 [completeError] 最多只能进行一次。

以错误完成一个 future 表示在尝试产生值的过程中抛出了异常。

如果 [error] 是一个 [Future](https://www.yuque.com/thyname/dart.async/future)，则将该 future 本身用作错误值。如果你想以该 future 的结果完成，可以使用：

```dart
thisCompleter.complete(theFuture)
```

或者，如果你只想处理来自该 future 的错误，可以使用：

```dart
theFuture.catchError(thisCompleter.completeError);
```

在调用 [completeError] 之前，[future] 必须已安装错误处理器，否则 [error] 会被视为未捕获的错误。

```dart
void doStuff() {
  // Outputs a message like:
  // Uncaught Error: Assertion failed: "future not consumed"
  Completer().completeError(AssertionError('future not consumed'));
}
```

你可以通过 [Future.catchError]、[Future.then] 或 `await` 操作来安装错误处理器。

```dart
void doStuff() {
  final c = Completer();
  c.future.catchError((e) {
    // Handle the error.
  });
  c.completeError(AssertionError('future not consumed'));
}
```

有关未捕获错误的详细信息，请参阅 [Zones 文章](https://dart.dev/articles/archive/zones#handling-uncaught-errors)。
