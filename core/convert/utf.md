# unicodeReplacementCharacterRune

```dart
int unicodeReplacementCharacterRune
```

The Unicode Replacement character `U+FFFD` (�).

# unicodeBomCharacterRune

```dart
int unicodeBomCharacterRune
```

The Unicode Byte Order Marker (BOM) character `U+FEFF`.

# utf8

```dart
Utf8Codec utf8
```

An instance of the default implementation of the [Utf8Codec].

This instance provides a convenient access to the most common UTF-8 use cases.

Examples:

```dart
var encoded = utf8.encode("Îñţérñåţîöñåļîžåţîờñ");
var decoded = utf8.decode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                           0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
```

# Utf8Codec

```dart
final class Utf8Codec extends Encoding {}
```

A [Utf8Codec] encodes strings to utf-8 code units (bytes) and decodes UTF-8 code units to strings.

### Utf8Codec()

```dart
Utf8Codec({bool _allowMalformed = false})
```

Instantiates a new [Utf8Codec].

The optional [_allowMalformed] argument defines how [decoder] (and [decode]) deal with invalid or unterminated character sequences.

If it is `true` (and not overridden at the method invocation) [decode] and the [decoder] replace invalid (or unterminated) octet sequences with the Unicode Replacement character `U+FFFD` (�). Otherwise they throw a [FormatException].

### name

```dart
String get name
```

The name of this codec is "utf-8".

### decode()

```dart
String decode(List<int> codeUnits, {bool? allowMalformed})
```

Decodes the UTF-8 [codeUnits] (a list of unsigned 8-bit integers) to the corresponding string.

If the [codeUnits] start with the encoding of a [unicodeBomCharacterRune], that character is discarded.

If [allowMalformed] is `true`, the decoder replaces invalid (or unterminated) character sequences with the Unicode Replacement character `U+FFFD` (�). Otherwise it throws a [FormatException].

If [allowMalformed] is not given, it defaults to the `allowMalformed` that was used to instantiate `this`.

### encode()

```dart
Uint8List encode(String string)
```

Encodes the [string] as UTF-8.

### encoder

```dart
Utf8Encoder get encoder
```

### decoder

```dart
Utf8Decoder get decoder
```

# Utf8Encoder

```dart
final class Utf8Encoder extends Converter<String, List<int>> {}
```

This class converts strings to their UTF-8 code units (a list of unsigned 8-bit integers).

Example:

```dart
final utf8Encoder = utf8.encoder;
const sample = 'Îñţérñåţîöñåļîžåţîờñ';
final encodedSample = utf8Encoder.convert(sample);
print(encodedSample);
```

### Utf8Encoder()

```dart
Utf8Encoder()
```

### convert()

```dart
Uint8List convert(String string, [int start = 0, int? end])
```

Converts [string] to its UTF-8 code units (a list of unsigned 8-bit integers).

If [start] and [end] are provided, only the substring `string.substring(start, end)` is converted.

Any unpaired surrogate character (`U+D800`-`U+DFFF`) in the input string is encoded as a Unicode Replacement character `U+FFFD` (�).

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<List<int>> sink)
```

Starts a chunked conversion.

The converter works more efficiently if the given [sink] is a [ByteConversionSink].

### bind()

```dart
Stream<List<int>> bind(Stream<String> stream)
```

# Utf8Decoder

```dart
final class Utf8Decoder extends Converter<List<int>, String> {}
```

This class converts UTF-8 code units (lists of unsigned 8-bit integers) to a string.

Example:

```dart
final utf8Decoder = utf8.decoder;
const encodedBytes = [
  195, 142, 195, 177, 197, 163, 195, 169, 114, 195, 177, 195, 165, 197,
  163, 195, 174, 195, 182, 195, 177, 195, 165, 196, 188, 195, 174, 197,
  190, 195, 165, 197, 163, 195, 174, 225, 187, 157, 195, 177];

final decodedBytes = utf8Decoder.convert(encodedBytes);
print(decodedBytes); // Îñţérñåţîöñåļîžåţîờñ
```

Throws a [FormatException] if the encoded input contains invalid UTF-8 byte sequences and [allowMalformed] is `false` (the default).

If [allowMalformed] is `true`, invalid byte sequences are converted into one or more Unicode replacement characters, U+FFFD ('�').

Example with `allowMalformed` set to true:

```dart
const utf8Decoder = Utf8Decoder(allowMalformed: true);
const encodedBytes = [0xFF];
final decodedBytes = utf8Decoder.convert(encodedBytes);
print(decodedBytes); // �
```

### Utf8Decoder()

```dart
Utf8Decoder({bool _allowMalformed = false})
```

Instantiates a new [Utf8Decoder].

The optional [_allowMalformed] argument defines how [convert] deals with invalid or unterminated character sequences.

If it is `true`, [convert] replaces invalid (or unterminated) character sequences with the Unicode Replacement character `U+FFFD` (�). Otherwise it throws a [FormatException].

### convert()

```dart
String convert(List<int> codeUnits, [int start = 0, int? end])
```

Converts the UTF-8 [codeUnits] (a list of unsigned 8-bit integers) to the corresponding string.

Uses the code units from [start] to, but not including, [end]. If [end] is omitted, it defaults to `codeUnits.length`.

If the [codeUnits] start with the encoding of a [unicodeBomCharacterRune], that character is discarded.

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<String> sink)
```

Starts a chunked conversion.

The converter works more efficiently if the given [sink] is a [StringConversionSink].

### bind()

```dart
Stream<String> bind(Stream<List<int>> stream)
```

### fuse()

```dart
Converter<List<int>, T> fuse<T>(Converter<String, T> next)
```
