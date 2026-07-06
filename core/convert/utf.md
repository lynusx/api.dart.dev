# unicodeReplacementCharacterRune

```dart
const int unicodeReplacementCharacterRune = 0xFFFD;
```

Unicode 替换字符 `U+FFFD`（�）。

---

# unicodeBomCharacterRune

```dart
const int unicodeBomCharacterRune = 0xFEFF;
```

Unicode 字节顺序标记（BOM）字符 `U+FEFF`。

---

# utf8

```dart
const Utf8Codec utf8 = Utf8Codec();
```

[Utf8Codec] 默认实现的一个实例。

该实例便于访问最常见的 UTF-8 使用场景。

示例：

```dart
var encoded = utf8.encode("Îñţérñåţîöñåļîžåţîờñ");
var decoded = utf8.decode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                           0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
```

---

# Utf8Codec

```dart
final class Utf8Codec extends Encoding {}
```

[Utf8Codec] 将字符串编码为 UTF-8 码元（字节），并将 UTF-8 码元解码为字符串。

## 构造函数

### Utf8Codec()

```dart
Utf8Codec({bool allowMalformed = false})
```

实例化一个新的 [Utf8Codec]。

可选参数 [allowMalformed] 定义 [decoder]（及 [decode]）如何处理无效或未终止的字符序列。

如果为 `true`（且在方法调用时未被覆盖），[decode] 和 [decoder] 会将无效（或未终止）的字节序列替换为 Unicode 替换字符 `U+FFFD`（�）。否则将抛出 [FormatException]。

## 属性

### name

```dart
String get name
```

该编解码器的名称是 "utf-8"。

### encoder

```dart
Utf8Encoder get encoder
```

### decoder

```dart
Utf8Decoder get decoder
```

## 方法

### decode()

```dart
String decode(
  List<int> codeUnits, {
  bool? allowMalformed
})
```

将 UTF-8 编码的 [codeUnits]（一个无符号 8 位整数列表）解码为对应的字符串。

如果 [codeUnits] 以 [unicodeBomCharacterRune] 的编码开头，该字符将被丢弃。

如果 [allowMalformed] 为 `true`，解码器会将无效（或未终止）的字符序列替换为 Unicode 替换字符 `U+FFFD`（�）。否则将抛出 [FormatException]。

如果未提供 [allowMalformed]，默认使用实例化 `this` 时使用的 `allowMalformed` 值。

### encode()

```dart
Uint8List encode(String string)
```

将 [string] 编码为 UTF-8。

---

# Utf8Encoder

```dart
final class Utf8Encoder extends Converter<String, List<int>> {}
```

此类将字符串转换为其 UTF-8 码元（无符号 8 位整数列表）。

示例：

```dart
final utf8Encoder = utf8.encoder;
const sample = 'Îñţérñåţîöñåļîžåţîờñ';
final encodedSample = utf8Encoder.convert(sample);
print(encodedSample);
```

## 构造函数

### Utf8Encoder()

```dart
const Utf8Encoder()
```

## 方法

### convert()

```dart
Uint8List convert(String string, [int start = 0, int? end])
```

将 [string] 转换为其 UTF-8 码元（无符号 8 位整数列表）。

如果提供了 [start] 和 [end]，则仅转换子字符串 `string.substring(start, end)`。

输入字符串中任何未配对的代理字符（`U+D800`-`U+DFFF`）都会被编码为 Unicode 替换字符 `U+FFFD`（�）。

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<List<int>> sink)
```

开始一次分块转换。

如果给定的 [sink] 是 [ByteConversionSink]，转换器的工作效率会更高。

### bind()

```dart
Stream<List<int>> bind(Stream<String> stream)
```

---

# Utf8Decoder

```dart
final class Utf8Decoder extends Converter<List<int>, String> {}
```

此类将 UTF-8 码元（无符号 8 位整数列表）转换为字符串。

示例：

```dart
final utf8Decoder = utf8.decoder;
const encodedBytes = [
  195, 142, 195, 177, 197, 163, 195, 169, 114, 195, 177, 195, 165, 197,
  163, 195, 174, 195, 182, 195, 177, 195, 165, 196, 188, 195, 174, 197,
  190, 195, 165, 197, 163, 195, 174, 225, 187, 157, 195, 177];

final decodedBytes = utf8Decoder.convert(encodedBytes);
print(decodedBytes); // Îñţérñåţîöñåļîžåţîờñ
```

如果编码输入包含无效的 UTF-8 字节序列，且 [allowMalformed] 为 `false`（默认值），则抛出 [FormatException]。

如果 [allowMalformed] 为 `true`，无效字节序列会被转换为一个或多个 Unicode 替换字符 U+FFFD（'�'）。

设置 `allowMalformed` 为 true 的示例：

```dart
const utf8Decoder = Utf8Decoder(allowMalformed: true);
const encodedBytes = [0xFF];
final decodedBytes = utf8Decoder.convert(encodedBytes);
print(decodedBytes); // �
```

## 构造函数

### Utf8Decoder()

```dart
Utf8Decoder({bool allowMalformed = false})
```

实例化一个新的 [Utf8Decoder]。

可选参数 [allowMalformed] 定义 [convert] 如何处理无效或未终止的字符序列。

如果为 `true`，[convert] 会将无效（或未终止）的字符序列替换为 Unicode 替换字符 `U+FFFD`（�）。否则将抛出 [FormatException]。

## 方法

### convert()

```dart
String convert(List<int> codeUnits, [int start = 0, int? end])
```

将 UTF-8 编码的 [codeUnits]（一个无符号 8 位整数列表）转换为对应的字符串。

使用从 [start] 到（不包括）[end] 的码元。如果省略 [end]，默认为 `codeUnits.length`。

如果 [codeUnits] 以 [unicodeBomCharacterRune] 的编码开头，该字符将被丢弃。

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

开始一次分块转换。

如果给定的 [sink] 是 [StringConversionSink]，转换器的工作效率会更高。

### bind()

```dart
Stream<String> bind(Stream<List<int>> stream)
```

### fuse()

```dart
Converter<List<int>, T> fuse<T>(Converter<String, T> next)
```
