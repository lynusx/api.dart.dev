# Zone

```dart
abstract final class Zone {}
```

区域（Zone）表示一个在异步调用过程中保持稳定的环境。

所有代码都在某个区域的上下文中执行，代码可通过 [Zone.current] 获取该区域。初始的 `main` 函数运行在默认区域（[Zone.root]）的上下文中。可以使用 [runZoned](https://www.yuque.com/thyname/dart.async/runzoned) 或 [runZonedGuarded](https://www.yuque.com/thyname/dart.async/runzonedguarded) 创建新区域并在其中运行代码，从而在不同的区域中运行代码；也可以使用 [Zone.run] 在此前可能已通过 [Zone.fork] 创建的现有区域的上下文中运行代码。

开发者可以创建一个新区域，以覆盖现有区域的部分功能。例如，自定义区域可以替换或修改 `print`、计时器、微任务，或未捕获错误的处理方式。

[Zone](https://www.yuque.com/thyname/dart.async/zone) 类不可被子类化，但用户可以通过使用 [ZoneSpecification](https://www.yuque.com/thyname/dart.async/zonespecification) 派生（fork）现有区域（通常是 [Zone.current]）来提供自定义区域。这类似于创建一个继承自基类 `Zone` 并重写部分方法的新类，只不过实际上并未创建新类，而是将重写的方法作为函数提供，这些函数显式地接收相当于自身类、"super" 类和 `this` 对象的参数。

异步回调始终在其被调度时所在区域的上下文中运行。这通过以下两个步骤实现：

1.  首先使用 [registerCallback]、[registerUnaryCallback] 或 [registerBinaryCallback] 之一注册回调。这使得区域能够记录某个回调的存在，并可能对其进行修改（通过返回不同的回调）。执行注册的代码（例如 `Future.then`）还会记住当前区域，以便之后能在该区域中运行该回调。
2.  之后，已注册的回调将使用 [run]、[runUnary] 或 [runBinary] 之一，在所记住的区域中运行。

这一切都由平台代码在内部处理，大多数用户无需为此担心。然而，为底层系统提供新异步操作的开发者，必须遵循该协议才能实现区域兼容性。

为方便起见，区域提供了 [bindCallback]（及相应的 [bindUnaryCallback] 和 [bindBinaryCallback]），以便更容易地遵守区域约定：这些函数首先调用相应的 `register` 函数，然后包装返回的函数，使其在稍后被异步调用时运行于当前区域。

类似地，当回调应通过 [Zone.runGuarded] 调用时，区域提供了 [bindCallbackGuarded]（及相应的 [bindUnaryCallbackGuarded] 和 [bindBinaryCallbackGuarded]）。

## 静态属性

### root

```dart
Zone root
```

根区域。

所有 isolate 入口函数（`main` 或被派生的函数）都从根区域开始运行（即在调用入口函数时，[Zone.current] 与 [Zone.root] 相同）。若未创建自定义区域，程序的其余部分将始终运行在根区域中。

根区域实现了所有区域操作的默认行为。许多方法（如 [registerCallback]）仅执行该函数所需的最基本操作，仅作为供自定义区域使用的钩子而提供；而其他方法（如 [scheduleMicrotask](https://www.yuque.com/thyname/dart.async/schedulemicrotask)）则与底层系统交互，以实现所需的行为。

### current

```dart
Zone get current
```

当前活动的区域。

## 属性

### parent

```dart
Zone? get parent
```

此区域的父区域。

若 `this` 为 [root] 区域，则为 `null`。

区域是通过在现有区域上调用 [fork]，或通过派生（fork）[current] 区域的 [runZoned](https://www.yuque.com/thyname/dart.async/runzoned) 创建的。新区域的父区域即为其派生自的区域。

### errorZone

```dart
Zone get errorZone
```

错误区域负责处理未捕获的错误。

它是此区域中最近的、提供了 [handleUncaughtError] 方法的父区域。

future 中的异步错误永远不会跨越具有不同错误处理程序的区域边界。

示例：

```dart
import 'dart:async';

main() {
  var future;
  runZonedGuarded(() {
    // The asynchronous error is caught by the custom zone which prints
    // 'asynchronous error'.
    future = Future.error("asynchronous error");
  }, (error) { print(error); });  // Creates a zone with an error handler.
  // The following `catchError` handler is never invoked, because the
  // custom zone created by the call to `runZonedGuarded` provides an
  // error handler.
  future.catchError((error) { throw "is never reached"; });
}
```

另请注意，错误同样无法进入具有不同错误处理程序的子区域：

```dart
import 'dart:async';

main() {
  runZonedGuarded(() {
    // The following asynchronous error is *not* caught by the `catchError`
    // in the nested zone, since errors are not to cross zone boundaries
    // with different error handlers.
    // Instead the error is handled by the current error handler,
    // printing "Caught by outer zone: asynchronous error".
    var future = Future.error("asynchronous error");
    runZonedGuarded(() {
      future.catchError((e) { throw "is never reached"; });
    }, (error, stack) { throw "is never reached"; });
  }, (error, stack) { print("Caught by outer zone: $error"); });
}
```

## 方法

### handleUncaughtError()

```dart
void handleUncaughtError(Object error, StackTrace stackTrace)
```

处理未捕获的异步错误。

此函数处理两种类型的异步错误：

1.  在异步回调中抛出但未被捕获的错误，例如传递给 [Timer.run] 的函数中的 `throw`。
2.  通过 [Future](https://www.yuque.com/thyname/dart.async/future) 和 [Stream](https://www.yuque.com/thyname/dart.async/stream) 链传递、但没有人为其注册错误处理程序的异步错误。大多数异步类（如 [Future](https://www.yuque.com/thyname/dart.async/future) 或 [Stream](https://www.yuque.com/thyname/dart.async/stream)）会将错误推送给其监听者。错误将以此方式持续传播，直到某个监听者处理该错误（例如通过 [Future.catchError]），或不再有可用的监听者为止。在后一种情况下，future 和 stream 将调用该区域的 [handleUncaughtError]。

默认情况下，当由根区域处理时，未捕获的异步错误将被视为未捕获的同步异常处理。

### inSameErrorZone()

```dart
bool inSameErrorZone(Zone otherZone)
```

此区域与 [otherZone] 是否处于同一错误区域。

若两个区域拥有相同的 [errorZone]，则它们处于同一错误区域。

### fork()

```dart
Zone fork({
  ZoneSpecification? specification,
  Map<Object?, Object?>? zoneValues
})
```

创建一个新区域，作为此区域的子区域。

新区域使用给定 [specification] 中的闭包来覆盖父区域的行为。规范中所有为 `null` 的条目均继承自父区域（`this`）的行为。

新区域继承此区域已存储的值（通过 [operator []] 访问），并使用来自 [zoneValues] 的值对其进行更新，这些值既可以添加新值，也可以覆盖已有的值。

请注意，派生（fork）操作是可拦截的。因此，区域可以更改区域规范（或区域值），从而使父区域完全控制子区域。

### run()

```dart
R run<R>(R action())
```

在此区域中执行 [action]。

默认情况下（如 [root] 区域中的实现），在将 [current] 设置为此区域的情况下运行 [action]。

若 [action] 抛出异常，该同步异常不会被区域的错误处理程序捕获。如需实现此效果，请使用 [runGuarded]。

由于根区域是唯一能够修改 [current] 值的区域，拦截 run 的自定义区域应始终委托给其父区域，可以在调用前后执行相应操作。

### runUnary()

```dart
R runUnary<R, T>(R action(T argument), T argument)
```

在此区域中使用 [argument] 执行给定的 [action]。

与 [run] 相同，区别在于 [action] 被调用时带有一个 [argument]，而非不带参数。

### runBinary()

```dart
R runBinary<R, T1, T2>(R action(T1 argument1, T2 argument2), T1 argument1, T2 argument2)
```

在此区域中使用 [argument1] 和 [argument2] 执行给定的 [action]。

与 [run] 相同，区别在于 [action] 被调用时带有两个参数，而非不带参数。

### runGuarded()

```dart
void runGuarded(void action())
```

在此区域中执行给定的 [action]，并捕获同步错误。

此函数等价于：

```dart
try {
  this.run(action);
} catch (e, s) {
  this.handleUncaughtError(e, s);
}
```

参见 [run]。

### runUnaryGuarded()

```dart
void runUnaryGuarded<T>(void action(T argument), T argument)
```

在此区域中使用 [argument] 执行给定的 [action]，并捕获同步错误。

参见 [runGuarded]。

### runBinaryGuarded()

```dart
void runBinaryGuarded<T1, T2>(
  void action(T1 argument1, T2 argument2),
  T1 argument1,
  T2 argument2
)
```

在此区域中使用 [argument1] 和 [argument2] 执行给定的 [action]，并捕获同步错误。

参见 [runGuarded]。

### registerCallback()

```dart
ZoneCallback<R> registerCallback<R>(R callback())
```

在此区域中注册给定的回调。

在实现使用回调的异步原语时，必须在用户提供回调的位置使用 [registerCallback] 对其进行注册。这使得区域能够同时记录其所需的其他信息，甚至可以包装该回调，以便在之后于同一区域中运行该回调时（使用 [run]）做好准备。例如，区域可以选择在注册时将堆栈跟踪与回调一同存储。

返回应替代所提供 [callback] 使用的回调。多数情况下，区域只是简单地返回原始回调。

自定义区域可以拦截此操作。[Zone.root] 中的默认实现将原封不动地返回原始回调。

### registerUnaryCallback()

```dart
ZoneUnaryCallback<R, T> registerUnaryCallback<R, T>(R callback(T arg))
```

在此区域中注册给定的回调。

与 [registerCallback] 类似，但使用一元回调。

### registerBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> registerBinaryCallback<R, T1, T2>(R callback(T1 arg1, T2 arg2))
```

在此区域中注册给定的回调。

与 [registerCallback] 类似，但使用二元回调。

### bindCallback()

```dart
ZoneCallback<R> bindCallback<R>(R callback())
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

等价于：

```dart
ZoneCallback registered = this.registerCallback(callback);
return () => this.run(registered);
```

### bindUnaryCallback()

```dart
ZoneUnaryCallback<R, T> bindUnaryCallback<R, T>(R callback(T argument))
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

等价于：

```dart
ZoneCallback registered = this.registerUnaryCallback(callback);
return (arg) => this.runUnary(registered, arg);
```

### bindBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> bindBinaryCallback<R, T1, T2>(R callback(T1 argument1, T2 argument2))
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

等价于：

```dart
ZoneCallback registered = registerBinaryCallback(callback);
return (arg1, arg2) => this.runBinary(registered, arg1, arg2);
```

### bindCallbackGuarded()

```dart
void Function() bindCallbackGuarded(void Function() callback)
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

当该函数执行时，错误将被捕获并被视为未捕获错误处理。

等价于：

```dart
ZoneCallback registered = this.registerCallback(callback);
return () => this.runGuarded(registered);
```

### bindUnaryCallbackGuarded()

```dart
void Function(T) bindUnaryCallbackGuarded<T>(void callback(T argument))
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

当该函数执行时，错误将被捕获并被视为未捕获错误处理。

等价于：

```dart
ZoneCallback registered = this.registerUnaryCallback(callback);
return (arg) => this.runUnaryGuarded(registered, arg);
```

### bindBinaryCallbackGuarded()

```dart
void Function(T1, T2) bindBinaryCallbackGuarded<T1, T2>(void callback(T1 argument1, T2 argument2))
```

注册所提供的 [callback]，并返回一个将在此区域中执行的函数。

等价于：

```dart
 ZoneCallback registered = registerBinaryCallback(callback);
 return (arg1, arg2) => this.runBinaryGuarded(registered, arg1, arg2);
```

### errorCallback()

```dart
AsyncError? errorCallback(Object error, StackTrace? stackTrace)
```

在以编程方式向 [Future](https://www.yuque.com/thyname/dart.async/future) 或 [Stream](https://www.yuque.com/thyname/dart.async/stream) 添加错误时对其进行拦截。

在调用 [Completer.completeError]、[StreamController.addError] 或某些 [Future](https://www.yuque.com/thyname/dart.async/future) 构造函数时，当前区域可以拦截并替换该错误。

当错误被直接接收时（例如使用 [Future.error]），或当错误被同步捕获时（例如使用 [Future.sync]），Future 构造函数将调用此函数。

无法保证某个错误只会通过 [errorCallback] 发送一次。使用中间控制器或 completer 的库最终可能会多次调用 [errorCallback]。

若不希望进行替换，则返回 `null`；否则返回一个 [AsyncError](https://www.yuque.com/thyname/dart.async/asyncerror) 实例，其中包含新的错误及堆栈跟踪对。

自定义区域可以拦截此操作。

将同步错误转换为异步错误的新异步原语实现很少需要调用 [errorCallback]，因为错误通常是通过 future 的 completer 或 stream 的 controller 报告的。

### scheduleMicrotask()

```dart
void scheduleMicrotask(void Function() callback)
```

在此区域中异步运行 [callback]。

全局的 `scheduleMicrotask` 委托给 [current] 区域的 [scheduleMicrotask](https://www.yuque.com/thyname/dart.async/schedulemicrotask)。根区域的实现与底层系统交互，将给定的回调作为微任务进行调度。

自定义区域可以拦截此操作（例如包装给定的 [callback]），或实现自己的微任务调度器。在后一种情况下，它们通常仍会使用父区域的 [ZoneDelegate.scheduleMicrotask]，以便挂接到现有的事件循环中。

### createTimer()

```dart
Timer createTimer(Duration duration, void Function() callback)
```

创建一个 [Timer](https://www.yuque.com/thyname/dart.async/timer)，其回调将在此区域中执行。

### createPeriodicTimer()

```dart
Timer createPeriodicTimer(Duration period, void callback(Timer timer))
```

创建一个周期性 [Timer](https://www.yuque.com/thyname/dart.async/timer)，其回调将在此区域中执行。

### print()

```dart
void print(String line)
```

打印给定的 [line]。

全局的 `print` 函数委托给当前区域的 [print](https://www.yuque.com/thyname/dart.core/print) 函数，从而使拦截打印操作成为可能。

示例：

```dart
import 'dart:async';

main() {
  runZoned(() {
    // Ends up printing: "Intercepted: in zone".
    print("in zone");
  }, zoneSpecification: new ZoneSpecification(
      print: (Zone self, ZoneDelegate parent, Zone zone, String line) {
    parent.print(zone, "Intercepted: $line");
  }));
}
```

## 运算符

### operator []

```dart
dynamic operator [](Object? key)
```

获取与 [key] 关联的区域值。

若此区域不包含该值，则在父区域中查找相同的键。若未找到该 [key]，则返回 `null`。

任何对象都可以用作键，只要它具有兼容的 `operator ==` 和 `hashCode` 实现。通过控制对键的访问，区域可以授予或拒绝对区域值的访问。
