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
