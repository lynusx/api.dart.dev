# ZoneDelegate

```dart
abstract final class ZoneDelegate {}
```

An adapted view of the parent zone.

This class allows the implementation of a zone method to invoke methods on the parent zone while retaining knowledge of the originating zone.

Custom zones (created through [Zone.fork] or [runZoned]) can provide implementations of most methods of zones. This is similar to overriding methods on [Zone], except that this mechanism doesn't require subclassing.

A custom zone function (provided through a [ZoneSpecification]) typically records or wraps its parameters and then delegates the operation to its parent zone using the provided [ZoneDelegate].

While zones have access to their parent zone (through [Zone.parent]) it is recommended to call the methods on the provided parent delegate for two reasons:

1.  the delegate methods take an additional `zone` argument which is the zone the action has been initiated in.
2.  delegate calls are more efficient, since the implementation knows how to skip zones that would just delegate to their parents.

## 方法

### handleUncaughtError()

```dart
void handleUncaughtError(Zone zone, Object error, StackTrace stackTrace)
```

Invoke the [HandleUncaughtErrorHandler] of the zone with a current zone.

### run()

```dart
R run<R>(Zone zone, R f())
```

Invokes the [RunHandler] of the zone with a current zone.

### runUnary()

```dart
R runUnary<R, T>(Zone zone, R f(T arg), T arg)
```

Invokes the [RunUnaryHandler] of the zone with a current zone.

### runBinary()

```dart
R runBinary<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2), T1 arg1, T2 arg2)
```

Invokes the [RunBinaryHandler] of the zone with a current zone.

### registerCallback()

```dart
ZoneCallback<R> registerCallback<R>(Zone zone, R f())
```

Invokes the [RegisterCallbackHandler] of the zone with a current zone.

### registerUnaryCallback()

```dart
ZoneUnaryCallback<R, T> registerUnaryCallback<R, T>(Zone zone, R f(T arg))
```

Invokes the [RegisterUnaryHandler] of the zone with a current zone.

### registerBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> registerBinaryCallback<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2))
```

Invokes the [RegisterBinaryHandler] of the zone with a current zone.

### errorCallback()

```dart
AsyncError? errorCallback(Zone zone, Object error, StackTrace? stackTrace)
```

Invokes the [ErrorCallbackHandler] of the zone with a current zone.

### scheduleMicrotask()

```dart
void scheduleMicrotask(Zone zone, void f())
```

Invokes the [ScheduleMicrotaskHandler] of the zone with a current zone.

### createTimer()

```dart
Timer createTimer(Zone zone, Duration duration, void f())
```

Invokes the [CreateTimerHandler] of the zone with a current zone.

### createPeriodicTimer()

```dart
Timer createPeriodicTimer(Zone zone, Duration period, void f(Timer timer))
```

Invokes the [CreatePeriodicTimerHandler] of the zone with a current zone.

### print()

```dart
void print(Zone zone, String line)
```

Invokes the [PrintHandler] of the zone with a current zone.

### fork()

```dart
Zone fork(Zone zone, ZoneSpecification? specification, Map? zoneValues)
```

Invokes the [ForkHandler] of the zone with a current zone.
