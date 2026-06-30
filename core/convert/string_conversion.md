# StringConversionSink

```dart
abstract mixin class StringConversionSink implements ChunkedConversionSink<String> {}
```

A sink for converters to efficiently transmit String data.

Instead of limiting the interface to one non-chunked [String] it accepts partial strings or can be transformed into a byte sink that accepts UTF-8 code units.

The [StringConversionSink] class provides a default implementation of [add], [asUtf8Sink] and [asStringSink].

### StringConversionSink()

```dart
StringConversionSink()
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

Creates a new instance wrapping the given [sink].

Every string that is added to the returned instance is forwarded to the [sink]. The instance is allowed to buffer and is not required to forward immediately.

### addSlice()

```dart
void addSlice(String chunk, int start, int end, bool isLast)
```

Adds the next [chunk] to `this`.

Adds the substring defined by [start] and [end]-exclusive to `this`.

If [isLast] is `true` closes `this`.

### add()

```dart
void add(String str)
```

### asUtf8Sink()

```dart
ByteConversionSink asUtf8Sink(bool allowMalformed)
```

Returns `this` as a sink that accepts UTF-8 input.

If used, this method must be the first and only call to `this`. It invalidates `this`. All further operations must be performed on the result.

### asStringSink()

```dart
ClosableStringSink asStringSink()
```

Returns `this` as a [ClosableStringSink].

If used, this method must be the first and only call to `this`. It invalidates `this`. All further operations must be performed on the result.

# ClosableStringSink

```dart
abstract interface class ClosableStringSink implements StringSink {}
```

A [ClosableStringSink] extends the [StringSink] interface by adding a `close` method.

### ClosableStringSink.fromStringSink()

```dart
ClosableStringSink.fromStringSink(StringSink sink, void Function() onClose)
```

Creates a new instance combining a [StringSink] [sink] and a callback [onClose] which is invoked when the returned instance is closed.

### close()

```dart
void close()
```

Closes `this` and flushes any outstanding data.

# StringConversionSinkBase

```dart
typedef StringConversionSinkBase = StringConversionSink
```

This class provides a base-class for converters that need to accept String inputs.

# StringConversionSinkMixin

```dart
typedef StringConversionSinkMixin = StringConversionSink
```

This class provides a mixin for converters that need to accept String inputs.
