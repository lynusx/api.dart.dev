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

### handleUncaughtError()

```dart
void handleUncaughtError(Zone zone, Object error, StackTrace stackTrace)
```

### run()

```dart
R run<R>(Zone zone, R f())
```

### runUnary()

```dart
R runUnary<R, T>(Zone zone, R f(T arg), T arg)
```

### runBinary()

```dart
R runBinary<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2), T1 arg1, T2 arg2)
```

### registerCallback()

```dart
ZoneCallback<R> registerCallback<R>(Zone zone, R f())
```

### registerUnaryCallback()

```dart
ZoneUnaryCallback<R, T> registerUnaryCallback<R, T>(Zone zone, R f(T arg))
```

### registerBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> registerBinaryCallback<R, T1, T2>(Zone zone, R f(T1 arg1, T2 arg2))
```

### errorCallback()

```dart
AsyncError? errorCallback(Zone zone, Object error, StackTrace? stackTrace)
```

### scheduleMicrotask()

```dart
void scheduleMicrotask(Zone zone, void f())
```

### createTimer()

```dart
Timer createTimer(Zone zone, Duration duration, void f())
```

### createPeriodicTimer()

```dart
Timer createPeriodicTimer(Zone zone, Duration period, void f(Timer timer))
```

### print()

```dart
void print(Zone zone, String line)
```

### fork()

```dart
Zone fork(Zone zone, ZoneSpecification? specification, Map? zoneValues)
```
