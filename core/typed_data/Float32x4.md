# Float32x4

```dart
abstract final class Float32x4 {}
```

四个 32 位浮点数值。

Float32x4 在“通道”（lane）中存储四个 32 位浮点数值。这些通道分别被命名为 [x]、[y]、[z] 和 [w]。

可以对一个或多个 `Float32x4` 执行单一操作，该操作会对操作数的每个通道执行相应运算，并返回一个新的 `Float32x4`（或类似的多值结果），其中包含每个通道的运算结果。

`Float32x4` 类不能被继承或实现。

### Float32x4()

```dart
Float32x4(double x, double y, double z, double w)
```

创建一个包含参数所对应的 32 位浮点数值的 `Float32x4`。

创建的值的 [Float32x4.x]、[Float32x4.y]、[Float32x4.z] 和 [Float32x4.w] 值，是通过将参数 [x]、[y]、[z] 和 [w] 从 64 位浮点数转换为 32 位浮点数得到的。

从 64 位浮点数转换为 32 位浮点数会丢失显著的精度，即使原始的 64 位浮点数值非零且有限，转换结果也可能变为零或无穷大。

### Float32x4.splat()

```dart
Float32x4.splat(double value)
```

创建一个四个通道具有相同 32 位浮点数值的 `Float32x4`。

创建的值的 [Float32x4.x]、[Float32x4.y]、[Float32x4.z] 和 [Float32x4.w] 值相同，均为将 64 位浮点数 [value] 转换为 32 位浮点数后得到的值。

从 64 位浮点数转换为 32 位浮点数会丢失显著的精度，即使原始的 64 位浮点数值非零且有限，转换结果也可能变为零或无穷大。

### Float32x4.zero()

```dart
Float32x4.zero()
```

创建一个所有值均为零的 `Float32x4`。

创建的值的 [Float32x4.x]、[Float32x4.y]、[Float32x4.z] 和 [Float32x4.w] 值相同，均为 32 位浮点数的零值。

### Float32x4.fromInt32x4Bits()

```dart
Float32x4.fromInt32x4Bits(Int32x4 bits)
```

根据提供的位创建一个 `Float32x4`，其值为对应的 32 位浮点数。

创建的值的 [Float32x4.x]、[Float32x4.y]、[Float32x4.z] 和 [Float32x4.w] 值，是 [bits] 中对应通道位表示所对应的 32 位浮点数值。

该转换对 32 位整数和 32 位浮点数均使用 _平台字节序_ 进行。

### Float32x4.fromFloat64x2()

```dart
Float32x4.fromFloat64x2(Float64x2 xy)
```

创建一个 `Float32x4`，其 [x] 和 [y] 通道的值来自 [xy]。

创建的值的 [Float32x4.x] 和 [Float32x4.y] 值，是 [xy] 的 [Float64x2.x] 和 [Float64x2.y] 通道转换为 32 位浮点数后得到的值。[Float32x4.z] 和 [Float32x4.w] 通道被设置为零值。

### operator +()

```dart
Float32x4 operator +(Float32x4 other)
```

逐通道加法。

将此值每个通道的值与 [other] 对应通道的值相加。

返回每个通道的运算结果。

### operator -()

```dart
Float32x4 operator -()
```

逐通道取负。

返回一个结果，其中每个通道是此值对应通道的相反数。

### operator -()

```dart
Float32x4 operator -(Float32x4 other)
```

逐通道减法。

用此值每个通道的值减去 [other] 对应通道的值。

返回每个通道的运算结果。

### operator \*()

```dart
Float32x4 operator *(Float32x4 other)
```

逐通道乘法。

将此值每个通道的值与 [other] 对应通道的值相乘。

返回每个通道的运算结果。

### operator /()

```dart
Float32x4 operator /(Float32x4 other)
```

逐通道除法。

用此值每个通道的值除以 [other] 对应通道的值。

返回每个通道的运算结果。

### lessThan()

```dart
Int32x4 lessThan(Float32x4 other)
```

逐通道小于比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _小于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### lessThanOrEqual()

```dart
Int32x4 lessThanOrEqual(Float32x4 other)
```

逐通道小于等于比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _小于或等于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### greaterThan()

```dart
Int32x4 greaterThan(Float32x4 other)
```

逐通道大于比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _大于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### greaterThanOrEqual()

```dart
Int32x4 greaterThanOrEqual(Float32x4 other)
```

逐通道大于等于比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _大于或等于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### equal()

```dart
Int32x4 equal(Float32x4 other)
```

逐通道相等比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _等于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### notEqual()

```dart
Int32x4 notEqual(Float32x4 other)
```

逐通道不等比较。

使用 32 位浮点数比较方式，将此值每个通道的 32 位浮点数值与 [other] 对应通道的值进行比较。_对于浮点数比较，与 NaN 值的比较结果始终为 false，且 -0.0（负零）被视为等于 0.0（正零），而非严格小于它。_ 若此对象的值 _不等于_ [other] 的值，该通道的结果为 32 位有符号整数 -1（所有位均置 1）；否则结果为 0（所有位均清零），包括任一值为 NaN 值的情况。

返回的四个值始终为 0 或 -1。

### scale()

```dart
Float32x4 scale(double scale)
```

逐通道乘以 [scale]。

返回一个结果，其中每个通道是此值对应通道与 [scale] 相乘所得的结果。此运算既可以先将通道值转换为 64 位浮点数、与 [scale] 相乘后再转换回 32 位浮点数来实现，也可以先将 [scale] 转换为 32 位浮点数并执行 32 位浮点数乘法来实现。

在后一种情况下，其等价于 `thisValue * Float32x4.splat(s)`。

### abs()

```dart
Float32x4 abs()
```

逐通道转换为绝对值。

若某通道的值为负，则将其取反；若为非负值，则保留原值，从而将每个通道的值转换为非负值。

返回每个通道的运算结果。

### clamp()

```dart
Float32x4 clamp(Float32x4 lowerLimit, Float32x4 upperLimit)
```

