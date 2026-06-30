# TimelineSyncFunction

```dart
typedef TimelineSyncFunction<T> = T Function()
```

A typedef for the function argument to [Timeline.timeSync].

# Flow

```dart
final class Flow {}
```

A class to represent Flow events.

[Flow] objects are used to thread flow events between timeline slices, for example, those created with the [Timeline] class below. Adding [Flow] objects cause arrows to be drawn between slices in Chrome's trace viewer. The arrows start at e.g [Timeline] events that are passed a [Flow.begin] object, go through [Timeline] events that are passed a [Flow.step] object, and end at [Timeline] events that are passed a [Flow.end] object, all having the same [Flow.id]. For example:

```dart
var flow = Flow.begin();
Timeline.timeSync('flow_test', () {
  doSomething();
}, flow: flow);

Timeline.timeSync('flow_test', () {
  doSomething();
}, flow: Flow.step(flow.id));

Timeline.timeSync('flow_test', () {
  doSomething();
}, flow: Flow.end(flow.id));
```

### id

```dart
int id
```

The flow id of the flow event.

### begin()

```dart
Flow begin({int? id})
```

A "begin" Flow event.

When passed to a [Timeline] method, generates a "begin" Flow event. If [id] is not provided, an id that conflicts with no other Dart-generated flow id's will be generated.

### step()

```dart
Flow step(int id)
```

A "step" Flow event.

When passed to a [Timeline] method, generates a "step" Flow event. The [id] argument is required. It can come either from another [Flow] event, or some id that comes from the environment.

### end()

```dart
Flow end(int id)
```

An "end" Flow event.

When passed to a [Timeline] method, generates a "end" Flow event. The [id] argument is required. It can come either from another [Flow] event, or some id that comes from the environment.

# Timeline

```dart
abstract final class Timeline {}
```

Add to the timeline.

[Timeline]'s methods add synchronous events to the timeline. When generating a timeline in Chrome's tracing format, using [Timeline] generates "Complete" events. [Timeline]'s [startSync] and [finishSync] can be used explicitly, or implicitly by wrapping a closure in [timeSync]. For example:

```dart
Timeline.startSync("Doing Something");
doSomething();
Timeline.finishSync();
```

Or:

```dart
Timeline.timeSync("Doing Something", () {
  doSomething();
});
```

### startSync()

```dart
void startSync(String name, {Map? arguments, Flow? flow})
```

Start a synchronous operation labeled [name]. Optionally takes a [Map] of [arguments]. This slice may also optionally be associated with a [Flow] event. This operation must be finished before returning to the event queue.

### finishSync()

```dart
void finishSync()
```

Finish the last synchronous operation that was started.

### instantSync()

```dart
void instantSync(String name, {Map? arguments})
```

Emit an instant event.

### timeSync()

```dart
T timeSync<T>(String name, TimelineSyncFunction<T> function, {Map? arguments, Flow? flow})
```

A utility method to time a synchronous [function]. Internally calls [function] bracketed by calls to [startSync] and [finishSync].

### now

```dart
int get now
```

The current time stamp from the clock used by the timeline. Units are microseconds.

When run on the Dart VM, uses the same monotonic clock as the embedding API's `Dart_TimelineGetMicros`.

# TimelineTask

```dart
final class TimelineTask {}
```

An asynchronous task on the timeline. An asynchronous task can have many (nested) synchronous operations. Synchronous operations can live longer than the current isolate event. To pass a [TimelineTask] to another isolate, you must first call [pass] to get the task id and then construct a new [TimelineTask] in the other isolate.

### TimelineTask()

```dart
TimelineTask({TimelineTask? parent, String? filterKey})
```

Create a task. The task ID will be set by the system.

If [parent] is provided, the parent's task ID is provided as argument 'parentId' when [start] is called. In DevTools, this argument will result in this [TimelineTask] being linked to the [parent] [TimelineTask].

If [filterKey] is provided, a property named `filterKey` will be inserted into the arguments of each event associated with this task. The `filterKey` will be set to the value of [filterKey].

### TimelineTask.withTaskId()

```dart
TimelineTask.withTaskId(int taskId, {String? filterKey})
```

Create a task with an explicit [taskId]. This is useful if you are passing a task from one isolate to another.

Important note: only provide task IDs which have been obtained as a result of invoking [TimelineTask.pass]. Specifying a custom ID can lead to ID collisions, resulting in incorrect rendering of timeline events.

If [filterKey] is provided, a property named `filterKey` will be inserted into the arguments of each event associated with this task. The `filterKey` will be set to the value of [filterKey].

### start()

```dart
void start(String name, {Map? arguments})
```

Start a synchronous operation within this task named [name]. Optionally takes a [Map] of [arguments].

### instant()

```dart
void instant(String name, {Map? arguments})
```

Emit an instant event for this task. Optionally takes a [Map] of [arguments].

### finish()

```dart
void finish({Map? arguments})
```

Finish the last synchronous operation that was started. Optionally takes a [Map] of [arguments].

### pass()

```dart
int pass()
```

Retrieve the [TimelineTask]'s task id. Will throw an exception if the stack is not empty.
