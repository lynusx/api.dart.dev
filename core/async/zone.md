# Zone

```dart
abstract final class Zone {}
```

A zone represents an environment that remains stable across asynchronous calls.

All code is executed in the context of a zone, available to the code as [Zone.current]. The initial `main` function runs in the context of the default zone ([Zone.root]). Code can be run in a different zone using either [runZoned] or [runZonedGuarded] to create a new zone and run code in it, or [Zone.run] to run code in the context of an existing zone which may have been created earlier using [Zone.fork].

Developers can create a new zone that overrides some of the functionality of an existing zone. For example, custom zones can replace or modify the behavior of `print`, timers, microtasks or how uncaught errors are handled.

The [Zone] class is not subclassable, but users can provide custom zones by forking an existing zone (usually [Zone.current]) with a [ZoneSpecification]. This is similar to creating a new class that extends the base `Zone` class and that overrides some methods, except without actually creating a new class. Instead the overriding methods are provided as functions that explicitly take the equivalent of their own class, the "super" class and the `this` object as parameters.

Asynchronous callbacks always run in the context of the zone where they were scheduled. This is implemented using two steps:

1.  the callback is first registered using one of [registerCallback], [registerUnaryCallback], or [registerBinaryCallback]. This allows the zone to record that a callback exists and potentially modify it (by returning a different callback). The code doing the registration (e.g., `Future.then`) also remembers the current zone so that it can later run the callback in that zone.
2.  At a later point the registered callback is run in the remembered zone, using one of [run], [runUnary] or [runBinary].

This is all handled internally by the platform code and most users don't need to worry about it. However, developers of new asynchronous operations, provided by the underlying system, must follow the protocol to be zone compatible.

For convenience, zones provide [bindCallback] (and the corresponding [bindUnaryCallback] and [bindBinaryCallback]) to make it easier to respect the zone contract: these functions first invoke the corresponding `register` functions and then wrap the returned function so that it runs in the current zone when it is later asynchronously invoked.

Similarly, zones provide [bindCallbackGuarded] (and the corresponding [bindUnaryCallbackGuarded] and [bindBinaryCallbackGuarded]), when the callback should be invoked through [Zone.runGuarded].

## 静态属性

### root

```dart
Zone root
```

The root zone.

All isolate entry functions (`main` or spawned functions) start running in the root zone (that is, [Zone.current] is identical to [Zone.root] when the entry function is called). If no custom zone is created, the rest of the program always runs in the root zone.

The root zone implements the default behavior of all zone operations. Many methods, like [registerCallback] do the bare minimum required of the function, and are only provided as a hook for custom zones. Others, like [scheduleMicrotask], interact with the underlying system to implement the desired behavior.

### current

```dart
Zone get current
```

The zone that is currently active.

## 属性

### parent

```dart
Zone? get parent
```

The parent zone of the this zone.

Is `null` if `this` is the [root] zone.

Zones are created by [fork] on an existing zone, or by [runZoned] which forks the [current] zone. The new zone's parent zone is the zone it was forked from.

### errorZone

```dart
Zone get errorZone
```

The error zone is responsible for dealing with uncaught errors.

This is the closest parent zone of this zone that provides a [handleUncaughtError] method.

Asynchronous errors in futures never cross zone boundaries between zones with different error handlers.

Example:

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

Note that errors cannot enter a child zone with a different error handler either:

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

Handles uncaught asynchronous errors.

There are two kind of asynchronous errors that are handled by this function:

1.  Uncaught errors that were thrown in asynchronous callbacks, for example, a `throw` in the function passed to [Timer.run].
2.  Asynchronous errors that are pushed through [Future] and [Stream] chains, but for which nobody registered an error handler. Most asynchronous classes, like [Future] or [Stream] push errors to their listeners. Errors are propagated this way until either a listener handles the error (for example with [Future.catchError]), or no listener is available anymore. In the latter case, futures and streams invoke the zone's [handleUncaughtError].

By default, when handled by the root zone, uncaught asynchronous errors are treated like uncaught synchronous exceptions.

