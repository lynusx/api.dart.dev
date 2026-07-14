# Int32x4

```dart
abstract final class Int32x4 {}
```

Int32x4 及其操作。

Int32x4 在“通道（lane）”中存储 4 个 32 位位掩码。这些通道分别为 "x"、"y"、"z" 和 "w"。

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

按位或运算符。

### operator &()

```dart
Int32x4 operator &(Int32x4 other)
```

按位与运算符。

### operator ^()

```dart
Int32x4 operator ^(Int32x4 other)
```

按位异或运算符。

### operator +()

```dart
Int32x4 operator +(Int32x4 other)
```

加法运算符。

### operator -()

```dart
Int32x4 operator -(Int32x4 other)
```

减法运算符。

### x

```dart
int get x
```

从 x 通道提取 32 位掩码。

### y

```dart
int get y
```

从 y 通道提取 32 位掩码。

### z

```dart
int get z
```

从 z 通道提取 32 位掩码。

### w

```dart
int get w
```

从 w 通道提取 32 位掩码。

### signMask

```dart
int get signMask
```

从每个通道提取最高位，并将它们放入前 4 位中返回。"x" 通道为第 0 位。"y" 通道为第 1 位。"z" 通道为第 2 位。"w" 通道为第 3 位。

### xxxx

```dart
int xxxx
```

传递给 [shuffle] 或 [shuffleMix] 的掩码。

### shuffle()

```dart
Int32x4 shuffle(int mask)
```

对通道值进行重排（shuffle）。[mask] 必须是 256 个重排常量之一。

### shuffleMix()

```dart
Int32x4 shuffleMix(Int32x4 other, int mask)
```

对此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 和 [other] 中的通道值进行重排。返回的 Int32x4 的 XY 通道来自此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)，ZW 通道来自 [other]。使用与 [shuffle] 相同的 [mask]。

### withX()

```dart
Int32x4 withX(int x)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 x 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withY()

```dart
Int32x4 withY(int y)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 y 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withZ()

```dart
Int32x4 withZ(int z)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 z 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withW()

```dart
Int32x4 withW(int w)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 w 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### flagX

```dart
bool get flagX
```

提取的 x 值。0 返回 false，其他任何值返回 true。

### flagY

```dart
bool get flagY
```

提取的 y 值。0 返回 false，其他任何值返回 true。

### flagZ

```dart
bool get flagZ
```

提取的 z 值。0 返回 false，其他任何值返回 true。

### flagW

```dart
bool get flagW
```

提取的 w 值。0 返回 false，其他任何值返回 true。

### withFlagX()

```dart
Int32x4 withFlagX(bool x)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 x 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withFlagY()

```dart
Int32x4 withFlagY(bool y)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 y 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withFlagZ()

```dart
Int32x4 withFlagZ(bool z)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 z 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### withFlagW()

```dart
Int32x4 withFlagW(bool w)
```

返回一个从此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 复制而来、具有新 w 值的新 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4)。

### select()

```dart
Float32x4 select(Float32x4 trueValue, Float32x4 falseValue)
```

根据此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 位掩码合并 [trueValue] 和 [falseValue]：当此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 中的位为开启状态时，选择 [trueValue] 中的位；当此 [Int32x4](https://www.yuque.com/thyname/dart.typed_data/int32x4) 中的位为关闭状态时，选择 [falseValue] 中的位。
