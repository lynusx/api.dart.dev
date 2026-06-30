# ControllerCallback

```dart
typedef ControllerCallback = void Function()
```

Type of a stream controller's `onListen`, `onPause` and `onResume` callbacks.

# ControllerCancelCallback

```dart
typedef ControllerCancelCallback = FutureOr<void> Function()
```

Type of stream controller `onCancel` callbacks.

# StreamController

```dart
abstract interface class StreamController<T> implements StreamSink<T> {}
```

A controller with the stream it controls.

This controller allows sending data, error and done events on its [stream].

This class can be used to create a simple stream that others can listen on, and to push events to that stream.

It's possible to check whether the stream is paused or not, and whether it has subscribers or not, as well as getting a callback when either of these change.

Example:

```dart
final streamController = StreamController(
  onPause: () => print('Paused'),
  onResume: () => print('Resumed'),
  onCancel: () => print('Cancelled'),
  onListen: () => print('Listens'),
);

streamController.stream.listen(
  (event) => print('Event: $event'),
  onDone: () => print('Done'),
  onError: (error) => print(error),
);
```

To check whether there is a subscriber on the stream, use [hasListener].

```dart continued
var hasListener = streamController.hasListener; // true
```

To send data events to the stream, use [add] or [addStream].

```dart continued
streamController.add(999);
final stream = Stream<int>.periodic(
    const Duration(milliseconds: 200), (count) => count * count).take(4);
await streamController.addStream(stream);
```

To send an error event to the stream, use [addError] or [addStream].

```dart continued
streamController.addError(Exception('Issue 101'));
await streamController.addStream(Stream.error(Exception('Issue 404')));
```

To check whether the stream is closed, use [isClosed].

```dart continued
var isClosed = streamController.isClosed; // false
```

To close the stream, use [close].

```dart continued
await streamController.close();
isClosed = streamController.isClosed; // true
```

### stream

```dart
Stream<T> get stream
```

The stream that this controller is controlling.

### StreamController()

```dart
StreamController({void onListen(), void onPause(), void onResume(), FutureOr<void> onCancel(), bool sync = false})
```

A controller with a [stream] that supports only one single subscriber.

If [sync] is true, the returned stream controller is a [SynchronousStreamController], and must be used with the care and attention necessary to not break the [Stream] contract. If in doubt, use the non-sync version.

Using an asynchronous controller will never give the wrong behavior, but using a synchronous controller incorrectly can cause otherwise correct programs to break.

A synchronous controller is only intended for optimizing event propagation when one asynchronous event immediately triggers another. It should not be used unless the calls to [add] or [addError] are guaranteed to occur in places where it won't break `Stream` invariants.

Use synchronous controllers only to forward (potentially transformed) events from another stream or a future.

A Stream should be inert until a subscriber starts listening on it (using the [onListen] callback to start producing events). Streams should not leak resources (like websockets) when no user ever listens on the stream.

The controller buffers all incoming events until a subscriber is registered, but this feature should only be used in rare circumstances.

The [onPause] function is called when the stream becomes paused. [onResume] is called when the stream resumed.

The [onListen] callback is called when the stream receives its listener and [onCancel] when the listener ends its subscription. If [onCancel] needs to perform an asynchronous operation, [onCancel] should return a future that completes when the cancel operation is done.

If the stream is canceled before the controller needs data the [onResume] call might not be executed.

### StreamController.broadcast()

```dart
StreamController.broadcast({void onListen(), void onCancel(), bool sync = false})
```

A controller where [stream] can be listened to more than once.

The [Stream] returned by [stream] is a broadcast stream. It can be listened to more than once.

A Stream should be inert until a subscriber starts listening on it (using the [onListen] callback to start producing events). Streams should not leak resources (like websockets) when no user ever listens on the stream.

Broadcast streams do not buffer events when there is no listener.

The controller distributes any events to all currently subscribed listeners at the time when [add], [addError] or [close] is called. It is not allowed to call `add`, `addError`, or `close` before a previous call has returned. The controller does not have any internal queue of events, and if there are no listeners at the time the event or error is added, it will just be dropped.

