# StringConversionSink

```dart
abstract mixin class StringConversionSink implements ChunkedConversionSink<String> {}
```

用于转换器高效传输 String 数据的 sink。

不同于将接口限制为单个非分块的 [String](https://www.yuque.com/thyname/dart.core/string)，它可以接受部分字符串，或转换为接受 UTF-8 码元的字节 sink。

[StringConversionSink](https://www.yuque.com/thyname/dart.convert/stringconversionsink) 类提供了 [add]、[asUtf8Sink] 和 [asStringSink] 的默认实现。

## 构造函数

### StringConversionSink()

```dart
const StringConversionSink();
```

### StringConversionSink.withCallback()

```dart
StringConversionSink.withCallback(void Function(String accumulated) callback)
```

### StringConversionSink.from()

```dart
StringConversionSink.from(Sink<String> sink)
```

### StringConversionSink.fromStringSink()

```dart
StringConversionSink.fromStringSink(StringSink sink)
```

创建一个包装给定 [sink] 的新实例。

添加到返回实例的每个字符串都会被转发到 [sink]。该实例允许缓冲，且不要求立即转发。

## 方法

### addSlice()

```dart
void addSlice(
  String chunk,
  int start,
  int end,
  bool isLast
)
```

将下一个 [chunk] 添加到 `this`。

添加由 [start] 和 [end]（不包括）定义的子字符串到 `this`。

如果 [isLast] 为 `true`，则关闭 `this`。

### add()

```dart
void add(String str)
```

将分块数据添加到此 sink。

当转换器用作 [StreamTransformer](https://www.yuque.com/thyname/dart.async/streamtransformer) 时，也会使用此方法。

### asUtf8Sink()

```dart
ByteConversionSink asUtf8Sink(bool allowMalformed)
```

将 `this` 作为接受 UTF-8 输入的 sink 返回。

如果使用此方法，必须是对 `this` 的第一次也是唯一一次调用。它会使 `this` 失效。所有后续操作都必须在结果上执行。

### asStringSink()

```dart
ClosableStringSink asStringSink()
```

将 `this` 作为 [ClosableStringSink](https://www.yuque.com/thyname/dart.convert/closablestringsink) 返回。

如果使用此方法，必须是对 `this` 的第一次也是唯一一次调用。它会使 `this` 失效。所有后续操作都必须在结果上执行。

---

# ClosableStringSink

```dart
abstract interface class ClosableStringSink implements StringSink {}
```

[ClosableStringSink](https://www.yuque.com/thyname/dart.convert/closablestringsink) 通过添加 `close` 方法扩展了 [StringSink](https://www.yuque.com/thyname/dart.core/stringsink) 接口。

## 构造函数

### ClosableStringSink.fromStringSink()

```dart
ClosableStringSink.fromStringSink(StringSink sink, void Function() onClose)
```

创建一个组合了 [StringSink](https://www.yuque.com/thyname/dart.core/stringsink) [sink] 和回调 [onClose] 的新实例，该回调会在返回的实例关闭时被调用。

## 方法

### close()

```dart
void close()
```

关闭 `this` 并刷新所有未处理的数据。

---

# StringConversionSinkBase

```dart
typedef StringConversionSinkBase = StringConversionSink
```

此类为需要接受 String 输入的转换器提供了一个基类。

---

# StringConversionSinkMixin

```dart
typedef StringConversionSinkMixin = StringConversionSink
```

此类为需要接受 String 输入的转换器提供了一个混入。