逐通道限定在一个范围内。

将每个通道的值限定在最小值（[lowerLimit] 对应通道的值）与最大值（[upperLimit] 对应通道的值）之间。若原始值低于最小值，结果为最小值；若原始值高于最大值，结果为最大值。若最大值低于最小值，或者三个值中任意一个为 NaN 值，则结果未指定；但除此之外，结果必为这三个值之一，若任一值为 NaN 值，也可能是另一个不同的 NaN 值。

返回每个通道的运算结果。

### x

```dart
double get x
```

“x” 通道的值。

### y

```dart
double get y
```

“y” 通道的值。

### z

```dart
double get z
```

“z” 通道的值。

### w

```dart
double get w
```

“w” 通道的值。

### signMask

```dart
int get signMask
```

每个通道的符号位（以单个比特位形式）。

每个通道的 32 位浮点数值的符号位存储在此值的低四位中：

- [x] 通道位于第 0 位。
- [y] 通道位于第 1 位。
- [z] 通道位于第 2 位。
- [w] 通道位于第 3 位。

### xxxx

```dart
int xxxx
```

混洗掩码 “xxxx”。

用于 [shuffle] 和 [shuffleMix]。

### xxxy

```dart
int xxxy
```

混洗掩码 “xxxy”。

用于 [shuffle] 和 [shuffleMix]。

### xxxz

```dart
int xxxz
```

混洗掩码 “xxxz”。

用于 [shuffle] 和 [shuffleMix]。

### xxxw

```dart
int xxxw
```

混洗掩码 “xxxw”。

用于 [shuffle] 和 [shuffleMix]。

### xxyx

```dart
int xxyx
```

混洗掩码 “xxyx”。

用于 [shuffle] 和 [shuffleMix]。

### xxyy

```dart
int xxyy
```

混洗掩码 “xxyy”。

用于 [shuffle] 和 [shuffleMix]。

### xxyz

```dart
int xxyz
```

混洗掩码 “xxyz”。

用于 [shuffle] 和 [shuffleMix]。

### xxyw

```dart
int xxyw
```

混洗掩码 “xxyw”。

用于 [shuffle] 和 [shuffleMix]。

### xxzx

```dart
int xxzx
```

混洗掩码 “xxzx”。

用于 [shuffle] 和 [shuffleMix]。

### xxzy

```dart
int xxzy
```

混洗掩码 “xxzy”。

用于 [shuffle] 和 [shuffleMix]。

### xxzz

```dart
int xxzz
```

混洗掩码 “xxzz”。

用于 [shuffle] 和 [shuffleMix]。

### xxzw

```dart
int xxzw
```

混洗掩码 “xxzw”。

用于 [shuffle] 和 [shuffleMix]。

### xxwx

```dart
int xxwx
```

混洗掩码 “xxwx”。

用于 [shuffle] 和 [shuffleMix]。

### xxwy

```dart
int xxwy
```

混洗掩码 “xxwy”。

用于 [shuffle] 和 [shuffleMix]。

### xxwz

```dart
int xxwz
```

混洗掩码 “xxwz”。

用于 [shuffle] 和 [shuffleMix]。

### xxww

```dart
int xxww
```

混洗掩码 “xxww”。

用于 [shuffle] 和 [shuffleMix]。

### xyxx

```dart
int xyxx
```

混洗掩码 “xyxx”。

用于 [shuffle] 和 [shuffleMix]。

### xyxy

```dart
int xyxy
```

混洗掩码 “xyxy”。

用于 [shuffle] 和 [shuffleMix]。

### xyxz

```dart
int xyxz
```

混洗掩码 “xyxz”。

用于 [shuffle] 和 [shuffleMix]。

### xyxw

```dart
int xyxw
```

混洗掩码 “xyxw”。

用于 [shuffle] 和 [shuffleMix]。

### xyyx

```dart
int xyyx
```

混洗掩码 “xyyx”。

用于 [shuffle] 和 [shuffleMix]。

### xyyy

```dart
int xyyy
```

混洗掩码 “xyyy”。

用于 [shuffle] 和 [shuffleMix]。

### xyyz

```dart
int xyyz
```

混洗掩码 “xyyz”。

用于 [shuffle] 和 [shuffleMix]。

### xyyw

```dart
int xyyw
```

混洗掩码 “xyyw”。

用于 [shuffle] 和 [shuffleMix]。

### xyzx

```dart
int xyzx
```

混洗掩码 “xyzx”。

用于 [shuffle] 和 [shuffleMix]。

### xyzy

```dart
int xyzy
```

混洗掩码 “xyzy”。

用于 [shuffle] 和 [shuffleMix]。

### xyzz

```dart
int xyzz
```

混洗掩码 “xyzz”。

用于 [shuffle] 和 [shuffleMix]。

### xyzw

```dart
int xyzw
```

混洗掩码 “xyzw”。

用于 [shuffle] 和 [shuffleMix]。

### xywx

```dart
int xywx
```

混洗掩码 “xywx”。

用于 [shuffle] 和 [shuffleMix]。

### xywy

```dart
int xywy
```

混洗掩码 “xywy”。

用于 [shuffle] 和 [shuffleMix]。

### xywz

```dart
int xywz
```

混洗掩码 “xywz”。

用于 [shuffle] 和 [shuffleMix]。

### xyww

```dart
int xyww
```

混洗掩码 “xyww”。

用于 [shuffle] 和 [shuffleMix]。

### xzxx

```dart
int xzxx
```

混洗掩码 “xzxx”。

用于 [shuffle] 和 [shuffleMix]。

### xzxy

```dart
int xzxy
```

混洗掩码 “xzxy”。

用于 [shuffle] 和 [shuffleMix]。

### xzxz

```dart
int xzxz
```

混洗掩码 “xzxz”。

用于 [shuffle] 和 [shuffleMix]。

### xzxw

```dart
int xzxw
```

混洗掩码 “xzxw”。

用于 [shuffle] 和 [shuffleMix]。

### xzyx

```dart
int xzyx
```

