Lists that efficiently handle fixed sized data (for example, unsigned 8 byte integers) and SIMD numeric types.

To use this library in your code:

```dart
import 'dart:typed_data';
```

{@category Core} {@canonicalFor dart:\_internal.BytesBuilder}

# ByteBuffer

```dart
abstract final class ByteBuffer {}
```

A sequence of bytes underlying a typed data object.

Used to process large quantities of binary or numerical data more efficiently using a typed view.

It is a compile-time error for a class to attempt to extend or implement `ByteBuffer`.

### lengthInBytes

```dart
int get lengthInBytes
```

The length of this byte buffer, in bytes.

### asUint8List()

```dart
Uint8List asUint8List([int offsetInBytes = 0, int? length])
```

Creates a [Uint8List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Uint8List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes] and contains [length] bytes. If [length] is omitted, the range extends to the end of the buffer.

The start index and length must describe a valid range of the buffer:

- `offsetInBytes` must not be negative,
- `length` must not be negative, and
- `offsetInBytes + length` must not be greater than [lengthInBytes].

### asInt8List()

```dart
Int8List asInt8List([int offsetInBytes = 0, int? length])
```

Creates a [Int8List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Int8List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes] and contains [length] bytes. If [length] is omitted, the range extends to the end of the buffer.

The start index and length must describe a valid range of the buffer:

- `offsetInBytes` must not be negative,
- `length` must not be negative, and
- `offsetInBytes + length` must not be greater than [lengthInBytes].

### asUint8ClampedList()

```dart
Uint8ClampedList asUint8ClampedList([int offsetInBytes = 0, int? length])
```

Creates a [Uint8ClampedList] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Uint8ClampedList` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes] and contains [length] bytes. If [length] is omitted, the range extends to the end of the buffer.

The start index and length must describe a valid range of the buffer:

- `offsetInBytes` must not be negative,
- `length` must not be negative, and
- `offsetInBytes + length` must not be greater than [lengthInBytes].

### asUint16List()

```dart
Uint16List asUint16List([int offsetInBytes = 0, int? length])
```

Creates a [Uint16List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Uint16List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 16-bit aligned, and contains [length] 16-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not even, the last byte can't be part of the view.

The start index and length must describe a valid 16-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by two,
- `length` must not be negative, and
- `offsetInBytes + length * 2` must not be greater than [lengthInBytes].

### asInt16List()

```dart
Int16List asInt16List([int offsetInBytes = 0, int? length])
```

Creates a [Int16List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Int16List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 16-bit aligned, and contains [length] 16-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not even, the last byte can't be part of the view.

The start index and length must describe a valid 16-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by two,
- `length` must not be negative, and
- `offsetInBytes + length * 2` must not be greater than [lengthInBytes].

### asUint32List()

```dart
Uint32List asUint32List([int offsetInBytes = 0, int? length])
```

Creates a [Uint32List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Uint32List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 32-bit aligned, and contains [length] 32-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by four, the last bytes can't be part of the view.

The start index and length must describe a valid 32-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by four,
- `length` must not be negative, and
- `offsetInBytes + length * 4` must not be greater than [lengthInBytes].

### asInt32List()

```dart
Int32List asInt32List([int offsetInBytes = 0, int? length])
```

Creates a [Int32List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Int32List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 32-bit aligned, and contains [length] 32-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by four, the last bytes can't be part of the view.

The start index and length must describe a valid 32-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by four,
- `length` must not be negative, and
- `offsetInBytes + length * 4` must not be greater than [lengthInBytes].

### asUint64List()

```dart
Uint64List asUint64List([int offsetInBytes = 0, int? length])
```

Creates a [Uint64List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Uint64List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 64-bit aligned, and contains [length] 64-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by eight, the last bytes can't be part of the view.

The start index and length must describe a valid 64-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by eight,
- `length` must not be negative, and
- `offsetInBytes + length * 8` must not be greater than [lengthInBytes].

### asInt64List()

```dart
Int64List asInt64List([int offsetInBytes = 0, int? length])
```

Creates a [Int64List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Int64List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 64-bit aligned, and contains [length] 64-bit integers with the same endianness as the host ([Endian.host]). If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by eight, the last bytes can't be part of the view.

The start index and length must describe a valid 64-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by eight,
- `length` must not be negative, and
- `offsetInBytes + length * 8` must not be greater than [lengthInBytes].

### asInt32x4List()

```dart
Int32x4List asInt32x4List([int offsetInBytes = 0, int? length])
```

Creates a [Int32x4List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Int32x4List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 128-bit aligned, and contains [length] 128-bit integers. If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by 16, the last bytes can't be part of the view.

The start index and length must describe a valid 128-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by sixteen,
- `length` must not be negative, and
- `offsetInBytes + length * 16` must not be greater than [lengthInBytes].

### asFloat32List()

```dart
Float32List asFloat32List([int offsetInBytes = 0, int? length])
```

Creates a [Float32List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Float32List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 32-bit aligned, and contains [length] 32-bit integers. If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by four, the last bytes can't be part of the view.

The start index and length must describe a valid 32-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by four,
- `length` must not be negative, and
- `offsetInBytes + length * 4` must not be greater than [lengthInBytes].

### asFloat64List()

```dart
Float64List asFloat64List([int offsetInBytes = 0, int? length])
```

Creates a [Float64List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Float64List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 64-bit aligned, and contains [length] 64-bit integers. If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by eight, the last bytes can't be part of the view.

The start index and length must describe a valid 64-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by eight,
- `length` must not be negative, and
- `offsetInBytes + length * 8` must not be greater than [lengthInBytes].

### asFloat32x4List()

```dart
Float32x4List asFloat32x4List([int offsetInBytes = 0, int? length])
```

Creates a [Float32x4List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Float32x4List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 128-bit aligned, and contains [length] 128-bit integers. If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by 16, the last bytes can't be part of the view.

The start index and length must describe a valid 128-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by sixteen,
- `length` must not be negative, and
- `offsetInBytes + length * 16` must not be greater than [lengthInBytes].

### asFloat64x2List()

```dart
Float64x2List asFloat64x2List([int offsetInBytes = 0, int? length])
```

Creates a [Float64x2List] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `Float64x2List` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes], which must be 128-bit aligned, and contains [length] 128-bit integers. If [length] is omitted, the range extends as far towards the end of the buffer as possible - if [lengthInBytes] is not divisible by 16, the last bytes can't be part of the view.

The start index and length must describe a valid 128-bit aligned range of the buffer:

- `offsetInBytes` must not be negative,
- `offsetInBytes` must be divisible by sixteen,
- `length` must not be negative, and
- `offsetInBytes + length * 16` must not be greater than [lengthInBytes].

### asByteData()

```dart
ByteData asByteData([int offsetInBytes = 0, int? length])
```

Creates a [ByteData] _view_ of a region of this byte buffer.

The view is backed by the bytes of this byte buffer. Any changes made to the `ByteData` will also change the buffer, and vice versa.

The viewed region start at [offsetInBytes] and contains [length] bytes. If [length] is omitted, the range extends to the end of the buffer.

The start index and length must describe a valid range of the buffer:

- `offsetInBytes` must not be negative,
- `length` must not be negative, and
- `offsetInBytes + length` must not be greater than [lengthInBytes].

# TypedData

```dart
abstract final class TypedData {}
```

A typed view of a sequence of bytes.

It is a compile-time error for a class to attempt to extend or implement `TypedData`.

### elementSizeInBytes

```dart
int get elementSizeInBytes
```

The number of bytes in the representation of each element in this list.

### offsetInBytes

```dart
int get offsetInBytes
```

The offset of this view into the underlying byte buffer, in bytes.

### lengthInBytes

```dart
int get lengthInBytes
```

The length of this view, in bytes.

### buffer

```dart
ByteBuffer get buffer
```

The byte buffer associated with this object.

# TypedDataList

```dart
abstract final class TypedDataList<E> implements TypedData, List<E> {}
```

A [TypedData] fixed-length [List]-view on the bytes of [buffer].

# Endian

```dart
final class Endian {}
```

Endianness of number representation.

The order of bytes in memory of a number representation, with [little] endian having the least significant byte first, and [big] endian (aka. network byte order) having the most significant byte first.

The [host] endian is the native endianness of the underlying platform, and the default endianness used by typed-data lists, like [Uint16List], on this platform. Always one of [little] or [big] endian.

Can be specified when accessing or updating a sequence of bytes using a [ByteData] view. The host endianness can be used if accessing larger numbers by their bytes, for example through a [Uint8List] view on a buffer written using an [Int64List] view of the same buffer..

It is a compile-time error for a class to attempt to extend or implement `Endian`.

### big

```dart
Endian big
```

### little

```dart
Endian little
```

### host

```dart
Endian host
```

# ByteData

```dart
abstract final class ByteData implements TypedData {}
```

A fixed-length, random-access sequence of bytes that also provides random and unaligned access to the fixed-width integers and floating point numbers represented by those bytes.

`ByteData` may be used to pack and unpack data from external sources (such as networks or files systems), and to process large quantities of numerical data more efficiently than would be possible with ordinary [List] implementations. `ByteData` can save space, by eliminating the need for object headers, and time, by eliminating the need for data copies.

If data comes in as bytes, they can be converted to `ByteData` by sharing the same buffer.

```dart
Uint8List bytes = ...;
var blob = ByteData.sublistView(bytes);
if (blob.getUint32(0, Endian.little) == 0x04034b50) { // Zip file marker
  ...
}
```

Finally, `ByteData` may be used to intentionally reinterpret the bytes representing one arithmetic type as another. For example this code fragment determine what 32-bit signed integer is represented by the bytes of a 32-bit floating point number (both stored as big endian):

```dart
var bdata = ByteData(8);
bdata.setFloat32(0, 3.04);
int huh = bdata.getInt32(0); // 0x40428f5c
```

It is a compile-time error for a class to attempt to extend or implement `ByteData`.

### ByteData()

```dart
ByteData(int length)
```

Creates a [ByteData] of the specified length (in elements), all of whose bytes are initially zero.

### ByteData.view()

```dart
ByteData.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [ByteData] _view_ of the specified region in [buffer].

Changes in the [ByteData] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + [length] must be less than or equal to the length of [buffer].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `ByteData.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
ByteData.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [ByteData.sublistView] which includes this computation:

```dart
ByteData.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### ByteData.sublistView()

```dart
ByteData.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [ByteData] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

### asUnmodifiableView()

```dart
ByteData asUnmodifiableView()
```

A read-only view of this [ByteData].

### getInt8()

```dart
int getInt8(int byteOffset)
```

The (possibly negative) integer represented by the byte at the specified [byteOffset] in this object, in two's complement binary representation.

The return value will be between -128 and 127, inclusive.

The [byteOffset] must be non-negative, and less than the length of this object.

### setInt8()

```dart
void setInt8(int byteOffset, int value)
```

Sets the byte at the specified [byteOffset] in this object to the two's complement binary representation of the specified [value], which must fit in a single byte.

In other words, [value] must be between -128 and 127, inclusive.

The [byteOffset] must be non-negative, and less than the length of this object.

### getUint8()

```dart
int getUint8(int byteOffset)
```

The positive integer represented by the byte at the specified [byteOffset] in this object, in unsigned binary form.

The return value will be between 0 and 255, inclusive.

The [byteOffset] must be non-negative, and less than the length of this object.

### setUint8()

```dart
void setUint8(int byteOffset, int value)
```

Sets the byte at the specified [byteOffset] in this object to the unsigned binary representation of the specified [value], which must fit in a single byte.

In other words, [value] must be between 0 and 255, inclusive.

The [byteOffset] must be non-negative, and less than the length of this object.

### getInt16()

```dart
int getInt16(int byteOffset, [Endian endian = Endian.big])
```

The (possibly negative) integer represented by the two bytes at the specified [byteOffset] in this object, in two's complement binary form.

The return value will be between -2<sup>15</sup> and 2<sup>15</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 2` must be less than or equal to the length of this object.

### setInt16()

```dart
void setInt16(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the two bytes starting at the specified [byteOffset] in this object to the two's complement binary representation of the specified [value], which must fit in two bytes.

In other words, [value] must lie between -2<sup>15</sup> and 2<sup>15</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 2` must be less than or equal to the length of this object.

