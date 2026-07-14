# ChunkedConversionSink

```dart
abstract interface class ChunkedConversionSink<T> implements Sink<T> {}
```

[ChunkedConversionSink](https://www.yuque.com/thyname/dart.convert/chunkedconversionsink) 用于在分块转换过程中，在两个转换器之间更高效地传输数据。

基本的 `ChunkedConversionSink` 只是一个 [Sink](https://www.yuque.com/thyname/dart.core/sink)，转换器应当能够配合普通的 `Sink` 工作，但对某些特殊类型的 `ChunkedConversionSink` 可能会工作得更高效。

## 构造函数

### ChunkedConversionSink.withCallback()

```dart
ChunkedConversionSink.withCallback(void Function(List<T> accumulated) callback)
```

## 方法

### add()

```dart
void add(T chunk)
```

将分块数据添加到此 sink。

当转换器用作 [StreamTransformer](https://www.yuque.com/thyname/dart.async/streamtransformer) 时，也会使用此方法。

### close()

```dart
void close()
```

关闭此 sink。

这表示分块转换的结束。当转换器用作 [StreamTransformer](https://www.yuque.com/thyname/dart.async/streamtransformer) 时，会调用此方法。
