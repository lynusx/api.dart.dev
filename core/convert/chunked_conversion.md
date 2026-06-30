# ChunkedConversionSink

```dart
abstract interface class ChunkedConversionSink<T> implements Sink<T> {}
```

A [ChunkedConversionSink] is used to transmit data more efficiently between two converters during chunked conversions.

The basic `ChunkedConversionSink` is just a [Sink], and converters should work with a plain `Sink`, but may work more efficiently with certain specialized types of `ChunkedConversionSink`.

### ChunkedConversionSink()

```dart
ChunkedConversionSink()
```

### ChunkedConversionSink.withCallback()

```dart
ChunkedConversionSink.withCallback(void Function(List<T> accumulated) callback)
```

### add()

```dart
void add(T chunk)
```

Adds chunked data to this sink.

This method is also used when converters are used as [StreamTransformer]s.

### close()

```dart
void close()
```

Closes the sink.

This signals the end of the chunked conversion. This method is called when converters are used as [StreamTransformer]'s.
