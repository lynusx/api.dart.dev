# num

```dart
sealed class num implements Comparable<num> {}
```

整数或浮点数。

除 [int] 和 [double] 之外的任何类型继承或实现 `num` 都是编译时错误。

**另请参阅：**

- [int]：整数。
- [double]：双精度浮点数。
- [内置数值类型](https://dart.dev/language/built-in-types#numbers)
- [数值表示](https://dart.dev/resources/language/number-representation)

## 静态方法

### parse()

```dart
num parse( String input, [ @deprecated num onError( String input )? ])
```

将包含数字字面量的字符串解析为数字。

该方法首先尝试将 [input] 作为整数读取（类似于不带进制的 [int.parse]）。如果失败，则尝试将 [input] 解析为双精度浮点数（类似于 [double.parse]）。如果仍然失败，则抛出 [FormatException]。

与其抛出并立即捕获 [FormatException]，不如使用 [tryParse] 来处理潜在的解析错误。

对于任意数字 `n`，此函数满足 `identical(n, num.parse(n.toString()))`（当 `n` 是带有效荷载的 NaN `double` 时除外）。

[onError] 参数已弃用，将被移除。应使用 `num.tryParse(string) ?? (...)`，而不是 `num.parse(string, (string) { ... })`。

示例：

```dart
var value = num.parse('2021'); // 2021
value = num.parse('3.14'); // 3.14
value = num.parse('  3.14 \xA0'); // 3.14
value = num.parse('0.'); // 0.0
value = num.parse('.0'); // 0.0
value = num.parse('-1.e3'); // -1000.0
value = num.parse('1234E+7'); // 12340000000.0
value = num.parse('+.12e-9'); // 1.2e-10
value = num.parse('-NaN'); // NaN
value = num.parse('0xFF'); // 255
value = num.parse(double.infinity.toString()); // Infinity
value = num.parse('1f'); // Throws.
```

### tryParse()

```dart
num? tryParse( String input )
```

将包含数字字面量的字符串解析为数字。

与 [parse] 类似，但对于无效输入，此函数返回 `null` 而不是抛出异常。

示例：

```dart
var value = num.tryParse('2021'); // 2021
value = num.tryParse('3.14'); // 3.14
value = num.tryParse('  3.14 \xA0'); // 3.14
value = num.tryParse('0.'); // 0.0
value = num.tryParse('.0'); // 0.0
value = num.tryParse('-1.e3'); // -1000.0
value = num.tryParse('1234E+7'); // 12340000000.0
value = num.tryParse('+.12e-9'); // 1.2e-10
value = num.tryParse('-NaN'); // NaN
value = num.tryParse('0xFF'); // 255
value = num.tryParse(double.infinity.toString()); // Infinity
value = num.tryParse('1f'); // null
```

## 属性

### hashCode

```dart
int get hashCode
```

返回数值的哈希码。

该哈希码与相等性兼容。对于具有相同数值的 [int] 和 [double]，返回相同的值，因此对双精度浮点数 0 和负 0 也返回相同的值。

不保证 NaN 值的哈希码。

### isNaN

```dart
bool get isNaN
```

此数字是否为 Not-a-Number（非数字）值。

如果此数字是 [double.nan] 值或任何其他可能的 [double] NaN 值，则为 `true`。如果此数字是整数、有限的双精度浮点数或无限的双精度浮点数（[double.infinity] 或 [double.negativeInfinity]），则为 `false`。

所有数字都恰好满足 [isInfinite]、[isFinite] 和 `isNaN` 三者之一。

### isNegative

```dart
bool get isNegative
```

此数字是否为负数。

如果一个数字小于零，或者它是双精度浮点数 `-0.0`，则该数字为负数。这排除了像 [double.nan] 这样的 NaN 值被视为负数的可能性。

### isInfinite

```dart
bool get isInfinite
```

此数字是否为正无穷大或负无穷大。

仅 [double.infinity] 和 [double.negativeInfinity] 满足此条件。

所有数字都恰好满足 `isInfinite`、[isFinite] 和 [isNaN] 三者之一。

### isFinite

```dart
bool get isFinite
```

此数字是否为有限数。

唯一的非有限数是 NaN 值、正无穷大和负无穷大。所有整数都是有限的。

所有数字都恰好满足 [isInfinite]、`isFinite` 和 [isNaN] 三者之一。

### sign

```dart
num get sign
```

根据此数字的符号和数值，返回负一、零或正一。

如果此数字小于零，则值为负一；如果大于零，则值为正一；如果等于零，则值为零。

如果此数字是 [double] 的 NaN 值，则返回 NaN。

返回与此数字类型相同的数字。对于双精度浮点数，`(-0.0).sign` 为 `-0.0`。

结果满足以下等式（NaN 除外，因为 NaN 不 `==` 自身）：

```dart
n == n.sign * n.abs()
```

## 方法

### abs()

```dart
num abs()
```

此数字的绝对值。

如果该值为非负数，则绝对值即为该值本身；如果该值为负数，则绝对值为 `-value`。

整数溢出可能导致 `-value` 的结果仍为负数。

```dart
print((2).abs()); // 2
print((-2.5).abs()); // 2.5
```

### round()

```dart
int round()
```

最接近此数字的整数。

当不存在唯一最接近的整数时，向远离零的方向舍入：`(3.5).round() == 4`，`(-3.5).round() == -4`。

该数字必须是有限的（参见 [isFinite]）。

如果该值大于可表示的最大正整数，则结果为该最大正整数。如果该值小于可表示的最大负整数，则结果为该最大负整数。

### floor()

```dart
int floor()
```

不大于此数字的最大整数。

将小数值向负无穷大方向舍入。

该数字必须是有限的（参见 [isFinite]）。

如果该值大于可表示的最大正整数，则结果为该最大正整数。如果该值小于可表示的最大负整数，则结果为该最大负整数。

### ceil()

```dart
int ceil()
```

不小于 `this` 的最小整数。

将小数值向正无穷大方向舍入。

该数字必须是有限的（参见 [isFinite]）。

如果该值大于可表示的最大正整数，则结果为该最大正整数。如果该值小于可表示的最大负整数，则结果为该最大负整数。

### truncate()

```dart
int truncate()
```

通过舍弃 `this` 的所有小数位得到的整数。

将小数值向零方向舍入。

该数字必须是有限的（参见 [isFinite]）。

如果该值大于可表示的最大正整数，则结果为该最大正整数。如果该值小于可表示的最大负整数，则结果为该最大负整数。

### roundToDouble()

```dart
double roundToDouble()
```

最接近此值的双精度浮点数整数值。

当不存在唯一最接近的整数时，向远离零的方向舍入：`(3.5).roundToDouble() == 4`，`(-3.5).roundToDouble() == -4`。

如果此值已经是整数值的双精度浮点数（包括 `-0.0`），或者是非有限的双精度浮点数值，则原样返回该值。

在舍入时，`-0.0` 被视为小于 `0.0`，因此对于负数，`-0.0` 被视为比 `0.0` 更接近。这意味着对于范围 `-0.5 < d < 0.0` 内的值 `d`，结果为 `-0.0`。

### floorToDouble()

```dart
double floorToDouble()
```

返回不大于 `this` 的最大双精度浮点数整数值。

如果此值已经是整数值的双精度浮点数（包括 `-0.0`），或者是非有限的双精度浮点数值，则原样返回该值。

在舍入时，`-0.0` 被视为小于 `0.0`。范围 `0.0 < d < 1.0` 内的数字 `d` 将返回 `0.0`。

### ceilToDouble()

```dart
double ceilToDouble()
```

返回不小于 `this` 的最小双精度浮点数整数值。

如果此值已经是整数值的双精度浮点数（包括 `-0.0`），或者是非有限的双精度浮点数值，则原样返回该值。

在舍入时，`-0.0` 被视为小于 `0.0`。范围 `-1.0 < d < 0.0` 内的数字 `d` 将返回 `-0.0`。

### truncateToDouble()

```dart
double truncateToDouble()
```

返回通过舍弃 `this` 的双精度浮点数值的所有小数位得到的双精度浮点数整数值。

如果此值已经是整数值的双精度浮点数（包括 `-0.0`），或者是非有限的双精度浮点数值，则原样返回该值。

在舍入时，`-0.0` 被视为小于 `0.0`。范围 `-1.0 < d < 0.0` 内的数字 `d` 将返回 `-0.0`，范围 `0.0 < d < 1.0` 内的数字将返回 `0.0`。

### clamp()

```dart
num clamp(num lowerLimit, num upperLimit)
```

返回将此 [num] 限制在 [lowerLimit] 到 [upperLimit] 范围内的结果。

比较使用 [compareTo] 进行，因此会考虑 `-0.0`。这也意味着 [double.nan] 被视为最大的双精度浮点数值。

参数 [lowerLimit] 和 [upperLimit] 必须构成一个有效范围，即 `lowerLimit.compareTo(upperLimit) <= 0`。

示例：

```dart
var result = 10.5.clamp(5, 10.0); // 10.0
result = 0.75.clamp(5, 10.0); // 5
result = (-10).clamp(-5, 5.0); // -5
result = (-0.0).clamp(-5, 5.0); // -0.0
```

### compareTo()

```dart
int compareTo(num other)
```

将此数字与 `other` 进行比较。

如果 `this` 小于 `other`，返回负数；如果相等，返回零；如果 `this` 大于 `other`，返回正数。

此方法表示的排序是 [num] 值的全序关系。所有不同的双精度浮点数互不相等，所有不同的整数也互不相等，但如果整数与双精度浮点数具有相同的数值，则二者相等。

对于双精度浮点数，`compareTo` 操作与 [operator==]、[operator<] 和 [operator>] 给出的偏序不同。例如，IEEE 双精度浮点数规定 `0.0 == -0.0`，且所有对 NaN 的比较操作都返回 false。

此函数为双精度浮点数强加了一个完全的排序。使用 `compareTo` 时，以下性质成立：

- 所有 NaN 值都被视为相等，且大于任何数值。
- -0.0 小于 0.0（以及整数 0），但大于任何非零负值。
- 负无穷大小于所有其他值，正无穷大大于所有非 NaN 值。
- 所有其他值按其数值进行比较。

示例：

```dart
print(1.compareTo(2)); // => -1
print(2.compareTo(1)); // => 1
print(1.compareTo(1)); // => 0

// The following comparisons yield different results than the
// corresponding comparison operators.
print((-0.0).compareTo(0.0));  // => -1
print(double.nan.compareTo(double.nan));  // => 0
print(double.infinity.compareTo(double.nan)); // => -1

// -0.0, and NaN comparison operators have rules imposed by the IEEE
// standard.
print(-0.0 == 0.0); // => true
print(double.nan == double.nan);  // => false
print(double.infinity < double.nan);  // => false
print(double.nan < double.infinity);  // => false
print(double.nan == double.infinity);  // => false
```

### toInt()

```dart
int toInt()
```

将此 [num] 截断为整数并将结果作为 [int] 返回。

等价于 [truncate]。

### toDouble()

```dart
double toDouble()
```

此数字作为 [double]。

如果整数无法精确表示为 [double]，则返回一个近似值。

### toStringAsFixed()

```dart
String toStringAsFixed(int fractionDigits)
```

此数字的小数点字符串表示形式。

在计算字符串表示之前，将此数字转换为 [double]，如同通过 [toDouble] 一样。

如果 `this` 的绝对值大于或等于 `10^21`，则此方法返回通过 `this.toStringAsExponential()` 计算的指数表示形式。否则，结果是小数点后恰好有 [fractionDigits] 位数字的最接近的字符串表示形式。如果 [fractionDigits] 等于 0，则省略小数点。

参数 [fractionDigits] 必须是满足以下条件的整数：`0 <= fractionDigits <= 20`。

示例：

```dart
1.toStringAsFixed(3);  // 1.000
(4321.12345678).toStringAsFixed(3);  // 4321.123
(4321.12345678).toStringAsFixed(5);  // 4321.12346
123456789012345.toStringAsFixed(3);  // 123456789012345.000
10000000000000000.toStringAsFixed(4); // 10000000000000000.0000
5.25.toStringAsFixed(0); // 5
```

### toStringAsExponential()

```dart
String toStringAsExponential([int? fractionDigits])
```

此数字的指数字符串表示形式。

在计算字符串表示之前，将此数字转换为 [double]。

如果给定了 [fractionDigits]，则它必须是满足以下条件的整数：`0 <= fractionDigits <= 20`。在这种情况下，字符串在小数点后恰好包含 [fractionDigits] 位数字。否则，如果不给定该参数，返回的字符串使用能准确表示此数字的最少位数。

如果 [fractionDigits] 等于 0，则省略小数点。示例：

```dart
1.toStringAsExponential();       // 1e+0
1.toStringAsExponential(3);      // 1.000e+0
123456.toStringAsExponential();  // 1.23456e+5
123456.toStringAsExponential(3); // 1.235e+5
123.toStringAsExponential(0);    // 1e+2
```

### toStringAsPrecision()

```dart
String toStringAsPrecision(int precision)
```

具有 [precision] 位有效数字的字符串表示形式。

将此数字转换为 [double]，并返回该值具有恰好 [precision] 位有效数字的字符串表示形式。

参数 [precision] 必须是满足以下条件的整数：`1 <= precision <= 21`。

示例：

```dart
1.toStringAsPrecision(2);       // 1.0
1e15.toStringAsPrecision(3);    // 1.00e+15
1234567.toStringAsPrecision(3); // 1.23e+6
1234567.toStringAsPrecision(9); // 1234567.00
12345678901234567890.toStringAsPrecision(20); // 12345678901234567168
12345678901234567890.toStringAsPrecision(14); // 1.2345678901235e+19
0.00000012345.toStringAsPrecision(15); // 1.23450000000000e-7
0.0000012345.toStringAsPrecision(15);  // 0.00000123450000000000
```

### toString()

```dart
String toString()
```

正确表示此数字的最短字符串。

范围在 `10^-6`（含）到 `10^21`（不含）之间的所有 [double] 都会转换为其十进制表示形式，且小数点后至少有一位数字。对于所有其他双精度浮点数，除了像 `NaN` 或 `Infinity` 这样的特殊值外，此方法返回指数表示形式（参见 [toStringAsExponential]）。

对于 [double.nan] 返回 `"NaN"`，对于 [double.infinity] 返回 `"Infinity"`，对于 [double.negativeInfinity] 返回 `"-Infinity"`。

[int] 转换为不带小数点的十进制表示形式。

示例：

```dart
(0.000001).toString();  // "0.000001"
(0.0000001).toString(); // "1e-7"
(111111111111111111111.0).toString();  // "111111111111111110000.0"
(100000000000000000000.0).toString();  // "100000000000000000000.0"
(1000000000000000000000.0).toString(); // "1e+21"
(1111111111111111111111.0).toString(); // "1.1111111111111111e+21"
1.toString(); // "1"
111111111111111111111.toString();  // "111111111111111110000"
100000000000000000000.toString();  // "100000000000000000000"
1000000000000000000000.toString(); // "1000000000000000000000"
1111111111111111111111.toString(); // "1111111111111111111111"
1.234e5.toString();   // 123400
1234.5e6.toString();  // 1234500000
12.345e67.toString(); // 1.2345e+68
```

注意：如果返回的字符串已足够精确以唯一标识输入数字，则转换可能会对输出进行舍入。例如，[double] 类型的 `9e59` 的最精确表示形式等于 `"899999999999999918767229449717619953810131273674690656206848"`，但此方法返回更短（但仍能唯一标识）的 `"9e59"`。

### remainder()

```dart
num remainder(num other)
```

`this` 除以 [other] 的截断除法余数。

此操作的结果 `r` 满足：`this == (this ~/ other) * other + r`。因此，余数 `r` 与被除数 `this` 具有相同的符号。

如果此数字和 [other] 都是整数，则结果为 [int]（如 [int.remainder] 所述），否则结果为 [double]。

示例：

```dart
print(5.remainder(3)); // 2
print(-5.remainder(3)); // -2
print(5.remainder(-3)); // 2
print(-5.remainder(-3)); // -2
```

## 运算符

### operator ==

```dart
bool operator ==(Object other)
```

测试此值在数值上是否等于 `other`。

如果两个操作数都是 [double]，则当它们具有相同的表示形式时相等，但存在以下例外：

- 零和负零（0.0 和 -0.0）被视为相等，二者的数值都为零。
- NaN 不等于任何值，包括 NaN 本身。如果任一操作数为 NaN，结果始终为 false。

如果一个操作数是 [double]，另一个是 [int]，则当该双精度浮点数具有整数值（有限且没有小数部分）且两个数字具有相同数值时，二者相等。

如果两个操作数都是整数，则当它们具有相同的值时相等。

如果 [other] 不是 [num]，则返回 false。

请注意，NaN 的这种行为是非自反的。这意味着双精度浮点数值的相等性并不是一个真正的等价关系，而 `operator==` 通常要求满足这一点。在诸如 [HashSet] 中使用 NaN 将无法正常工作。此行为是标准的 IEEE-754 双精度浮点数相等性。

如果可以避免使用 NaN 值，其余的双精度浮点数确实具有真正的等价关系，可以安全使用。

使用 [compareTo] 进行能够区分零和负零、并将 NaN 值视为相等的比较。

### operator +

```dart
num operator +(num other)
```

将 [other] 加到此数字上。

如果此数字和 [other] 都是整数，则结果为 [int]（如 [int.+] 所述），否则结果为 [double]。

### operator -

```dart
num operator -(num other)
```

从此数字中减去 [other]。

如果此数字和 [other] 都是整数，则结果为 [int]（如 [int.-] 所述），否则结果为 [double]。

### operator \*

```dart
num operator *(num other)
```

将此数字与 [other] 相乘。

如果此数字和 [other] 都是整数，则结果为 [int]（如 [int.*] 所述），否则结果为 [double]。

### operator %

```dart
num operator %(num other)
```

此数字除以 [other] 的欧几里得取模。

返回欧几里得除法的余数。两个整数 `a` 和 `b` 的欧几里得除法产生两个整数 `q` 和 `r`，满足 `a == b * q + r` 且 `0 <= r < b.abs()`。

欧几里得除法仅对整数有定义，但可以很容易地扩展到双精度浮点数。在这种情况下，`q` 仍然是整数，但 `r` 可能具有非整数值，且仍满足 `0 <= r < |b|`。

返回值 `r` 的符号始终为正。

有关截断除法的余数，请参见 [remainder]。

如果此数字和 [other] 都是整数，则结果为 [int]（如 [int.%] 所述），否则结果为 [double]。

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

将此数字除以 [other]。

### operator ~/

```dart
int operator ~/(num other)
```

截断除法运算符。

对此数字执行除以 [other] 的截断除法。截断除法是指将小数结果通过向零方向舍入转换为整数的除法。

如果两个操作数都是 [int]，则 [other] 不得为零。此时 `a ~/ b` 对应于 `a.remainder(b)`，满足 `a == (a ~/ b) * b + a.remainder(b)`。

如果任一操作数是 [double]，则在执行除法和结果截断之前，会将另一操作数转换为双精度浮点数。此时 `a ~/ b` 等价于 `(a / b).truncate()`。这意味着双精度除法的中间结果必须是有限整数（不能是无穷大或 [double.nan]）。

### operator unary-

```dart
num operator -()
```

此值的负值。

如果数字的负值存在，则该负值是与原数字相同类型（`int` 或 `double`）的数字，表示该数字数值的负数（即用零减去该数字得到的结果）。

对双精度浮点数取负，会得到一个与原值大小相同（`number.abs() == (-number).abs()`）、符号相反（`-(number.sign) == (-number).sign`）的数字。

对整数取负，`-number`，等价于用零减去它，即 `0 - number`。

（这两个性质通常也适用于另一种类型，但存在少数边界情况例外。）

### operator <

```dart
bool operator <(num other)
```

此数字在数值上是否小于 [other]。

如果此数字小于 [other]，返回 `true`。如果此数字大于或等于 [other]，或者任一值是像 [double.nan] 这样的 NaN 值，返回 `false`。

### operator <=

```dart
bool operator <=(num other)
```

此数字在数值上是否小于或等于 [other]。

如果此数字小于或等于 [other]，返回 `true`。如果此数字大于 [other]，或者任一值是像 [double.nan] 这样的 NaN 值，返回 `false`。

### operator >

```dart
bool operator >(num other)
```

此数字在数值上是否大于 [other]。

如果此数字大于 [other]，返回 `true`。如果此数字小于或等于 [other]，或者任一值是像 [double.nan] 这样的 NaN 值，返回 `false`。

### operator >=

```dart
bool operator >=(num other)
```

此数字在数值上是否大于或等于 [other]。

如果此数字大于或等于 [other]，返回 `true`。如果此数字小于 [other]，或者任一值是像 [double.nan] 这样的 NaN 值，返回 `false`。
