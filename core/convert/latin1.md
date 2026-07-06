# latin1

```dart
Latin1Codec latin1
```

An instance of the default implementation of the [Latin1Codec].

This instance provides a convenient access to the most common ISO Latin 1 use cases.

Examples:

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

A [Latin1Codec] encodes strings to ISO Latin-1 (aka ISO-8859-1) bytes and decodes Latin-1 bytes to strings.

## 构造函数

### Latin1Codec()

```dart
Latin1Codec({bool allowInvalid = false})
```

Instantiates a new [Latin1Codec].

If [allowInvalid] is true, the [decode] method and the converter returned by [decoder] will default to allowing invalid values. Invalid values are decoded into the Unicode Replacement character (U+FFFD). Calls to the [decode] method can override this default.

Encoders will not accept invalid (non Latin-1) characters.

## 属性

### name

```dart
String get name
```

The name of this codec, "iso-8859-1".

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

Decodes the Latin-1 [bytes] (a list of unsigned 8-bit integers) to the corresponding string.

If [bytes] contains values that are not in the range 0 .. 255, the decoder will eventually throw a [FormatException].

If [allowInvalid] is not provided, it defaults to the value used to create this [Latin1Codec].

---



# Latin1Encoder

```dart
final class Latin1Encoder extends _UnicodeSubsetEncoder {}
```

This class converts strings of only ISO Latin-1 characters to bytes.

Example:

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

This class converts Latin-1 bytes (lists of unsigned 8-bit integers) to a string.

Example:

```dart
final latin1Decoder = latin1.decoder;

const encodedBytes = [224, 225, 226, 227, 228, 229];
final decoded = latin1Decoder.convert(encodedBytes);
print(decoded); // àáâãäå

// Hexadecimal values as source
const hexBytes = [0xe0, 0xe1, 0xe2, 0xe3, 0xe4, 0xe5];
final decodedHexBytes = latin1Decoder.convert(hexBytes);
print(decodedHexBytes); // àáâãäå
```

Throws a [FormatException] if the encoded input contains values that are not in the range 0 .. 255 and [allowInvalid] is false ( the default ).

If [allowInvalid] is true, invalid bytes are converted to Unicode Replacement character U+FFFD (�).

Example with `allowInvalid` set to true:

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

Instantiates a new [Latin1Decoder].

The optional [allowInvalid] argument defines how [convert] deals with invalid bytes.

If it is `true`, [convert] replaces invalid bytes with the Unicode Replacement character `U+FFFD` (�). Otherwise it throws a [FormatException].

## 方法

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

Starts a chunked conversion.

The converter works more efficiently if the given [sink] is a [StringConversionSink].
