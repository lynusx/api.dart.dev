# Converter

```dart
abstract mixin class Converter<S, T> implements StreamTransformerBase<S, T> {}
```

A [Converter] converts data from one representation into another.

The [Converter] class provides a default implementation for every method other than [convert].

### Converter()

```dart
Converter()
```

### castFrom()

```dart
Converter<TS, TT> castFrom<SS, ST, TS, TT>(Converter<SS, ST> source)
```

Adapts [source] to be a `Converter<TS, TT>`.

This allows [source] to be used at the new type, but at run-time it must satisfy the requirements of both the new type and its original type.

Conversion input must be both [SS] and [TS] and the output created by [source] for those input must be both [ST] and [TT].

### convert()

```dart
T convert(S input)
```

Converts [input] and returns the result of the conversion.

### fuse()

```dart
Converter<S, TT> fuse<TT>(Converter<T, TT> other)
```

Fuses `this` with [other].

Encoding with the resulting converter is equivalent to converting with `this` before converting with `other`.

### startChunkedConversion()

```dart
Sink<S> startChunkedConversion(Sink<T> sink)
```

Starts a chunked conversion.

The returned sink serves as input for the long-running conversion. The given [sink] serves as output.

### bind()

```dart
Stream<T> bind(Stream<S> stream)
```

### cast()

```dart
Converter<RS, RT> cast<RS, RT>()
```

Provides a `Converter<RS, RT>` view of this stream transformer.

The resulting transformer will check at run-time that all conversion inputs are actually instances of [S], and it will check that all conversion output produced by this converter are actually instances of [RT].
