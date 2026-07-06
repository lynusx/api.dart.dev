# Codec

```dart
abstract mixin class Codec<S, T> {}
```

[Codec] 用于编码，并且（如果支持）解码数据。

编解码器（Codec）可以被融合（fuse）。例如，将 [json] 与 [utf8] 融合会生成一个能够将 Json 对象直接转换为字节，或将字节直接解码为 json 对象的编码器。

融合后的编解码器通常会尝试对操作进行优化，其速度可能比单独执行每一步编码更快。

[Codec] 类提供了 [encode]、[decode]、[fuse] 和 [inverted] 的默认实现。子类可以选择提供这些方法更高效的实现。

## 构造函数

### Codec()

```dart
const Codec<S, T>()
```

## 属性

### encoder

```dart
Converter<S, T> get encoder
```

返回从 [S] 到 [T] 的编码器。

它可能是有状态的，不应被重复使用。

### decoder

```dart
Converter<T, S> get decoder
```

返回 `this` 的解码器，用于将 [T] 转换为 [S]。

它可能是有状态的，不应被重复使用。

### inverted

```dart
Codec<T, S> get inverted
```

对 `this` 进行反转。

结果编解码器的 [encoder] 与 [decoder] 将互换。

## 方法

### encode()

```dart
T encode(S input)
```

对 [input] 进行编码。

输入将按照 `encoder.convert` 的方式进行编码。

### decode()

```dart
S decode(T encoded)
```

解码 [encoded] 数据。

输入将按照 `decoder.convert` 的方式进行解码。

### fuse()

```dart
Codec<S, R> fuse<R>(Codec<T, R> other)
```

将 `this` 与 `other` 融合。

编码时，融合后的编解码器会先使用 `this` 进行编码，再使用 [other] 进行编码。

解码时，融合后的编解码器会先使用 [other] 进行解码，再使用 `this` 进行解码。

在某些情况下，需要使用 [inverted] 编解码器才能正确地进行融合。也就是说，`this` 的输出类型（[T]）必须与第二个编解码器 [other] 的输入类型相匹配。

示例：

```dart
final jsonToBytes = json.fuse(utf8);
List<int> bytes = jsonToBytes.encode(["json-object"]);
var decoded = jsonToBytes.decode(bytes);
assert(decoded is List && decoded[0] == "json-object");

var inverted = json.inverted;
var jsonIdentity = json.fuse(inverted);
var jsonObject = jsonIdentity.encode(["1", 2]);
assert(jsonObject is List && jsonObject[0] == "1" && jsonObject[1] == 2);
```
