# base64

```dart
Base64Codec const base64
```

A [base64](https://tools.ietf.org/html/rfc4648) encoder and decoder.

It encodes using the default base64 alphabet, decodes using both the base64 and base64url alphabets, does not allow invalid characters and requires padding.

Examples:

```dart
var encoded = base64.encode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                             0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
var decoded = base64.decode("YmzDpWLDpnJncsO4ZAo=");
```

The top-level [base64Encode] and [base64Decode] functions may be used instead if a local variable shadows the [base64] constant.

---



# base64Url

```dart
Base64Codec base64Url
```

A [base64url](https://tools.ietf.org/html/rfc4648) encoder and decoder.

It encodes and decodes using the base64url alphabet, decodes using both the base64 and base64url alphabets, does not allow invalid characters and requires padding.

Examples:

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

Encodes [bytes] using [base64](https://tools.ietf.org/html/rfc4648) encoding.

Shorthand for `base64.encode(bytes)`. Useful if a local variable shadows the global [base64] constant.

---



# base64UrlEncode()

```dart
String base64UrlEncode(List<int> bytes)
```

Encodes [bytes] using [base64url](https://tools.ietf.org/html/rfc4648) encoding.

Shorthand for `base64url.encode(bytes)`.

---



# base64Decode()

```dart
Uint8List base64Decode(String source)
```

Decodes [base64](https://tools.ietf.org/html/rfc4648) or [base64url](https://tools.ietf.org/html/rfc4648) encoded bytes.

Shorthand for `base64.decode(bytes)`. Useful if a local variable shadows the global [base64] constant.

---



# Base64Codec

```dart
final class Base64Codec extends Codec<List<int>, String> {}
```

A [base64](https://tools.ietf.org/html/rfc4648) encoder and decoder.

A [Base64Codec] allows base64 encoding bytes into ASCII strings and decoding valid encodings back to bytes.

This implementation only handles the simplest RFC 4648 base64 and base64url encodings. It does not allow invalid characters when decoding and it requires, and generates, padding so that the input is always a multiple of four characters.

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

Returns the encoder from `S` to `T`.

It may be stateful and should not be reused.

### decoder

```dart
Base64Decoder get decoder
```

Returns the decoder of `this`, converting from `T` to `S`.

It may be stateful and should not be reused.

## 方法

### decode()

```dart
Uint8List decode(String encoded)
```

Decodes [encoded].

The input is decoded as if by `decoder.convert`.

The returned [Uint8List] contains exactly the decoded bytes, so the [Uint8List.length] is precisely the number of decoded bytes. The [Uint8List.buffer] may be larger than the decoded bytes.

### normalize()

```dart
String normalize(String source, [int start = 0, int? end])
```

Validates and normalizes the base64 encoded data in [source].

Only acts on the substring from [start] to [end], with [end] defaulting to the end of the string.

Normalization will:

- Unescape any `%`-escapes.
- Only allow valid characters (`A`-`Z`, `a`-`z`, `0`-`9`, `/` and `+`).
- Normalize a `_` or `-` character to `/` or `+`.
- Validate that existing padding (trailing `=` characters) is correct.
- If no padding exists, add correct padding if necessary and possible.
- Validate that the length is correct (a multiple of four).

---



# Base64Encoder

```dart
final class Base64Encoder extends Converter<List<int>, String> {}
```

Base64 and base64url encoding converter.

Encodes lists of bytes using base64 or base64url encoding.

The results are ASCII strings using a restricted alphabet.

Example:

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

Converts `input` and returns the result of the conversion.

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

Starts a chunked conversion.

The returned sink serves as input for the long-running conversion. The given `sink` serves as output.

---



# Base64Decoder

```dart
final class Base64Decoder extends Converter<String, List<int>> {}
```

Decoder for base64 encoded data.

This decoder accepts both base64 and base64url ("url-safe") encodings.

The encoding is required to be properly padded.

Throws a [FormatException] if the input is not valid base64 data.

Example:

```dart
final base64Decoder = base64.decoder;
const base64Bytes = 'RGFydCBpcyBvcGVuIHNvdXJjZQ==';
final decodedBytes = base64Decoder.convert(base64Bytes);
// decodedBytes: [68, 97, 114, 116, 32, 105, 115, 32, 111, 112, 101, 110,
// 32, 115, 111, 117, 114, 99, 101]

// Print as string using UTF-8 decoder
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

Decodes the characters of [input] from [start] to [end] as base64.

If [start] is omitted, it defaults to the start of [input]. If [end] is omitted, it defaults to the end of [input].

The returned [Uint8List] contains exactly the decoded bytes, so the [Uint8List.length] is precisely the number of decoded bytes. The [Uint8List.buffer] may be larger than the decoded bytes.

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<List<int>> sink)
```

Starts a chunked conversion.

The returned sink serves as input for the long-running conversion. The given `sink` serves as output.
