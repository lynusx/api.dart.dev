# Timer

```dart
abstract interface class Timer {}
```

一个倒计时定时器，可配置为触发一次或重复触发。

该定时器从指定的时长倒数到 0。当定时器到达 0 时，会调用指定的回调函数。使用周期性定时器可以按相同的时间间隔重复倒计时。

负的时长会被视为与 [Duration.zero] 相同。如果时长在静态上已知为 0，可考虑使用 [run]。

```dart
void main() {
  Timer(const Duration(seconds: 5), handleTimeout);
}

void handleTimeout() {  // callback function
  // Do some work.
}
```

**注意：** 如果使用 [Timer](https://www.yuque.com/thyname/dart.async/timer) 的 Dart 代码被编译为 JavaScript，浏览器所能提供的最细粒度为 4 毫秒。

另请参阅：

- [Stopwatch](https://www.yuque.com/thyname/dart.core/stopwatch)，用于测量经过的时间。

## 构造函数

### Timer()

```dart
Timer(Duration duration, void Function() callback)
```

创建一个新的定时器。

[callback] 函数会在给定的 [duration] 之后被调用。

示例：

```dart
final timer =
    Timer(const Duration(seconds: 5), () => print('Timer finished'));
// Outputs after 5 seconds: "Timer finished".
```

### Timer.periodic()

```dart
Timer.periodic(Duration duration, void callback(Timer timer))
```

创建一个新的重复定时器。

[callback] 会以 [duration] 为间隔重复调用，直到通过 [cancel] 函数取消为止。

具体的触发时机取决于底层定时器的实现。在 `duration * n` 的时间内不会发生超过 `n` 次回调调用，但两次连续回调之间的时间间隔可能比 `duration` 更短或更长。

具体而言，实现可以将下一次回调安排在——例如——上一次回调结束之后、上一次回调开始之后，或上一次回调被安排的时间点之后的一个 `duration`，即使实际的回调被延迟了。

负的 [duration] 会被视为与 [Duration.zero] 相同。

示例：

```dart
var counter = 3;
Timer.periodic(const Duration(seconds: 2), (timer) {
  print(timer.tick);
  counter--;
  if (counter == 0) {
    print('Cancel timer');
    timer.cancel();
  }
});
// Outputs:
// 1
// 2
// 3
// "Cancel timer"
```

## 静态方法

### run()

```dart
void run(void Function() callback)
```

尽快异步运行给定的 [callback]。

此函数等同于 `new Timer(Duration.zero, callback)`。

示例：

```dart
Timer.run(() => print('timer run'));
```

## 属性

### tick

```dart
int get tick
```

最近一次定时器事件之前所经历的时长次数。

该值从零开始，每次发生定时器事件时递增，因此每次回调看到的值都会比上一次更大。

如果一个时长非零的周期性定时器被延迟过多，导致本应发生不止一次触发，那么除最后一次触发外，过去的所有触发都会被视为“错过”，不会为它们调用回调。[tick] 计数反映的是已经过去的时长次数，而不是实际发生的回调调用次数。

示例：

```dart
final stopwatch = Stopwatch()..start();
Timer.periodic(const Duration(seconds: 1), (timer) {
  print(timer.tick);
  if (timer.tick == 1) {
    while (stopwatch.elapsedMilliseconds < 4500) {
      // Run uninterrupted for another 3.5 seconds!
      // The latest due tick after that is the 4-second tick.
    }
  } else {
    timer.cancel();
  }
});
// Outputs:
// 1
// 4
```

### isActive

```dart
bool get isActive
```

返回该定时器是否仍处于活动状态。

对于非周期性定时器，如果其回调尚未执行且定时器未被取消，则视为活动状态。

对于周期性定时器，只要未被取消，就视为活动状态。

## 方法

### cancel()

```dart
void cancel()
```

取消该定时器。

一旦 [Timer](https://www.yuque.com/thyname/dart.async/timer) 被取消，定时器将不再调用其回调函数。对同一个 [Timer](https://www.yuque.com/thyname/dart.async/timer) 多次调用 [cancel] 是允许的，并且不会产生进一步的效果。

示例：

```dart
final timer =
    Timer(const Duration(seconds: 5), () => print('Timer finished'));
// Cancel timer, callback never called.
timer.cancel();
```
