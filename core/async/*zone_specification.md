# ZoneSpecification

```dart
final class ZoneSpecification {}
```

一个参数对象，包含用于 [Zone.fork] 的自定义区域函数处理程序。

区域规范（zone specification）是传递给 [Zone.fork] 及其底层 [ForkHandler](https://www.yuque.com/thyname/dart.async/forkhandler) 自定义实现的参数对象。若各个处理程序被设置为非空值，它们将作为使用此区域规范创建的派生区域（forked zone）中对应 [Zone](https://www.yuque.com/thyname/dart.async/zone) 方法的实现。

处理程序与 [Zone](https://www.yuque.com/thyname/dart.async/zone) 上同名方法具有相同的签名，但额外接收三个参数：

1.  处理程序所依附的区域（即 "self" 区域）。这是由 [Zone.fork] 创建的区域，处理程序作为区域委托（zone delegation）的一部分传入。
2.  指向父区域的 [ZoneDelegate](https://www.yuque.com/thyname/dart.async/zonedelegate)。
3.  发出请求时的 "当前" 区域。self 区域始终是当前区域的父区域。

处理程序既可以停止请求的传播（即不调用父处理程序），也可以将请求转发给父区域，并在转发过程中修改参数。

## 构造函数

### ZoneSpecification()

```dart
ZoneSpecification({
  HandleUncaughtErrorHandler? handleUncaughtError,
  RunHandler? run,
  RunUnaryHandler? runUnary,
  RunBinaryHandler? runBinary,
  RegisterCallbackHandler? registerCallback,
  RegisterUnaryCallbackHandler? registerUnaryCallback,
  RegisterBinaryCallbackHandler? registerBinaryCallback,
  ErrorCallbackHandler? errorCallback,
  ScheduleMicrotaskHandler? scheduleMicrotask,
  CreateTimerHandler? createTimer,
  CreatePeriodicTimerHandler? createPeriodicTimer,
  PrintHandler? print,
  ForkHandler? fork
})
```

使用提供的处理程序创建一个规范。

若提供了 [handleUncaughtError]，新区域将成为一个新的"错误区域"（error zone），以防止错误流入其他错误区域（参见 [Zone.errorZone]、[Zone.inSameErrorZone]）。

### ZoneSpecification.from()

```dart
ZoneSpecification.from(
  ZoneSpecification other, {
  HandleUncaughtErrorHandler? handleUncaughtError,
  RunHandler? run,
  RunUnaryHandler? runUnary,
  RunBinaryHandler? runBinary,
  RegisterCallbackHandler? registerCallback,
  RegisterUnaryCallbackHandler? registerUnaryCallback,
  RegisterBinaryCallbackHandler? registerBinaryCallback,
  ErrorCallbackHandler? errorCallback,
  ScheduleMicrotaskHandler? scheduleMicrotask,
  CreateTimerHandler? createTimer,
  CreatePeriodicTimerHandler? createPeriodicTimer,
  PrintHandler? print,
  ForkHandler? fork,
})
```

基于 [other] 和提供的处理程序创建一个规范。

创建的区域规范包含 [other] 的处理程序以及任何单独提供的处理程序。若某个处理程序同时通过 [other] 和单独参数提供，则单独提供的处理程序将覆盖来自 [other] 的处理程序。

## 方法

### handleUncaughtError

```dart
HandleUncaughtErrorHandler? handleUncaughtError
```

新区域的自定义 [Zone.handleUncaughtError] 实现。

### run

```dart
RunHandler? run
```

新区域的自定义 [Zone.run] 实现。

### runUnary

```dart
RunUnaryHandler? runUnary
```

新区域的自定义 [Zone.runUnary] 实现。

### runBinary

```dart
RunBinaryHandler? runBinary
```

新区域的自定义 [Zone.runBinary] 实现。

### registerCallback

```dart
RegisterCallbackHandler? registerCallback
```

新区域的自定义 [Zone.registerCallback] 实现。

### registerUnaryCallback

```dart
RegisterUnaryCallbackHandler? registerUnaryCallback
```

新区域的自定义 [Zone.registerUnaryCallback] 实现。

### registerBinaryCallback

```dart
RegisterBinaryCallbackHandler? registerBinaryCallback
```

新区域的自定义 [Zone.registerBinaryCallback] 实现。

### errorCallback

```dart
ErrorCallbackHandler? errorCallback
```

新区域的自定义 [Zone.errorCallback] 实现。

### scheduleMicrotask

```dart
ScheduleMicrotaskHandler? scheduleMicrotask
```

新区域的自定义 [Zone.scheduleMicrotask] 实现。

### createTimer

```dart
CreateTimerHandler? createTimer
```

新区域的自定义 [Zone.createTimer] 实现。

### createPeriodicTimer

```dart
CreatePeriodicTimerHandler? createPeriodicTimer
```

新区域的自定义 [Zone.createPeriodicTimer] 实现。

### print

```dart
PrintHandler? print
```

新区域的自定义 [Zone.print] 实现。

### fork

```dart
ForkHandler? fork
```

新区域的自定义 [Zone.handleUncaughtError] 实现。

---

# HandleUncaughtErrorHandler

```dart
typedef HandleUncaughtErrorHandler = void Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  Object error,
  StackTrace stackTrace
)
```

自定义 [Zone.handleUncaughtError] 实现函数的类型。

用作 [ZoneSpecification.handleUncaughtError] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

[error] 和 [stackTrace] 是在 [zone] 中未被捕获的错误及其堆栈跟踪。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

若未捕获错误处理程序抛出异常，该错误将被传递给 `parent.handleUncaughtError`。若抛出的对象即为 [error]，则该抛出被视为重新抛出（re-throw），并保留原始的 [stackTrace]。这使得异步错误得以脱离错误区域。

---

# RunHandler

```dart
typedef RunHandler = R Function<R>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function() f
)
```

自定义 [Zone.run] 实现函数的类型。

用作 [ZoneSpecification.run] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 是传递给 [zone] 的 [Zone.run] 的函数。

[Zone.run] 的默认行为是在当前区域 [zone] 中调用 [f]。自定义处理程序可以在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# RunUnaryHandler

```dart
typedef RunUnaryHandler = R Function<R, T>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function(T arg) f,
  T arg
)
```

自定义 [Zone.runUnary] 实现函数的类型。

用作 [ZoneSpecification.runUnary] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 和值 [arg] 是传递给 [zone] 的 [Zone.runUnary] 的函数和参数。

[Zone.runUnary] 的默认行为是在当前区域 [zone] 中使用参数 [arg] 调用 [f]。自定义处理程序可以在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# RunBinaryHandler

```dart
typedef RunBinaryHandler = R Function<R, T1, T2>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function(T1 arg1, T2 arg2) f,
  T1 arg1,
  T2 arg2
)
```

自定义 [Zone.runBinary] 实现函数的类型。

用作 [ZoneSpecification.runBinary] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 以及值 [arg1] 和 [arg2] 是传递给 [zone] 的 [Zone.runBinary] 的函数和参数。

[Zone.runUnary] 的默认行为是在当前区域 [zone] 中使用参数 [arg1] 和 [arg2] 调用 [f]。自定义处理程序可以在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# RegisterCallbackHandler

```dart
typedef RegisterCallbackHandler = ZoneCallback<R> Function<R>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function() f
)
```

自定义 [Zone.registerCallback] 实现函数的类型。

用作 [ZoneSpecification.registerCallback] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 是传递给 [zone] 的 [Zone.registerCallback] 的函数。

处理程序应返回函数 [f] 本身，或返回替代 [f] 的另一个函数，通常做法是将 [f] 包装在一个函数中，在调用 [f] 前后执行额外操作。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# RegisterUnaryCallbackHandler

```dart
typedef RegisterUnaryCallbackHandler = ZoneUnaryCallback<R, T> Function<R, T>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function(T arg) f
)
```

自定义 [Zone.registerUnaryCallback] 实现函数的类型。

用作 [ZoneSpecification.registerUnaryCallback] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 是传递给 [zone] 的 [Zone.registerUnaryCallback] 的函数。

处理程序应返回函数 [f] 本身，或返回替代 [f] 的另一个函数，通常做法是将 [f] 包装在一个函数中，在调用 [f] 前后执行额外操作。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# RegisterBinaryCallbackHandler

```dart
typedef RegisterBinaryCallbackHandler = ZoneBinaryCallback<R, T1, T2> Function<R, T1, T2>(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  R Function(T1 arg1, T2 arg2) f
)
```

自定义 [Zone.registerBinaryCallback] 实现函数的类型。

用作 [ZoneSpecification.registerBinaryCallback] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 是传递给 [zone] 的 [Zone.registerBinaryCallback] 的函数。

处理程序应返回函数 [f] 本身，或返回替代 [f] 的另一个函数，通常做法是将 [f] 包装在一个函数中，在调用 [f] 前后执行额外操作。

---

# ErrorCallbackHandler

```dart
typedef ErrorCallbackHandler = AsyncError? Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  Object error,
  StackTrace? stackTrace
)
```

自定义 [Zone.errorCallback] 实现函数的类型。

用作 [ZoneSpecification.errorCallback] 的函数，用于定制新区域的行为。

[error] 和 [stackTrace] 是传递给 [zone] 的 [Zone.errorCallback] 的错误及堆栈跟踪。

当同步错误转变为异步错误时，该函数将被调用——例如通过 `Future.then` 回调抛出错误而被捕获，或通过编程方式创建异步错误，例如使用 `Future.error` 或 `Completer.complete`。

若该函数不希望替换错误或堆栈跟踪，则应直接返回 `parent.errorCallback(zone, error, stackTrace)`，从而让父区域有机会进行拦截。

若该函数确实希望替换错误和/或堆栈跟踪（例如替换为 `error2` 和 `stackTrace2`），仍应允许父区域拦截这些错误，例如：

```dart
  return parent.errorCallback(zone, error, stackTrace) ??
      AsyncError(error, stackTrace);
```

若原始错误和堆栈跟踪未发生变化，该函数返回 `null`，以避免在最常见的情况下进行任何内存分配；否则返回一个包含替换错误和堆栈跟踪的 [AsyncError](https://www.yuque.com/thyname/dart.async/asyncerror)，该替换值将作为异步错误取代原始值。

[self] [Zone](https://www.yuque.com/thyname/dart.async/zone) 是注册该处理程序的区域，[parent] 委托转发至 [self] 的父区域处理程序，[zone] 是发生未捕获错误的当前区域，该区域将以 [self] 作为其祖先区域。

错误回调处理程序**不得**抛出异常。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# ScheduleMicrotaskHandler

```dart
typedef ScheduleMicrotaskHandler = void Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  void Function() f
)
```

自定义 [Zone.scheduleMicrotask] 实现函数的类型。

用作 [ZoneSpecification.scheduleMicrotask] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

函数 [f] 是传递给 [zone] 的 [Zone.scheduleMicrotask] 的函数。

自定义处理程序可以选择用另一个函数替换 [f]，该函数在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]，然后调用 `parent.scheduleMicrotask(zone, replacement)`；也可以实现自己的微任务调度队列，但通常仍需依赖 `parent.scheduleMicrotask` 作为启动方式。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# CreateTimerHandler

```dart
typedef CreateTimerHandler = Timer Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  Duration duration,
  void Function() f
)
```

自定义 [Zone.createTimer] 实现函数的类型。

用作 [ZoneSpecification.createTimer] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

回调函数 [f] 和 [duration] 是传递给 [zone] 的 [Zone.createTimer] 的回调函数和持续时间（可能通过 [Timer](https://www.yuque.com/thyname/dart.async/timer) 构造函数传入）。

自定义处理程序可以选择用另一个函数替换 [f]，该函数在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]，然后调用 `parent.createTimer(zone, replacement)`；也可以实现自己的计时器队列，但通常仍需依赖 `parent.createTimer` 作为启动方式。

该函数应返回一个 [Timer](https://www.yuque.com/thyname/dart.async/timer) 对象，用于检查和控制已调度的计时器回调。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# CreatePeriodicTimerHandler

```dart
typedef CreatePeriodicTimerHandler = Timer Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  Duration period,
  void Function(Timer timer) f
)
```

自定义 [Zone.createPeriodicTimer] 实现函数的类型。

用作 [ZoneSpecification.createPeriodicTimer] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

回调函数 [f] 和 [period] 是传递给 [zone] 的 [Zone.createPeriodicTimer] 的回调函数和周期（可能通过 [Timer.periodic] 构造函数传入）。

自定义处理程序可以选择用另一个函数替换 [f]，该函数在调用 [f] 之前、之后执行其他操作，或完全代替调用 [f]，然后调用 `parent.createTimer(zone, replacement)`；也可以实现自己的计时器队列，但通常仍需依赖 `parent.createTimer` 作为启动方式。

该函数应返回一个 [Timer](https://www.yuque.com/thyname/dart.async/timer) 对象，用于检查和控制已调度的计时器回调。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# PrintHandler

```dart
typedef PrintHandler = void Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  String line
)
```

自定义 [Zone.print] 实现函数的类型。

用作 [ZoneSpecification.print] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

字符串 [line] 是传递给 [zone] 的 [Zone.print] 的字符串（可能通过全局 [print](https://www.yuque.com/thyname/dart.core/print) 函数传入）。

自定义处理程序可以拦截打印操作，并将其重定向到控制台以外的其他目标。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。

---

# ForkHandler

```dart
typedef ForkHandler = Zone Function(
  Zone self,
  ZoneDelegate parent,
  Zone zone,
  ZoneSpecification? specification,
  Map<Object?, Object?>? zoneValues
)
```

自定义 [Zone.fork] 实现函数的类型。

用作 [ZoneSpecification.fork] 的函数，用于定制新区域的行为。

接收注册该处理程序的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 作为 [self]，转发至 [self] 的父区域处理程序的委托作为 [parent]，以及发生未捕获错误的当前区域作为 [zone]，该区域将以 [self] 作为其父区域。

处理程序应创建一个以 [zone] 作为直接父区域的新区域。

[specification] 和 [zoneValues] 是传递给 [zone] 的 [Zone.fork] 的参数，它们指定了新区域应具有的自定义区域处理程序和区域变量。

自定义处理程序可以在调用 `parent.fork(zone, specification, zoneValues)` 之前修改规范或区域变量，但必须调用 [parent] 的 [ZoneDelegate.fork] 才能创建有效的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 对象。

该函数只能通过 [self]、[parent] 或 [zone] 访问与区域相关的功能，不应依赖于当前区域（[Zone.current]）。
