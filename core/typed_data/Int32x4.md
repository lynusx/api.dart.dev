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
