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
