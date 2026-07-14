# Float64x2

```dart
abstract final class Float64x2 {}
```

Float64x2 不可变值类型及其操作。

Float64x2 在“通道（lane）”中存储 2 个 64 位浮点值。这些通道分别为 "x" 和 "y"。

类尝试继承或实现 `Float64x2` 是编译时错误。

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

使用 [v] 中的 "x" 和 "y" 通道。

### operator +()

```dart
Float64x2 operator +(Float64x2 other)
```

加法运算符。

### operator -()

```dart
Float64x2 operator -()
```

取负运算符。

### operator -()

```dart
Float64x2 operator -(Float64x2 other)
```

减法运算符。

### operator \*()

```dart
Float64x2 operator *(Float64x2 other)
```

乘法运算符。

### operator /()

```dart
Float64x2 operator /(Float64x2 other)
```

除法运算符。

### scale()

```dart
Float64x2 scale(double s)
```

返回此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 的一个副本，其中每个通道都按 [s] 缩放。等价于 this \* new Float64x2.splat(s)

### abs()

```dart
Float64x2 abs()
```

此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 按通道计算的绝对值。

### clamp()

```dart
Float64x2 clamp(Float64x2 lowerLimit, Float64x2 upperLimit)
```

按通道将此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 限制在 [lowerLimit]-[upperLimit] 范围内。

### x

```dart
double get x
```

提取的 x 值。

### y

```dart
double get y
```

提取的 y 值。

### signMask

```dart
int get signMask
```

从每个通道提取符号位，并将它们放入前 2 位中返回。"x" 通道为第 0 位。"y" 通道为第 1 位。

### withX()

```dart
Float64x2 withX(double x)
```

返回一个从此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 复制而来、具有新 x 值的新 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2)。

### withY()

```dart
Float64x2 withY(double y)
```

返回一个从此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 复制而来、具有新 y 值的新 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2)。

### min()

```dart
Float64x2 min(Float64x2 other)
```

此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 与 [other] 中按通道的最小值。

### max()

```dart
Float64x2 max(Float64x2 other)
```

此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 与 [other] 中按通道的最大值。

### sqrt()

```dart
Float64x2 sqrt()
```

此 [Float64x2](https://www.yuque.com/thyname/dart.typed_data/float64x2) 按通道计算的平方根。
