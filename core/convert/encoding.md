# Encoding

```dart
abstract class Encoding extends Codec<String, List<int>> {}
```

Open-ended set of encodings.

An encoding is a [Codec] encoding strings to lists of byte.

This class provides a default implementation of [decodeStream], which is not incremental. It collects the entire input before decoding. Subclasses can choose to use that implementation, or implement a more efficient stream decoding.

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

Returns an [Encoding] for a named character set.

The names used are the IANA official names for the character set (see [IANA character sets](http://www.iana.org/assignments/character-sets/character-sets.xml)). The names are case insensitive.

If character set is not supported `null` is returned.

## 属性

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

### name

```dart
String get name
```

Name of the encoding.

If the encoding is standardized, this is the lower-case version of one of the IANA official names for the character set (see http://www.iana.org/assignments/character-sets/character-sets.xml)

## 方法

### decodeStream()

```dart
Future<String> decodeStream(Stream<List<int>> byteStream)
```
