# double

```dart
abstract final class double extends num {}
```

双精度浮点数。

Dart 双精度浮点数的表示，包含 double 特定的常量和操作，以及从 [num] 继承的操作的特化实现。Dart 的 double 是符合 IEEE 754 标准的 64 位浮点数。

[double] 类型具有传染性。对 [double] 的操作返回 [double] 结果。

尝试扩展或实现 double 是编译时错误。

**另请参阅：**

- [num]，[double] 的父类。
- [内置数字类型](https://dart.dev/language/built-in-types#numbers)
- [数字表示](https://dart.dev/resources/language/number-representation)

## 静态属性

### nan

```dart
static const double nan = 0.0 / 0.0;
```

### infinity

```dart
static const double infinity = 1.0 / 0.0;
```

### negativeInfinity

```dart
static const double negativeInfinity = -infinity;
```

### minPositive

```dart
static const double minPositive = 5e-324;
```

### maxFinite

```dart
static const double maxFinite = 1.7976931348623157e+308;
```

## 静态方法

### parse()

```dart
double parse(String source)
```

将 [source] 解析为 double 字面量并返回其值。

接受一个可选的符号（`+` 或 `-`），后跟字符串 "Infinity"、字符串 "NaN"，或一个浮点数表示形式。浮点数表示形式由尾数和可选的指数部分组成。尾数是一个小数点（`.`）后跟一串（十进制）数字，或者是一串数字后可选地跟一个小数点和更多数字。（可选的）指数部分由字符 "e" 或 "E"、一个可选的符号以及一位或多位数字组成。[source] 不能为 `null`。

忽略前导和尾随空白字符。

如果 [source] 字符串不是有效的 double 字面量，将抛出 [FormatException]。

与其抛出异常后立即捕获 [FormatException]，不如使用 [tryParse] 来处理可能的解析错误。

接受的字符串示例：

```
"3.14"
"  3.14 \xA0"
"0."
".0"
"-1.e3"
"1234E+7"
"+.12e-9"
"-NaN"
```

### tryParse()

```dart
double? tryParse(String source)
```

将 [source] 解析为 double 字面量并返回其值。

与 [parse] 类似，但此函数在输入无效时返回 `null` 而不是抛出异常。

示例：

```dart
var value = double.tryParse('3.14'); // 3.14
value = double.tryParse('  3.14 \xA0'); // 3.14
value = double.tryParse('0.'); // 0.0
value = double.tryParse('.0'); // 0.0
value = double.tryParse('-1.e3'); // -1000.0
value = double.tryParse('1234E+7'); // 12340000000.0
value = double.tryParse('+.12e-9'); // 1.2e-10
value = double.tryParse('-NaN'); // NaN
value = double.tryParse('0xFF'); // null
value = double.tryParse(double.infinity.toString()); // Infinity
```

## 属性

### sign

```dart
double get sign
```

该 double 数值的符号。

如果值小于零，返回 -1.0；如果值大于零，返回 +1.0；如果值为 -0.0、0.0 或 NaN，则返回该值本身。

## 方法

### remainder()

```dart
double remainder(num other)
```

`this` 除以 `other` 的截断除法余数。

此操作的结果 `r` 满足：`this == (this ~/ other) * other + r`。因此，余数 `r` 与被除数 `this` 的符号相同。

如 [int.remainder](https://api.dart.dev/dart-core/num/remainder.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。

示例：

```dart
print(5.remainder(3)); // 2
print(-5.remainder(3)); // -2
print(5.remainder(-3)); // 2
print(-5.remainder(-3)); // -2
```

### abs()

```dart
double abs()
```

此数字的绝对值。

如果值为非负数，绝对值即为该值本身；如果值为负数，则绝对值为 `-value`。

整数溢出可能导致 `-value` 的结果仍为负数。

```dart
print((2).abs()); // 2
print((-2.5).abs()); // 2.5
```

### round()

```dart
int round()
```

返回最接近此数字的整数。

当没有唯一最接近的整数时，向远离零的方向舍入：`(3.5).round() == 4`，`(-3.5).round() == -4`。

如果此数字不是有限数（NaN 或无穷大），将抛出 [UnsupportedError]。

```dart
print(3.0.round()); // 3
print(3.25.round()); // 3
print(3.5.round()); // 4
print(3.75.round()); // 4
print((-3.5).round()); // -4
```

### floor()

```dart
int floor()
```

返回不大于此数字的最大整数。

将数字向负无穷方向舍入。

如果此数字不是有限数（NaN 或无穷大），将抛出 [UnsupportedError]。

```dart
print(1.99999.floor()); // 1
print(2.0.floor()); // 2
print(2.99999.floor()); // 2
print((-1.99999).floor()); // -2
print((-2.0).floor()); // -2
print((-2.00001).floor()); // -3
```

### ceil()

```dart
int ceil()
```

返回不小于此数字的最小整数。

将数字向正无穷方向舍入。

如果此数字不是有限数（NaN 或无穷大），将抛出 [UnsupportedError]。

```dart
print(1.99999.ceil()); // 2
print(2.0.ceil()); // 2
print(2.00001.ceil()); // 3
print((-1.99999).ceil()); // -1
print((-2.0).ceil()); // -2
print((-2.00001).ceil()); // -2
```

### truncate()

```dart
int truncate()
```

返回通过舍弃此数字的小数部分得到的整数。

将数字向零方向舍入。

如果此数字不是有限数（NaN 或无穷大），将抛出 [UnsupportedError]。

```dart
print(2.00001.truncate()); // 2
print(1.99999.truncate()); // 1
print(0.5.truncate()); // 0
print((-0.5).truncate()); // 0
print((-1.5).truncate()); // -1
print((-2.5).truncate()); // -2
```

### roundToDouble()

```dart
double roundToDouble()
```

返回最接近 `this` 的整数值 double。

当没有唯一最接近的整数时，向远离零的方向舍入：`(3.5).roundToDouble() == 4`，`(-3.5).roundToDouble() == -4`。

如果此值已经是整数值的 double（包括 `-0.0`），或者不是有限值，则原样返回该值。

在舍入时，`-0.0` 被视为低于 `0.0`，因此 `-0.0` 被认为比 `0.0` 更接近负数。这意味着对于范围 `-0.5 < d < 0.0` 内的值 `d`，结果为 `-0.0`。

```dart
print(3.0.roundToDouble()); // 3.0
print(3.25.roundToDouble()); // 3.0
print(3.5.roundToDouble()); // 4.0
print(3.75.roundToDouble()); // 4.0
print((-3.5).roundToDouble()); // -4.0
```

### floorToDouble()

```dart
double floorToDouble()
```

返回不大于 `this` 的最大整数值 double。

如果此值已经是整数值的 double（包括 `-0.0`），或者不是有限值，则原样返回该值。

在舍入时，`-0.0` 被视为低于 `0.0`。范围 `0.0 < d < 1.0` 内的数字 `d` 将返回 `0.0`。

```dart
print(1.99999.floorToDouble()); // 1.0
print(2.0.floorToDouble()); // 2.0
print(2.99999.floorToDouble()); // 2.0
print((-1.99999).floorToDouble()); // -2.0
print((-2.0).floorToDouble()); // -2.0
print((-2.00001).floorToDouble()); // -3.0
```

### ceilToDouble()

```dart
double ceilToDouble()
```

返回不小于 `this` 的最小整数值 double。

如果此值已经是整数值的 double（包括 `-0.0`），或者不是有限值，则原样返回该值。

在舍入时，`-0.0` 被视为低于 `0.0`。范围 `-1.0 < d < 0.0` 内的数字 `d` 将返回 `-0.0`。

```dart
print(1.99999.ceilToDouble()); // 2.0
print(2.0.ceilToDouble()); // 2.0
print(2.00001.ceilToDouble()); // 3.0
print((-1.99999).ceilToDouble()); // -1.0
print((-2.0).ceilToDouble()); // -2.0
print((-2.00001).ceilToDouble()); // -2.0
```

### truncateToDouble()

```dart
double truncateToDouble()
```

返回通过舍弃 `this` 的小数位得到的整数值 double。

如果此值已经是整数值的 double（包括 `-0.0`），或者不是有限值，则原样返回该值。

在舍入时，`-0.0` 被视为低于 `0.0`。范围 `-1.0 < d < 0.0` 内的数字 `d` 将返回 `-0.0`，范围 `0.0 < d < 1.0` 内的数字将返回 `0.0`。

```dart
print(2.5.truncateToDouble()); // 2.0
print(2.00001.truncateToDouble()); // 2.0
print(1.99999.truncateToDouble()); // 1.0
print(0.5.truncateToDouble()); // 0.0
print((-0.5).truncateToDouble()); // -0.0
print((-1.5).truncateToDouble()); // -1.0
print((-2.5).truncateToDouble()); // -2.0
```

### toString()

```dart
String toString()
```

提供此 [double] 值的表示形式。

该表示形式是一个数字字面量，使得最接近该字面量数学值的 double 值就是此 [double]。

对于 Not-a-Number 值返回 "NaN"。对于正无穷和负无穷分别返回 "Infinity" 和 "-Infinity"。对于负零返回 "-0.0"。

对于所有 double 值 `d`，将其转换为字符串后再解析回来会得到相同的值：`d == double.parse(d.toString())`（NaN 除外）。

## 运算符

### operator +

```dart
double operator +(num other)
```

将 `other` 加到此数字上。

如 [int.+](https://api.dart.dev/dart-core/num/operator_plus.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。

### operator -

```dart
double operator -(num other)
```

从此数字中减去 `other`。

如 [int.-](https://api.dart.dev/dart-core/num/operator_minus.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。

### operator \*

```dart
double operator *(num other)
```

将此数字乘以 `other`。

如 [int.\*](https://api.dart.dev/dart-core/num/operator_multiply.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。

### operator %

```dart
double operator %(num other)
```

此数字除以 `other` 的欧几里得取模运算。

返回欧几里得除法的余数。两个整数 `a` 和 `b` 的欧几里得除法得到两个整数 `q` 和 `r`，满足 `a == b * q + r` 且 `0 <= r < b.abs()`。

欧几里得除法仅对整数定义，但可以很容易地扩展到 double。在这种情况下，`q` 仍为整数，但 `r` 可能是仍满足 `0 <= r < |b|` 的非整数值。

返回值 `r` 的符号始终为正。

有关截断除法的余数，请参见 [remainder](https://api.dart.dev/dart-core/double/remainder.html)。

如 [int.%](https://api.dart.dev/dart-core/num/operator_modulo.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。

示例：

```dart
print(5 % 3); // 2
print(-5 % 3); // 1
print(5 % -3); // 2
print(-5 % -3); // 1
```

### operator /

```dart
double operator /(num other)
```

将此数字除以 `other`。

### operator ~/

```dart
int operator ~/(num other)
```

截断除法运算符。

对此数字除以 `other` 执行截断除法。截断除法是指将带小数的结果通过向零方向舍入转换为整数的除法。

如果两个操作数都是 [int](https://api.dart.dev/dart-core/int-class.html)，则 `other` 不能为零。此时 `a ~/ b` 对应于 `a.remainder(b)`，满足 `a == (a ~/ b) * b + a.remainder(b)`。

如果任一操作数为 [double](https://api.dart.dev/dart-core/double-class.html)，则在执行除法之前会将另一个操作数转换为 double，并对结果进行截断。此时 `a ~/ b` 等价于 `(a / b).truncate()`。这意味着 double 除法的中间结果必须是有限整数（不能是无穷大或 [double.nan](https://api.dart.dev/dart-core/double/nan-constant.html)）。

### operator unary-

```dart
double operator -()
```

从此数字中减去 `other`。

如 [int.-](https://api.dart.dev/dart-core/num/operator_minus.html) 所述，如果此数字和 `other` 都是整数，则结果为 [int](https://api.dart.dev/dart-core/int-class.html)，否则结果为 [double](https://api.dart.dev/dart-core/double-class.html)。
