# ascii

```dart
AsciiCodec const ascii
```

An instance of the default implementation of the [AsciiCodec].

This instance provides a convenient access to the most common ASCII use cases.

Examples:

```dart
var encoded = ascii.encode("This is ASCII!");
var decoded = ascii.decode([0x54, 0x68, 0x69, 0x73, 0x20, 0x69, 0x73,
                            0x20, 0x41, 0x53, 0x43, 0x49, 0x49, 0x21]);
```

# AsciiCodec

```dart
final class AsciiCodec extends Encoding {}
```

Encoding and decoding of ASCII characters.

An [Encoding] (conversion between strings and bytes) that converts ASCII characters (U+0000..U+007F) to and from their corresponding byte values.

Treats any non-ASCII character (U+0080..U+10FFFF) as an invalid input to endcoding, and any byte &ge; 128 as an invalid input to decoding.

## 构造函数

### AsciiCodec()

```dart
AsciiCodec({bool allowInvalid = false})
```

Instantiates a new [AsciiCodec].

If [allowInvalid] is `true`, the [decode] method and the converter returned by [decoder] will default to allowing invalid values, which are byte values greater than 127. If allowing invalid values, invalid values will be decoded to the Unicode Replacement character (U+FFFD). If not, a [FormatException] is be thrown. Calls to the [decode] method can choose to override this default.

Encoders will not accept invalid (non-ASCII) characters.

## 属性

### name

```dart
String get name
```

The name of this codec is "us-ascii".

### encoder

```dart
AsciiEncoder get encoder
```

Returns the encoder from `String` to `List<int>`.

It may be stateful and should not be reused.

### decoder

```dart
AsciiDecoder get decoder
```

Returns the decoder of `this`, converting from `List<int>` to `String`.

It may be stateful and should not be reused.

## 方法

### encode()

```dart
Uint8List encode(String source)
```

Encodes `input`.

The input is encoded as if by `encoder.convert`.

### decode()

```dart
String decode(
  List<int> bytes, {
  bool? allowInvalid
})
```

Decodes the ASCII [bytes] (a list of unsigned 7-bit integers) to the corresponding string.

If [bytes] contains values that are not in the range 0 .. 127, the decoder will eventually throw a [FormatException].

If [allowInvalid] is not provided, it defaults to the value used to create this [AsciiCodec].

---



# AsciiEncoder

```dart
final class AsciiEncoder extends _UnicodeSubsetEncoder {}
```

Converts strings of only ASCII characters to bytes.

Example:

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

Converts ASCII bytes to string.

Example:

```dart
const asciiDecoder = AsciiDecoder();
final asciiValues = [68, 97, 114, 116];
final result = asciiDecoder.convert(asciiValues);
print(result); // Dart
```

Throws a [FormatException] if [bytes] contains values that are not in the range 0 .. 127, and [allowInvalid] is `false` (the default).

If [allowInvalid] is `true`, any byte outside the range 0..127 is replaced by the Unicode replacement character, U+FFFD ('�').

Example with `allowInvalid` set to true:

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

Starts a chunked conversion.

The converter works more efficiently if the given [sink] is a [StringConversionSink].