混洗掩码 “xzyx”。

用于 [shuffle] 和 [shuffleMix]。

### xzyy

```dart
int xzyy
```

混洗掩码 “xzyy”。

用于 [shuffle] 和 [shuffleMix]。

### xzyz

```dart
int xzyz
```

混洗掩码 “xzyz”。

用于 [shuffle] 和 [shuffleMix]。

### xzyw

```dart
int xzyw
```

混洗掩码 “xzyw”。

用于 [shuffle] 和 [shuffleMix]。

### xzzx

```dart
int xzzx
```

混洗掩码 “xzzx”。

用于 [shuffle] 和 [shuffleMix]。

### xzzy

```dart
int xzzy
```

混洗掩码 “xzzy”。

用于 [shuffle] 和 [shuffleMix]。

### xzzz

```dart
int xzzz
```

混洗掩码 “xzzz”。

用于 [shuffle] 和 [shuffleMix]。

### xzzw

```dart
int xzzw
```

混洗掩码 “xzzw”。

用于 [shuffle] 和 [shuffleMix]。

### xzwx

```dart
int xzwx
```

混洗掩码 “xzwx”。

用于 [shuffle] 和 [shuffleMix]。

### xzwy

```dart
int xzwy
```

混洗掩码 “xzwy”。

用于 [shuffle] 和 [shuffleMix]。

### xzwz

```dart
int xzwz
```

混洗掩码 “xzwz”。

用于 [shuffle] 和 [shuffleMix]。

### xzww

```dart
int xzww
```

混洗掩码 “xzww”。

用于 [shuffle] 和 [shuffleMix]。

### xwxx

```dart
int xwxx
```

混洗掩码 “xwxx”。

用于 [shuffle] 和 [shuffleMix]。

### xwxy

```dart
int xwxy
```

混洗掩码 “xwxy”。

用于 [shuffle] 和 [shuffleMix]。

### xwxz

```dart
int xwxz
```

混洗掩码 “xwxz”。

用于 [shuffle] 和 [shuffleMix]。

### xwxw

```dart
int xwxw
```

混洗掩码 “xwxw”。

用于 [shuffle] 和 [shuffleMix]。

### xwyx

```dart
int xwyx
```

混洗掩码 “xwyx”。

用于 [shuffle] 和 [shuffleMix]。

### xwyy

```dart
int xwyy
```

混洗掩码 “xwyy”。

用于 [shuffle] 和 [shuffleMix]。

### xwyz

```dart
int xwyz
```

混洗掩码 “xwyz”。

用于 [shuffle] 和 [shuffleMix]。

### xwyw

```dart
int xwyw
```

混洗掩码 “xwyw”。

用于 [shuffle] 和 [shuffleMix]。

### xwzx

```dart
int xwzx
```

混洗掩码 “xwzx”。

用于 [shuffle] 和 [shuffleMix]。

### xwzy

```dart
int xwzy
```

混洗掩码 “xwzy”。

用于 [shuffle] 和 [shuffleMix]。

### xwzz

```dart
int xwzz
```

混洗掩码 “xwzz”。

用于 [shuffle] 和 [shuffleMix]。

### xwzw

```dart
int xwzw
```

混洗掩码 “xwzw”。

用于 [shuffle] 和 [shuffleMix]。

### xwwx

```dart
int xwwx
```

混洗掩码 “xwwx”。

用于 [shuffle] 和 [shuffleMix]。

### xwwy

```dart
int xwwy
```

混洗掩码 “xwwy”。

用于 [shuffle] 和 [shuffleMix]。

### xwwz

```dart
int xwwz
```

混洗掩码 “xwwz”。

用于 [shuffle] 和 [shuffleMix]。

### xwww

```dart
int xwww
```

混洗掩码 “xwww”。

用于 [shuffle] 和 [shuffleMix]。

### yxxx

```dart
int yxxx
```

混洗掩码 “yxxx”。

用于 [shuffle] 和 [shuffleMix]。

### yxxy

```dart
int yxxy
```

混洗掩码 “yxxy”。

用于 [shuffle] 和 [shuffleMix]。

### yxxz

```dart
int yxxz
```

混洗掩码 “yxxz”。

用于 [shuffle] 和 [shuffleMix]。

### yxxw

```dart
int yxxw
```

混洗掩码 “yxxw”。

用于 [shuffle] 和 [shuffleMix]。

### yxyx

```dart
int yxyx
```

混洗掩码 “yxyx”。

用于 [shuffle] 和 [shuffleMix]。

### yxyy

```dart
int yxyy
```

混洗掩码 “yxyy”。

用于 [shuffle] 和 [shuffleMix]。

### yxyz

```dart
int yxyz
```

混洗掩码 “yxyz”。

用于 [shuffle] 和 [shuffleMix]。

### yxyw

```dart
int yxyw
```

混洗掩码 “yxyw”。

用于 [shuffle] 和 [shuffleMix]。

### yxzx

```dart
int yxzx
```

混洗掩码 “yxzx”。

用于 [shuffle] 和 [shuffleMix]。

### yxzy

```dart
int yxzy
```

混洗掩码 “yxzy”。

用于 [shuffle] 和 [shuffleMix]。

### yxzz

```dart
int yxzz
```

混洗掩码 “yxzz”。

用于 [shuffle] 和 [shuffleMix]。

### yxzw

```dart
int yxzw
```

混洗掩码 “yxzw”。

用于 [shuffle] 和 [shuffleMix]。

### yxwx

```dart
int yxwx
```

混洗掩码 “yxwx”。

用于 [shuffle] 和 [shuffleMix]。

### yxwy

```dart
int yxwy
```

混洗掩码 “yxwy”。

用于 [shuffle] 和 [shuffleMix]。

### yxwz

```dart
int yxwz
```

混洗掩码 “yxwz”。

用于 [shuffle] 和 [shuffleMix]。

### yxww

```dart
int yxww
```

混洗掩码 “yxww”。

用于 [shuffle] 和 [shuffleMix]。

### yyxx