Each listener subscription is handled independently, and if one pauses, only the pausing listener is affected. A paused listener will buffer events internally until unpaused or canceled.

If [sync] is true, events may be fired directly by the stream's subscriptions during an [add], [addError] or [close] call. The returned stream controller is a [SynchronousStreamController], and must be used with the care and attention necessary to not break the [Stream] contract. See [Completer.sync] for some explanations on when a synchronous dispatching can be used. If in doubt, keep the controller non-sync.

If [sync] is false, the event will always be fired at a later time, after the code adding the event has completed. In that case, no guarantees are given with regard to when multiple listeners get the events, except that each listener will get all events in the correct order. Each subscription handles the events individually. If two events are sent on an async controller with two listeners, one of the listeners may get both events before the other listener gets any. A listener must be subscribed both when the event is initiated (that is, when [add] is called) and when the event is later delivered, in order to receive the event.

The [onListen] callback is called when the first listener is subscribed, and the [onCancel] is called when there are no longer any active listeners. If a listener is added again later, after the [onCancel] was called, the [onListen] will be called again.

### onListen

```dart
void Function()? onListen
```

The callback which is called when the stream is listened to.

May be set to `null`, in which case no callback will happen.

### onPause

```dart
void Function()? onPause
```

The callback which is called when the stream is paused.

May be set to `null`, in which case no callback will happen.

Pause related callbacks are not supported on broadcast stream controllers.

### onResume

```dart
void Function()? onResume
```

The callback which is called when the stream is resumed.

May be set to `null`, in which case no callback will happen.

Pause related callbacks are not supported on broadcast stream controllers.

### onCancel

```dart
FutureOr<void> Function()? onCancel
```

The callback which is called when the stream is canceled.

May be set to `null`, in which case no callback will happen.

### sink

```dart
StreamSink<T> get sink
```

Returns a view of this object that only exposes the [StreamSink] interface.

### isClosed

```dart
bool get isClosed
```

Whether the stream controller is closed for adding more events.

The controller becomes closed by calling the [close] method. New events cannot be added, by calling [add] or [addError], to a closed controller.

If the controller is closed, the "done" event might not have been delivered yet, but it has been scheduled, and it is too late to add more events.

### isPaused

```dart
bool get isPaused
```

Whether the subscription would need to buffer events.

This is the case if the controller's stream has a listener and it is paused, or if it has not received a listener yet. In that case, the controller is considered paused as well.

A broadcast stream controller is never considered paused. It always forwards its events to all uncanceled subscriptions, if any, and let the subscriptions handle their own pausing and buffering.

### hasListener

```dart
bool get hasListener
```

Whether there is a subscriber on the [Stream].

### add()

```dart
void add(T event)
```

Sends a data [event].

Listeners receive this event in a later microtask.

Note that a synchronous controller (created by passing true to the `sync` parameter of the `StreamController` constructor) delivers events immediately. Since this behavior violates the contract mentioned here, synchronous controllers should only be used as described in the documentation to ensure that the delivered events always _appear_ as if they were delivered in a separate microtask.

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

Sends or enqueues an error event.

Listeners receive this event at a later microtask. This behavior can be overridden by using `sync` controllers. Note, however, that sync controllers have to satisfy the preconditions mentioned in the documentation of the constructors.

### close()

```dart
Future close()
```

Closes the stream.

No further events can be added to a closed stream.

The returned future is the same future provided by [done]. It is completed when the stream listeners is done sending events, This happens either when the done event has been sent, or when the subscriber on a single-subscription stream is canceled.

A stream controller will not complete the returned future until all listeners present when the done event is sent have stopped listening. A listener will stop listening if it is cancelled, or if it has handled the done event. A paused listener will not process the done even until it is resumed, so completion of the returned Future will be delayed until all paused listeners have been resumed or cancelled.

If no one listens to a non-broadcast stream, or the listener pauses and never resumes, the done event will not be sent and this future will never complete.

### done

```dart
Future get done
```

A future which is completed when the stream controller is done sending events.

This happens either when the done event has been sent, or if the subscriber on a single-subscription stream is canceled.

