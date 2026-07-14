# scheduleMicrotask()

```dart
void scheduleMicrotask(void Function() callback)
```

异步运行一个函数。

通过此函数注册的回调始终按顺序执行，并保证在其他异步事件（如 [Timer](https://www.yuque.com/thyname/dart.async/timer) 事件或 DOM 事件）之前运行。

**警告：** 通过此方法注册异步回调可能会导致 DOM 被"饿死"（得不到执行机会）。例如，以下程序会一直运行回调，而永远不会让 Timer 回调有机会执行：

```dart
main() {
  Timer.run(() { print("executed"); });  // Will never be executed.
  foo() {
    scheduleMicrotask(foo);  // Schedules [foo] in front of other events.
  }
  foo();
}
```

## 其他资源

- [事件循环与 Dart](https://dart.dev/articles/event-loop/)：了解 Dart 如何处理事件队列和微任务队列，从而编写更好、更少意外的异步代码。
