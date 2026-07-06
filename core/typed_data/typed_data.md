Lists that efficiently handle fixed sized data (for example, unsigned 8 byte integers) and SIMD numeric types.

To use this library in your code:

```dart
import 'dart:typed_data';
```

# TypedData

```dart
abstract final class TypedData {}
```

A typed view of a sequence of bytes.

It is a compile-time error for a class to attempt to extend or implement `TypedData`.

## 属性

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

---

# TypedDataList

```dart
abstract final class TypedDataList<E> implements TypedData, List<E> {}
```

A [TypedData] fixed-length [List]-view on the bytes of [buffer].

---

# Endian

```dart
final class Endian {}
```

Endianness of number representation.

The order of bytes in memory of a number representation, with [little] endian having the least significant byte first, and [big] endian (aka. network byte order) having the most significant byte first.

The [host] endian is the native endianness of the underlying platform, and the default endianness used by typed-data lists, like [Uint16List], on this platform. Always one of [little] or [big] endian.

Can be specified when accessing or updating a sequence of bytes using a [ByteData] view. The host endianness can be used if accessing larger numbers by their bytes, for example through a [Uint8List] view on a buffer written using an [Int64List] view of the same buffer..

It is a compile-time error for a class to attempt to extend or implement `Endian`.

## constant

### big

```dart
static const Endian big = Endian._(false);
```

### little

```dart
static const Endian little = Endian._(true);
```

## 静态属性

### host

```dart
Endian host
```

---

# Int8List

```dart
abstract final class Int8List implements _TypedIntList {}
```

A fixed-length list of 8-bit signed integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low eight bits, interpreted as a signed 8-bit two's complement integer with values in the range -128 to +127.

It is a compile-time error for a class to attempt to extend or implement `Int8List`.

## 构造函数

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

## 静态常量

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
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

---

# Uint8List

```dart
abstract final class Uint8List implements _TypedIntList {}
```

A fixed-length list of 8-bit unsigned integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low eight bits, interpreted as an unsigned 8-bit integer with values in the range 0 to 255.

It is a compile-time error for a class to attempt to extend or implement `Uint8List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Uint8List asUnmodifiableView()
```

A read-only view of this [Uint8List].

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

## 运算符

### operator +

```dart
List<int> operator +(List<int> other)
```

Returns a concatenation of this list and [other].

If [other] is also a typed-data list, then the return list will be a typed data list capable of holding both unsigned 8-bit integers and the elements of [other], otherwise it'll be a normal list of integers.

---

# Uint8ClampedList

```dart
abstract final class Uint8ClampedList implements _TypedIntList {}
```

A fixed-length list of 8-bit unsigned integers.

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are clamped to an unsigned eight bit value. That is, all values below zero are stored as zero and all values above 255 are stored as 255.

It is a compile-time error for a class to attempt to extend or implement `Uint8ClampedList`.

## 构造函数

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
@Since.new("3.3")
Uint8ClampedList.sublistView( TypedData data, [int start = 0, int? end])
```

Creates a [Uint8ClampedList] view on a range of elements of [data].

Creates a view on the range of `data.buffer` which corresponds to the elements of [data] from [start] until [end]. If [data] is a typed data list, like [Uint16List], then the view is on the bytes of the elements with indices from [start] until [end]. If [data] is a [ByteData], it's treated like a list of bytes.

If provided, [start] and [end] must satisfy

0 &le; `start` &le; `end` &le; _elementCount_

where _elementCount_ is the number of elements in [data], which is the same as the [List.length] of a typed data list.

If omitted, [start] defaults to zero and [end] to _elementCount_.

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
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

---

# Int16List

```dart
abstract final class Int16List implements _TypedIntList {}
```

A fixed-length list of 16-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 16 bits, interpreted as a signed 16-bit two's complement integer with values in the range -32768 to +32767.

It is a compile-time error for a class to attempt to extend or implement `Int16List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 2;
```

## 方法

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

---

# Uint16List

```dart
abstract final class Uint16List implements _TypedIntList {}
```

A fixed-length list of 16-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 16 bits, interpreted as an unsigned 16-bit integer with values in the range 0 to 65535.

It is a compile-time error for a class to attempt to extend or implement `Uint16List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 2;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
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

---

# Int32List

```dart
abstract final class Int32List implements _TypedIntList {}
```

A fixed-length list of 32-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 32 bits, interpreted as a signed 32-bit two's complement integer with values in the range -2147483648 to 2147483647.

It is a compile-time error for a class to attempt to extend or implement `Int32List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 4;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Int32List asUnmodifiableView()
```

A read-only view of this [Int32List].

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

---

# Uint32List

```dart
abstract final class Uint32List implements _TypedIntList {}
```

A fixed-length list of 32-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 32 bits, interpreted as an unsigned 32-bit integer with values in the range 0 to 4294967295.

It is a compile-time error for a class to attempt to extend or implement `Uint32List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 4;
```

## 方法

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

---

# Int64List

```dart
abstract final class Int64List implements _TypedIntList {}
```

A fixed-length list of 64-bit signed integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 64 bits, interpreted as a signed 64-bit two's complement integer with values in the range -9223372036854775808 to +9223372036854775807.

It is a compile-time error for a class to attempt to extend or implement `Int64List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 8;
```

## 方法

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

---

# Uint64List

```dart
abstract final class Uint64List implements _TypedIntList {}
```

A fixed-length list of 64-bit unsigned integers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Integers stored in the list are truncated to their low 64 bits, interpreted as an unsigned 64-bit integer with values in the range 0 to 18446744073709551615.

It is a compile-time error for a class to attempt to extend or implement `Uint64List`.

## 构造函数

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

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 8;
```

## 方法

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

---

# Float32List

```dart
abstract final class Float32List implements _TypedFloatList {}
```

A fixed-length list of IEEE 754 single-precision binary floating-point numbers that is viewable as a [TypedData].

For long lists, this implementation can be considerably more space- and time-efficient than the default [List] implementation.

Double values stored in the list are converted to the nearest single-precision value. Values read are converted to a double value with the same value.

It is a compile-time error for a class to attempt to extend or implement `Float32List`.

## 构造函数

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
