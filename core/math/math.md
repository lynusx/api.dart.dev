## 常量

### e

```dart
const double e = 2.718281828459045;
```

自然对数的底数。

通常写作 "e"。

### ln10

```dart
const double ln10 = 2.302585092994046;
```

10 的自然对数。

10 的自然对数是满足 `pow(E, LN10) == 10` 的数值。该值并非精确值，而是最接近该数学精确值的可表示 double 值。

### ln2

```dart
const double ln2 = 0.6931471805599453;
```

2 的自然对数。

2 的自然对数是满足 `pow(E, LN2) == 2` 的数值。该值并非精确值，而是最接近该数学精确值的可表示 double 值。

### log2e

```dart
const double log2e = 1.4426950408889634;
```

[e] 的以 2 为底的对数。

### log10e

```dart
const double log10e = 0.4342944819032518;
```

[e] 的以 10 为底的对数。

### pi

```dart
const double pi = 3.1415926535897932;
```

PI 常量。

### sqrt1_2

```dart
const double sqrt1_2 = 0.7071067811865476;
```

1/2 的平方根。

### sqrt2

```dart
const double sqrt2 = 1.4142135623730951;
```

2 的平方根。

## 函数

### min()

```dart
T min<T extends num>(T a, T b)
```

返回两个数值中较小的一个。

如果任一参数为 NaN，则返回 NaN。`-0.0` 与 `0.0` 中较小的值为 `-0.0`。如果两个参数在其他方面相等（包括数学值相同的 int 和 double），则返回哪一个参数是不确定的。

### max()

```dart
T max<T extends num>(T a, T b)
```

返回两个数值中较大的一个。

如果任一参数为 NaN，则返回 NaN。`-0.0` 与 `0.0` 中较大的值为 `0.0`。如果两个参数在其他方面相等（包括数学值相同的 int 和 double），则返回哪一个参数是不确定的。

### atan2()

```dart
double atan2(num a, num b)
```

[atan] 的一种变体。

将两个参数都转换为 [double](https://www.yuque.com/thyname/dart.core/double)。

返回正 x 轴与向量 ([b],[a]) 之间的夹角（以弧度表示）。结果范围为 -PI..PI。

如果 [b] 为正数，该结果与 `atan(a/b)` 相同。

当 [a] 为负数（包括 [a] 为 double 类型的 -0.0 时）时，结果为负。

如果 [a] 等于零，则即使 [b] 也等于零，向量 ([b],[a]) 也被视为与 x 轴平行。[b] 的符号决定了该向量沿 x 轴的方向。

如果任一参数为 NaN，则返回 NaN。

### pow()

```dart
num pow(num x, num exponent)
```

返回 [x] 的 [exponent] 次幂。

如果 [x] 是 [int](https://www.yuque.com/thyname/dart.core/int) 且 [exponent] 是非负 [int](https://www.yuque.com/thyname/dart.core/int)，则结果为 [int](https://www.yuque.com/thyname/dart.core/int)；否则两个参数都会先转换为 double，结果为 [double](https://www.yuque.com/thyname/dart.core/double)。

对于整数，幂运算的结果始终等于 `x` 的 `exponent` 次方的数学结果，仅受可用内存限制。

对于 double，`pow(x, y)` 按如下方式处理边界情况：

- 如果 `y` 为零（0.0 或 -0.0），结果始终为 1.0。
- 如果 `x` 为 1.0，结果始终为 1.0。
- 否则，如果 `x` 或 `y` 为 NaN，则结果为 NaN。
- 如果 `x` 为负数（但不是 -0.0）且 `y` 是有限的非整数，则结果为 NaN。
- 如果 `x` 为 Infinity 且 `y` 为负数，结果为 0.0。
- 如果 `x` 为 Infinity 且 `y` 为正数，结果为 Infinity。
- 如果 `x` 为 0.0 且 `y` 为负数，结果为 Infinity。
- 如果 `x` 为 0.0 且 `y` 为正数，结果为 0.0。
- 如果 `x` 为 -Infinity 或 -0.0，且 `y` 为奇数整数，则结果为 `-pow(-x ,y)`。
- 如果 `x` 为 -Infinity 或 -0.0，且 `y` 不是奇数整数，则结果与 `pow(-x , y)` 相同。
- 如果 `y` 为 Infinity 且 `x` 的绝对值小于 1，结果为 0.0。
- 如果 `y` 为 Infinity 且 `x` 为 -1，结果为 1.0。
- 如果 `y` 为 Infinity 且 `x` 的绝对值大于 1，结果为 Infinity。
- 如果 `y` 为 -Infinity，结果为 `1/pow(x, Infinity)`。

这与 IEEE 754-2008 标准中定义的 `pow` 函数一致。

请注意，结果可能会溢出。如果整数用 64 位表示，整数结果可能被截断，而 double 结果可能溢出为正或负 [double.infinity]。

### sin()

```dart
double sin(num radians)
```

将 [radians] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回该值的正弦值。

如果 [radians] 不是有限数值，结果为 NaN。

### cos()

```dart
double cos(num radians)
```

将 [radians] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回该值的余弦值。

如果 [radians] 不是有限数值，结果为 NaN。

### tan()

```dart
double tan(num radians)
```

将 [radians] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回该值的正切值。

正切函数等价于 `sin(radians)/cos(radians)`，当 `cos(radians)` 等于零时，结果可能为无穷大（正或负）。如果 [radians] 不是有限数值，结果为 NaN。

### acos()

```dart
double acos(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回其反余弦值（以弧度表示）。

返回值范围为 0..PI；如果 [x] 超出 -1..1 的范围，则返回 NaN。

### asin()

```dart
double asin(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回其反正弦值（以弧度表示）。

返回值范围为 -PI/2..PI/2；如果 [x] 超出 -1..1 的范围，则返回 NaN。

### atan()

```dart
double atan(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回其反正切值（以弧度表示）。

返回值范围为 -PI/2..PI/2；如果 [x] 为 NaN，则返回 NaN。

### sqrt()

```dart
double sqrt(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回该值的正平方根。

如果 [x] 为 -0.0，返回 -0.0；如果 [x] 为其他负数或 NaN，返回 NaN。

```dart
var result = sqrt(9.3);
print(result); // 3.0495901363953815
result = sqrt(2);
print(result); // 1.4142135623730951
result = sqrt(0);
print(result); // 0.0
result = sqrt(-2.2);
print(result); // NaN
```

### exp()

```dart
double exp(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回自然常数 [e] 的 [x] 次幂。

如果 [x] 为 NaN，结果为 NaN。

### log()

```dart
double log(num x)
```

将 [x] 转换为 [double](https://www.yuque.com/thyname/dart.core/double) 并返回该值的自然对数。

如果 [x] 等于零，返回负无穷；如果 [x] 为 NaN 或小于零，返回 NaN。
