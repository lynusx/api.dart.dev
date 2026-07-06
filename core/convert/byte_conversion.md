# ByteConversionSink

```dart
abstract mixin class ByteConversionSink implements ChunkedConversionSink<List<int>> {}
```

The [ByteConversionSink] provides an interface for converters to efficiently transmit byte data.

Instead of limiting the interface to one non-chunked list of bytes it accepts its input in chunks (themselves being lists of bytes).

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

Adds the next [chunk] to `this`.

Adds the bytes defined by [start] and [end]-exclusive to `this`.

If [isLast] is `true` closes `this`.

Contrary to `add` the given [chunk] must not be held onto. Once the method returns, it is safe to overwrite the data in it.

---



# ByteConversionSinkBase

```dart
typedef ByteConversionSinkBase = ByteConversionSink
```

This class provides a base-class for converters that need to accept byte inputs.