### getUint16()

```dart
int getUint16(int byteOffset, [Endian endian = Endian.big])
```

The positive integer represented by the two bytes starting at the specified [byteOffset] in this object, in unsigned binary form.

The return value will be between 0 and 2<sup>16</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 2` must be less than or equal to the length of this object.

### setUint16()

```dart
void setUint16(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the two bytes starting at the specified [byteOffset] in this object to the unsigned binary representation of the specified [value], which must fit in two bytes.

In other words, [value] must be between 0 and 2<sup>16</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 2` must be less than or equal to the length of this object.

### getInt32()

```dart
int getInt32(int byteOffset, [Endian endian = Endian.big])
```

The (possibly negative) integer represented by the four bytes at the specified [byteOffset] in this object, in two's complement binary form.

The return value will be between -2<sup>31</sup> and 2<sup>31</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### setInt32()

```dart
void setInt32(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the four bytes starting at the specified [byteOffset] in this object to the two's complement binary representation of the specified [value], which must fit in four bytes.

In other words, [value] must lie between -2<sup>31</sup> and 2<sup>31</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### getUint32()

```dart
int getUint32(int byteOffset, [Endian endian = Endian.big])
```

The positive integer represented by the four bytes starting at the specified [byteOffset] in this object, in unsigned binary form.

The return value will be between 0 and 2<sup>32</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### setUint32()

```dart
void setUint32(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the four bytes starting at the specified [byteOffset] in this object to the unsigned binary representation of the specified [value], which must fit in four bytes.

In other words, [value] must be between 0 and 2<sup>32</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### getInt64()

```dart
int getInt64(int byteOffset, [Endian endian = Endian.big])
```

The (possibly negative) integer represented by the eight bytes at the specified [byteOffset] in this object, in two's complement binary form.

The return value will be between -2<sup>63</sup> and 2<sup>63</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

### setInt64()

```dart
void setInt64(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the eight bytes starting at the specified [byteOffset] in this object to the two's complement binary representation of the specified [value], which must fit in eight bytes.

In other words, [value] must lie between -2<sup>63</sup> and 2<sup>63</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

### getUint64()

```dart
int getUint64(int byteOffset, [Endian endian = Endian.big])
```

The positive integer represented by the eight bytes starting at the specified [byteOffset] in this object, in unsigned binary form.

The return value will be between 0 and 2<sup>64</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

### setUint64()

```dart
void setUint64(int byteOffset, int value, [Endian endian = Endian.big])
```

Sets the eight bytes starting at the specified [byteOffset] in this object to the unsigned binary representation of the specified [value], which must fit in eight bytes.

In other words, [value] must be between 0 and 2<sup>64</sup> - 1, inclusive.

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

### getFloat32()

```dart
double getFloat32(int byteOffset, [Endian endian = Endian.big])
```

The floating point number represented by the four bytes at the specified [byteOffset] in this object, in IEEE 754 single-precision binary floating-point format (binary32).

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### setFloat32()

```dart
void setFloat32(int byteOffset, double value, [Endian endian = Endian.big])
```

Sets the four bytes starting at the specified [byteOffset] in this object to the IEEE 754 single-precision binary floating-point (binary32) representation of the specified [value].

**Note that this method can lose precision.** The input [value] is a 64-bit floating point value, which will be converted to 32-bit floating point value by IEEE 754 rounding rules before it is stored. If [value] cannot be represented exactly as a binary32, it will be converted to the nearest binary32 value. If two binary32 values are equally close, the one whose least significant bit is zero will be used. Note that finite (but large) values can be converted to infinity, and small non-zero values can be converted to zero.

The [byteOffset] must be non-negative, and `byteOffset + 4` must be less than or equal to the length of this object.

### getFloat64()

```dart
double getFloat64(int byteOffset, [Endian endian = Endian.big])
```

The floating point number represented by the eight bytes at the specified [byteOffset] in this object, in IEEE 754 double-precision binary floating-point format (binary64).

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

### setFloat64()

```dart
void setFloat64(int byteOffset, double value, [Endian endian = Endian.big])
```

Sets the eight bytes starting at the specified [byteOffset] in this object to the IEEE 754 double-precision binary floating-point (binary64) representation of the specified [value].

The [byteOffset] must be non-negative, and `byteOffset + 8` must be less than or equal to the length of this object.

# Int8List

```dart
abstract final class Int8List implements _TypedIntList {}
```

A fixed-length list of 8-bit signed integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low eight bits, interpreted as a signed 8-bit two's complement integer with values in the range -128 to +127.

It is a compile-time error for a class to attempt to extend or implement `Int8List`.

### Int8List()

```dart
Int8List(int length)
```

Creates an [Int8List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] bytes.

### Int8List.fromList()

```dart
Int8List.fromList(List<int> elements)
```

Creates a [Int8List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` bytes.

### Int8List.view()

```dart
Int8List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [Int8List] _view_ of the specified region in [buffer].

Changes in the [Int8List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Int8List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Int8List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Int8List.sublistView] which includes this computation:

```dart
Int8List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Int8List.sublistView()

```dart
Int8List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Int8List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

### asUnmodifiableView()

```dart
Int8List asUnmodifiableView()
```

A read-only view of this [Int8List];

### sublist()

```dart
Int8List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is an `Int8List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Int8List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int8List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Uint8List

```dart
abstract final class Uint8List implements _TypedIntList {}
```

A fixed-length list of 8-bit unsigned integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low eight bits, interpreted as an unsigned 8-bit integer with values in the range 0 to 255.

It is a compile-time error for a class to attempt to extend or implement `Uint8List`.

### Uint8List()

```dart
Uint8List(int length)
```

Creates a [Uint8List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] bytes.

### Uint8List.fromList()

```dart
Uint8List.fromList(List<int> elements)
```

Creates a [Uint8List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` bytes.

### Uint8List.view()

```dart
Uint8List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Uint8List] _view_ of the specified region in [buffer].

Changes in the [Uint8List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Uint8List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Uint8List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Uint8List.sublistView] which includes this computation:

```dart
Uint8List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Uint8List.sublistView()

```dart
Uint8List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Uint8List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

### asUnmodifiableView()

```dart
Uint8List asUnmodifiableView()
```

A read-only view of this [Uint8List].

### operator +()

```dart
List<int> operator +(List<int> other)
```

Returns a concatenation of this list and [other].

If [other] is also a typed-data list, then the return list will be a typed data list capable of holding both unsigned 8-bit integers and the elements of [other], otherwise it'll be a normal list of integers.

### sublist()

```dart
Uint8List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Uint8List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Uint8List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint8List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Uint8ClampedList

```dart
abstract final class Uint8ClampedList implements _TypedIntList {}
```

A fixed-length list of 8-bit unsigned integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are clamped to an unsigned eight bit value. That is, all values below zero are stored as zero and all values above 255 are stored as 255.

It is a compile-time error for a class to attempt to extend or implement `Uint8ClampedList`.

### Uint8ClampedList()

```dart
Uint8ClampedList(int length)
```

Creates a [Uint8ClampedList] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] bytes.

### Uint8ClampedList.fromList()

```dart
Uint8ClampedList.fromList(List<int> elements)
```

Creates a [Uint8ClampedList] of the same size as the [elements] list and copies over the values clamping when needed.

Values are clamped to fit in the list when they are copied, the same way storing values clamps them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` bytes.

### Uint8ClampedList.view()

```dart
Uint8ClampedList.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Uint8ClampedList] _view_ of the specified region in the specified byte [buffer].

Changes in the [Uint8List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Uint8ClampedList.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Uint8ClampedList.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Uint8ClampedList.sublistView] which includes this computation:

```dart
Uint8ClampedList.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Uint8ClampedList.sublistView()

```dart
Uint8ClampedList.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Uint8ClampedList] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

### asUnmodifiableView()

```dart
Uint8ClampedList asUnmodifiableView()
```

A read-only view of this [Uint8ClampedList].

### sublist()

```dart
Uint8ClampedList sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Uint8ClampedList` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Uint8ClampedList.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint8ClampedList
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Int16List

```dart
abstract final class Int16List implements _TypedIntList {}
```

A fixed-length list of 16-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 16 bits, interpreted as a signed 16-bit two's complement integer with values in the range -32768 to +32767.

It is a compile-time error for a class to attempt to extend or implement `Int16List`.

### Int16List()

```dart
Int16List(int length)
```

Creates an [Int16List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 2 bytes.

### Int16List.fromList()

```dart
Int16List.fromList(List<int> elements)
```

Creates a [Int16List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 2 bytes.

### Int16List.view()

```dart
Int16List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [Int16List] _view_ of the specified region in [buffer].

Changes in the [Int16List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Int16List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Int16List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Int16List.sublistView] which includes this computation:

```dart
Int16List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Int16List.sublistView()

```dart
Int16List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Int16List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of two.

### asUnmodifiableView()

```dart
Int16List asUnmodifiableView()
```

A read-only view of this [Int16List].

### sublist()

```dart
Int16List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is an `Int16List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Int16List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int16List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Uint16List

```dart
abstract final class Uint16List implements _TypedIntList {}
```

A fixed-length list of 16-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 16 bits, interpreted as an unsigned 16-bit integer with values in the range 0 to 65535.

It is a compile-time error for a class to attempt to extend or implement `Uint16List`.

### Uint16List()

```dart
Uint16List(int length)
```

Creates a [Uint16List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 2 bytes.

### Uint16List.fromList()

```dart
Uint16List.fromList(List<int> elements)
```

Creates a [Uint16List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 2 bytes.

### Uint16List.view()

```dart
Uint16List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Uint16List] _view_ of the specified region in the specified byte buffer.

Changes in the [Uint16List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Uint16List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Uint16List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Uint16List.sublistView] which includes this computation:

```dart
Uint16List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Uint16List.sublistView()

```dart
Uint16List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Uint16List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of two.

### asUnmodifiableView()

```dart
Uint16List asUnmodifiableView()
```

A read-only view of this [Uint16List].

### sublist()

```dart
Uint16List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Uint16List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Uint16List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint16List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Int32List

```dart
abstract final class Int32List implements _TypedIntList {}
```

A fixed-length list of 32-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 32 bits, interpreted as a signed 32-bit two's complement integer with values in the range -2147483648 to 2147483647.

It is a compile-time error for a class to attempt to extend or implement `Int32List`.

### Int32List()

```dart
Int32List(int length)
```

Creates an [Int32List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 4 bytes.

### Int32List.fromList()

```dart
Int32List.fromList(List<int> elements)
```

Creates a [Int32List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 4 bytes.

### Int32List.view()

```dart
Int32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [Int32List] _view_ of the specified region in [buffer].

Changes in the [Int32List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Int32List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Int32List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Int32List.sublistView] which includes this computation:

```dart
Int32List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Int32List.sublistView()

```dart
Int32List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Int32List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of four.

### asUnmodifiableView()

```dart
Int32List asUnmodifiableView()
```

A read-only view of this [Int16List].

### sublist()

```dart
Int32List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is an `Int32List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Int32List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int32List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Uint32List

```dart
abstract final class Uint32List implements _TypedIntList {}
```

A fixed-length list of 32-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 32 bits, interpreted as an unsigned 32-bit integer with values in the range 0 to 4294967295.

It is a compile-time error for a class to attempt to extend or implement `Uint32List`.

### Uint32List()

```dart
Uint32List(int length)
```

Creates a [Uint32List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 4 bytes.

### Uint32List.fromList()

```dart
Uint32List.fromList(List<int> elements)
```

Creates a [Uint32List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 4 bytes.

### Uint32List.view()

```dart
Uint32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Uint32List] _view_ of the specified region in the specified byte buffer.

Changes in the [Uint32List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Uint32List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Uint32List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Uint32List.sublistView] which includes this computation:

```dart
Uint32List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Uint32List.sublistView()

```dart
Uint32List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Uint32List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of four.

### asUnmodifiableView()

```dart
Uint32List asUnmodifiableView()
```

A read-only view of this [Uint32List].

### sublist()

```dart
Uint32List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Uint32List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Uint32List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint32List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Int64List

```dart
abstract final class Int64List implements _TypedIntList {}
```

A fixed-length list of 64-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 64 bits, interpreted as a signed 64-bit two's complement integer with values in the range -9223372036854775808 to +9223372036854775807.

It is a compile-time error for a class to attempt to extend or implement `Int64List`.

### Int64List()

```dart
Int64List(int length)
```

Creates an [Int64List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 8 bytes.

### Int64List.fromList()

```dart
Int64List.fromList(List<int> elements)
```

Creates a [Int64List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 8 bytes.

### Int64List.view()

```dart
Int64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [Int64List] _view_ of the specified region in [buffer].

Changes in the [Int64List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Int64List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Int64List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Int64List.sublistView] which includes this computation:

```dart
Int64List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Int64List.sublistView()

```dart
Int64List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Int64List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of eight.

### asUnmodifiableView()

```dart
Int64List asUnmodifiableView()
```

A read-only view of this [Int64List].

### sublist()

```dart
Int64List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is an `Int64List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Int64List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int64List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Uint64List

```dart
abstract final class Uint64List implements _TypedIntList {}
```

A fixed-length list of 64-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 64 bits, interpreted as an unsigned 64-bit integer with values in the range 0 to 18446744073709551615.

It is a compile-time error for a class to attempt to extend or implement `Uint64List`.

### Uint64List()

```dart
Uint64List(int length)
```

Creates a [Uint64List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 8 bytes.

### Uint64List.fromList()

```dart
Uint64List.fromList(List<int> elements)
```

Creates a [Uint64List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 8 bytes.

### Uint64List.view()

```dart
Uint64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates an [Uint64List] _view_ of the specified region in the specified byte buffer.

Changes in the [Uint64List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Uint64List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Uint64List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Uint64List.sublistView] which includes this computation:

```dart
Uint64List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Uint64List.sublistView()

```dart
Uint64List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Uint64List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of eight.

### asUnmodifiableView()

```dart
Uint64List asUnmodifiableView()
```

A read-only view of this [Uint64List].

### sublist()

```dart
Uint64List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Uint64List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Uint64List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint64List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Float32List

```dart
abstract final class Float32List implements _TypedFloatList {}
```

A fixed-length list of IEEE 754 single-precision binary floating-point numbers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Double values stored in the list are converted to the nearest single-precision value. Values read are converted to a double value with the same value.

It is a compile-time error for a class to attempt to extend or implement `Float32List`.

### Float32List()

```dart
Float32List(int length)
```

Creates a [Float32List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 4 bytes.

### Float32List.fromList()

```dart
Float32List.fromList(List<double> elements)
```

Creates a [Float32List] with the same length as the [elements] list and copies over the elements.

Values are truncated to fit in the list when they are copied, the same way storing values truncates them.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 4 bytes.

### Float32List.view()

```dart
Float32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Float32List] _view_ of the specified region in [buffer].

Changes in the [Float32List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Float32List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Float32List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Float32List.sublistView] which includes this computation:

```dart
Float32List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Float32List.sublistView()

```dart
Float32List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Float32List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of four.

### asUnmodifiableView()

```dart
Float32List asUnmodifiableView()
```

A read-only view of this [Float32List].

### sublist()

```dart
Float32List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Float32List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Float32List.fromList([0.0, 1.0, 2.0, 3.0, 4.0]);
print(numbers.sublist(1, 3)); // [1.0, 2.0]
print(numbers.sublist(1, 3).runtimeType); // Float32List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1.0, 2.0, 3.0, 4.0]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Float64List

```dart
abstract final class Float64List implements _TypedFloatList {}
```

A fixed-length list of IEEE 754 double-precision binary floating-point numbers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

It is a compile-time error for a class to attempt to extend or implement `Float64List`.

### Float64List()

```dart
Float64List(int length)
```

Creates a [Float64List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 8 bytes.

### Float64List.fromList()

```dart
Float64List.fromList(List<double> elements)
```

Creates a [Float64List] with the same length as the [elements] list and copies over the elements.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 8 bytes.

### Float64List.view()

```dart
Float64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Float64List] _view_ of the specified region in [buffer].

Changes in the [Float64List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Float64List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Float64List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Float64List.sublistView] which includes this computation:

```dart
Float64List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Float64List.sublistView()

```dart
Float64List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Float64List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of eight.

### asUnmodifiableView()

```dart
Float64List asUnmodifiableView()
```

A read-only view of this [Float64List].

### sublist()

```dart
Float64List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Float64List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Float64List.fromList([0.0, 1.0, 2.0, 3.0, 4.0]);
print(numbers.sublist(1, 3)); // [1.0, 2.0]
print(numbers.sublist(1, 3).runtimeType); // Float64List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(1)); // [1.0, 2.0, 3.0, 4.0]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Float32x4List

```dart
abstract final class Float32x4List implements TypedDataList<Float32x4>, TypedData {}
```

A fixed-length list of [Float32x4] numbers that is viewable as a [TypedData].

For long lists, this implementation will be considerably more space- and time-efficient than the default [List] implementation.

It is a compile-time error for a class to attempt to extend or implement `Float32x4List`.

### Float32x4List()

```dart
Float32x4List(int length)
```

Creates a [Float32x4List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 16 bytes.

### Float32x4List.fromList()

```dart
Float32x4List.fromList(List<Float32x4> elements)
```

Creates a [Float32x4List] with the same length as the [elements] list and copies over the elements.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 16 bytes.

### Float32x4List.view()

```dart
Float32x4List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Float32x4List] _view_ of the specified region in [buffer].

Changes in the [Float32x4List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Float32x4List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Float32x4List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Float32x4List.sublistView] which includes this computation:

```dart
Float32x4List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Float32x4List.sublistView()

```dart
Float32x4List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates a [Float32x4List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of sixteen.

### asUnmodifiableView()

```dart
Float32x4List asUnmodifiableView()
```

A read-only view of this [Float32x4List].

### operator +()

```dart
List<Float32x4> operator +(List<Float32x4> other)
```

The concatenation of this list and [other].

If [other] is also a [Float32x4List], the result is a new [Float32x4List], otherwise the result is a normal growable `List<Float32x4>`.

### sublist()

```dart
Float32x4List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Float32x4List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Float32x4List.fromList([
  Float32x4(0, 1, 2, 3),
  Float32x4(1, 2, 3, 4),
  Float32x4(2, 3, 4, 5),
  Float32x4(3, 4, 5, 6),
  Float32x4(4, 5, 6, 7),
]);
print(numbers.sublist(1, 2)); // [Float32x4(1, 2, 3, 4)]
print(numbers.sublist(1, 2).runtimeType); // Float32x4List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(4)); // [Float32x4(4, 5, 6, 7)]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Int32x4List

```dart
abstract final class Int32x4List implements TypedDataList<Int32x4>, TypedData {}
```

A fixed-length list of [Int32x4] numbers that is viewable as a [TypedData].

For long lists, this implementation will be considerably more space- and time-efficient than the default [List] implementation.

### Int32x4List()

```dart
Int32x4List(int length)
```

Creates a [Int32x4List] of the specified length (in elements), all of whose elements are initially zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 16 bytes.

### Int32x4List.fromList()

```dart
Int32x4List.fromList(List<Int32x4> elements)
```

Creates a [Int32x4List] with the same length as the [elements] list and copies over the elements.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 16 bytes.

### Int32x4List.view()

```dart
Int32x4List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Int32x4List] _view_ of the specified region in [buffer].

Changes in the [Int32x4List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Int32x4List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Int32x4List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Int32x4List.sublistView] which includes this computation:

```dart
Int32x4List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Int32x4List.sublistView()

```dart
Int32x4List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Int32x4List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of sixteen.

### asUnmodifiableView()

```dart
Int32x4List asUnmodifiableView()
```

A read-only view of this [Int32x4List].

### operator +()

```dart
List<Int32x4> operator +(List<Int32x4> other)
```

The concatenation of this list and [other].

If [other] is also a [Int32x4List], the result is a new [Int32x4List], otherwise the result is a normal growable `List<Int32x4>`.

### sublist()

```dart
Int32x4List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is an `Int32x4List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Int32x4List.fromList([
  Int32x4(0, 1, 2, 3),
  Int32x4(1, 2, 3, 4),
  Int32x4(2, 3, 4, 5),
  Int32x4(3, 4, 5, 6),
  Int32x4(4, 5, 6, 7),
]);
print(numbers.sublist(1, 2)); // [Int32x4(1, 2, 3, 4)]
print(numbers.sublist(1, 2).runtimeType); // Int32x4List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(3)); // [Int32x4(3, 4, 5, 6), Int32x4(4, 5, 6, 7)]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Float64x2List

```dart
abstract final class Float64x2List implements TypedDataList<Float64x2>, TypedData {}
```

A fixed-length list of [Float64x2] numbers that is viewable as a [TypedData].

For long lists, this implementation will be considerably more space- and time-efficient than the default [List] implementation.

It is a compile-time error for a class to attempt to extend or implement `Float64x2List`.

### Float64x2List()

```dart
Float64x2List(int length)
```

Creates a [Float64x2List] of the specified length (in elements), all of whose elements have all lanes set to zero.

The list is backed by a [ByteBuffer] containing precisely [length] times 16 bytes.

### Float64x2List.fromList()

```dart
Float64x2List.fromList(List<Float64x2> elements)
```

Creates a [Float64x2List] with the same length as the [elements] list and copies over the elements.

The list is backed by a [ByteBuffer] containing precisely `elements.length` times 16 bytes.

### operator +()

```dart
List<Float64x2> operator +(List<Float64x2> other)
```

The concatenation of this list and [other].

If [other] is also a [Float64x2List], the result is a new [Float64x2List], otherwise the result is a normal growable `List<Float64x2>`.

### Float64x2List.view()

```dart
Float64x2List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

Creates a [Float64x2List] _view_ of the specified region in [buffer].

Changes in the [Float64x2List] will be visible in the byte buffer and vice versa. If the [offsetInBytes] index of the region is not specified, it defaults to zero (the first byte in the byte buffer). If the length is not provided, the view extends to the end of the byte buffer.

The [offsetInBytes] and [length] must be non-negative, and [offsetInBytes] + ([length] \* [bytesPerElement]) must be less than or equal to the length of [buffer].

The [offsetInBytes] must be a multiple of [bytesPerElement].

Note that when creating a view from a [TypedData] list or byte data, that list or byte data may itself be a view on a larger buffer with a [TypedData.offsetInBytes] greater than zero. Merely doing `Float64x2List.view(other.buffer, 0, count)` may not point to the bytes you intended. Instead you may need to do:

```dart
Float64x2List.view(other.buffer, other.offsetInBytes, count)
```

Alternatively, use [Float64x2List.sublistView] which includes this computation:

```dart
Float64x2List.sublistView(other, 0, count);
```

(The third argument is an end index rather than a length, so if you start from a position greater than zero, you need not reduce the count correspondingly).

### Float64x2List.sublistView()

```dart
Float64x2List.sublistView(TypedData data, [int start = 0, int? end])
```

Creates an [Float64x2List] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

The start and end indices of the range of bytes being viewed must be multiples of sixteen.

### asUnmodifiableView()

```dart
Float64x2List asUnmodifiableView()
```

A read-only view of this [Float64x2List].

### sublist()

```dart
Float64x2List sublist(int start, [int? end])
```

Creates a new list containing the elements between [start] and [end].

The new list is a `Float64x2List` containing the elements of this list at positions greater than or equal to [start] and less than [end] in the same order as they occur in this list.

```dart
var numbers = Float64x2List.fromList([
  Float64x2(0, 1),
  Float64x2(1, 2),
  Float64x2(2, 3),
  Float64x2(3, 4),
  Float64x2(4, 5),
]);
print(numbers.sublist(1, 3)); // [Float64x2(1, 2), Float64x2(2, 3)]
print(numbers.sublist(1, 3).runtimeType); // Float64x2List
```

If [end] is omitted, it defaults to the [length] of this list.

```dart
print(numbers.sublist(3)); // [Float64x2(3, 4), Float64x2(4, 5)]
```

The `start` and `end` positions must satisfy the relations 0 ≤ `start` ≤ `end` ≤ `this.length`. If `end` is equal to `start`, then the returned list is empty.

### bytesPerElement

```dart
int bytesPerElement
```

# Float32x4

```dart
abstract final class Float32x4 {}
```

Four 32-bit floating point values.

Float32x4 stores four 32-bit floating point values in "lanes". The lanes are named [x], [y], [z], and [w] respectively.

Single operations can be performed on the multiple values of one or more `Float32x4`s, which will perform the corresponding operation for each lane of the operands, and provide a new `Float32x4` (or similar multi-value) result with the results from each lane.

The `Float32x4` class cannot be extended or implemented.

### Float32x4()

```dart
Float32x4(double x, double y, double z, double w)
```

Creates a `Float32x4` containing the 32-bit float values of the arguments.

The created value has [Float32x4.x], [Float32x4.y], [Float32x4.z] and [Float32x4.w] values that are the 32-bit floating point values created from the [x], [y], [z] and [w] arguments by conversion from 64-bit floating point to 32-bit floating point values.

The conversion from 64-bit float to 32-bit float loses significant precision, and may become zero or infinite even if the original 64-bit floating point value was non-zero and finite.

### Float32x4.splat()

```dart
Float32x4.splat(double value)
```

Creates a `Float32x4` with the same 32-bit float value four times.

The created value has the same [Float32x4.x], [Float32x4.y], [Float32x4.z] and [Float32x4.w] value, which is the 32-bit floating point value created by converting the 64-bit floating point [value] to a 32-bit floating point value.

The conversion from 64-bit float to 32-bit float loses significant precision, and may become zero or infinite even if the original 64-bit floating point value was non-zero and finite.

### Float32x4.zero()

```dart
Float32x4.zero()
```

Creates a `Float32x4` with all values being zero.

The created value has the same [Float32x4.x], [Float32x4.y], [Float32x4.z] and [Float32x4.w] value, which is the 32-bit floating point zero value.

### Float32x4.fromInt32x4Bits()

```dart
Float32x4.fromInt32x4Bits(Int32x4 bits)
```

Creates a `Float32x4` with 32-bit float values from the provided bits.

The created value has [Float32x4.x], [Float32x4.y], [Float32x4.z] and [Float32x4.w] values, which are the 32-bit floating point values of the bit-representations of the corresponding lanes of [bits].

The conversion is performed using the _platform endianness_ for both 32-bit integers and 32-bit floating point numbers.

### Float32x4.fromFloat64x2()

```dart
Float32x4.fromFloat64x2(Float64x2 xy)
```

Creates a `Float32x4` with its [x] and [y] lanes set to values from [xy].

The created value has [Float32x4.x] and [Float32x4.y] values which are the conversions of the [Float64x2.x] and [Float64x2.y] lanes of [xy] to 32-bit floating point values. The [Float32x4.z] and [Float32x4.w] lanes are set to the zero value.

### operator +()

```dart
Float32x4 operator +(Float32x4 other)
```

Lane-wise addition.

Adds the value of each lane of this value to the value of the corresponding lane of [other].

Returns the result for each lane.

### operator -()

```dart
Float32x4 operator -()
```

Lane-wise negation.

Returns a result where each lane is the negation of the corresponding lane of this value.

### operator -()

```dart
Float32x4 operator -(Float32x4 other)
```

Lane-wise subtraction.

Subtracts the value of each lane of [other] from the value of the corresponding lane of this value.

Returns the result for each lane.

### operator \*()

```dart
Float32x4 operator *(Float32x4 other)
```

Lane-wise multiplication.

Multiplies the value of each lane of this value with the value of the corresponding lane of [other].

Returns the result for each lane.

### operator /()

```dart
Float32x4 operator /(Float32x4 other)
```

Lane-wise division.

Divides the value of each lane of this value with the value of the corresponding lane of [other].

Returns the result for each lane.

### lessThan()

```dart
Int32x4 lessThan(Float32x4 other)
```

Lane-wise less-than comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _less than_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### lessThanOrEqual()

```dart
Int32x4 lessThanOrEqual(Float32x4 other)
```

Lane-wise less-than-or-equal comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _less than or equal to_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### greaterThan()

```dart
Int32x4 greaterThan(Float32x4 other)
```

Lane-wise greater-than comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _greater than_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### greaterThanOrEqual()

```dart
Int32x4 greaterThanOrEqual(Float32x4 other)
```

Lane-wise greater-than-or-equal comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _greater than or equal to_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### equal()

```dart
Int32x4 equal(Float32x4 other)
```

Lane-wise equals comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _equal to_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### notEqual()

```dart
Int32x4 notEqual(Float32x4 other)
```

Lane-wise not-equals comparison.

Compares the 32-bit floating point value of each lane of this to the value of the corresponding lane of [other], using 32-bit floating point comparison. _For floating point comparisons, a comparison with a NaN value is always false, and -0.0 (negative zero) is considered equal to 0.0 (positive zero), and not less strictly than it._ The result for a lane is a 32-bit signed integer which is -1 (all bits set) if the value from this object is _not equal to_ the value from [other], and the result is 0 (all bits cleared) if not, including if either value is a NaN value.

Returns four values that are always either 0 or -1.

### scale()

```dart
Float32x4 scale(double scale)
```

Lane-wise multiplication by [scale].

Returns a result where each lane is the result of multiplying the corresponding lane of this value with [scale]. This can happen either by converting the lane value to a 64-bit floating point value, multiplying it with [scale] and converting the result back to a 32-bit floating point value, or by converting [scale] to a 32-bit floating point value and performing a 32-bit floating point multiplication.

In the latter case it is equivalent to `thisValue * Float32x4.splat(s)`.

### abs()

```dart
Float32x4 abs()
```

Lane-wise conversion to absolute value.

Converts each lane's value to a non-negative value by negating the value if it is negative, and keeping the original value if it is not negative.

Returns the result for each lane.

### clamp()

```dart
Float32x4 clamp(Float32x4 lowerLimit, Float32x4 upperLimit)
```

Lane-wise clamp to a range.

Clamps the value of each lane to a minimum value of the corresponding lane of [lowerLimit] and a maximum value of the corresponding lane of [upperLimit]. If the original value is lower than the minimum value, the result is the minimum value, and if original value is greater than the maximum value, the result is the maximum value. The result is unspecified if the maximum value is lower than the minimum value, or if any of the three values is a NaN value, other than that the result will be one of those three values, or possibly a different NaN value if any value is a NaN value.

Returns the result for each lane.

### x

```dart
double get x
```

The value of the "x" lane.

### y

```dart
double get y
```

The value of the "y" lane.

### z

```dart
double get z
```

The value of the "z" lane.

### w

```dart
double get w
```

The value of the "w" lane.

### signMask

```dart
int get signMask
```

The sign bits of each lane as single bits.

The sign bits of each lane's 32-bit floating point value are stored in the low four bits of this value:

- The [x] lane in bit 0.
- The [y] lane in bit 1.
- The [z] lane in bit 2.
- The [w] lane in bit 3.

### xxxx

```dart
int xxxx
```

Shuffle mask "xxxx".

Used by [shuffle] and [shuffleMix].

### xxxy

```dart
int xxxy
```

Shuffle mask "xxxy".

Used by [shuffle] and [shuffleMix].

### xxxz

```dart
int xxxz
```

Shuffle mask "xxxz".

Used by [shuffle] and [shuffleMix].

### xxxw

```dart
int xxxw
```

Shuffle mask "xxxw".

Used by [shuffle] and [shuffleMix].

### xxyx

```dart
int xxyx
```

Shuffle mask "xxyx".

Used by [shuffle] and [shuffleMix].

### xxyy

```dart
int xxyy
```

Shuffle mask "xxyy".

Used by [shuffle] and [shuffleMix].

### xxyz

```dart
int xxyz
```

Shuffle mask "xxyz".

Used by [shuffle] and [shuffleMix].

### xxyw

```dart
int xxyw
```

Shuffle mask "xxyw".

Used by [shuffle] and [shuffleMix].

### xxzx

```dart
int xxzx
```

Shuffle mask "xxzx".

Used by [shuffle] and [shuffleMix].

### xxzy

```dart
int xxzy
```

Shuffle mask "xxzy".

Used by [shuffle] and [shuffleMix].

### xxzz

```dart
int xxzz
```

Shuffle mask "xxzz".

Used by [shuffle] and [shuffleMix].

### xxzw

```dart
int xxzw
```

Shuffle mask "xxzw".

Used by [shuffle] and [shuffleMix].

### xxwx

```dart
int xxwx
```

Shuffle mask "xxwx".

Used by [shuffle] and [shuffleMix].

### xxwy

```dart
int xxwy
```

Shuffle mask "xxwy".

Used by [shuffle] and [shuffleMix].

### xxwz

```dart
int xxwz
```

Shuffle mask "xxwz".

Used by [shuffle] and [shuffleMix].

### xxww

```dart
int xxww
```

Shuffle mask "xxww".

Used by [shuffle] and [shuffleMix].

### xyxx

```dart
int xyxx
```

Shuffle mask "xyxx".

Used by [shuffle] and [shuffleMix].

### xyxy

```dart
int xyxy
```

Shuffle mask "xyxy".

Used by [shuffle] and [shuffleMix].

### xyxz

```dart
int xyxz
```

Shuffle mask "xyxz".

Used by [shuffle] and [shuffleMix].

### xyxw

```dart
int xyxw
```

Shuffle mask "xyxw".

Used by [shuffle] and [shuffleMix].

### xyyx

```dart
int xyyx
```

Shuffle mask "xyyx".

Used by [shuffle] and [shuffleMix].

### xyyy

```dart
int xyyy
```

Shuffle mask "xyyy".

Used by [shuffle] and [shuffleMix].

### xyyz

```dart
int xyyz
```

Shuffle mask "xyyz".

Used by [shuffle] and [shuffleMix].

### xyyw

```dart
int xyyw
```

Shuffle mask "xyyw".

Used by [shuffle] and [shuffleMix].

### xyzx

```dart
int xyzx
```

Shuffle mask "xyzx".

Used by [shuffle] and [shuffleMix].

### xyzy

```dart
int xyzy
```

Shuffle mask "xyzy".

Used by [shuffle] and [shuffleMix].

### xyzz

```dart
int xyzz
```

Shuffle mask "xyzz".

Used by [shuffle] and [shuffleMix].

### xyzw

```dart
int xyzw
```

Shuffle mask "xyzw".

Used by [shuffle] and [shuffleMix].

### xywx

```dart
int xywx
```

Shuffle mask "xywx".

Used by [shuffle] and [shuffleMix].

### xywy

```dart
int xywy
```

Shuffle mask "xywy".

Used by [shuffle] and [shuffleMix].

### xywz

```dart
int xywz
```

Shuffle mask "xywz".

Used by [shuffle] and [shuffleMix].

### xyww

```dart
int xyww
```

Shuffle mask "xyww".

Used by [shuffle] and [shuffleMix].

### xzxx

```dart
int xzxx
```

Shuffle mask "xzxx".

Used by [shuffle] and [shuffleMix].

### xzxy

```dart
int xzxy
```

Shuffle mask "xzxy".

Used by [shuffle] and [shuffleMix].

### xzxz

```dart
int xzxz
```

Shuffle mask "xzxz".

Used by [shuffle] and [shuffleMix].

### xzxw

```dart
int xzxw
```

Shuffle mask "xzxw".

Used by [shuffle] and [shuffleMix].

### xzyx

```dart
int xzyx
```

Shuffle mask "xzyx".

Used by [shuffle] and [shuffleMix].

### xzyy

```dart
int xzyy
```

Shuffle mask "xzyy".

Used by [shuffle] and [shuffleMix].

### xzyz

```dart
int xzyz
```

Shuffle mask "xzyz".

Used by [shuffle] and [shuffleMix].

### xzyw

```dart
int xzyw
```

Shuffle mask "xzyw".

Used by [shuffle] and [shuffleMix].

### xzzx

```dart
int xzzx
```

Shuffle mask "xzzx".

Used by [shuffle] and [shuffleMix].

### xzzy

```dart
int xzzy
```

Shuffle mask "xzzy".

Used by [shuffle] and [shuffleMix].

### xzzz

```dart
int xzzz
```

Shuffle mask "xzzz".

Used by [shuffle] and [shuffleMix].

### xzzw

```dart
int xzzw
```

Shuffle mask "xzzw".

Used by [shuffle] and [shuffleMix].

### xzwx

```dart
int xzwx
```

Shuffle mask "xzwx".

Used by [shuffle] and [shuffleMix].

### xzwy

```dart
int xzwy
```

Shuffle mask "xzwy".

Used by [shuffle] and [shuffleMix].

### xzwz

```dart
int xzwz
```

Shuffle mask "xzwz".

Used by [shuffle] and [shuffleMix].

### xzww

```dart
int xzww
```

Shuffle mask "xzww".

Used by [shuffle] and [shuffleMix].

### xwxx

```dart
int xwxx
```

Shuffle mask "xwxx".

Used by [shuffle] and [shuffleMix].

### xwxy

```dart
int xwxy
```

Shuffle mask "xwxy".

Used by [shuffle] and [shuffleMix].

### xwxz

```dart
int xwxz
```

Shuffle mask "xwxz".

Used by [shuffle] and [shuffleMix].

### xwxw

```dart
int xwxw
```

Shuffle mask "xwxw".

Used by [shuffle] and [shuffleMix].

### xwyx

```dart
int xwyx
```

Shuffle mask "xwyx".

Used by [shuffle] and [shuffleMix].

### xwyy

```dart
int xwyy
```

Shuffle mask "xwyy".

Used by [shuffle] and [shuffleMix].

### xwyz

```dart
int xwyz
```

Shuffle mask "xwyz".

Used by [shuffle] and [shuffleMix].

### xwyw

```dart
int xwyw
```

Shuffle mask "xwyw".

Used by [shuffle] and [shuffleMix].

### xwzx

```dart
int xwzx
```

Shuffle mask "xwzx".

Used by [shuffle] and [shuffleMix].

### xwzy

```dart
int xwzy
```

Shuffle mask "xwzy".

Used by [shuffle] and [shuffleMix].

### xwzz

```dart
int xwzz
```

Shuffle mask "xwzz".

Used by [shuffle] and [shuffleMix].

### xwzw

```dart
int xwzw
```

Shuffle mask "xwzw".

Used by [shuffle] and [shuffleMix].

### xwwx

```dart
int xwwx
```

Shuffle mask "xwwx".

Used by [shuffle] and [shuffleMix].

### xwwy

```dart
int xwwy
```

Shuffle mask "xwwy".

Used by [shuffle] and [shuffleMix].

### xwwz

```dart
int xwwz
```

Shuffle mask "xwwz".

Used by [shuffle] and [shuffleMix].

### xwww

```dart
int xwww
```

Shuffle mask "xwww".

Used by [shuffle] and [shuffleMix].

### yxxx

```dart
int yxxx
```

Shuffle mask "yxxx".

Used by [shuffle] and [shuffleMix].

### yxxy

```dart
int yxxy
```

Shuffle mask "yxxy".

Used by [shuffle] and [shuffleMix].

### yxxz

```dart
int yxxz
```

Shuffle mask "yxxz".

Used by [shuffle] and [shuffleMix].

### yxxw

```dart
int yxxw
```

Shuffle mask "yxxw".

Used by [shuffle] and [shuffleMix].

### yxyx

```dart
int yxyx
```

Shuffle mask "yxyx".

Used by [shuffle] and [shuffleMix].

### yxyy

```dart
int yxyy
```

Shuffle mask "yxyy".

Used by [shuffle] and [shuffleMix].

### yxyz

```dart
int yxyz
```

Shuffle mask "yxyz".

Used by [shuffle] and [shuffleMix].

### yxyw

```dart
int yxyw
```

Shuffle mask "yxyw".

Used by [shuffle] and [shuffleMix].

### yxzx

```dart
int yxzx
```

Shuffle mask "yxzx".

Used by [shuffle] and [shuffleMix].

### yxzy

```dart
int yxzy
```

Shuffle mask "yxzy".

Used by [shuffle] and [shuffleMix].

### yxzz

```dart
int yxzz
```

Shuffle mask "yxzz".

Used by [shuffle] and [shuffleMix].

### yxzw

```dart
int yxzw
```

Shuffle mask "yxzw".

Used by [shuffle] and [shuffleMix].

### yxwx

```dart
int yxwx
```

Shuffle mask "yxwx".

Used by [shuffle] and [shuffleMix].

### yxwy

```dart
int yxwy
```

Shuffle mask "yxwy".

Used by [shuffle] and [shuffleMix].

### yxwz

```dart
int yxwz
```

Shuffle mask "yxwz".

Used by [shuffle] and [shuffleMix].

### yxww

```dart
int yxww
```

Shuffle mask "yxww".

Used by [shuffle] and [shuffleMix].

### yyxx

```dart
int yyxx
```

Shuffle mask "yyxx".

Used by [shuffle] and [shuffleMix].

### yyxy

```dart
int yyxy
```

Shuffle mask "yyxy".

Used by [shuffle] and [shuffleMix].

### yyxz

```dart
int yyxz
```

Shuffle mask "yyxz".

Used by [shuffle] and [shuffleMix].

### yyxw

```dart
int yyxw
```

Shuffle mask "yyxw".

Used by [shuffle] and [shuffleMix].

### yyyx

```dart
int yyyx
```

Shuffle mask "yyyx".

Used by [shuffle] and [shuffleMix].

### yyyy

```dart
int yyyy
```

Shuffle mask "yyyy".

Used by [shuffle] and [shuffleMix].

### yyyz

```dart
int yyyz
```

Shuffle mask "yyyz".

Used by [shuffle] and [shuffleMix].

### yyyw

```dart
int yyyw
```

Shuffle mask "yyyw".

Used by [shuffle] and [shuffleMix].

### yyzx

```dart
int yyzx
```

Shuffle mask "yyzx".

Used by [shuffle] and [shuffleMix].

### yyzy

```dart
int yyzy
```

Shuffle mask "yyzy".

Used by [shuffle] and [shuffleMix].

### yyzz

```dart
int yyzz
```

Shuffle mask "yyzz".

Used by [shuffle] and [shuffleMix].

### yyzw

```dart
int yyzw
```

Shuffle mask "yyzw".

Used by [shuffle] and [shuffleMix].

### yywx

```dart
int yywx
```

Shuffle mask "yywx".

Used by [shuffle] and [shuffleMix].

### yywy

```dart
int yywy
```

Shuffle mask "yywy".

Used by [shuffle] and [shuffleMix].

### yywz

```dart
int yywz
```

Shuffle mask "yywz".

Used by [shuffle] and [shuffleMix].

### yyww

```dart
int yyww
```

Shuffle mask "yyww".

Used by [shuffle] and [shuffleMix].

### yzxx

```dart
int yzxx
```

Shuffle mask "yzxx".

Used by [shuffle] and [shuffleMix].

### yzxy

```dart
int yzxy
```

Shuffle mask "yzxy".

Used by [shuffle] and [shuffleMix].

### yzxz

```dart
int yzxz
```

Shuffle mask "yzxz".

Used by [shuffle] and [shuffleMix].

### yzxw

```dart
int yzxw
```

Shuffle mask "yzxw".

Used by [shuffle] and [shuffleMix].

### yzyx

```dart
int yzyx
```

Shuffle mask "yzyx".

Used by [shuffle] and [shuffleMix].

### yzyy

```dart
int yzyy
```

Shuffle mask "yzyy".

Used by [shuffle] and [shuffleMix].

### yzyz

```dart
int yzyz
```

Shuffle mask "yzyz".

Used by [shuffle] and [shuffleMix].

### yzyw

```dart
int yzyw
```

Shuffle mask "yzyw".

Used by [shuffle] and [shuffleMix].

### yzzx

```dart
int yzzx
```

Shuffle mask "yzzx".

Used by [shuffle] and [shuffleMix].

### yzzy

```dart
int yzzy
```

Shuffle mask "yzzy".

Used by [shuffle] and [shuffleMix].

### yzzz

```dart
int yzzz
```

Shuffle mask "yzzz".

Used by [shuffle] and [shuffleMix].

### yzzw

```dart
int yzzw
```

Shuffle mask "yzzw".

Used by [shuffle] and [shuffleMix].

### yzwx

```dart
int yzwx
```

Shuffle mask "yzwx".

Used by [shuffle] and [shuffleMix].

### yzwy

```dart
int yzwy
```

Shuffle mask "yzwy".

Used by [shuffle] and [shuffleMix].

### yzwz

```dart
int yzwz
```

Shuffle mask "yzwz".

Used by [shuffle] and [shuffleMix].

### yzww

```dart
int yzww
```

Shuffle mask "yzww".

Used by [shuffle] and [shuffleMix].

### ywxx

```dart
int ywxx
```

Shuffle mask "ywxx".

Used by [shuffle] and [shuffleMix].

### ywxy

```dart
int ywxy
```

Shuffle mask "ywxy".

Used by [shuffle] and [shuffleMix].

### ywxz

```dart
int ywxz
```

Shuffle mask "ywxz".

Used by [shuffle] and [shuffleMix].

### ywxw

```dart
int ywxw
```

Shuffle mask "ywxw".

Used by [shuffle] and [shuffleMix].

### ywyx

```dart
int ywyx
```

Shuffle mask "ywyx".

Used by [shuffle] and [shuffleMix].

### ywyy

```dart
int ywyy
```

Shuffle mask "ywyy".

Used by [shuffle] and [shuffleMix].

### ywyz

```dart
int ywyz
```

Shuffle mask "ywyz".

Used by [shuffle] and [shuffleMix].

### ywyw

```dart
int ywyw
```

Shuffle mask "ywyw".

Used by [shuffle] and [shuffleMix].

### ywzx

```dart
int ywzx
```

Shuffle mask "ywzx".

Used by [shuffle] and [shuffleMix].

### ywzy

```dart
int ywzy
```

Shuffle mask "ywzy".

Used by [shuffle] and [shuffleMix].

### ywzz

```dart
int ywzz
```

Shuffle mask "ywzz".

Used by [shuffle] and [shuffleMix].

### ywzw

```dart
int ywzw
```

Shuffle mask "ywzw".

Used by [shuffle] and [shuffleMix].

### ywwx

```dart
int ywwx
```

Shuffle mask "ywwx".

Used by [shuffle] and [shuffleMix].

### ywwy

```dart
int ywwy
```

Shuffle mask "ywwy".

Used by [shuffle] and [shuffleMix].

### ywwz

```dart
int ywwz
```

Shuffle mask "ywwz".

Used by [shuffle] and [shuffleMix].

### ywww

```dart
int ywww
```

Shuffle mask "ywww".

Used by [shuffle] and [shuffleMix].

### zxxx

```dart
int zxxx
```

Shuffle mask "zxxx".

Used by [shuffle] and [shuffleMix].

### zxxy

```dart
int zxxy
```

Shuffle mask "zxxy".

Used by [shuffle] and [shuffleMix].

### zxxz

```dart
int zxxz
```

Shuffle mask "zxxz".

Used by [shuffle] and [shuffleMix].

### zxxw

```dart
int zxxw
```

Shuffle mask "zxxw".

Used by [shuffle] and [shuffleMix].

### zxyx

```dart
int zxyx
```

Shuffle mask "zxyx".

Used by [shuffle] and [shuffleMix].

### zxyy

```dart
int zxyy
```

Shuffle mask "zxyy".

Used by [shuffle] and [shuffleMix].

### zxyz

```dart
int zxyz
```

Shuffle mask "zxyz".

Used by [shuffle] and [shuffleMix].

### zxyw

```dart
int zxyw
```

Shuffle mask "zxyw".

Used by [shuffle] and [shuffleMix].

### zxzx

```dart
int zxzx
```

Shuffle mask "zxzx".

Used by [shuffle] and [shuffleMix].

### zxzy

```dart
int zxzy
```

Shuffle mask "zxzy".

Used by [shuffle] and [shuffleMix].

### zxzz

```dart
int zxzz
```

Shuffle mask "zxzz".

Used by [shuffle] and [shuffleMix].

### zxzw

```dart
int zxzw
```

Shuffle mask "zxzw".

Used by [shuffle] and [shuffleMix].

### zxwx

```dart
int zxwx
```

Shuffle mask "zxwx".

Used by [shuffle] and [shuffleMix].

### zxwy

```dart
int zxwy
```

Shuffle mask "zxwy".

Used by [shuffle] and [shuffleMix].

### zxwz

```dart
int zxwz
```

Shuffle mask "zxwz".

Used by [shuffle] and [shuffleMix].

### zxww

```dart
int zxww
```

Shuffle mask "zxww".

Used by [shuffle] and [shuffleMix].

### zyxx

```dart
int zyxx
```

Shuffle mask "zyxx".

Used by [shuffle] and [shuffleMix].

### zyxy

```dart
int zyxy
```

Shuffle mask "zyxy".

Used by [shuffle] and [shuffleMix].

### zyxz

```dart
int zyxz
```

Shuffle mask "zyxz".

Used by [shuffle] and [shuffleMix].

### zyxw

```dart
int zyxw
```

Shuffle mask "zyxw".

Used by [shuffle] and [shuffleMix].

### zyyx

```dart
int zyyx
```

Shuffle mask "zyyx".

Used by [shuffle] and [shuffleMix].

### zyyy

```dart
int zyyy
```

Shuffle mask "zyyy".

Used by [shuffle] and [shuffleMix].

### zyyz

```dart
int zyyz
```

Shuffle mask "zyyz".

Used by [shuffle] and [shuffleMix].

### zyyw

```dart
int zyyw
```

Shuffle mask "zyyw".

Used by [shuffle] and [shuffleMix].

### zyzx

```dart
int zyzx
```

Shuffle mask "zyzx".

Used by [shuffle] and [shuffleMix].

### zyzy

```dart
int zyzy
```

Shuffle mask "zyzy".

Used by [shuffle] and [shuffleMix].

### zyzz

```dart
int zyzz
```

Shuffle mask "zyzz".

Used by [shuffle] and [shuffleMix].

### zyzw

```dart
int zyzw
```

Shuffle mask "zyzw".

Used by [shuffle] and [shuffleMix].

### zywx

```dart
int zywx
```

Shuffle mask "zywx".

Used by [shuffle] and [shuffleMix].

### zywy

```dart
int zywy
```

Shuffle mask "zywy".

Used by [shuffle] and [shuffleMix].

### zywz

```dart
int zywz
```

Shuffle mask "zywz".

Used by [shuffle] and [shuffleMix].

### zyww

```dart
int zyww
```

Shuffle mask "zyww".

Used by [shuffle] and [shuffleMix].

### zzxx

```dart
int zzxx
```

Shuffle mask "zzxx".

Used by [shuffle] and [shuffleMix].

### zzxy

```dart
int zzxy
```

Shuffle mask "zzxy".

Used by [shuffle] and [shuffleMix].

### zzxz

```dart
int zzxz
```

Shuffle mask "zzxz".

Used by [shuffle] and [shuffleMix].

### zzxw

```dart
int zzxw
```

Shuffle mask "zzxw".

Used by [shuffle] and [shuffleMix].

### zzyx

```dart
int zzyx
```

Shuffle mask "zzyx".

Used by [shuffle] and [shuffleMix].

### zzyy

```dart
int zzyy
```

Shuffle mask "zzyy".

Used by [shuffle] and [shuffleMix].

### zzyz

```dart
int zzyz
```

Shuffle mask "zzyz".

Used by [shuffle] and [shuffleMix].

### zzyw

```dart
int zzyw
```

Shuffle mask "zzyw".

Used by [shuffle] and [shuffleMix].

### zzzx

```dart
int zzzx
```

Shuffle mask "zzzx".

Used by [shuffle] and [shuffleMix].

### zzzy

```dart
int zzzy
```

Shuffle mask "zzzy".

Used by [shuffle] and [shuffleMix].

### zzzz

```dart
int zzzz
```

Shuffle mask "zzzz".

Used by [shuffle] and [shuffleMix].

### zzzw

```dart
int zzzw
```

Shuffle mask "zzzw".

Used by [shuffle] and [shuffleMix].

### zzwx

```dart
int zzwx
```

Shuffle mask "zzwx".

Used by [shuffle] and [shuffleMix].

### zzwy

```dart
int zzwy
```

Shuffle mask "zzwy".

Used by [shuffle] and [shuffleMix].

### zzwz

```dart
int zzwz
```

Shuffle mask "zzwz".

Used by [shuffle] and [shuffleMix].

### zzww

```dart
int zzww
```

Shuffle mask "zzww".

Used by [shuffle] and [shuffleMix].

### zwxx

```dart
int zwxx
```

Shuffle mask "zwxx".

Used by [shuffle] and [shuffleMix].

### zwxy

```dart
int zwxy
```

Shuffle mask "zwxy".

Used by [shuffle] and [shuffleMix].

### zwxz

```dart
int zwxz
```

Shuffle mask "zwxz".

Used by [shuffle] and [shuffleMix].

### zwxw

```dart
int zwxw
```

Shuffle mask "zwxw".

Used by [shuffle] and [shuffleMix].

### zwyx

```dart
int zwyx
```

Shuffle mask "zwyx".

Used by [shuffle] and [shuffleMix].

### zwyy

```dart
int zwyy
```

Shuffle mask "zwyy".

Used by [shuffle] and [shuffleMix].

### zwyz

```dart
int zwyz
```

Shuffle mask "zwyz".

Used by [shuffle] and [shuffleMix].

### zwyw

```dart
int zwyw
```

Shuffle mask "zwyw".

Used by [shuffle] and [shuffleMix].

### zwzx

```dart
int zwzx
```

Shuffle mask "zwzx".

Used by [shuffle] and [shuffleMix].

### zwzy

```dart
int zwzy
```

Shuffle mask "zwzy".

Used by [shuffle] and [shuffleMix].

### zwzz

```dart
int zwzz
```

Shuffle mask "zwzz".

Used by [shuffle] and [shuffleMix].

### zwzw

```dart
int zwzw
```

Shuffle mask "zwzw".

Used by [shuffle] and [shuffleMix].

### zwwx

```dart
int zwwx
```

Shuffle mask "zwwx".

Used by [shuffle] and [shuffleMix].

### zwwy

```dart
int zwwy
```

Shuffle mask "zwwy".

Used by [shuffle] and [shuffleMix].

### zwwz

```dart
int zwwz
```

Shuffle mask "zwwz".

Used by [shuffle] and [shuffleMix].

### zwww

```dart
int zwww
```

Shuffle mask "zwww".

Used by [shuffle] and [shuffleMix].

### wxxx

```dart
int wxxx
```

Shuffle mask "wxxx".

Used by [shuffle] and [shuffleMix].

### wxxy

```dart
int wxxy
```

Shuffle mask "wxxy".

Used by [shuffle] and [shuffleMix].

### wxxz

```dart
int wxxz
```

Shuffle mask "wxxz".

Used by [shuffle] and [shuffleMix].

### wxxw

```dart
int wxxw
```

Shuffle mask "wxxw".

Used by [shuffle] and [shuffleMix].

### wxyx

```dart
int wxyx
```

Shuffle mask "wxyx".

Used by [shuffle] and [shuffleMix].

### wxyy

```dart
int wxyy
```

Shuffle mask "wxyy".

Used by [shuffle] and [shuffleMix].

### wxyz

```dart
int wxyz
```

Shuffle mask "wxyz".

Used by [shuffle] and [shuffleMix].

### wxyw

```dart
int wxyw
```

Shuffle mask "wxyw".

Used by [shuffle] and [shuffleMix].

### wxzx

```dart
int wxzx
```

Shuffle mask "wxzx".

Used by [shuffle] and [shuffleMix].

### wxzy

```dart
int wxzy
```

Shuffle mask "wxzy".

Used by [shuffle] and [shuffleMix].

### wxzz

```dart
int wxzz
```

Shuffle mask "wxzz".

Used by [shuffle] and [shuffleMix].

### wxzw

```dart
int wxzw
```

Shuffle mask "wxzw".

Used by [shuffle] and [shuffleMix].

### wxwx

```dart
int wxwx
```

Shuffle mask "wxwx".

Used by [shuffle] and [shuffleMix].

### wxwy

```dart
int wxwy
```

Shuffle mask "wxwy".

Used by [shuffle] and [shuffleMix].

### wxwz

```dart
int wxwz
```

Shuffle mask "wxwz".

Used by [shuffle] and [shuffleMix].

### wxww

```dart
int wxww
```

Shuffle mask "wxww".

Used by [shuffle] and [shuffleMix].

### wyxx

```dart
int wyxx
```

Shuffle mask "wyxx".

Used by [shuffle] and [shuffleMix].

### wyxy

```dart
int wyxy
```

Shuffle mask "wyxy".

Used by [shuffle] and [shuffleMix].

### wyxz

```dart
int wyxz
```

Shuffle mask "wyxz".

Used by [shuffle] and [shuffleMix].

### wyxw

```dart
int wyxw
```

Shuffle mask "wyxw".

Used by [shuffle] and [shuffleMix].

### wyyx

```dart
int wyyx
```

Shuffle mask "wyyx".

Used by [shuffle] and [shuffleMix].

### wyyy

```dart
int wyyy
```

Shuffle mask "wyyy".

Used by [shuffle] and [shuffleMix].

### wyyz

```dart
int wyyz
```

Shuffle mask "wyyz".

Used by [shuffle] and [shuffleMix].

### wyyw

```dart
int wyyw
```

Shuffle mask "wyyw".

Used by [shuffle] and [shuffleMix].

### wyzx

```dart
int wyzx
```

Shuffle mask "wyzx".

Used by [shuffle] and [shuffleMix].

### wyzy

```dart
int wyzy
```

Shuffle mask "wyzy".

Used by [shuffle] and [shuffleMix].

### wyzz

```dart
int wyzz
```

Shuffle mask "wyzz".

Used by [shuffle] and [shuffleMix].

### wyzw

```dart
int wyzw
```

Shuffle mask "wyzw".

Used by [shuffle] and [shuffleMix].

### wywx

```dart
int wywx
```

Shuffle mask "wywx".

Used by [shuffle] and [shuffleMix].

### wywy

```dart
int wywy
```

Shuffle mask "wywy".

Used by [shuffle] and [shuffleMix].

### wywz

```dart
int wywz
```

Shuffle mask "wywz".

Used by [shuffle] and [shuffleMix].

### wyww

```dart
int wyww
```

Shuffle mask "wyww".

Used by [shuffle] and [shuffleMix].

### wzxx

```dart
int wzxx
```

Shuffle mask "wzxx".

Used by [shuffle] and [shuffleMix].

### wzxy

```dart
int wzxy
```

Shuffle mask "wzxy".

Used by [shuffle] and [shuffleMix].

### wzxz

```dart
int wzxz
```

Shuffle mask "wzxz".

Used by [shuffle] and [shuffleMix].

### wzxw

```dart
int wzxw
```

Shuffle mask "wzxw".

Used by [shuffle] and [shuffleMix].

### wzyx

```dart
int wzyx
```

Shuffle mask "wzyx".

Used by [shuffle] and [shuffleMix].

### wzyy

```dart
int wzyy
```

Shuffle mask "wzyy".

Used by [shuffle] and [shuffleMix].

### wzyz

```dart
int wzyz
```

Shuffle mask "wzyz".

Used by [shuffle] and [shuffleMix].

### wzyw

```dart
int wzyw
```

Shuffle mask "wzyw".

Used by [shuffle] and [shuffleMix].

### wzzx

```dart
int wzzx
```

Shuffle mask "wzzx".

Used by [shuffle] and [shuffleMix].

### wzzy

```dart
int wzzy
```

Shuffle mask "wzzy".

Used by [shuffle] and [shuffleMix].

### wzzz

```dart
int wzzz
```

Shuffle mask "wzzz".

Used by [shuffle] and [shuffleMix].

### wzzw

```dart
int wzzw
```

Shuffle mask "wzzw".

Used by [shuffle] and [shuffleMix].

### wzwx

```dart
int wzwx
```

Shuffle mask "wzwx".

Used by [shuffle] and [shuffleMix].

### wzwy

```dart
int wzwy
```

Shuffle mask "wzwy".

Used by [shuffle] and [shuffleMix].

### wzwz

```dart
int wzwz
```

Shuffle mask "wzwz".

Used by [shuffle] and [shuffleMix].

### wzww

```dart
int wzww
```

Shuffle mask "wzww".

Used by [shuffle] and [shuffleMix].

### wwxx

```dart
int wwxx
```

Shuffle mask "wwxx".

Used by [shuffle] and [shuffleMix].

### wwxy

```dart
int wwxy
```

Shuffle mask "wwxy".

Used by [shuffle] and [shuffleMix].

### wwxz

```dart
int wwxz
```

Shuffle mask "wwxz".

Used by [shuffle] and [shuffleMix].

### wwxw

```dart
int wwxw
```

Shuffle mask "wwxw".

Used by [shuffle] and [shuffleMix].

### wwyx

```dart
int wwyx
```

Shuffle mask "wwyx".

Used by [shuffle] and [shuffleMix].

### wwyy

```dart
int wwyy
```

Shuffle mask "wwyy".

Used by [shuffle] and [shuffleMix].

### wwyz

```dart
int wwyz
```

Shuffle mask "wwyz".

Used by [shuffle] and [shuffleMix].

### wwyw

```dart
int wwyw
```

Shuffle mask "wwyw".

Used by [shuffle] and [shuffleMix].

### wwzx

```dart
int wwzx
```

Shuffle mask "wwzx".

Used by [shuffle] and [shuffleMix].

### wwzy

```dart
int wwzy
```

Shuffle mask "wwzy".

Used by [shuffle] and [shuffleMix].

### wwzz

```dart
int wwzz
```

Shuffle mask "wwzz".

Used by [shuffle] and [shuffleMix].

### wwzw

```dart
int wwzw
```

Shuffle mask "wwzw".

Used by [shuffle] and [shuffleMix].

### wwwx

```dart
int wwwx
```

Shuffle mask "wwwx".

Used by [shuffle] and [shuffleMix].

### wwwy

```dart
int wwwy
```

Shuffle mask "wwwy".

Used by [shuffle] and [shuffleMix].

### wwwz

```dart
int wwwz
```

Shuffle mask "wwwz".

Used by [shuffle] and [shuffleMix].

### wwww

```dart
int wwww
```

Shuffle mask "wwww".

Used by [shuffle] and [shuffleMix].

### shuffle()

```dart
Float32x4 shuffle(int mask)
```

Shuffle the lane values based on the [mask].

The [mask] must be one of the 256 shuffle masks from [xxxx] to [wwww].

Creates a new [Float32x4] whose lane values are taken from the lanes of this value based on the lanes of the shuffle mask, with the result's [x] lane being taken from the lane of the first letter of the shuffle mask's name, the [y] lane from the second letter, [z] lane from the third letter and [w] lane from the fourth letter.

For example, the shuffle mask [wxyz] creates a new `Float32x4` whose [x] lane is the [w] lane of this value, because the first letter of the shuffle mask's name, `wxyz` is "w". Then the `y`, `z` and `w` lanes of the result are the values of the `x`, `y` and `z` lanes of this value.

The [xyzw] "identity shuffle" mask gives a result with the same lanes as the original.

Some masks preserve the values of all lanes, but may permute them. Other masks duplicates some lanes and discards the values of others.

For example, doing `v1.shuffle(yyyy)` is equivalent to `Float32x4.splat(v1.y)`.

### shuffleMix()

```dart
Float32x4 shuffleMix(Float32x4 other, int mask)
```

Mixes lanes chosen from two [Float32x4] values using a [mask].

Creates a new [Float32x4] where the [x] and [y] lanes are chosen from the lanes of this value selected by the first two letters of the [mask]'s name, and the [z] and [w] lanes are the lanes of [other] selected by the last two letters of the `mask`'s name.

For example, `v1.shuffleMix(v2, Float32x4.xyzw)` is equivalent to `Float32x4(v1.x, v1.y, v2.z, v2.w)`.

If [other] is the same value as this `Float32x4`, this function is the same as [shuffle]. That is, doing `v1.shuffleMix(v1, mask)` is equivalent to `v1.shuffle(mask)`.

### withX()

```dart
Float32x4 withX(double x)
```

This value, but with the value of the [Float32x4.x] lane set to [x].

Returns a new [Float32x4] with the same values for the [y], [z] and [w] lanes as this value, and with a [Float32x4.x] lane having the value [x] converted to a 32-bit floating point number.

### withY()

```dart
Float32x4 withY(double y)
```

This value, but with the value of the [Float32x4.y] lane set to [y].

Returns a new [Float32x4] with the same values for the [x], [z] and [w] lanes as this value, and with a [Float32x4.y] lane having the value [y] converted to a 32-bit floating point number.

### withZ()

```dart
Float32x4 withZ(double z)
```

This value, but with the value of the [Float32x4.z] lane set to [z].

Returns a new [Float32x4] with the same values for the [x], [y] and [w] lanes as this value, and with a [Float32x4.z] lane having the value [z] converted to a 32-bit floating point number.

### withW()

```dart
Float32x4 withW(double w)
```

This value, but with the value of the [Float32x4.w] lane set to [w].

Returns a new [Float32x4] with the same values for the [x], [y] and [z] lanes as this value, and with a [Float32x4.w] lane having the value [w] converted to a 32-bit floating point number.

### min()

```dart
Float32x4 min(Float32x4 other)
```

Lane-wise minimum.

For each lane select the lesser of the lane value of this and [other].

The result is the lesser of the two lane values if either is lesser. The result is unspecified if either lane contains a NaN value or if the values are -0.0 and 0.0, so that neither value is smaller or greater than the other. Different platforms may give different results in those cases, but always one of the lane values.

Returns the result for each lane.

### max()

```dart
Float32x4 max(Float32x4 other)
```

Lane-wise maximum.

For each lane select the greater of the lane value of this and [other].

The result is the greater of the two lane values if either is greater. The result is unspecified if either lane contains a NaN value or if the values are -0.0 and 0.0, so that neither value is smaller or greater than the other. Different platforms may give different results in those cases, but always one of the lane values.

Returns the result for each lane.

### sqrt()

```dart
Float32x4 sqrt()
```

Lane-wise square root.

For each lane compute the 32-bit floating point square root of the lane's value.

The result for a lane is a NaN value if the original value is less than zero or if it is a NaN value. The result for a negative zero, -0.0, is the same value again. The result for positive infinity is positive infinity. Otherwise the result is a positive value which approximates the mathematical square root of the original value.

Returns the result for each lane.

### reciprocal()

```dart
Float32x4 reciprocal()
```

Lane-wise reciprocal.

For each lane compute the result of dividing 1.0 by the lane's value.

If the value is a NaN value, so is the result. If the value is infinite, the result is a zero value with the same sign. If the value is zero, the result is infinite with the same sign. Otherwise the result is an approximation of the mathematical result of dividing 1 by the (finite, non-zero) value of the lane.

Returns the result for each lane.

### reciprocalSqrt()

```dart
Float32x4 reciprocalSqrt()
```

Lane-wise approximation of reciprocal square root.

Approximates the same result as [reciprocal] followed by [sqrt], or [sqrt] followed by [reciprocal], but may be more precise and/or efficient due to computing the result directly, rather than not creating a an intermediate result, and possibly by working entirely at a reduced precision.

The result can differ between platforms due to differences in approximation and precision, and for values where the order of [sqrt] and [reciprocal] makes a difference. The latter applies specifically to `-0.0` where `sqrt(-0.0)` is defined to be -0.0, and `reciprocal` of that is -Infinity. In the opposite order it computes `sqrt` of -Infinity which is NaN.

# Int32x4

```dart
abstract final class Int32x4 {}
```

Int32x4 and operations.

Int32x4 stores 4 32-bit bit-masks in "lanes". The lanes are "x", "y", "z", and "w" respectively.

### Int32x4()

```dart
Int32x4(int x, int y, int z, int w)
```

### Int32x4.bool()

```dart
Int32x4.bool(bool x, bool y, bool z, bool w)
```

### Int32x4.fromFloat32x4Bits()

```dart
Int32x4.fromFloat32x4Bits(Float32x4 x)
```

### operator |()

```dart
Int32x4 operator |(Int32x4 other)
```

The bit-wise or operator.

### operator &()

```dart
Int32x4 operator &(Int32x4 other)
```

The bit-wise and operator.

### operator ^()

```dart
Int32x4 operator ^(Int32x4 other)
```

The bit-wise xor operator.

### operator +()

```dart
Int32x4 operator +(Int32x4 other)
```

Addition operator.

### operator -()

```dart
Int32x4 operator -(Int32x4 other)
```

Subtraction operator.

### x

```dart
int get x
```

Extract 32-bit mask from x lane.

### y

```dart
int get y
```

Extract 32-bit mask from y lane.

### z

```dart
int get z
```

Extract 32-bit mask from z lane.

### w

```dart
int get w
```

Extract 32-bit mask from w lane.

### signMask

```dart
int get signMask
```

Extract the top bit from each lane return them in the first 4 bits. "x" lane is bit 0. "y" lane is bit 1. "z" lane is bit 2. "w" lane is bit 3.

### xxxx

```dart
int xxxx
```

Mask passed to [shuffle] or [shuffleMix].

### xxxy

```dart
int xxxy
```

### xxxz

```dart
int xxxz
```

### xxxw

```dart
int xxxw
```

### xxyx

```dart
int xxyx
```

### xxyy

```dart
int xxyy
```

### xxyz

```dart
int xxyz
```

### xxyw

```dart
int xxyw
```

### xxzx

```dart
int xxzx
```

### xxzy

```dart
int xxzy
```

### xxzz

```dart
int xxzz
```

### xxzw

```dart
int xxzw
```

### xxwx

```dart
int xxwx
```

### xxwy

```dart
int xxwy
```

### xxwz

```dart
int xxwz
```

### xxww

```dart
int xxww
```

### xyxx

```dart
int xyxx
```

### xyxy

```dart
int xyxy
```

### xyxz

```dart
int xyxz
```

### xyxw

```dart
int xyxw
```

### xyyx

```dart
int xyyx
```

### xyyy

```dart
int xyyy
```

### xyyz

```dart
int xyyz
```

### xyyw

```dart
int xyyw
```

### xyzx

```dart
int xyzx
```

### xyzy

```dart
int xyzy
```

### xyzz

```dart
int xyzz
```

### xyzw

```dart
int xyzw
```

### xywx

```dart
int xywx
```

### xywy

```dart
int xywy
```

### xywz

```dart
int xywz
```

### xyww

```dart
int xyww
```

### xzxx

```dart
int xzxx
```

### xzxy

```dart
int xzxy
```

### xzxz

```dart
int xzxz
```

### xzxw

```dart
int xzxw
```

### xzyx

```dart
int xzyx
```

### xzyy

```dart
int xzyy
```

### xzyz

```dart
int xzyz
```

### xzyw

```dart
int xzyw
```

### xzzx

```dart
int xzzx
```

### xzzy

```dart
int xzzy
```

### xzzz

```dart
int xzzz
```

### xzzw

```dart
int xzzw
```

### xzwx

```dart
int xzwx
```

### xzwy

```dart
int xzwy
```

### xzwz

```dart
int xzwz
```

### xzww

```dart
int xzww
```

### xwxx

```dart
int xwxx
```

### xwxy

```dart
int xwxy
```

### xwxz

```dart
int xwxz
```

### xwxw

```dart
int xwxw
```

### xwyx

```dart
int xwyx
```

### xwyy

```dart
int xwyy
```

### xwyz

```dart
int xwyz
```

### xwyw

```dart
int xwyw
```

### xwzx

```dart
int xwzx
```

### xwzy

```dart
int xwzy
```

### xwzz

```dart
int xwzz
```

### xwzw

```dart
int xwzw
```

### xwwx

```dart
int xwwx
```

### xwwy

```dart
int xwwy
```

### xwwz

```dart
int xwwz
```

### xwww

```dart
int xwww
```

### yxxx

```dart
int yxxx
```

### yxxy

```dart
int yxxy
```

### yxxz

```dart
int yxxz
```

### yxxw

```dart
int yxxw
```

### yxyx

```dart
int yxyx
```

### yxyy

```dart
int yxyy
```

### yxyz

```dart
int yxyz
```

### yxyw

```dart
int yxyw
```

### yxzx

```dart
int yxzx
```

### yxzy

```dart
int yxzy
```

### yxzz

```dart
int yxzz
```

### yxzw

```dart
int yxzw
```

### yxwx

```dart
int yxwx
```

### yxwy

```dart
int yxwy
```

### yxwz

```dart
int yxwz
```

### yxww

```dart
int yxww
```

### yyxx

```dart
int yyxx
```

### yyxy

```dart
int yyxy
```

### yyxz

```dart
int yyxz
```

### yyxw

```dart
int yyxw
```

### yyyx

```dart
int yyyx
```

### yyyy

```dart
int yyyy
```

### yyyz

```dart
int yyyz
```

### yyyw

```dart
int yyyw
```

### yyzx

```dart
int yyzx
```

### yyzy

```dart
int yyzy
```

### yyzz

```dart
int yyzz
```

### yyzw

```dart
int yyzw
```

### yywx

```dart
int yywx
```

### yywy

```dart
int yywy
```

### yywz

```dart
int yywz
```

### yyww

```dart
int yyww
```

### yzxx

```dart
int yzxx
```

### yzxy

```dart
int yzxy
```

### yzxz

```dart
int yzxz
```

### yzxw

```dart
int yzxw
```

### yzyx

```dart
int yzyx
```

### yzyy

```dart
int yzyy
```

### yzyz

```dart
int yzyz
```

### yzyw

```dart
int yzyw
```

### yzzx

```dart
int yzzx
```

### yzzy

```dart
int yzzy
```

### yzzz

```dart
int yzzz
```

### yzzw

```dart
int yzzw
```

### yzwx

```dart
int yzwx
```

### yzwy

```dart
int yzwy
```

### yzwz

```dart
int yzwz
```

### yzww

```dart
int yzww
```

### ywxx

```dart
int ywxx
```

### ywxy

```dart
int ywxy
```

### ywxz

```dart
int ywxz
```

### ywxw

```dart
int ywxw
```

### ywyx

```dart
int ywyx
```

### ywyy

```dart
int ywyy
```

### ywyz

```dart
int ywyz
```

### ywyw

```dart
int ywyw
```

### ywzx

```dart
int ywzx
```

### ywzy

```dart
int ywzy
```

### ywzz

```dart
int ywzz
```

### ywzw

```dart
int ywzw
```

### ywwx

```dart
int ywwx
```

### ywwy

```dart
int ywwy
```

### ywwz

```dart
int ywwz
```

### ywww

```dart
int ywww
```

### zxxx

```dart
int zxxx
```

### zxxy

```dart
int zxxy
```

### zxxz

```dart
int zxxz
```

### zxxw

```dart
int zxxw
```

### zxyx

```dart
int zxyx
```

### zxyy

```dart
int zxyy
```

### zxyz

```dart
int zxyz
```

### zxyw

```dart
int zxyw
```

### zxzx

```dart
int zxzx
```

### zxzy

```dart
int zxzy
```

### zxzz

```dart
int zxzz
```

### zxzw

```dart
int zxzw
```

### zxwx

```dart
int zxwx
```

### zxwy

```dart
int zxwy
```

### zxwz

```dart
int zxwz
```

### zxww

```dart
int zxww
```

### zyxx

```dart
int zyxx
```

### zyxy

```dart
int zyxy
```

### zyxz

```dart
int zyxz
```

### zyxw

```dart
int zyxw
```

### zyyx

```dart
int zyyx
```

### zyyy

```dart
int zyyy
```

### zyyz

```dart
int zyyz
```

### zyyw

```dart
int zyyw
```

### zyzx

```dart
int zyzx
```

### zyzy

```dart
int zyzy
```

### zyzz

```dart
int zyzz
```

### zyzw

```dart
int zyzw
```

### zywx

```dart
int zywx
```

### zywy

```dart
int zywy
```

### zywz

```dart
int zywz
```

### zyww

```dart
int zyww
```

### zzxx

```dart
int zzxx
```

### zzxy

```dart
int zzxy
```

### zzxz

```dart
int zzxz
```

### zzxw

```dart
int zzxw
```

### zzyx

```dart
int zzyx
```

### zzyy

```dart
int zzyy
```

### zzyz

```dart
int zzyz
```

### zzyw

```dart
int zzyw
```

### zzzx

```dart
int zzzx
```

### zzzy

```dart
int zzzy
```

### zzzz

```dart
int zzzz
```

### zzzw

```dart
int zzzw
```

### zzwx

```dart
int zzwx
```

### zzwy

```dart
int zzwy
```

### zzwz

```dart
int zzwz
```

### zzww

```dart
int zzww
```

### zwxx

```dart
int zwxx
```

### zwxy

```dart
int zwxy
```

### zwxz

```dart
int zwxz
```

### zwxw

```dart
int zwxw
```

### zwyx

```dart
int zwyx
```

### zwyy

```dart
int zwyy
```

### zwyz

```dart
int zwyz
```

### zwyw

```dart
int zwyw
```

### zwzx

```dart
int zwzx
```

### zwzy

```dart
int zwzy
```

### zwzz

```dart
int zwzz
```

### zwzw

```dart
int zwzw
```

### zwwx

```dart
int zwwx
```

### zwwy

```dart
int zwwy
```

### zwwz

```dart
int zwwz
```

### zwww

```dart
int zwww
```

### wxxx

```dart
int wxxx
```

### wxxy

```dart
int wxxy
```

### wxxz

```dart
int wxxz
```

### wxxw

```dart
int wxxw
```

### wxyx

```dart
int wxyx
```

### wxyy

```dart
int wxyy
```

### wxyz

```dart
int wxyz
```

### wxyw

```dart
int wxyw
```

### wxzx

```dart
int wxzx
```

### wxzy

```dart
int wxzy
```

### wxzz

```dart
int wxzz
```

### wxzw

```dart
int wxzw
```

### wxwx

```dart
int wxwx
```

### wxwy

```dart
int wxwy
```

### wxwz

```dart
int wxwz
```

### wxww

```dart
int wxww
```

### wyxx

```dart
int wyxx
```

### wyxy

```dart
int wyxy
```

### wyxz

```dart
int wyxz
```

### wyxw

```dart
int wyxw
```

### wyyx

```dart
int wyyx
```

### wyyy

```dart
int wyyy
```

### wyyz

```dart
int wyyz
```

### wyyw

```dart
int wyyw
```

### wyzx

```dart
int wyzx
```

### wyzy

```dart
int wyzy
```

### wyzz

```dart
int wyzz
```

### wyzw

```dart
int wyzw
```

### wywx

```dart
int wywx
```

### wywy

```dart
int wywy
```

### wywz

```dart
int wywz
```

### wyww

```dart
int wyww
```

### wzxx

```dart
int wzxx
```

### wzxy

```dart
int wzxy
```

### wzxz

```dart
int wzxz
```

### wzxw

```dart
int wzxw
```

### wzyx

```dart
int wzyx
```

### wzyy

```dart
int wzyy
```

### wzyz

```dart
int wzyz
```

### wzyw

```dart
int wzyw
```

### wzzx

```dart
int wzzx
```

### wzzy

```dart
int wzzy
```

### wzzz

```dart
int wzzz
```

### wzzw

```dart
int wzzw
```

### wzwx

```dart
int wzwx
```

### wzwy

```dart
int wzwy
```

### wzwz

```dart
int wzwz
```

### wzww

```dart
int wzww
```

### wwxx

```dart
int wwxx
```

### wwxy

```dart
int wwxy
```

### wwxz

```dart
int wwxz
```

### wwxw

```dart
int wwxw
```

### wwyx

```dart
int wwyx
```

### wwyy

```dart
int wwyy
```

### wwyz

```dart
int wwyz
```

### wwyw

```dart
int wwyw
```

### wwzx

```dart
int wwzx
```

### wwzy

```dart
int wwzy
```

### wwzz

```dart
int wwzz
```

### wwzw

```dart
int wwzw
```

### wwwx

```dart
int wwwx
```

### wwwy

```dart
int wwwy
```

### wwwz

```dart
int wwwz
```

### wwww

```dart
int wwww
```

### shuffle()

```dart
Int32x4 shuffle(int mask)
```

Shuffle the lane values. [mask] must be one of the 256 shuffle constants.

### shuffleMix()

```dart
Int32x4 shuffleMix(Int32x4 other, int mask)
```

Shuffle the lane values in this [Int32x4] and [other]. The returned Int32x4 will have XY lanes from this [Int32x4] and ZW lanes from [other]. Uses the same [mask] as [shuffle].

### withX()

```dart
Int32x4 withX(int x)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new x value.

### withY()

```dart
Int32x4 withY(int y)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new y value.

### withZ()

```dart
Int32x4 withZ(int z)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new z value.

### withW()

```dart
Int32x4 withW(int w)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new w value.

### flagX

```dart
bool get flagX
```

Extracted x value. Returns false for 0, true for any other value.

### flagY

```dart
bool get flagY
```

Extracted y value. Returns false for 0, true for any other value.

### flagZ

```dart
bool get flagZ
```

Extracted z value. Returns false for 0, true for any other value.

### flagW

```dart
bool get flagW
```

Extracted w value. Returns false for 0, true for any other value.

### withFlagX()

```dart
Int32x4 withFlagX(bool x)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new x value.

### withFlagY()

```dart
Int32x4 withFlagY(bool y)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new y value.

### withFlagZ()

```dart
Int32x4 withFlagZ(bool z)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new z value.

### withFlagW()

```dart
Int32x4 withFlagW(bool w)
```

Returns a new [Int32x4] copied from this [Int32x4] with a new w value.

### select()

```dart
Float32x4 select(Float32x4 trueValue, Float32x4 falseValue)
```

Merge [trueValue] and [falseValue] based on this [Int32x4] bit mask: Select bit from [trueValue] when bit in this [Int32x4] is on. Select bit from [falseValue] when bit in this [Int32x4] is off.

# Float64x2

```dart
abstract final class Float64x2 {}
```

Float64x2 immutable value type and operations.

Float64x2 stores 2 64-bit floating point values in "lanes". The lanes are "x" and "y" respectively.

It is a compile-time error for a class to attempt to extend or implement `Float64x2`.

### Float64x2()

```dart
Float64x2(double x, double y)
```

### Float64x2.splat()

```dart
Float64x2.splat(double v)
```

### Float64x2.zero()

```dart
Float64x2.zero()
```

### Float64x2.fromFloat32x4()

```dart
Float64x2.fromFloat32x4(Float32x4 v)
```

Uses the "x" and "y" lanes from [v].

### operator +()

```dart
Float64x2 operator +(Float64x2 other)
```

Addition operator.

### operator -()

```dart
Float64x2 operator -()
```

Negate operator.

### operator -()

```dart
Float64x2 operator -(Float64x2 other)
```

Subtraction operator.

### operator \*()

```dart
Float64x2 operator *(Float64x2 other)
```

Multiplication operator.

### operator /()

```dart
Float64x2 operator /(Float64x2 other)
```

Division operator.

### scale()

```dart
Float64x2 scale(double s)
```

Returns a copy of this [Float64x2] each lane being scaled by [s]. Equivalent to this \* new Float64x2.splat(s)

### abs()

```dart
Float64x2 abs()
```

The lane-wise absolute value of this [Float64x2].

### clamp()

```dart
Float64x2 clamp(Float64x2 lowerLimit, Float64x2 upperLimit)
```

Lane-wise clamp this [Float64x2] to be in the range [lowerLimit]-[upperLimit].

### x

```dart
double get x
```

Extracted x value.

### y

```dart
double get y
```

Extracted y value.

### signMask

```dart
int get signMask
```

Extract the sign bits from each lane return them in the first 2 bits. "x" lane is bit 0. "y" lane is bit 1.

### withX()

```dart
Float64x2 withX(double x)
```

Returns a new [Float64x2] copied from this [Float64x2] with a new x value.

### withY()

```dart
Float64x2 withY(double y)
```

Returns a new [Float64x2] copied from this [Float64x2] with a new y value.

### min()

```dart
Float64x2 min(Float64x2 other)
```

The lane-wise minimum value in this [Float64x2] or [other].

### max()

```dart
Float64x2 max(Float64x2 other)
```

The lane-wise maximum value in this [Float64x2] or [other].

### sqrt()

```dart
Float64x2 sqrt()
```

The lane-wise square root of this [Float64x2].