```dart
int yyxx
```

混洗掩码 “yyxx”。

用于 [shuffle] 和 [shuffleMix]。

### yyxy

```dart
int yyxy
```

混洗掩码 “yyxy”。

用于 [shuffle] 和 [shuffleMix]。

### yyxz

```dart
int yyxz
```

混洗掩码 “yyxz”。

用于 [shuffle] 和 [shuffleMix]。

### yyxw

```dart
int yyxw
```

混洗掩码 “yyxw”。

用于 [shuffle] 和 [shuffleMix]。

### yyyx

```dart
int yyyx
```

混洗掩码 “yyyx”。

用于 [shuffle] 和 [shuffleMix]。

### yyyy

```dart
int yyyy
```

混洗掩码 “yyyy”。

用于 [shuffle] 和 [shuffleMix]。

### yyyz

```dart
int yyyz
```

混洗掩码 “yyyz”。

用于 [shuffle] 和 [shuffleMix]。

### yyyw

```dart
int yyyw
```

混洗掩码 “yyyw”。

用于 [shuffle] 和 [shuffleMix]。

### yyzx

```dart
int yyzx
```

混洗掩码 “yyzx”。

用于 [shuffle] 和 [shuffleMix]。

### yyzy

```dart
int yyzy
```

混洗掩码 “yyzy”。

用于 [shuffle] 和 [shuffleMix]。

### yyzz

```dart
int yyzz
```

混洗掩码 “yyzz”。

用于 [shuffle] 和 [shuffleMix]。

### yyzw

```dart
int yyzw
```

混洗掩码 “yyzw”。

用于 [shuffle] 和 [shuffleMix]。

### yywx

```dart
int yywx
```

混洗掩码 “yywx”。

用于 [shuffle] 和 [shuffleMix]。

### yywy

```dart
int yywy
```

混洗掩码 “yywy”。

用于 [shuffle] 和 [shuffleMix]。

### yywz

```dart
int yywz
```

混洗掩码 “yywz”。

用于 [shuffle] 和 [shuffleMix]。

### yyww

```dart
int yyww
```

混洗掩码 “yyww”。

用于 [shuffle] 和 [shuffleMix]。

### yzxx

```dart
int yzxx
```

混洗掩码 “yzxx”。

用于 [shuffle] 和 [shuffleMix]。

### yzxy

```dart
int yzxy
```

混洗掩码 “yzxy”。

用于 [shuffle] 和 [shuffleMix]。

### yzxz

```dart
int yzxz
```

混洗掩码 “yzxz”。

用于 [shuffle] 和 [shuffleMix]。

### yzxw

```dart
int yzxw
```

混洗掩码 “yzxw”。

用于 [shuffle] 和 [shuffleMix]。

### yzyx

```dart
int yzyx
```

混洗掩码 “yzyx”。

用于 [shuffle] 和 [shuffleMix]。

### yzyy

```dart
int yzyy
```

混洗掩码 “yzyy”。

用于 [shuffle] 和 [shuffleMix]。

### yzyz

```dart
int yzyz
```

混洗掩码 “yzyz”。

用于 [shuffle] 和 [shuffleMix]。

### yzyw

```dart
int yzyw
```

混洗掩码 “yzyw”。

用于 [shuffle] 和 [shuffleMix]。

### yzzx

```dart
int yzzx
```

混洗掩码 “yzzx”。

用于 [shuffle] 和 [shuffleMix]。

### yzzy

```dart
int yzzy
```

混洗掩码 “yzzy”。

用于 [shuffle] 和 [shuffleMix]。

### yzzz

```dart
int yzzz
```

混洗掩码 “yzzz”。

用于 [shuffle] 和 [shuffleMix]。

### yzzw

```dart
int yzzw
```

混洗掩码 “yzzw”。

用于 [shuffle] 和 [shuffleMix]。

### yzwx

```dart
int yzwx
```

混洗掩码 “yzwx”。

用于 [shuffle] 和 [shuffleMix]。

### yzwy

```dart
int yzwy
```

混洗掩码 “yzwy”。

用于 [shuffle] 和 [shuffleMix]。

### yzwz

```dart
int yzwz
```

混洗掩码 “yzwz”。

用于 [shuffle] 和 [shuffleMix]。

### yzww

```dart
int yzww
```

混洗掩码 “yzww”。

用于 [shuffle] 和 [shuffleMix]。

### ywxx

```dart
int ywxx
```

混洗掩码 “ywxx”。

用于 [shuffle] 和 [shuffleMix]。

### ywxy

```dart
int ywxy
```

混洗掩码 “ywxy”。

用于 [shuffle] 和 [shuffleMix]。

### ywxz

```dart
int ywxz
```

混洗掩码 “ywxz”。

用于 [shuffle] 和 [shuffleMix]。

### ywxw

```dart
int ywxw
```

混洗掩码 “ywxw”。

用于 [shuffle] 和 [shuffleMix]。

### ywyx

```dart
int ywyx
```

混洗掩码 “ywyx”。

用于 [shuffle] 和 [shuffleMix]。

### ywyy

```dart
int ywyy
```

混洗掩码 “ywyy”。

用于 [shuffle] 和 [shuffleMix]。

### ywyz

```dart
int ywyz
```

混洗掩码 “ywyz”。

用于 [shuffle] 和 [shuffleMix]。

### ywyw

```dart
int ywyw
```

混洗掩码 “ywyw”。

用于 [shuffle] 和 [shuffleMix]。

### ywzx

```dart
int ywzx
```

混洗掩码 “ywzx”。

用于 [shuffle] 和 [shuffleMix]。

### ywzy

```dart
int ywzy
```

混洗掩码 “ywzy”。

用于 [shuffle] 和 [shuffleMix]。

### ywzz

```dart
int ywzz
```

混洗掩码 “ywzz”。

用于 [shuffle] 和 [shuffleMix]。

### ywzw

```dart
int ywzw
```

混洗掩码 “ywzw”。

用于 [shuffle] 和 [shuffleMix]。