A stream controller will not complete the returned future until all listeners present when the done event is sent have stopped listening. A listener will stop listening if it is cancelled, or if it has handled the done event. A paused listener will not process the done even until it is resumed, so completion of the returned Future will be delayed until all paused listeners have been resumed or cancelled.

If there is no listener on a non-broadcast stream, or the listener pauses and never resumes, the done event will not be sent and this future will never complete.

### addStream()

```dart
Future addStream(Stream<T> source, {bool? cancelOnError})
```

Receives events from [source] and puts them into this controller's stream.

Returns a future which completes when the source stream is done.

Events must not be added directly to this controller using [add], [addError], [close] or [addStream], until the returned future is complete.

Data and error events are forwarded to this controller's stream. A done event on the source will end the `addStream` operation and complete the returned future.

If [cancelOnError] is `true`, only the first error on [source] is forwarded to the controller's stream, and the `addStream` ends after this. If [cancelOnError] is false, all errors are forwarded and only a done event will end the `addStream`. If [cancelOnError] is omitted or `null`, it defaults to `false`.

# SynchronousStreamController

```dart
abstract interface class SynchronousStreamController<T> implements StreamController<T> {}
```

A stream controller that delivers its events synchronously.

A synchronous stream controller is intended for cases where an already asynchronous event triggers an event on a stream.

Instead of adding the event to the stream in a later microtask, causing extra latency, the event is instead fired immediately by the synchronous stream controller, as if the stream event was the current event or microtask.

The synchronous stream controller can be used to break the contract on [Stream], and it must be used carefully to avoid doing so.

The only advantage to using a [SynchronousStreamController] over a normal [StreamController] is the improved latency. Only use the synchronous version if the improvement is significant, and if its use is safe. Otherwise just use a normal stream controller, which will always have the correct behavior for a [Stream], and won't accidentally break other code.

Adding events to a synchronous controller should only happen as the very last part of the handling of the original event. At that point, adding an event to the stream is equivalent to returning to the event loop and adding the event in the next microtask.

Each listener callback will be run as if it was a top-level event or microtask. This means that if it throws, the error will be reported as uncaught as soon as possible. This is one reason to add the event as the last thing in the original event handler – any action done after adding the event will delay the report of errors in the event listener callbacks.

If an event is added in a setting that isn't known to be another event, it may cause the stream's listener to get that event before the listener is ready to handle it. We promise that after calling [Stream.listen], you won't get any events until the code doing the listen has completed. Calling [add] in response to a function call of unknown origin may break that promise.

An [onListen] callback from the controller is _not_ an asynchronous event, and adding events to the controller in the `onListen` callback is always wrong. The events will be delivered before the listener has even received the subscription yet.

The synchronous broadcast stream controller also has a restrictions that a normal stream controller does not: The [add], [addError], [close] and [addStream] methods _must not_ be called while an event is being delivered. That is, if a callback on a subscription on the controller's stream causes a call to any of the functions above, the call will fail. A broadcast stream may have more than one listener, and if an event is added synchronously while another is being also in the process of being added, the latter event might reach some listeners before the former. To prevent that, an event cannot be added while a previous event is being fired. This guarantees that an event is fully delivered when the first [add], [addError] or [close] returns, and further events will be delivered in the correct order.

This still only guarantees that the event is delivered to the subscription. If the subscription is paused, the actual callback may still happen later, and the event will instead be buffered by the subscription. Barring pausing, and the following buffered events that haven't been delivered yet, callbacks will be called synchronously when an event is added.

Adding an event to a synchronous non-broadcast stream controller while another event is in progress may cause the second event to be delayed and not be delivered synchronously, and until that event is delivered, the controller will not act synchronously.

### add()

```dart
void add(T data)
```

Adds event to the controller's stream.

As [StreamController.add], but must not be called while an event is being added by [add], [addError] or [close].

### addError()

```dart
void addError(Object error, [StackTrace? stackTrace])
```

Adds error to the controller's stream.

As [StreamController.addError], but must not be called while an event is being added by [add], [addError] or [close].

### close()

```dart
Future close()
```

Closes the controller's stream.

As [StreamController.close], but must not be called while an event is being added by [add], [addError] or [close].
