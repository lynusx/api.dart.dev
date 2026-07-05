# BigInt

```dart
abstract final class BigInt implements Comparable<BigInt> {}
```

任意大小的整数值。

大整数是有符号的，可以有任意数量的有效数字，仅受内存限制。

要根据提供的数字创建一个大整数，请使用 [BigInt.from]。

```dart
var bigInteger = BigInt.from(-1); // -1
bigInteger = BigInt.from(0.9999); // 0
bigInteger = BigInt.from(-10.99); // -10
bigInteger = BigInt.from(0x7FFFFFFFFFFFFFFF); // 9223372036854775807
bigInteger = BigInt.from(1e+30); // 1000000000000000019884624838656
```

要从字符串解析大整数值，请使用 [parse] 或 [tryParse]。

```dart
var value = BigInt.parse('0x1ffffffffffffffff'); // 36893488147419103231
value = BigInt.parse('12345678901234567890'); // 12345678901234567890
```

要检查大整数是否可以在不丢失精度的情况下表示为 [int]，请使用 [isValidInt]。

```dart continued
print(bigNumber.isValidInt); // false
```

要将大整数转换为 [int]，请使用 [toInt]。要将大整数转换为 [double]，请使用 [toDouble]。

```dart
var bigValue = BigInt.from(10).pow(3);
print(bigValue.isValidInt); // true
print(bigValue.toInt()); // 1000
print(bigValue.toDouble()); // 1000.0
```

**另请参阅：**

