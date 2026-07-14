# TimelineSyncFunction

```dart
typedef TimelineSyncFunction<T> = T Function()
```

[Timeline.timeSync] 函数参数的类型定义。

# Flow

```dart
final class Flow {}
```

用于表示 Flow 事件的类。

[Flow](https://www.yuque.com/thyname/flutter.widgets/flow) 对象用于在时间线切片（例如下方 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 类创建的切片）之间穿引流事件（flow event）。添加 [Flow](https://www.yuque.com/thyname/flutter.widgets/flow) 对象会使 Chrome 追踪查看器中的切片之间绘制箭头。箭头起始于传入了 [Flow.begin] 对象的 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 事件，经过传入了 [Flow.step] 对象的 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 事件，最终结束于传入了 [Flow.end] 对象、且具有相同 [Flow.id] 的事件。例如：

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

该 flow 事件的 flow id。

### begin()

```dart
Flow begin({int? id})
```

一个 “begin” 类型的 Flow 事件。

当传递给 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 的方法时，生成一个 “begin” Flow 事件。如果未提供 [id]，将生成一个不与任何其他 Dart 生成的 flow id 冲突的 id。

### step()

```dart
Flow step(int id)
```

一个 “step” 类型的 Flow 事件。

当传递给 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 的方法时，生成一个 “step” Flow 事件。[id] 参数为必需项，可以来自另一个 [Flow](https://www.yuque.com/thyname/flutter.widgets/flow) 事件，也可以是来自环境的某个 id。

### end()

```dart
Flow end(int id)
```

一个 “end” 类型的 Flow 事件。

当传递给 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 的方法时，生成一个 “end” Flow 事件。[id] 参数为必需项，可以来自另一个 [Flow](https://www.yuque.com/thyname/flutter.widgets/flow) 事件，也可以是来自环境的某个 id。

# Timeline

```dart
abstract final class Timeline {}
```

向时间线添加内容。

[Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 的方法向时间线添加同步事件。在生成 Chrome 追踪格式的时间线时，使用 [Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 会生成 “Complete” 事件。[Timeline](https://www.yuque.com/thyname/dart.developer/timeline) 的 [startSync] 和 [finishSync] 可以显式调用，也可以通过将闭包包装在 [timeSync] 中隐式调用。例如：

```dart
Timeline.startSync("Doing Something");
doSomething();
Timeline.finishSync();
```

或者：

```dart
Timeline.timeSync("Doing Something", () {
  doSomething();
});
```

### startSync()

```dart
void startSync(String name, {Map? arguments, Flow? flow})
```

开始一个标记为 [name] 的同步操作。可选地接受一个 [arguments] 映射。此切片还可以选择关联一个 [Flow](https://www.yuque.com/thyname/flutter.widgets/flow) 事件。此操作必须在返回事件队列之前结束。

### finishSync()

```dart
void finishSync()
```

结束最后一个已开始的同步操作。

### instantSync()

```dart
void instantSync(String name, {Map? arguments})
```

发出一个即时事件。

### timeSync()

```dart
T timeSync<T>(String name, TimelineSyncFunction<T> function, {Map? arguments, Flow? flow})
```

用于为同步函数 [function] 计时的实用方法。内部通过 [startSync] 和 [finishSync] 包裹 [function] 的调用来实现。

### now

```dart
int get now
```

时间线所用时钟的当前时间戳。单位为微秒。

在 Dart VM 上运行时，使用与嵌入 API 的 `Dart_TimelineGetMicros` 相同的单调时钟。

# TimelineTask

```dart
final class TimelineTask {}
```

时间线上的一个异步任务。一个异步任务可以包含许多（嵌套的）同步操作。同步操作的存活时间可以长于当前的 isolate 事件。要将 [TimelineTask](https://www.yuque.com/thyname/dart.developer/timelinetask) 传递给另一个 isolate，必须先调用 [pass] 获取任务 id，然后在另一个 isolate 中构造一个新的 [TimelineTask](https://www.yuque.com/thyname/dart.developer/timelinetask)。

### TimelineTask()

```dart
TimelineTask({TimelineTask? parent, String? filterKey})
```

创建一个任务。任务 ID 将由系统设置。

如果提供了 [parent]，则在调用 [start] 时会将父任务的任务 ID 作为参数 'parentId' 提供。在 DevTools 中，此参数会使该 [TimelineTask](https://www.yuque.com/thyname/dart.developer/timelinetask) 与 [parent] [TimelineTask](https://www.yuque.com/thyname/dart.developer/timelinetask) 建立关联。

如果提供了 [filterKey]，将在与此任务关联的每个事件的参数中插入一个名为 `filterKey` 的属性，该属性的值将设置为 [filterKey] 的值。

### TimelineTask.withTaskId()

```dart
TimelineTask.withTaskId(int taskId, {String? filterKey})
```

使用显式的 [taskId] 创建一个任务。当需要将一个任务从一个 isolate 传递到另一个 isolate 时，此方法非常有用。

重要提示：仅可提供通过调用 [TimelineTask.pass] 获得的任务 ID。指定自定义 ID 可能导致 ID 冲突，从而导致时间线事件渲染错误。

如果提供了 [filterKey]，将在与此任务关联的每个事件的参数中插入一个名为 `filterKey` 的属性，该属性的值将设置为 [filterKey] 的值。

### start()

```dart
void start(String name, {Map? arguments})
```

在此任务中开始一个名为 [name] 的同步操作。可选地接受一个 [arguments] 映射。

### instant()

```dart
void instant(String name, {Map? arguments})
```

为此任务发出一个即时事件。可选地接受一个 [arguments] 映射。

### finish()

```dart
void finish({Map? arguments})
```

结束最后一个已开始的同步操作。可选地接受一个 [arguments] 映射。

### pass()

```dart
int pass()
```

获取该 [TimelineTask](https://www.yuque.com/thyname/dart.developer/timelinetask) 的任务 id。如果操作栈不为空，将抛出异常。
