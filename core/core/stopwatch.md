# Stopwatch

```dart
class Stopwatch {}
```

A stopwatch which measures time while it's running.

A stopwatch is either running or stopped. It measures the elapsed time that passes while the stopwatch is running.

When a stopwatch is initially created, it is stopped and has measured no elapsed time.

The elapsed time can be accessed in various formats using [elapsed], [elapsedMilliseconds], [elapsedMicroseconds] or [elapsedTicks].

The stopwatch is started by calling [start].

Example:

```dart
final stopwatch = Stopwatch();
print(stopwatch.elapsedMilliseconds); // 0
print(stopwatch.isRunning); // false
stopwatch.start();
print(stopwatch.isRunning); // true
```

To stop or pause the stopwatch, use [stop]. Use [start] to continue again when only pausing temporarily.

```
stopwatch.stop();
print(stopwatch.isRunning); // false
Duration elapsed = stopwatch.elapsed;
await Future.delayed(const Duration(seconds: 1));
assert(stopwatch.elapsed == elapsed); // No measured time elapsed.
stopwatch.start(); // Continue measuring.
```

The [reset] method sets the elapsed time back to zero. It can be called whether the stopwatch is running or not, and doesn't change whether it's running.

```
// Do some work.
stopwatch.stop();
print(stopwatch.elapsedMilliseconds); // Likely > 0.
stopwatch.reset();
print(stopwatch.elapsedMilliseconds); // 0
```

### Stopwatch()

```dart
Stopwatch()
```

Creates a [Stopwatch] in stopped state with a zero elapsed count.

The following example shows how to start a [Stopwatch] immediately after allocation.

```dart
final stopwatch = Stopwatch()..start();
```

### frequency

```dart
int get frequency
```

Frequency of the elapsed counter in Hz.

### start()

```dart
void start()
```

Starts the [Stopwatch].

The [elapsed] count increases monotonically. If the [Stopwatch] has been stopped, then calling start again restarts it without resetting the [elapsed] count.

If the [Stopwatch] is currently running, then calling start does nothing.

### stop()

```dart
void stop()
```

Stops the [Stopwatch].

The [elapsedTicks] count stops increasing after this call. If the [Stopwatch] is currently not running, then calling this method has no effect.

### reset()

```dart
void reset()
```

Resets the [elapsed] count to zero.

This method does not stop or start the [Stopwatch].

### elapsedTicks

```dart
int get elapsedTicks
```

The elapsed number of clock ticks since calling [start] while the [Stopwatch] is running.

This is the elapsed number of clock ticks between calling [start] and calling [stop].

Is 0 if the [Stopwatch] has never been started.

The elapsed number of clock ticks increases by [frequency] every second.

### elapsed

```dart
Duration get elapsed
```

The [elapsedTicks] counter converted to a [Duration].

### elapsedMicroseconds

```dart
int get elapsedMicroseconds
```

The [elapsedTicks] counter converted to microseconds.

### elapsedMilliseconds

```dart
int get elapsedMilliseconds
```

The [elapsedTicks] counter converted to milliseconds.

### isRunning

```dart
bool get isRunning
```

Whether the [Stopwatch] is currently running.