### ywwx

```dart
int ywwx
```

混洗掩码 “ywwx”。

用于 [shuffle] 和 [shuffleMix]。

### ywwy

```dart
int ywwy
```

混洗掩码 “ywwy”。

用于 [shuffle] 和 [shuffleMix]。

### ywwz

```dart
int ywwz
```

混洗掩码 “ywwz”。

用于 [shuffle] 和 [shuffleMix]。

### ywww

```dart
int ywww
```

混洗掩码 “ywww”。

用于 [shuffle] 和 [shuffleMix]。

### zxxx

```dart
int zxxx
```

混洗掩码 “zxxx”。

用于 [shuffle] 和 [shuffleMix]。

### zxxy

```dart
int zxxy
```

混洗掩码 “zxxy”。

用于 [shuffle] 和 [shuffleMix]。

### zxxz

```dart
int zxxz
```

混洗掩码 “zxxz”。

用于 [shuffle] 和 [shuffleMix]。

### zxxw

```dart
int zxxw
```

混洗掩码 “zxxw”。

用于 [shuffle] 和 [shuffleMix]。

### zxyx

```dart
int zxyx
```

混洗掩码 “zxyx”。

用于 [shuffle] 和 [shuffleMix]。

### zxyy

```dart
int zxyy
```

混洗掩码 “zxyy”。

用于 [shuffle] 和 [shuffleMix]。

### zxyz

```dart
int zxyz
```

混洗掩码 “zxyz”。

用于 [shuffle] 和 [shuffleMix]。

### zxyw

```dart
int zxyw
```

混洗掩码 “zxyw”。

用于 [shuffle] 和 [shuffleMix]。

### zxzx

```dart
int zxzx
```

混洗掩码 “zxzx”。

用于 [shuffle] 和 [shuffleMix]。

### zxzy

```dart
int zxzy
```

混洗掩码 “zxzy”。

用于 [shuffle] 和 [shuffleMix]。

### zxzz

```dart
int zxzz
```

混洗掩码 “zxzz”。

用于 [shuffle] 和 [shuffleMix]。

### zxzw

```dart
int zxzw
```

混洗掩码 “zxzw”。

用于 [shuffle] 和 [shuffleMix]。

### zxwx

```dart
int zxwx
```

混洗掩码 “zxwx”。

用于 [shuffle] 和 [shuffleMix]。

### zxwy

```dart
int zxwy
```

混洗掩码 “zxwy”。

用于 [shuffle] 和 [shuffleMix]。

### zxwz

```dart
int zxwz
```

混洗掩码 “zxwz”。

用于 [shuffle] 和 [shuffleMix]。

### zxww

```dart
int zxww
```

混洗掩码 “zxww”。

用于 [shuffle] 和 [shuffleMix]。

### zyxx

```dart
int zyxx
```

混洗掩码 “zyxx”。

用于 [shuffle] 和 [shuffleMix]。

### zyxy

```dart
int zyxy
```

混洗掩码 “zyxy”。

用于 [shuffle] 和 [shuffleMix]。

### zyxz

```dart
int zyxz
```

混洗掩码 “zyxz”。

用于 [shuffle] 和 [shuffleMix]。

### zyxw

```dart
int zyxw
```

混洗掩码 “zyxw”。

用于 [shuffle] 和 [shuffleMix]。

### zyyx

```dart
int zyyx
```

混洗掩码 “zyyx”。

用于 [shuffle] 和 [shuffleMix]。

### zyyy

```dart
int zyyy
```

混洗掩码 “zyyy”。

用于 [shuffle] 和 [shuffleMix]。

### zyyz

```dart
int zyyz
```

混洗掩码 “zyyz”。

用于 [shuffle] 和 [shuffleMix]。

### zyyw

```dart
int zyyw
```

混洗掩码 “zyyw”。

用于 [shuffle] 和 [shuffleMix]。

### zyzx

```dart
int zyzx
```

混洗掩码 “zyzx”。

用于 [shuffle] 和 [shuffleMix]。

### zyzy

```dart
int zyzy
```

混洗掩码 “zyzy”。

用于 [shuffle] 和 [shuffleMix]。

### zyzz

```dart
int zyzz
```

混洗掩码 “zyzz”。

用于 [shuffle] 和 [shuffleMix]。

### zyzw

```dart
int zyzw
```

混洗掩码 “zyzw”。

用于 [shuffle] 和 [shuffleMix]。

### zywx

```dart
int zywx
```

混洗掩码 “zywx”。

用于 [shuffle] 和 [shuffleMix]。

### zywy

```dart
int zywy
```

混洗掩码 “zywy”。

用于 [shuffle] 和 [shuffleMix]。

### zywz

```dart
int zywz
```

混洗掩码 “zywz”。

用于 [shuffle] 和 [shuffleMix]。

### zyww

```dart
int zyww
```

混洗掩码 “zyww”。

用于 [shuffle] 和 [shuffleMix]。

### zzxx

```dart
int zzxx
```

混洗掩码 “zzxx”。

用于 [shuffle] 和 [shuffleMix]。

### zzxy

```dart
int zzxy
```

混洗掩码 “zzxy”。

用于 [shuffle] 和 [shuffleMix]。

### zzxz

```dart
int zzxz
```

混洗掩码 “zzxz”。

用于 [shuffle] 和 [shuffleMix]。

### zzxw

```dart
int zzxw
```

混洗掩码 “zzxw”。

用于 [shuffle] 和 [shuffleMix]。

### zzyx

```dart
int zzyx
```

混洗掩码 “zzyx”。

用于 [shuffle] 和 [shuffleMix]。

### zzyy

```dart
int zzyy
```

混洗掩码 “zzyy”。

用于 [shuffle] 和 [shuffleMix]。

### zzyz

```dart
int zzyz
```

混洗掩码 “zzyz”。

用于 [shuffle] 和 [shuffleMix]。

### zzyw

```dart
int zzyw
```

混洗掩码 “zzyw”。

用于 [shuffle] 和 [shuffleMix]。

