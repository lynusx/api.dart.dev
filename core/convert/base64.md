# base64

```dart
Base64Codec const base64
```

一个 [base64](https://tools.ietf.org/html/rfc4648) 编码器和解码器。

它使用默认的 base64 字母表进行编码，使用 base64 和 base64url 两种字母表进行解码，不允许无效字符，并且要求填充（padding）。

示例：

```dart
var encoded = base64.encode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                             0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
var decoded = base64.decode("YmzDpWLDpnJncsO4ZAo=");
```

如果局部变量遮蔽了 [base64] 常量，可以改用顶层函数 [base64Encode] 和 [base64Decode]。

---

# base64Url

```dart
Base64Codec base64Url
```

一个 [base64url](https://tools.ietf.org/html/rfc4648) 编码器和解码器。

它使用 base64url 字母表进行编码和解码，使用 base64 和 base64url 两种字母表进行解码，不允许无效字符，并且要求填充。

示例：

```dart
var encoded = base64Url.encode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                                0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
var decoded = base64Url.decode("YmzDpWLDpnJncsO4ZAo=");
```

---

# base64Encode()

```dart
String base64Encode(List<int> bytes)
```

使用 [base64](https://tools.ietf.org/html/rfc4648) 编码方式对 [bytes] 进行编码。

是 `base64.encode(bytes)` 的简写形式。适用于局部变量遮蔽了全局 [base64] 常量的情况。

---

# base64UrlEncode()

```dart
String base64UrlEncode(List<int> bytes)
```

使用 [base64url](https://tools.ietf.org/html/rfc4648) 编码方式对 [bytes] 进行编码。

是 `base64url.encode(bytes)` 的简写形式。

---

# base64Decode()

```dart
Uint8List base64Decode(String source)
```

解码 [base64](https://tools.ietf.org/html/rfc4648) 或 [base64url](https://tools.ietf.org/html/rfc4648) 编码的字节。

是 `base64.decode(bytes)` 的简写形式。适用于局部变量遮蔽了全局 [base64] 常量的情况。

---

# Base64Codec

```dart
final class Base64Codec extends Codec<List<int>, String> {}
```

一个 [base64](https://tools.ietf.org/html/rfc4648) 编码器和解码器。

[Base64Codec] 支持将字节以 base64 方式编码为 ASCII 字符串，并将有效的编码结果解码回字节。

此实现仅处理最简单的 RFC 4648 base64 及 base64url 编码。解码时不允许出现无效字符，并且要求（同时也会生成）填充，以使输入始终为 4 个字符的整数倍。

## 构造函数

### Base64Codec()

```dart
const Base64Codec()
```

### Base64Codec.urlSafe()

```dart
const Base64Codec.urlSafe()
```

## 属性

### encoder

```dart
Base64Encoder get encoder
```

返回从 `S` 到 `T` 的编码器。

它可能是有状态的，不应被重复使用。

### decoder

```dart
Base64Decoder get decoder
```

返回 `this` 的解码器，用于将 `T` 转换为 `S`。

它可能是有状态的，不应被重复使用。

## 方法

### decode()

```dart
Uint8List decode(String encoded)
```

解码 [encoded]。

输入将按照 `decoder.convert` 的方式进行解码。

返回的 [Uint8List] 恰好包含解码后的字节，因此 [Uint8List.length] 正是解码后字节的数量。[Uint8List.buffer] 可能比解码后的字节更大。

### normalize()

```dart
String normalize(String source, [int start = 0, int? end])
```

验证并规范化 [source] 中的 base64 编码数据。

仅对从 [start] 到 [end] 的子字符串进行处理，[end] 默认为字符串末尾。

规范化将执行以下操作：

- 反转义任何 `%` 转义序列。
- 仅允许有效字符（`A`-`Z`、`a`-`z`、`0`-`9`、`/` 和 `+`）。
- 将 `_` 或 `-` 字符规范化为 `/` 或 `+`。
- 验证现有填充（末尾的 `=` 字符）是否正确。
- 如果不存在填充，则在必要且可行的情况下添加正确的填充。
- 验证长度是否正确（是 4 的整数倍）。

---

# Base64Encoder

```dart
final class Base64Encoder extends Converter<List<int>, String> {}
```

Base64 与 base64url 编码转换器。

使用 base64 或 base64url 编码方式对字节列表进行编码。

结果为使用受限字母表的 ASCII 字符串。

示例：

```dart
final base64Encoder = base64.encoder;
const sample = 'Dart is open source';
final encodedSample = base64Encoder.convert(sample.codeUnits);
print(encodedSample); // RGFydCBpcyBvcGVuIHNvdXJjZQ==
```

## 构造函数

### Base64Encoder()

```dart
const Base64Encoder()
```

### Base64Encoder.urlSafe()

```dart
const Base64Encoder.urlSafe()
```

## 方法

### convert()

```dart
String convert(List<int> input)
```

转换 `input` 并返回转换结果。

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

启动分块转换。

返回的 sink 作为长时间运行转换过程的输入。给定的 `sink` 作为输出。

---

# Base64Decoder

```dart
final class Base64Decoder extends Converter<String, List<int>> {}
```

base64 编码数据的解码器。

此解码器同时接受 base64 和 base64url（"URL 安全"）两种编码方式。

要求编码内容具有正确的填充。

如果输入不是有效的 base64 数据，将抛出 [FormatException]。

示例：

```dart
final base64Decoder = base64.decoder;
const base64Bytes = 'RGFydCBpcyBvcGVuIHNvdXJjZQ==';
final decodedBytes = base64Decoder.convert(base64Bytes);
// decodedBytes: [68, 97, 114, 116, 32, 105, 115, 32, 111, 112, 101, 110,
// 32, 115, 111, 117, 114, 99, 101]

// 使用 UTF-8 解码器打印为字符串
print(utf8.decode(decodedBytes)); // Dart is open source
```

## 构造函数

### Base64Decoder()

```dart
const Base64Decoder()
```

## 方法

### convert()

```dart
Uint8List convert(String input, [int start = 0, int? end])
```

将 [input] 中从 [start] 到 [end] 的字符解码为 base64。

如果省略 [start]，默认为 [input] 的起始位置。如果省略 [end]，默认为 [input] 的结束位置。

返回的 [Uint8List] 恰好包含解码后的字节，因此 [Uint8List.length] 正是解码后字节的数量。[Uint8List.buffer] 可能比解码后的字节更大。

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<List<int>> sink)
```

启动分块转换。

返回的 sink 作为长时间运行转换过程的输入。给定的 `sink` 作为输出。
