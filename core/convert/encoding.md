# Encoding

```dart
abstract class Encoding extends Codec<String, List<int>> {}
```

Open-ended set of encodings.

An encoding is a [Codec] encoding strings to lists of byte.

This class provides a default implementation of [decodeStream], which is not incremental. It collects the entire input before decoding. Subclasses can choose to use that implementation, or implement a more efficient stream decoding.

### Encoding()

```dart
Encoding()
```

### encoder

```dart
Converter<String, List<int>> get encoder
```

Returns the encoder from `String` to `List<int>`.

It may be stateful and should not be reused.

### decoder

```dart
Converter<List<int>, String> get decoder
```

Returns the decoder of `this`, converting from `List<int>` to `String`.

It may be stateful and should not be reused.

### decodeStream()

```dart
Future<String> decodeStream(Stream<List<int>> byteStream)
```

### name

```dart
String get name
```

Name of the encoding.

If the encoding is standardized, this is the lower-case version of one of the IANA official names for the character set (see http://www.iana.org/assignments/character-sets/character-sets.xml)

### getByName()

```dart
Encoding? getByName(String? name)
```

Returns an [Encoding] for a named character set.

The names used are the IANA official names for the character set (see [IANA character sets][]). The names are case insensitive.

[IANA character sets]: http://www.iana.org/assignments/character-sets/character-sets.xml

If character set is not supported `null` is returned.
