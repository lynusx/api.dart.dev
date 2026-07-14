# IOSink

```dart
abstract interface class IOSink implements StreamSink<List<int>>, StringSink {}
```

字节与文本输出的组合。

[IOSink](https://www.yuque.com/thyname/dart.io/iosink) 将字节 [StreamSink](https://www.yuque.com/thyname/dart.async/streamsink) 与 [StringSink](https://www.yuque.com/thyname/dart.core/stringsink) 组合在一起，可以方便地输出字节和文本。

`IOSink` 主要用于写入字节。通过 [write] 或 [writeCharCode] 写入的字符串会使用 [encoding] 转换为字节。通过 [add] 或 [addStream] 添加的整数数据会被视为字节数据，并会像使用 [int.toUnsigned] 一样截断为无符号 8 位值。由于这取决于 sink 背后的具体实现，因此不保证这种转换发生的具体时机。

写入文本（[write]）和添加字节（[add]）可以自由交替进行。

在使用 [addStream] 添加流的过程中，任何进一步向 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 添加或写入数据的尝试都将失败，直到 [addStream] 完成为止。

在 sink 关闭后向其添加数据是错误的。

### IOSink()

```dart
IOSink(StreamConsumer<List<int>> target, {Encoding encoding = utf8})
```

创建一个将输出发送到字节 [StreamConsumer](https://www.yuque.com/thyname/dart.async/streamconsumer) [target] 的 [IOSink](https://www.yuque.com/thyname/dart.io/iosink)。

通过 [StreamSink](https://www.yuque.com/thyname/dart.async/streamsink) 方法写入的文本会在输出到 [target] 之前使用 [encoding] 编码为字节。

### encoding

```dart
Encoding encoding
```

写入字符串时使用的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding)。

根据底层消费者（consumer）的不同，此属性可能是可变的。

### add()

```dart
void add(List<int> data)
```

将字节 [data] 添加到目标消费者，忽略 [encoding]。

[encoding] 不适用于此方法，[data] 列表会作为流事件直接传递给目标消费者。

调用此方法时，不得同时正在使用 [addStream] 添加流。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

传递给 `add` 后不应再修改该数据列表，因为目标消费者接收到的列表是原始状态还是修改后的状态是未定义的。

[data] 中不在 0 到 255 范围内的各个值，在使用之前会像 [int.toUnsigned] 一样被截断为其低 8 位。

### write()

```dart
void write(Object? object)
```

通过调用 [Object.toString] 将 [object] 转换为字符串，并将转换结果编码后 [add] 到目标消费者。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

### writeAll()

```dart
void writeAll(Iterable objects, [String separator = ""])
```

依次遍历给定的 [objects] 并 [write] 它们。

如果提供了 [separator]，则会在 objects 中任意两个元素之间执行一次带有 `separator` 的 `write` 操作。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

### writeln()

```dart
void writeln([Object? object = ""])
```

通过调用 [Object.toString] 将 [object] 转换为字符串，将结果写入 `this`，并在其后添加一个换行符。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

### writeCharCode()

```dart
void writeCharCode(int charCode)
```

写入 [charCode] 对应的字符。

此方法等价于 `write(String.fromCharCode(charCode))`。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

### addError()

```dart
void addError(dynamic error, [StackTrace? stackTrace])
```

将该错误作为错误事件传递给目标消费者。

调用此方法时，不得同时正在使用 [addStream] 添加流。

此操作是非阻塞的。有关如何获取此调用产生的任何错误，请参阅 [flush] 或 [done]。

### addStream()

```dart
Future addStream(Stream<List<int>> stream)
```

添加给定 [stream] 的所有元素。

返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，当给定 [stream] 的所有元素都添加完成后完成。

如果流中包含错误，`addStream` 会在该错误处结束，返回的 Future 会以该错误完成。

不得在使用此方法添加流未完成时再次调用此方法。

[stream] 发出的列表中不在 0 到 255 范围内的各个值，在使用之前会像 [int.toUnsigned] 一样被截断为其低 8 位。

### flush()

```dart
Future flush()
```

返回一个 [Future](https://www.yuque.com/thyname/dart.async/future)，当底层 [StreamConsumer](https://www.yuque.com/thyname/dart.async/streamconsumer) 接受了所有缓冲数据后完成。

在 [addStream] 未完成时不得调用此方法。

注意：这不一定等同于数据被操作系统刷新（flush）。

### close()

```dart
Future close()
```

关闭目标消费者。

注意：写入 [IOSink](https://www.yuque.com/thyname/dart.io/iosink) 的数据可能会被缓冲，调用 `close()` 不一定会刷新这些数据。要刷新所有缓冲的写入操作，请在调用 `close()` 之前调用 `flush()`。

### done

```dart
Future get done
```

一个 Future，当消费者关闭或发生错误时完成。

此 Future 与 [close] 返回的 Future 相同。
