# ZoneDelegate

```dart
abstract final class ZoneDelegate {}
```

父 zone 的一个适配视图。

此类允许 zone 方法的实现在保留原始 zone 相关信息的同时，调用父 zone 上的方法。

自定义 zone（通过 [Zone.fork] 或 [runZoned](https://www.yuque.com/thyname/dart.async/runzoned) 创建）可以提供大多数 zone 方法的实现。这类似于重写 [Zone](https://www.yuque.com/thyname/dart.async/zone) 的方法，但此机制不需要子类化。

自定义 zone 函数（通过 [ZoneSpecification](https://www.yuque.com/thyname/dart.async/zonespecification) 提供）通常会记录或包装其参数，然后使用提供的 [ZoneDelegate](https://www.yuque.com/thyname/dart.async/zonedelegate) 将操作委托给其父 zone。

虽然 zone 可以通过 [Zone.parent] 访问其父 zone，但仍建议调用所提供的父委托（parent delegate）上的方法，原因有二：

1.  委托方法接受一个额外的 `zone` 参数，即发起该操作的 zone。
2.  委托调用效率更高，因为其实现知道如何跳过那些只是委托给其父 zone 的中间 zone。

## 方法

### handleUncaughtError()

```dart
void handleUncaughtError(Zone zone, Object error, StackTrace stackTrace)
```

以当前 zone 调用该 zone 的 [HandleUncaughtErrorHandler](https://www.yuque.com/thyname/dart.async/handleuncaughterrorhandler)。

### run()

```dart
R run<R>(Zone zone, R f())
```

以当前 zone 调用该 zone 的 [RunHandler](https://www.yuque.com/thyname/dart.async/runhandler)。

### runUnary()

```dart
R runUnary<R, T>(Zone zone, R f(T arg), T arg)
```

以当前 zone 调用该 zone 的 [RunUnaryHandler](https://www.yuque.com/thyname/dart.async/rununaryhandler)。

### runBinary()

```dart
R runBinary<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2), T1 arg1, T2 arg2)
```

以当前 zone 调用该 zone 的 [RunBinaryHandler](https://www.yuque.com/thyname/dart.async/runbinaryhandler)。

### registerCallback()

```dart
ZoneCallback<R> registerCallback<R>(Zone zone, R f())
```

以当前 zone 调用该 zone 的 [RegisterCallbackHandler](https://www.yuque.com/thyname/dart.async/registercallbackhandler)。

### registerUnaryCallback()

```dart
ZoneUnaryCallback<R, T> registerUnaryCallback<R, T>(Zone zone, R f(T arg))
```

以当前 zone 调用该 zone 的 [RegisterUnaryHandler]。

### registerBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> registerBinaryCallback<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2))
```

以当前 zone 调用该 zone 的 [RegisterBinaryHandler]。

### errorCallback()

```dart
AsyncError? errorCallback(Zone zone, Object error, StackTrace? stackTrace)
```

以当前 zone 调用该 zone 的 [ErrorCallbackHandler](https://www.yuque.com/thyname/dart.async/errorcallbackhandler)。

### scheduleMicrotask()

```dart
void scheduleMicrotask(Zone zone, void f())
```

以当前 zone 调用该 zone 的 [ScheduleMicrotaskHandler](https://www.yuque.com/thyname/dart.async/schedulemicrotaskhandler)。

### createTimer()

```dart
Timer createTimer(Zone zone, Duration duration, void f())
```

以当前 zone 调用该 zone 的 [CreateTimerHandler](https://www.yuque.com/thyname/dart.async/createtimerhandler)。

### createPeriodicTimer()

```dart
Timer createPeriodicTimer(Zone zone, Duration period, void f(Timer timer))
```

以当前 zone 调用该 zone 的 [CreatePeriodicTimerHandler](https://www.yuque.com/thyname/dart.async/createperiodictimerhandler)。

### print()

```dart
void print(Zone zone, String line)
```

以当前 zone 调用该 zone 的 [PrintHandler](https://www.yuque.com/thyname/dart.async/printhandler)。

### fork()

```dart
Zone fork(Zone zone, ZoneSpecification? specification, Map? zoneValues)
```

以当前 zone 调用该 zone 的 [ForkHandler](https://www.yuque.com/thyname/dart.async/forkhandler)。
