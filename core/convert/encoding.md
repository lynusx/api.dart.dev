# Encoding

```dart
abstract class Encoding extends Codec<String, List<int>> {}
```

开放式的编码集合。

一种编码（encoding）是将字符串编码为字节列表的 [Codec](https://www.yuque.com/thyname/dart.ui/codec)。

此类提供了 [decodeStream] 的默认实现，该实现不是增量式的，它会先收集全部输入，然后再进行解码。子类可以选择使用该默认实现，也可以实现更高效的流式解码。

## 构造函数

### Encoding()

```dart
const Encoding()
```

## 静态方法

### getByName()

```dart
Encoding? getByName(String? name)
```

返回指定名称字符集对应的 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding)。

使用的名称为 IANA 官方字符集名称（参见 [IANA character sets](http://www.iana.org/assignments/character-sets/character-sets.xml)）。名称不区分大小写。

如果不支持该字符集，则返回 `null`。

## 属性

### encoder

```dart
Converter<String, List<int>> get encoder
```

返回从 `String` 到 `List<int>` 的编码器。

它可能是有状态的，不应被重复使用。

### decoder

```dart
Converter<List<int>, String> get decoder
```

返回 `this` 的解码器，用于将 `List<int>` 转换为 `String`。

它可能是有状态的，不应被重复使用。

### name

```dart
String get name
```

编码的名称。

如果该编码是标准化的，则此名称为其 IANA 官方字符集名称（参见 http://www.iana.org/assignments/character-sets/character-sets.xml ）之一的小写形式。

## 方法

### decodeStream()

```dart
Future<String> decodeStream(Stream<List<int>> byteStream)
```