### zzzx

```dart
int zzzx
```

混洗掩码 “zzzx”。

用于 [shuffle] 和 [shuffleMix]。

### zzzy

```dart
int zzzy
```

混洗掩码 “zzzy”。

用于 [shuffle] 和 [shuffleMix]。

### zzzz

```dart
int zzzz
```

混洗掩码 “zzzz”。

用于 [shuffle] 和 [shuffleMix]。

### zzzw

```dart
int zzzw
```

混洗掩码 “zzzw”。

用于 [shuffle] 和 [shuffleMix]。

### zzwx

```dart
int zzwx
```

混洗掩码 “zzwx”。

用于 [shuffle] 和 [shuffleMix]。

### zzwy

```dart
int zzwy
```

混洗掩码 “zzwy”。

用于 [shuffle] 和 [shuffleMix]。

### zzwz

```dart
int zzwz
```

混洗掩码 “zzwz”。

用于 [shuffle] 和 [shuffleMix]。

### zzww

```dart
int zzww
```

混洗掩码 “zzww”。

用于 [shuffle] 和 [shuffleMix]。

### zwxx

```dart
int zwxx
```

混洗掩码 “zwxx”。

用于 [shuffle] 和 [shuffleMix]。

### zwxy

```dart
int zwxy
```

混洗掩码 “zwxy”。

用于 [shuffle] 和 [shuffleMix]。

### zwxz

```dart
int zwxz
```

混洗掩码 “zwxz”。

用于 [shuffle] 和 [shuffleMix]。

### zwxw

```dart
int zwxw
```

混洗掩码 “zwxw”。

用于 [shuffle] 和 [shuffleMix]。

### zwyx

```dart
int zwyx
```

混洗掩码 “zwyx”。

用于 [shuffle] 和 [shuffleMix]。

### zwyy

```dart
int zwyy
```

混洗掩码 “zwyy”。

用于 [shuffle] 和 [shuffleMix]。

### zwyz

```dart
int zwyz
```

混洗掩码 “zwyz”。

用于 [shuffle] 和 [shuffleMix]。

### zwyw

```dart
int zwyw
```

混洗掩码 “zwyw”。

用于 [shuffle] 和 [shuffleMix]。

### zwzx

```dart
int zwzx
```

混洗掩码 “zwzx”。

用于 [shuffle] 和 [shuffleMix]。

### zwzy

```dart
int zwzy
```

混洗掩码 “zwzy”。

用于 [shuffle] 和 [shuffleMix]。

### zwzz

```dart
int zwzz
```

混洗掩码 “zwzz”。

用于 [shuffle] 和 [shuffleMix]。

### zwzw

```dart
int zwzw
```

混洗掩码 “zwzw”。

用于 [shuffle] 和 [shuffleMix]。

### zwwx

```dart
int zwwx
```

混洗掩码 “zwwx”。

用于 [shuffle] 和 [shuffleMix]。

### zwwy

```dart
int zwwy
```

混洗掩码 “zwwy”。

用于 [shuffle] 和 [shuffleMix]。

### zwwz

```dart
int zwwz
```

混洗掩码 “zwwz”。

用于 [shuffle] 和 [shuffleMix]。

### zwww

```dart
int zwww
```

混洗掩码 “zwww”。

用于 [shuffle] 和 [shuffleMix]。

### wxxx

```dart
int wxxx
```

混洗掩码 “wxxx”。

用于 [shuffle] 和 [shuffleMix]。

### wxxy

```dart
int wxxy
```

混洗掩码 “wxxy”。

用于 [shuffle] 和 [shuffleMix]。

### wxxz

```dart
int wxxz
```

混洗掩码 “wxxz”。

用于 [shuffle] 和 [shuffleMix]。

### wxxw

```dart
int wxxw
```

混洗掩码 “wxxw”。

用于 [shuffle] 和 [shuffleMix]。

### wxyx

```dart
int wxyx
```

混洗掩码 “wxyx”。

用于 [shuffle] 和 [shuffleMix]。

### wxyy

```dart
int wxyy
```

混洗掩码 “wxyy”。

用于 [shuffle] 和 [shuffleMix]。

### wxyz

```dart
int wxyz
```

混洗掩码 “wxyz”。

用于 [shuffle] 和 [shuffleMix]。

### wxyw

```dart
int wxyw
```

混洗掩码 “wxyw”。

用于 [shuffle] 和 [shuffleMix]。

### wxzx

```dart
int wxzx
```

混洗掩码 “wxzx”。

用于 [shuffle] 和 [shuffleMix]。

### wxzy

```dart
int wxzy
```

混洗掩码 “wxzy”。

用于 [shuffle] 和 [shuffleMix]。

### wxzz

```dart
int wxzz
```

混洗掩码 “wxzz”。

用于 [shuffle] 和 [shuffleMix]。

### wxzw

```dart
int wxzw
```

混洗掩码 “wxzw”。

用于 [shuffle] 和 [shuffleMix]。

### wxwx

```dart
int wxwx
```

混洗掩码 “wxwx”。

用于 [shuffle] 和 [shuffleMix]。

### wxwy

```dart
int wxwy
```

混洗掩码 “wxwy”。

用于 [shuffle] 和 [shuffleMix]。

### wxwz

```dart
int wxwz
```

混洗掩码 “wxwz”。

用于 [shuffle] 和 [shuffleMix]。

### wxww

```dart
int wxww
```

混洗掩码 “wxww”。

用于 [shuffle] 和 [shuffleMix]。

### wyxx

```dart
int wyxx
```

混洗掩码 “wyxx”。

用于 [shuffle] 和 [shuffleMix]。

### wyxy

```dart
int wyxy
```

混洗掩码 “wyxy”。

用于 [shuffle] 和 [shuffleMix]。

### wyxz

```dart
int wyxz
```

混洗掩码 “wyxz”。

用于 [shuffle] 和 [shuffleMix]。

### wyxw

```dart
int wyxw
```

混洗掩码 “wyxw”。

