# latin1

```dart
Latin1Codec latin1
```

[Latin1Codec] 默认实现的一个实例。

该实例便于访问最常见的 ISO Latin 1 使用场景。

示例：

```dart
var encoded = latin1.encode("blåbærgrød");
var decoded = latin1.decode([0x62, 0x6c, 0xe5, 0x62, 0xe6,
                             0x72, 0x67, 0x72, 0xf8, 0x64]);
```

---

# Latin1Codec

```dart
final class Latin1Codec extends Encoding {}
```

[Latin1Codec] 将字符串编码为 ISO Latin-1（即 ISO-8859-1）字节，并将 Latin-1 字节解码为字符串。

## 构造函数

### Latin1Codec()

```dart
Latin1Codec({bool allowInvalid = false})
```

实例化一个新的 [Latin1Codec]。

如果 [allowInvalid] 为 true，则 [decode] 方法以及 [decoder] 返回的转换器默认将允许无效值。无效值会被解码为 Unicode 替换字符（U+FFFD）。调用 [decode] 方法时可以覆盖此默认设置。

编码器不会接受无效（非 Latin-1）字符。

## 属性

### name

```dart
String get name
```

该编解码器的名称，"iso-8859-1"。

### encoder

```dart
Latin1Encoder get encoder
```

### decoder

```dart
Latin1Decoder get decoder
```

## 方法

### encode()

```dart
Uint8List encode(String source)
```

### decode()

```dart
String decode(List<int> bytes, {bool? allowInvalid})
```

将 Latin-1 编码的 [bytes]（一个无符号 8 位整数列表）解码为对应的字符串。

如果 [bytes] 包含不在 0 到 255 范围内的值，解码器最终会抛出 [FormatException]。

如果未提供 [allowInvalid]，其默认值为创建此 [Latin1Codec] 时使用的值。

---

# Latin1Encoder

```dart
final class Latin1Encoder extends _UnicodeSubsetEncoder {}
```

此类将仅包含 ISO Latin-1 字符的字符串转换为字节。

示例：

```dart
final latin1Encoder = latin1.encoder;

const sample = 'àáâãäå';
final encoded = latin1Encoder.convert(sample);
print(encoded); // [224, 225, 226, 227, 228, 229]
```

### Latin1Encoder()

```dart
const Latin1Encoder()
```

---

# Latin1Decoder

```dart
final class Latin1Decoder extends _UnicodeSubsetDecoder {}
```

此类将 Latin-1 字节（无符号 8 位整数列表）转换为字符串。

示例：

```dart
final latin1Decoder = latin1.decoder;

const encodedBytes = [224, 225, 226, 227, 228, 229];
final decoded = latin1Decoder.convert(encodedBytes);
print(decoded); // àáâãäå

// 使用十六进制值作为输入
const hexBytes = [0xe0, 0xe1, 0xe2, 0xe3, 0xe4, 0xe5];
final decodedHexBytes = latin1Decoder.convert(hexBytes);
print(decodedHexBytes); // àáâãäå
```

如果编码输入包含不在 0 到 255 范围内的值，且 [allowInvalid] 为 false（默认值），则抛出 [FormatException]。

如果 [allowInvalid] 为 true，无效字节将被转换为 Unicode 替换字符 U+FFFD（�）。

设置 `allowInvalid` 为 true 的示例：

```dart
const latin1Decoder = Latin1Decoder(allowInvalid: true);
const encodedBytes = [300];
final decoded = latin1Decoder.convert(encodedBytes);
print(decoded); // �
```

## 构造函数

### Latin1Decoder()

```dart
Latin1Decoder({bool allowInvalid = false})
```

实例化一个新的 [Latin1Decoder]。

可选参数 [allowInvalid] 定义 [convert] 如何处理无效字节。

如果为 `true`，[convert] 将无效字节替换为 Unicode 替换字符 `U+FFFD`（�）。否则将抛出 [FormatException]。

## 方法

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

开始一次分块转换。

如果给定的 [sink] 是 [StringConversionSink]，转换器的工作效率会更高。
