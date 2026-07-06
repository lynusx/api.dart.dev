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