用于 [shuffle] 和 [shuffleMix]。

### wyyx

```dart
int wyyx
```

混洗掩码 “wyyx”。

用于 [shuffle] 和 [shuffleMix]。

### wyyy

```dart
int wyyy
```

混洗掩码 “wyyy”。

用于 [shuffle] 和 [shuffleMix]。

### wyyz

```dart
int wyyz
```

混洗掩码 “wyyz”。

用于 [shuffle] 和 [shuffleMix]。

### wyyw

```dart
int wyyw
```

混洗掩码 “wyyw”。

用于 [shuffle] 和 [shuffleMix]。

### wyzx

```dart
int wyzx
```

混洗掩码 “wyzx”。

用于 [shuffle] 和 [shuffleMix]。

### wyzy

```dart
int wyzy
```

混洗掩码 “wyzy”。

用于 [shuffle] 和 [shuffleMix]。

### wyzz

```dart
int wyzz
```

混洗掩码 “wyzz”。

用于 [shuffle] 和 [shuffleMix]。

### wyzw

```dart
int wyzw
```

混洗掩码 “wyzw”。

用于 [shuffle] 和 [shuffleMix]。

### wywx

```dart
int wywx
```

混洗掩码 “wywx”。

用于 [shuffle] 和 [shuffleMix]。

### wywy

```dart
int wywy
```

混洗掩码 “wywy”。

用于 [shuffle] 和 [shuffleMix]。

### wywz

```dart
int wywz
```

混洗掩码 “wywz”。

用于 [shuffle] 和 [shuffleMix]。

### wyww

```dart
int wyww
```

混洗掩码 “wyww”。

用于 [shuffle] 和 [shuffleMix]。

### wzxx

```dart
int wzxx
```

混洗掩码 “wzxx”。

用于 [shuffle] 和 [shuffleMix]。

### wzxy

```dart
int wzxy
```

混洗掩码 “wzxy”。

用于 [shuffle] 和 [shuffleMix]。

### wzxz

```dart
int wzxz
```

混洗掩码 “wzxz”。

用于 [shuffle] 和 [shuffleMix]。

### wzxw

```dart
int wzxw
```

混洗掩码 “wzxw”。

用于 [shuffle] 和 [shuffleMix]。

### wzyx

```dart
int wzyx
```

混洗掩码 “wzyx”。

用于 [shuffle] 和 [shuffleMix]。

### wzyy

```dart
int wzyy
```

混洗掩码 “wzyy”。

用于 [shuffle] 和 [shuffleMix]。

### wzyz

```dart
int wzyz
```

混洗掩码 “wzyz”。

用于 [shuffle] 和 [shuffleMix]。

### wzyw

```dart
int wzyw
```

混洗掩码 “wzyw”。

用于 [shuffle] 和 [shuffleMix]。

### wzzx

```dart
int wzzx
```

混洗掩码 “wzzx”。

用于 [shuffle] 和 [shuffleMix]。

### wzzy

```dart
int wzzy
```

混洗掩码 “wzzy”。

用于 [shuffle] 和 [shuffleMix]。

### wzzz

```dart
int wzzz
```

混洗掩码 “wzzz”。

用于 [shuffle] 和 [shuffleMix]。

### wzzw

```dart
int wzzw
```

混洗掩码 “wzzw”。

用于 [shuffle] 和 [shuffleMix]。

### wzwx

```dart
int wzwx
```

混洗掩码 “wzwx”。

用于 [shuffle] 和 [shuffleMix]。

### wzwy

```dart
int wzwy
```

混洗掩码 “wzwy”。

用于 [shuffle] 和 [shuffleMix]。

### wzwz

```dart
int wzwz
```

混洗掩码 “wzwz”。

用于 [shuffle] 和 [shuffleMix]。

### wzww

```dart
int wzww
```

混洗掩码 “wzww”。

用于 [shuffle] 和 [shuffleMix]。

### wwxx

```dart
int wwxx
```

混洗掩码 “wwxx”。

用于 [shuffle] 和 [shuffleMix]。

### wwxy

```dart
int wwxy
```

混洗掩码 “wwxy”。

用于 [shuffle] 和 [shuffleMix]。

### wwxz

```dart
int wwxz
```

混洗掩码 “wwxz”。

用于 [shuffle] 和 [shuffleMix]。

### wwxw

```dart
int wwxw
```

混洗掩码 “wwxw”。

用于 [shuffle] 和 [shuffleMix]。

### wwyx

```dart
int wwyx
```

混洗掩码 “wwyx”。

用于 [shuffle] 和 [shuffleMix]。

### wwyy

```dart
int wwyy
```

混洗掩码 “wwyy”。

用于 [shuffle] 和 [shuffleMix]。

### wwyz

```dart
int wwyz
```

混洗掩码 “wwyz”。

用于 [shuffle] 和 [shuffleMix]。

### wwyw

```dart
int wwyw
```

混洗掩码 “wwyw”。

用于 [shuffle] 和 [shuffleMix]。

### wwzx

```dart
int wwzx
```

混洗掩码 “wwzx”。

用于 [shuffle] 和 [shuffleMix]。

### wwzy

```dart
int wwzy
```

混洗掩码 “wwzy”。

用于 [shuffle] 和 [shuffleMix]。

### wwzz

```dart
int wwzz
```

混洗掩码 “wwzz”。

用于 [shuffle] 和 [shuffleMix]。

### wwzw

```dart
int wwzw
```

混洗掩码 “wwzw”。

用于 [shuffle] 和 [shuffleMix]。

### wwwx

```dart
int wwwx
```

混洗掩码 “wwwx”。

用于 [shuffle] 和 [shuffleMix]。

### wwwy

```dart
int wwwy
```

混洗掩码 “wwwy”。

用于 [shuffle] 和 [shuffleMix]。

### wwwz

```dart
int wwwz
```

混洗掩码 “wwwz”。

用于 [shuffle] 和 [shuffleMix]。

### wwww

```dart
int wwww
```

混洗掩码 “wwww”。

