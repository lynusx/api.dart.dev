# scheduleMicrotask()

```dart
void scheduleMicrotask(void Function() callback)
```

Runs a function asynchronously.

Callbacks registered through this function are always executed in order and are guaranteed to run before other asynchronous events (like [Timer] events, or DOM events).

**Warning:** it is possible to starve the DOM by registering asynchronous callbacks through this method. For example the following program runs the callbacks without ever giving the Timer callback a chance to execute:

```dart
main() {
  Timer.run(() { print("executed"); });  // Will never be executed.
  foo() {
    scheduleMicrotask(foo);  // Schedules [foo] in front of other events.
  }
  foo();
}
```

## Other resources

- [The Event Loop and Dart](https://dart.dev/articles/event-loop/): Learn how Dart handles the event queue and microtask queue, so you can write better asynchronous code with fewer surprises.
