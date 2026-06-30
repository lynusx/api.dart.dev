# Codec

```dart
abstract mixin class Codec<S, T> {}
```

A [Codec] encodes and (if supported) decodes data.

Codecs can be fused. For example fusing [json] and [utf8] produces an encoder that can convert Json objects directly to bytes, or can decode bytes directly to json objects.

Fused codecs generally attempt to optimize the operations and can be faster than executing each step of an encoding separately.

The [Codec] class provides a default implementation of [encode], [decode], [fuse] and [inverted]. Subclasses can choose to provide more efficient implementations of these.

### Codec()

```dart
Codec()
```

### encode()

```dart
T encode(S input)
```

Encodes [input].

The input is encoded as if by `encoder.convert`.

### decode()

```dart
S decode(T encoded)
```

Decodes [encoded] data.

The input is decoded as if by `decoder.convert`.

### encoder

```dart
Converter<S, T> get encoder
```

Returns the encoder from [S] to [T].

It may be stateful and should not be reused.

### decoder

```dart
Converter<T, S> get decoder
```

Returns the decoder of `this`, converting from [T] to [S].

It may be stateful and should not be reused.

### fuse()

```dart
Codec<S, R> fuse<R>(Codec<T, R> other)
```

Fuses `this` with `other`.

When encoding, the resulting codec encodes with `this` before encoding with [other].

When decoding, the resulting codec decodes with [other] before decoding with `this`.

In some cases one needs to use the [inverted] codecs to be able to fuse them correctly. That is, the output type of `this` ([T]) must match the input type of the second codec [other].

Examples:

```dart
final jsonToBytes = json.fuse(utf8);
List<int> bytes = jsonToBytes.encode(["json-object"]);
var decoded = jsonToBytes.decode(bytes);
assert(decoded is List && decoded[0] == "json-object");

var inverted = json.inverted;
var jsonIdentity = json.fuse(inverted);
var jsonObject = jsonIdentity.encode(["1", 2]);
assert(jsonObject is List && jsonObject[0] == "1" && jsonObject[1] == 2);
```

### inverted

```dart
Codec<T, S> get inverted
```

Inverts `this`.

The [encoder] and [decoder] of the resulting codec are swapped.
