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
