# Converter

```dart
abstract mixin class Converter<S, T> implements StreamTransformerBase<S, T> {}
```

[Converter] 用于将数据从一种表示形式转换为另一种表示形式。

[Converter] 类为除 [convert] 之外的每个方法都提供了默认实现。

## 构造函数

### Converter()

```dart
const Converter<S, T>()
```

## 静态方法

### castFrom()

```dart
Converter<TS, TT> castFrom<SS, ST, TS, TT>(Converter<SS, ST> source)
```

将 [source] 适配为 `Converter<TS, TT>`。

这使得 [source] 可以在新类型下使用，但在运行时，它必须同时满足新类型及其原始类型的要求。

转换的输入必须同时是 [SS] 和 [TS] 类型，且 [source] 针对这些输入生成的输出必须同时是 [ST] 和 [TT] 类型。

## 方法

### convert()

```dart
T convert(S input)
```

转换 [input] 并返回转换结果。

### fuse()

```dart
Converter<S, TT> fuse<TT>(Converter<T, TT> other)
```

将 `this` 与 [other] 融合。

使用融合后的转换器进行编码，等价于先使用 `this` 进行转换，再使用 `other` 进行转换。

### startChunkedConversion()

```dart
Sink<S> startChunkedConversion(Sink<T> sink)
```

启动分块转换。

返回的 sink 作为长时间运行转换过程的输入。给定的 [sink] 作为输出。

### bind()

```dart
Stream<T> bind(Stream<S> stream)
```

### cast()

```dart
Converter<RS, RT> cast<RS, RT>()
```

提供此流转换器的 `Converter<RS, RT>` 视图。

生成的转换器将在运行时检查所有转换输入是否确实为 [S] 的实例，并检查此转换器生成的所有转换输出是否确实为 [RT] 的实例。