用于 [shuffle] 和 [shuffleMix]。

### shuffle()

```dart
Float32x4 shuffle(int mask)
```

根据 [mask] 对通道值进行混洗。

[mask] 必须是从 [xxxx] 到 [wwww] 的 256 个混洗掩码之一。

创建一个新的 [Float32x4]，其通道值取自此值的各个通道：结果的 [x] 通道取自混洗掩码名称中第一个字母所指示的通道，[y] 通道取自第二个字母所指示的通道，[z] 通道取自第三个字母所指示的通道，[w] 通道取自第四个字母所指示的通道。

例如，混洗掩码 [wxyz] 会创建一个新的 `Float32x4`，其 [x] 通道是此值的 [w] 通道，因为混洗掩码名称 `wxyz` 的第一个字母是 “w”。随后，结果的 `y`、`z` 和 `w` 通道分别是此值的 `x`、`y` 和 `z` 通道的值。

[xyzw] “恒等混洗” 掩码所得结果与原始值的通道完全相同。

有些掩码会保留所有通道的值，但可能对其进行置换；另一些掩码则会复制某些通道的值，同时丢弃其他通道的值。

例如，执行 `v1.shuffle(yyyy)` 等价于 `Float32x4.splat(v1.y)`。

### shuffleMix()

```dart
Float32x4 shuffleMix(Float32x4 other, int mask)
```

使用 [mask] 从两个 [Float32x4] 值中选取通道进行混合。

创建一个新的 [Float32x4]，其中 [x] 和 [y] 通道选自此值，具体由 [mask] 名称的前两个字母指定；[z] 和 [w] 通道选自 [other]，具体由 `mask` 名称的后两个字母指定。

例如，`v1.shuffleMix(v2, Float32x4.xyzw)` 等价于 `Float32x4(v1.x, v1.y, v2.z, v2.w)`。

若 [other] 与此 `Float32x4` 是同一个值，则此函数与 [shuffle] 效果相同。也就是说，执行 `v1.shuffleMix(v1, mask)` 等价于 `v1.shuffle(mask)`。

### withX()

```dart
Float32x4 withX(double x)
```

此值，但将 [Float32x4.x] 通道的值设置为 [x]。

返回一个新的 [Float32x4]，其 [y]、[z] 和 [w] 通道的值与此值相同，[Float32x4.x] 通道的值为 [x] 转换后得到的 32 位浮点数。

### withY()

```dart
Float32x4 withY(double y)
```

此值，但将 [Float32x4.y] 通道的值设置为 [y]。

返回一个新的 [Float32x4]，其 [x]、[z] 和 [w] 通道的值与此值相同，[Float32x4.y] 通道的值为 [y] 转换后得到的 32 位浮点数。

### withZ()

```dart
Float32x4 withZ(double z)
```

此值，但将 [Float32x4.z] 通道的值设置为 [z]。

返回一个新的 [Float32x4]，其 [x]、[y] 和 [w] 通道的值与此值相同，[Float32x4.z] 通道的值为 [z] 转换后得到的 32 位浮点数。

### withW()

```dart
Float32x4 withW(double w)
```

此值，但将 [Float32x4.w] 通道的值设置为 [w]。

返回一个新的 [Float32x4]，其 [x]、[y] 和 [z] 通道的值与此值相同，[Float32x4.w] 通道的值为 [w] 转换后得到的 32 位浮点数。

### min()

```dart
Float32x4 min(Float32x4 other)
```

逐通道取最小值。

对每个通道，选取此值与 [other] 对应通道值中较小的一个。

若两个通道值中有一个较小，则结果为较小的那个值。若任一通道包含 NaN 值，或者两个值分别为 -0.0 和 0.0（此时两者互不大于或小于对方），则结果未指定。不同平台在这些情况下可能给出不同的结果，但结果始终是两个通道值之一。

返回每个通道的运算结果。

### max()

```dart
Float32x4 max(Float32x4 other)
```

逐通道取最大值。

对每个通道，选取此值与 [other] 对应通道值中较大的一个。

若两个通道值中有一个较大，则结果为较大的那个值。若任一通道包含 NaN 值，或者两个值分别为 -0.0 和 0.0（此时两者互不大于或小于对方），则结果未指定。不同平台在这些情况下可能给出不同的结果，但结果始终是两个通道值之一。

返回每个通道的运算结果。

### sqrt()

```dart
Float32x4 sqrt()
```

逐通道计算平方根。

对每个通道计算该通道值的 32 位浮点数平方根。

若原始值小于零或为 NaN 值，则该通道的结果为 NaN 值。对于负零 -0.0，结果仍为该值本身。对于正无穷大，结果为正无穷大。否则，结果为一个正值，是原始值数学平方根的近似值。

返回每个通道的运算结果。

### reciprocal()

```dart
Float32x4 reciprocal()
```

逐通道取倒数。

对每个通道计算 1.0 除以该通道值所得的结果。

若该值为 NaN 值，则结果也是 NaN 值。若该值为无穷大，则结果为符号相同的零值。若该值为零，则结果为符号相同的无穷大。否则，结果是将 1 除以该（有限、非零）通道值所得数学结果的近似值。

返回每个通道的运算结果。

### reciprocalSqrt()

```dart
Float32x4 reciprocalSqrt()
```

逐通道计算平方根倒数的近似值。

近似于先执行 [reciprocal] 再执行 [sqrt]，或先执行 [sqrt] 再执行 [reciprocal] 所得到的结果，但由于直接计算结果、无需生成中间结果，并且可能以降低的精度进行运算，因此可能更加精确和/或高效。

由于近似方法和精度上的差异，以及对于某些值而言 [sqrt] 与 [reciprocal] 的先后顺序会影响结果，不同平台间的结果可能有所不同。这一点尤其适用于 `-0.0`：`sqrt(-0.0)` 被定义为 -0.0，对其取 `reciprocal` 得到 -Infinity；而按相反顺序计算，则是对 -Infinity 计算 `sqrt`，结果为 NaN。