- [int]：整数。
- [double]：双精度浮点数。
- [num]：[int] 和 [double] 的父类。
- [内置数字类型](https://dart.dev/language/built-in-types#numbers)
- [数字表示](https://dart.dev/resources/language/number-representation)

## 构造函数

### BigInt.from()

```dart
BigInt.from(num value)
```

根据提供的数字 [value] 创建一个大整数。

示例：

```dart
var bigInteger = BigInt.from(1); // 1
bigInteger = BigInt.from(0.9999); // 0
bigInteger = BigInt.from(-10.99); // -10
```

## 静态属性

### zero

```dart
BigInt get zero
```

数值为 0 的大整数。

### one

```dart
BigInt get one
```

数值为 1 的大整数。

### two

```dart
BigInt get two
```

数值为 2 的大整数。

## 静态方法

### parse()

```dart
BigInt parse(
  String source, {
  int? radix,
})
```

将 [source] 解析为一个可能带符号的整数字面量，并返回其值。

[source] 必须是一个非空的 [radix] 进制数字序列，可以选择性地以负号或正号（'-' 或 '+'）作为前缀。

[radix] 必须在 2 到 36 的范围内。所使用的数字首先是十进制数字 0 到 9，然后是字母 'a' 到 'z'，其值为 10 到 35。也接受大写字母，其值与对应的小写字母相同。

如果未指定 [radix]，则默认为 10。在这种情况下，[source] 数字也可以以 `0x` 开头，此时该数字将被解释为十六进制字面量，实际上意味着 `0x` 会被忽略，并将进制设置为 16。

对于任何整数 `n` 和进制 `r`，可以保证 `n == int.parse(n.toRadixString(r), radix: r)`。

如果 [source] 不是有效的整数字面量（可选带符号前缀），则抛出 [FormatException]。示例：

```dart
print(BigInt.parse('-12345678901234567890')); // -12345678901234567890
print(BigInt.parse('0xFF')); // 255
print(BigInt.parse('0xffffffffffffffff')); // 18446744073709551615

// From binary (base 2) value.
print(BigInt.parse('1100', radix: 2)); // 12
print(BigInt.parse('00011111', radix: 2)); // 31
print(BigInt.parse('011111100101', radix: 2)); // 2021
// From octal (base 8) value.
print(BigInt.parse('14', radix: 8)); // 12
print(BigInt.parse('37', radix: 8)); // 31
print(BigInt.parse('3745', radix: 8)); // 2021
// From hexadecimal (base 16) value.
print(BigInt.parse('c', radix: 16)); // 12
print(BigInt.parse('1f', radix: 16)); // 31
print(BigInt.parse('7e5', radix: 16)); // 2021
// From base 35 value.
print(BigInt.parse('y1', radix: 35)); // 1191 == 34 * 35 + 1
print(BigInt.parse('z1', radix: 35)); // Throws.
// From base 36 value.
print(BigInt.parse('y1', radix: 36)); // 1225 == 34 * 36 + 1
print(BigInt.parse('z1', radix: 36)); // 1261 == 35 * 36 + 1
```

### tryParse()

```dart
BigInt? tryParse(
  String source, {
  int? radix,
})
```

将 [source] 解析为一个可能带符号的整数字面量，并返回其值。

与 [parse] 相同，不同之处在于如果输入无效，此方法将返回 `null`。

示例：

```dart
print(BigInt.tryParse('-12345678901234567890')); // -12345678901234567890
print(BigInt.tryParse('0xFF')); // 255
print(BigInt.tryParse('0xffffffffffffffff')); // 18446744073709551615

// From binary (base 2) value.
print(BigInt.tryParse('1100', radix: 2)); // 12
print(BigInt.tryParse('00011111', radix: 2)); // 31
print(BigInt.tryParse('011111100101', radix: 2)); // 2021
// From octal (base 8) value.
print(BigInt.tryParse('14', radix: 8)); // 12
print(BigInt.tryParse('37', radix: 8)); // 31
print(BigInt.tryParse('3745', radix: 8)); // 2021
// From hexadecimal (base 16) value.
print(BigInt.tryParse('c', radix: 16)); // 12
print(BigInt.tryParse('1f', radix: 16)); // 31
print(BigInt.tryParse('7e5', radix: 16)); // 2021
// From base 35 value.
print(BigInt.tryParse('y1', radix: 35)); // 1191 == 34 * 35 + 1
print(BigInt.tryParse('z1', radix: 35)); // null
// From base 36 value.
print(BigInt.tryParse('y1', radix: 36)); // 1225 == 34 * 36 + 1
print(BigInt.tryParse('z1', radix: 36)); // 1261 == 35 * 36 + 1
```

## 属性

### bitLength

```dart
int get bitLength
```

返回存储此大整数所需的最小位数。

该位数不包括符号位，这为非负（无符号）值提供了自然长度。对于负值，会通过补码运算返回第一个与符号位不同的位所在的位置。

要计算将该值存储为有符号值所需的位数，请加 1，即使用 `x.bitLength + 1`。

```dart
x.bitLength == (-x-1).bitLength;

BigInt.from(3).bitLength == 2;   // 00000011
BigInt.from(2).bitLength == 2;   // 00000010
BigInt.from(1).bitLength == 1;   // 00000001
BigInt.from(0).bitLength == 0;   // 00000000
BigInt.from(-1).bitLength == 0;  // 11111111
BigInt.from(-2).bitLength == 1;  // 11111110
BigInt.from(-3).bitLength == 2;  // 11111101
BigInt.from(-4).bitLength == 2;  // 11111100
```

### sign

```dart
int get sign
```

返回此大整数的符号。

对于零返回 0，对于小于零的值返回 -1，对于大于零的值返回 +1。

### isEven

```dart
bool get isEven
```

此大整数是否为偶数。

### isOdd

```dart
bool get isOdd
```

此大整数是否为奇数。

### isNegative

```dart
bool get isNegative
```

此数字是否为负数。

### isValidInt

```dart
bool get isValidInt
```

此大整数是否可以在不丢失精度的情况下表示为 `int`。

**警告：** 由于整数精度的差异，此函数在 dart2js、dev compiler 和 VM 上可能会给出不同的结果。

示例：

```dart
var bigNumber = BigInt.parse('100000000000000000000000');
print(bigNumber.isValidInt); // false

var value = BigInt.parse('0xFF'); // 255
print(value.isValidInt); // true
```

## 方法

### abs()

```dart
BigInt abs()
```

返回此整数的绝对值。

对于任何整数 `x`，其结果与 `x < 0 ? -x : x` 相同。

### remainder()

```dart
BigInt remainder(BigInt other)
```

返回 `this` 除以 [other] 的截断除法余数。

此操作的结果 `r` 满足：`this == (this ~/ other) * other + r`。因此，余数 `r` 与被除数 `this` 具有相同的符号。

示例：

```dart
print(BigInt.from(5).remainder(BigInt.from(3))); // 2
print(BigInt.from(-5).remainder(BigInt.from(3))); // -2
print(BigInt.from(5).remainder(BigInt.from(-3))); // 2
print(BigInt.from(-5).remainder(BigInt.from(-3))); // -2
```

### compareTo()

```dart
int compareTo(BigInt other)
```

将此值与 `other` 进行比较。

如果 `this` 小于 `other`，则返回负数；如果两者相等，则返回零；如果 `this` 大于 `other`，则返回正数。

示例：

```dart
print(BigInt.from(1).compareTo(BigInt.from(2))); // => -1
print(BigInt.from(2).compareTo(BigInt.from(1))); // => 1
print(BigInt.from(1).compareTo(BigInt.from(1))); // => 0
```

### pow()

```dart
BigInt pow(int exponent)
```

返回 `this` 的 [exponent] 次幂。

如果 [exponent] 等于 0，则返回 [one]。

否则 [exponent] 必须为正数。

结果始终等于 this 的 [exponent] 次幂的数学结果，仅受可用内存限制。

示例：

```dart
var value = BigInt.from(1000);
print(value.pow(0)); // 1
print(value.pow(1)); // 1000
print(value.pow(2)); // 1000000
print(value.pow(3)); // 1000000000
print(value.pow(4)); // 1000000000000
print(value.pow(5)); // 1000000000000000
print(value.pow(6)); // 1000000000000000000
print(value.pow(7)); // 1000000000000000000000
print(value.pow(8)); // 1000000000000000000000000
```

### modPow()

```dart
BigInt modPow(BigInt exponent, BigInt modulus)
```

返回此整数的 [exponent] 次幂对 [modulus] 取模的结果。

[exponent] 必须为非负数，且 [modulus] 必须为正数。

### modInverse()

```dart
BigInt modInverse(BigInt modulus)
```

返回此大整数对 [modulus] 取模的模乘法逆元。

[modulus] 必须为正数。

如果不存在模逆元，则会发生错误。

### gcd()

```dart
BigInt gcd(BigInt other)
```

返回此大整数与 [other] 的最大公约数。

如果任一数字非零，则结果是能同时整除 `this` 和 `other` 的数值最大的整数。

最大公约数与顺序无关，因此 `x.gcd(y)` 始终与 `y.gcd(x)` 相同。

对于任何整数 `x`，`x.gcd(x)` 为 `x.abs()`。

如果 `this` 和 `other` 都为零，则结果也为零。

示例：

```dart
print(BigInt.from(4).gcd(BigInt.from(2))); // 2
print(BigInt.from(8).gcd(BigInt.from(4))); // 4
print(BigInt.from(10).gcd(BigInt.from(12))); // 2
print(BigInt.from(10).gcd(BigInt.from(10))); // 10
print(BigInt.from(-2).gcd(BigInt.from(-3))); // 1
```

### toUnsigned()

```dart
BigInt toUnsigned(int width)
```

以非负数（即无符号表示）形式返回此大整数的最低有效 [width] 位。返回值在高于 [width] 的所有位上均为零。

```dart
BigInt.from(-1).toUnsigned(5) == 31   // 11111111  ->  00011111
```

此操作可用于模拟底层语言中的算术运算。例如，要递增一个 8 位数量：

```dart
q = (q + 1).toUnsigned(8);
```

`q` 将从 `0` 计数到 `255`，然后回绕到 `0`。

如果输入在不截断的情况下能容纳于 [width] 位内，则结果与输入相同。避免 `x` 被截断所需的最小位宽由 `x.bitLength` 给出，即：

```dart
x == x.toUnsigned(x.bitLength);
```

### toSigned()

```dart
BigInt toSigned(int width)
```

返回此整数的最低有效 [width] 位，并将保留的最高位扩展为符号位。这等同于使用有符号二进制补码表示法将该值截断以适应 [width] 位。返回值在高于 [width] 的所有位置上具有相同的位值。

```dart
var big15 = BigInt.from(15);
var big16 = BigInt.from(16);
var big239 = BigInt.from(239);
                               //     V--sign bit-V
big16.toSigned(5) == -big16;   //  00010000 -> 11110000
big239.toSigned(5) == big15;   //  11101111 -> 00001111
                               //     ^           ^
```

此操作可用于模拟底层语言中的算术运算。例如，要递增一个 8 位有符号数量：

```dart
q = (q + 1).toSigned(8);
```

`q` 将从 `0` 计数到 `127`，然后回绕到 `-128`，再重新计数到 `127`。

如果输入值在不截断的情况下能容纳于 [width] 位内，则结果与输入相同。避免 `x` 被截断所需的最小位宽为 `x.bitLength + 1`，即：

```dart
x == x.toSigned(x.bitLength + 1);
```

### toInt()

```dart
int toInt()
```

将此 [BigInt] 作为 [int] 返回。

如果该数字不适合，则会被限制为最大（或最小）整数。

**警告：** 由于整数精度的差异，此限制行为在 Web 平台和原生平台上有所不同。

示例：

```dart
var bigNumber = BigInt.parse('100000000000000000000000');
print(bigNumber.isValidInt); // false
print(bigNumber.toInt()); // 9223372036854775807
```

### toDouble()

```dart
double toDouble()
```

将此 [BigInt] 作为 [double] 返回。

如果该数字无法表示为 [double]，则返回一个近似值。对于数值非常大的整数，该近似值可能为无穷大。

示例：

```dart
var bigNumber = BigInt.parse('100000000000000000000000');
print(bigNumber.toDouble()); // 1e+23
```

### toString()

```dart
String toString()
```

返回此整数的字符串表示形式。

返回的字符串可以被 [parse] 解析。对于任何 `BigInt` 类型的 `i`，可以保证 `i == BigInt.parse(i.toString())`。

示例：

```dart
var bigNumber = BigInt.parse('100000000000000000000000');
print(bigNumber.toString()); // "100000000000000000000000"
```

### toRadixString()

```dart
String toRadixString(int radix)
```

将此 [BigInt] 转换为给定 [radix] 进制的字符串表示形式。

在字符串表示形式中，大于 '9' 的数字使用小写字母表示，其中 'a' 表示 10，'z' 表示 35。

[radix] 参数必须是 2 到 36 范围内的整数。

示例：

```dart
// Binary (base 2).
print(BigInt.from(12).toRadixString(2)); // 1100
print(BigInt.from(31).toRadixString(2)); // 11111
print(BigInt.from(2021).toRadixString(2)); // 11111100101
print(BigInt.from(-12).toRadixString(2)); // -1100
// Octal (base 8).
print(BigInt.from(12).toRadixString(8)); // 14
print(BigInt.from(31).toRadixString(8)); // 37
print(BigInt.from(2021).toRadixString(8)); // 3745
// Hexadecimal (base 16).
print(BigInt.from(12).toRadixString(16)); // c
print(BigInt.from(31).toRadixString(16)); // 1f
print(BigInt.from(2021).toRadixString(16)); // 7e5
// Base 36.
print(BigInt.from(35 * 36 + 1).toRadixString(36)); // z1
```

## 运算符

### operator unary-

```dart
BigInt operator -()
```

返回此整数的相反数。

对整数取负的结果始终具有相反的符号，零除外，零的负值是其本身。

### operator +

```dart
BigInt operator +(BigInt other)
```

将 [other] 与此大整数相加。

结果仍然是一个大整数。

### operator -

```dart
BigInt operator -(BigInt other)
```

从此大整数中减去 [other]。

结果仍然是一个大整数。

### operator \*

```dart
BigInt operator *(BigInt other)
```

将此大整数与 [other] 相乘。

结果仍然是一个大整数。

### operator /

```dart
double operator /(BigInt other)
```

双精度除法运算符。

与 [int] 上的类似运算符一致，此操作首先对该大整数和 [other] 分别执行 [toDouble]，然后对这些值执行 [double.operator/] 并返回结果。

**注意：** 初始的 [toDouble] 转换可能会丢失精度。

示例：

```dart
print(BigInt.from(1) / BigInt.from(2)); // 0.5
print(BigInt.from(1.99999) / BigInt.from(2)); // 0.5
```

### operator ~/

```dart
BigInt operator ~/(BigInt other)
```

截断整数除法运算符。

执行截断整数除法，余数会被舍弃。

可以使用 [remainder] 方法计算余数。

示例：

```dart
var seven = BigInt.from(7);
var three = BigInt.from(3);
seven ~/ three;    // => 2
(-seven) ~/ three; // => -2
seven ~/ -three;   // => -2
seven.remainder(three);    // => 1
(-seven).remainder(three); // => -1
seven.remainder(-three);   // => 1
```

### operator %

```dart
BigInt operator %(BigInt other)
```

欧几里得取模运算符。

返回欧几里得除法的余数。两个整数 `a` 和 `b` 的欧几里得除法会得到两个整数 `q` 和 `r`，使得 `a == b * q + r` 且 `0 <= r < b.abs()`。

返回值 `r` 的符号始终为正。

有关截断除法的余数，请参见 [remainder]。

示例：

```dart
print(BigInt.from(5) % BigInt.from(3)); // 2
print(BigInt.from(-5) % BigInt.from(3)); // 1
print(BigInt.from(5) % BigInt.from(-3)); // 2
print(BigInt.from(-5) % BigInt.from(-3)); // 1
```

### operator <<()

```dart
BigInt operator <<(int shiftAmount)
```

将此整数的位向左移动 [shiftAmount] 位。

向左移位会使该数字变大，实际效果相当于将该数字乘以 `pow(2, shiftIndex)`。

结果的大小没有限制。可以考虑使用适当的掩码结合"与"运算符来限制中间值。

如果 [shiftAmount] 为负数，则会发生错误。

### operator >>

```dart
BigInt operator >>(int shiftAmount)
```

将此整数的位向右移动 [shiftAmount] 位。

向右移位会使该数字变小并丢弃最低有效位，实际效果相当于执行除以 `pow(2, shiftIndex)` 的整数除法。

如果 [shiftAmount] 为负数，则会发生错误。

### operator &

```dart
BigInt operator &(BigInt other)
```

按位与运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中仅在 `this` 和 [other] 中都被置位的位才会被置位。

如果两个操作数都为负数，则结果为负数，否则结果为非负数。

### operator |

```dart
BigInt operator |(BigInt other)
```

按位或运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中在 `this` 或 [other] 任一个中被置位的位都会被置位。

如果两个操作数都为非负数，则结果为非负数，否则结果为负数。

### operator ^

```dart
BigInt operator ^(BigInt other)
```

按位异或运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中仅在 `this` 和 [other] 中恰好有一个被置位的位才会被置位。

如果操作数具有相同的符号，则结果为非负数，否则结果为负数。

### operator ~

```dart
BigInt operator ~()
```

按位取反运算符。

将 `this` 视为足够大的二进制补码整数，结果是一个各位均取反的数字。

这会将任何整数 `x` 映射为 `-x - 1`。

### operator <

```dart
bool operator <(BigInt other)
```

此大整数在数值上是否小于 [other]。

### operator <=

```dart
bool operator <=(BigInt other)
```

[other] 在数值上是否大于此大整数。

### operator >

```dart
bool operator >(BigInt other)
```

此大整数在数值上是否大于 [other]。

### operator >=

```dart
bool operator >=(BigInt other)
```

[other] 在数值上是否小于此大整数。
