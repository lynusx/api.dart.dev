# Stopwatch

```dart
class Stopwatch {}
```

用于在运行时测量时间的秒表。

秒表要么在运行，要么已停止。它测量秒表运行期间经过的时间。

秒表刚创建时处于停止状态，并且尚未测量任何经过的时间。

可以使用 [elapsed]、[elapsedMilliseconds]、[elapsedMicroseconds] 或 [elapsedTicks] 以不同格式访问经过的时间。

通过调用 [start] 启动秒表。

示例：

```dart
final stopwatch = Stopwatch();
print(stopwatch.elapsedMilliseconds); // 0
print(stopwatch.isRunning); // false
stopwatch.start();
print(stopwatch.isRunning); // true
```

要停止或暂停秒表，请使用 [stop]。如果只是暂时暂停，可使用 [start] 继续。

```
stopwatch.stop();
print(stopwatch.isRunning); // false
Duration elapsed = stopwatch.elapsed;
await Future.delayed(const Duration(seconds: 1));
assert(stopwatch.elapsed == elapsed); // No measured time elapsed.
stopwatch.start(); // Continue measuring.
```

[reset] 方法将经过的时间重置为零。无论秒表是否在运行都可以调用该方法，且不会改变其运行状态。

```
// Do some work.
stopwatch.stop();
print(stopwatch.elapsedMilliseconds); // Likely > 0.
stopwatch.reset();
print(stopwatch.elapsedMilliseconds); // 0
```

## 构造函数

### Stopwatch()

```dart
Stopwatch()
```

创建一个处于停止状态、经过计数为零的 [Stopwatch]。

以下示例展示了如何在分配后立即启动 [Stopwatch]。

```dart
final stopwatch = Stopwatch()..start();
```

## 属性

### frequency

```dart
int get frequency
```

经过计数器的频率，单位为 Hz。

### elapsedTicks

```dart
int get elapsedTicks
```

自调用 [start] 以来，[Stopwatch] 运行期间经过的时钟滴答数。

即调用 [start] 和调用 [stop] 之间经过的时钟滴答数。

如果 [Stopwatch] 从未启动过，则为 0。

经过的时钟滴答数每秒增加 [frequency]。

### elapsed

```dart
Duration get elapsed
```

将 [elapsedTicks] 计数器转换为 [Duration]。

### elapsedMicroseconds

```dart
int get elapsedMicroseconds
```

将 [elapsedTicks] 计数器转换为微秒。

### elapsedMilliseconds

```dart
int get elapsedMilliseconds
```

将 [elapsedTicks] 计数器转换为毫秒。

### isRunning

```dart
bool get isRunning
```

[Stopwatch] 当前是否正在运行。

## 方法

### start()

```dart
void start()
```

启动 [Stopwatch]。

[elapsed] 计数单调递增。如果 [Stopwatch] 已停止，再次调用 start 会重新启动它，但不会重置 [elapsed] 计数。

如果 [Stopwatch] 当前正在运行，则调用 start 不会产生任何效果。

### stop()

```dart
void stop()
```

停止 [Stopwatch]。

调用此方法后，[elapsedTicks] 计数将停止增加。如果 [Stopwatch] 当前未在运行，则调用此方法不会产生任何效果。

### reset()

```dart
void reset()
```

将 [elapsed] 计数重置为零。

此方法不会停止或启动 [Stopwatch]。
