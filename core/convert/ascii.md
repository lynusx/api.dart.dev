# ascii

```dart
AsciiCodec const ascii
```

[AsciiCodec] 默认实现的一个实例。

该实例为最常见的 ASCII 使用场景提供了便捷的访问方式。

示例：

```dart
var encoded = ascii.encode("This is ASCII!");
var decoded = ascii.decode([0x54, 0x68, 0x69, 0x73, 0x20, 0x69, 0x73,
                            0x20, 0x41, 0x53, 0x43, 0x49, 0x49, 0x21]);
```

# AsciiCodec

```dart
final class AsciiCodec extends Encoding {}
```

ASCII 字符的编码与解码。

一个 [Encoding]（字符串与字节之间的转换）实现，用于将 ASCII 字符（U+0000..U+007F）与其对应的字节值相互转换。

将任何非 ASCII 字符（U+0080..U+10FFFF）视为编码的无效输入，将任何大于等于 128 的字节视为解码的无效输入。

## 构造函数

### AsciiCodec()

```dart
AsciiCodec({bool allowInvalid = false})
```

实例化一个新的 [AsciiCodec]。

如果 [allowInvalid] 为 `true`，则 [decode] 方法以及由 [decoder] 返回的转换器默认允许无效值（即大于 127 的字节值）。如果允许无效值，无效值将被解码为 Unicode 替换字符（U+FFFD）。否则将抛出 [FormatException]。调用 [decode] 方法时可以选择覆盖此默认设置。

编码器不接受无效（非 ASCII）字符。

## 属性

### name

```dart
String get name
```

该编解码器的名称为 "us-ascii"。

### encoder

```dart
AsciiEncoder get encoder
```

返回从 `String` 到 `List<int>` 的编码器。

它可能是有状态的，不应被重复使用。

### decoder

```dart
AsciiDecoder get decoder
```

返回 `this` 的解码器，用于将 `List<int>` 转换为 `String`。

它可能是有状态的，不应被重复使用。

## 方法

### encode()

```dart
Uint8List encode(String source)
```

对 `input` 进行编码。

输入将按照 `encoder.convert` 的方式进行编码。

### decode()

```dart
String decode(
  List<int> bytes, {
  bool? allowInvalid
})
```

将 ASCII 字节 [bytes]（一个由无符号 7 位整数组成的列表）解码为对应的字符串。

如果 [bytes] 中包含不在 0 到 127 范围内的值，解码器最终将抛出 [FormatException]。

如果未提供 [allowInvalid]，则默认使用创建此 [AsciiCodec] 时所用的值。

---

# AsciiEncoder

```dart
final class AsciiEncoder extends _UnicodeSubsetEncoder {}
```

将仅包含 ASCII 字符的字符串转换为字节。

示例：

```dart
const asciiEncoder = AsciiEncoder();
const sample = 'Dart';
final asciiValues = asciiEncoder.convert(sample);
print(asciiValues); // [68, 97, 114, 116]
```

---

# AsciiDecoder

```dart
final class AsciiDecoder extends _UnicodeSubsetDecoder {}
```

将 ASCII 字节转换为字符串。

示例：

```dart
const asciiDecoder = AsciiDecoder();
final asciiValues = [68, 97, 114, 116];
final result = asciiDecoder.convert(asciiValues);
print(result); // Dart
```

如果 [bytes] 中包含不在 0 到 127 范围内的值，且 [allowInvalid] 为 `false`（默认值），则抛出 [FormatException]。

如果 [allowInvalid] 为 `true`，任何超出 0 到 127 范围的字节都将被替换为 Unicode 替换字符 U+FFFD（'�'）。

设置 `allowInvalid` 为 true 的示例：

```dart
const asciiDecoder = AsciiDecoder(allowInvalid: true);
final asciiValues = [68, 97, 114, 116, 20, 0xFF];
final result = asciiDecoder.convert(asciiValues);
print(result); // Dart �
print(result.codeUnits.last.toRadixString(16)); // fffd
```

## 方法

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

启动分块转换。

如果给定的 [sink] 是 [StringConversionSink]，转换器的工作效率会更高。
