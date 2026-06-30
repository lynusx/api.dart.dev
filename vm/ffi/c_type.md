This library defines [NativeType]s for common C types.

Many C types only define a minimal size in the C standard, but they are consistent per [Abi]. Therefore we use [AbiSpecificInteger]s to define these C types in this library.

# Char

```dart
final class Char extends AbiSpecificInteger {}
```

The C `char` type.

Typically a signed or unsigned 8-bit integer. For a guaranteed 8-bit integer, use [Int8] with the C `int8_t` type or [Uint8] with the C `uint8_t` type. For a specifically `signed` or `unsigned` `char`, use [SignedChar] or [UnsignedChar].

The [Char] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### Char()

```dart
Char()
```

# SignedChar

```dart
final class SignedChar extends AbiSpecificInteger {}
```

The C `signed char` type.

Typically a signed 8-bit integer. For a guaranteed 8-bit integer, use [Int8] with the C `int8_t` type. For an `unsigned char`, use [UnsignedChar].

The [SignedChar] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### SignedChar()

```dart
SignedChar()
```

# UnsignedChar

```dart
final class UnsignedChar extends AbiSpecificInteger {}
```

The C `unsigned char` type.

Typically an unsigned 8-bit integer. For a guaranteed 8-bit integer, use [Uint8] with the C `uint8_t` type. For a `signed char`, use [SignedChar].

The [UnsignedChar] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UnsignedChar()

```dart
UnsignedChar()
```

# Short

```dart
final class Short extends AbiSpecificInteger {}
```

The C `short` type.

Typically a signed 16-bit integer. For a guaranteed 16-bit integer, use [Int16] with the C `int16_t` type. For an `unsigned short`, use [UnsignedShort].

The [Short] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### Short()

```dart
Short()
```

# UnsignedShort

```dart
final class UnsignedShort extends AbiSpecificInteger {}
```

The C `unsigned short` type.

Typically an unsigned 16-bit integer. For a guaranteed 16-bit integer, use [Uint16] with the C `uint16_t` type. For a signed `short`, use [Short].

The [UnsignedShort] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UnsignedShort()

```dart
UnsignedShort()
```

# Int

```dart
final class Int extends AbiSpecificInteger {}
```

The C `int` type.

Typically a signed 32-bit integer. For a guaranteed 32-bit integer, use [Int32] with the C `int32_t` type. For an `unsigned int`, use [UnsignedInt].

The [Int] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### Int()

```dart
Int()
```

# UnsignedInt

```dart
final class UnsignedInt extends AbiSpecificInteger {}
```

The C `unsigned int` type.

Typically an unsigned 32-bit integer. For a guaranteed 32-bit integer, use [Uint32] with the C `uint32_t` type. For a signed `int`, use [Int].

The [UnsignedInt] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UnsignedInt()

```dart
UnsignedInt()
```

# Long

```dart
final class Long extends AbiSpecificInteger {}
```

The C `long int`, aka. `long`, type.

Typically a signed 32- or 64-bit integer. For a guaranteed 32-bit integer, use [Int32] with the C `int32_t` type. For a guaranteed 64-bit integer, use [Int64] with the C `int64_t` type. For an `unsigned long`, use [UnsignedLong].

The [Long] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### Long()

```dart
Long()
```

# UnsignedLong

```dart
final class UnsignedLong extends AbiSpecificInteger {}
```

The C `unsigned long int`, aka. `unsigned long`, type.

Typically an unsigned 32- or 64-bit integer. For a guaranteed 32-bit integer, use [Uint32] with the C `uint32_t` type. For a guaranteed 64-bit integer, use [Uint64] with the C `uint64_t` type. For a signed `long`, use [Long].

The [UnsignedLong] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UnsignedLong()

```dart
UnsignedLong()
```

# LongLong

```dart
final class LongLong extends AbiSpecificInteger {}
```

The C `long long` type.

Typically a signed 64-bit integer. For a guaranteed 64-bit integer, use [Int64] with the C `int64_t` type. For an `unsigned long long`, use [UnsignedLongLong].

The [LongLong] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### LongLong()

```dart
LongLong()
```

# UnsignedLongLong

```dart
final class UnsignedLongLong extends AbiSpecificInteger {}
```

The C `unsigned long long` type.

Typically an unsigned 64-bit integer. For a guaranteed 64-bit integer, use [Uint64] with the C `uint64_t` type. For a signed `long long`, use [LongLong].

The [UnsignedLongLong] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UnsignedLongLong()

```dart
UnsignedLongLong()
```

# IntPtr

```dart
final class IntPtr extends AbiSpecificInteger {}
```

The C `intptr_t` type.

The [IntPtr] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### IntPtr()

```dart
IntPtr()
```

# UintPtr

```dart
final class UintPtr extends AbiSpecificInteger {}
```

The C `uintptr_t` type.

The [UintPtr] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### UintPtr()

```dart
UintPtr()
```

# Size

```dart
final class Size extends AbiSpecificInteger {}
```

The C `size_t` type.

The [Size] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### Size()

```dart
Size()
```

# WChar

```dart
final class WChar extends AbiSpecificInteger {}
```

The C `wchar_t` type.

The signedness of `wchar_t` is undefined in C. Here, it is exposed as the defaults on the tested [Abi]s.

The [WChar] type is a native type, and should not be constructed in Dart code. It occurs only in native type signatures and as annotation on [Struct] and [Union] fields.

### WChar()

```dart
WChar()
```