### inSameErrorZone()

```dart
bool inSameErrorZone(Zone otherZone)
```

Whether this zone and [otherZone] are in the same error zone.

Two zones are in the same error zone if they have the same [errorZone].

### fork()

```dart
Zone fork({
  ZoneSpecification? specification, 
  Map<Object?, Object?>? zoneValues
})
```

Creates a new zone as a child zone of this zone.

The new zone uses the closures in the given [specification] to override the parent zone's behavior. All specification entries that are `null` inherit the behavior from the parent zone (`this`).

The new zone inherits the stored values (accessed through [operator []]) of this zone and updates them with values from [zoneValues], which either adds new values or overrides existing ones.

Note that the fork operation is interceptable. A zone can thus change the zone specification (or zone values), giving the parent zone full control over the child zone.

### run()

```dart
R run<R>(R action())
```

Executes [action] in this zone.

By default (as implemented in the [root] zone), runs [action] with [current] set to this zone.

If [action] throws, the synchronous exception is not caught by the zone's error handler. Use [runGuarded] to achieve that.

Since the root zone is the only zone that can modify the value of [current], custom zones intercepting run should always delegate to their parent zone. They may take actions before and after the call.

### runUnary()

```dart
R runUnary<R, T>(R action(T argument), T argument)
```

Executes the given [action] with [argument] in this zone.

As [run] except that [action] is called with one [argument] instead of none.

### runBinary()

```dart
R runBinary<R, T1, T2>(R action(T1 argument1, T2 argument2), T1 argument1, T2 argument2)
```

Executes the given [action] with [argument1] and [argument2] in this zone.

As [run] except that [action] is called with two arguments instead of none.

### runGuarded()

```dart
void runGuarded(void action())
```

Executes the given [action] in this zone and catches synchronous errors.

This function is equivalent to:

```dart
try {
  this.run(action);
} catch (e, s) {
  this.handleUncaughtError(e, s);
}
```

See [run].

### runUnaryGuarded()

```dart
void runUnaryGuarded<T>(void action(T argument), T argument)
```

Executes the given [action] with [argument] in this zone and catches synchronous errors.

See [runGuarded].

### runBinaryGuarded()

```dart
void runBinaryGuarded<T1, T2>(
  void action(T1 argument1, T2 argument2), 
  T1 argument1, 
  T2 argument2
)
```

Executes the given [action] with [argument1] and [argument2] in this zone and catches synchronous errors.

See [runGuarded].

### registerCallback()

```dart
ZoneCallback<R> registerCallback<R>(R callback())
```

Registers the given callback in this zone.

When implementing an asynchronous primitive that uses callbacks, the callback must be registered using [registerCallback] at the point where the user provides the callback. This allows zones to record other information that they need at the same time, perhaps even wrapping the callback, so that the callback is prepared when it is later run in the same zones (using [run]). For example, a zone may decide to store the stack trace (at the time of the registration) with the callback.

Returns the callback that should be used in place of the provided [callback]. Frequently zones simply return the original callback.

Custom zones may intercept this operation. The default implementation in [Zone.root] returns the original callback unchanged.

### registerUnaryCallback()

```dart
ZoneUnaryCallback<R, T> registerUnaryCallback<R, T>(R callback(T arg))
```

Registers the given callback in this zone.

Similar to [registerCallback] but with a unary callback.

### registerBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> registerBinaryCallback<R, T1, T2>(R callback(T1 arg1, T2 arg2))
```

Registers the given callback in this zone.

Similar to [registerCallback] but with a binary callback.

### bindCallback()

```dart
ZoneCallback<R> bindCallback<R>(R callback())
```

Registers the provided [callback] and returns a function that will execute in this zone.

Equivalent to:

```dart
ZoneCallback registered = this.registerCallback(callback);
return () => this.run(registered);
```

### bindUnaryCallback()

```dart
ZoneUnaryCallback<R, T> bindUnaryCallback<R, T>(R callback(T argument))
```

Registers the provided [callback] and returns a function that will execute in this zone.

Equivalent to:

```dart
ZoneCallback registered = this.registerUnaryCallback(callback);
return (arg) => this.runUnary(registered, arg);
```

### bindBinaryCallback()

```dart
ZoneBinaryCallback<R, T1, T2> bindBinaryCallback<R, T1, T2>(R callback(T1 argument1, T2 argument2))
```

Registers the provided [callback] and returns a function that will execute in this zone.

Equivalent to:

```dart
ZoneCallback registered = registerBinaryCallback(callback);
return (arg1, arg2) => this.runBinary(registered, arg1, arg2);
```

### bindCallbackGuarded()

```dart
void Function() bindCallbackGuarded(void Function() callback)
```

Registers the provided [callback] and returns a function that will execute in this zone.

When the function executes, errors are caught and treated as uncaught errors.

Equivalent to:

```dart
ZoneCallback registered = this.registerCallback(callback);
return () => this.runGuarded(registered);
```

### bindUnaryCallbackGuarded()

```dart
void Function(T) bindUnaryCallbackGuarded<T>(void callback(T argument))
```

Registers the provided [callback] and returns a function that will execute in this zone.

When the function executes, errors are caught and treated as uncaught errors.

Equivalent to:

```dart
ZoneCallback registered = this.registerUnaryCallback(callback);
return (arg) => this.runUnaryGuarded(registered, arg);
```

### bindBinaryCallbackGuarded()

```dart
void Function(T1, T2) bindBinaryCallbackGuarded<T1, T2>(void callback(T1 argument1, T2 argument2))
```

Registers the provided [callback] and returns a function that will execute in this zone.

Equivalent to:

```dart
 ZoneCallback registered = registerBinaryCallback(callback);
 return (arg1, arg2) => this.runBinaryGuarded(registered, arg1, arg2);
```

### errorCallback()

```dart
AsyncError? errorCallback(Object error, StackTrace? stackTrace)
```

Intercepts errors when added programmatically to a [Future] or [Stream].

When calling [Completer.completeError], [StreamController.addError], or some [Future] constructors, the current zone is allowed to intercept and replace the error.

Future constructors invoke this function when the error is received directly, for example with [Future.error], or when the error is caught synchronously, for example with [Future.sync].

There is no guarantee that an error is only sent through [errorCallback] once. Libraries that use intermediate controllers or completers might end up invoking [errorCallback] multiple times.

Returns `null` if no replacement is desired. Otherwise returns an instance of [AsyncError] holding the new pair of error and stack trace.

Custom zones may intercept this operation.

Implementations of a new asynchronous primitive that converts synchronous errors to asynchronous errors rarely need to invoke [errorCallback], since errors are usually reported through future completers or stream controllers.

### scheduleMicrotask()

```dart
void scheduleMicrotask(void Function() callback)
```

Runs [callback] asynchronously in this zone.

The global `scheduleMicrotask` delegates to the [current] zone's [scheduleMicrotask]. The root zone's implementation interacts with the underlying system to schedule the given callback as a microtask.

Custom zones may intercept this operation (for example to wrap the given [callback]), or to implement their own microtask scheduler. In the latter case, they will usually still use the parent zone's [ZoneDelegate.scheduleMicrotask] to attach themselves to the existing event loop.

### createTimer()

```dart
Timer createTimer(Duration duration, void Function() callback)
```

Creates a [Timer] where the callback is executed in this zone.

### createPeriodicTimer()

```dart
Timer createPeriodicTimer(Duration period, void callback(Timer timer))
```

Creates a periodic [Timer] where the callback is executed in this zone.

### print()

```dart
void print(String line)
```

Prints the given [line].

The global `print` function delegates to the current zone's [print] function which makes it possible to intercept printing.

Example:

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

Retrieves the zone-value associated with [key].

If this zone does not contain the value looks up the same key in the parent zone. If the [key] is not found returns `null`.

Any object can be used as key, as long as it has compatible `operator ==` and `hashCode` implementations. By controlling access to the key, a zone can grant or deny access to the zone value.
