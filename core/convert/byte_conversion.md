# ByteConversionSink

```dart
abstract mixin class ByteConversionSink implements ChunkedConversionSink<List<int>> {}
```

[ByteConversionSink] 为转换器提供了一个用于高效传输字节数据的接口。

它不再局限于接受单个非分块的字节列表作为输入，而是以分块（每块本身也是字节列表）的形式接受输入。

## 构造函数

### ByteConversionSink()

```dart
const ByteConversionSink()
```

### ByteConversionSink.withCallback()

```dart
ByteConversionSink.withCallback(void Function(List<int> accumulated) callback)
```

### ByteConversionSink.from()

```dart
ByteConversionSink.from(Sink<List<int>> sink)
```

## 方法

### addSlice()

```dart
void addSlice(
  List<int> chunk,
  int start,
  int end,
  bool isLast
)
```

将下一个 [chunk] 添加到 `this`。

添加由 [start] 和 [end]（不包含）界定的字节。

如果 [isLast] 为 `true`，则关闭 `this`。

与 `add` 不同的是，传入的 [chunk] 不会被持续持有。方法返回后，即可安全地覆写其中的数据。

---

# ByteConversionSinkBase

```dart
typedef ByteConversionSinkBase = ByteConversionSink
```

该类为需要接受字节输入的转换器提供了一个基类。
