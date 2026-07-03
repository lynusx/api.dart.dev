# int

```dart
abstract final class int extends num {}
```

整数。

`int` 的默认实现是 64 位二进制补码整数，其运算在发生溢出时会环绕到该范围内。

**注意：** 编译为 JavaScript 时，整数被限制为可由双精度浮点数精确表示的值。可用的整数值包括 -2^53 到 2^53 之间的所有整数，以及一些量级更大的整数，其中包括一些大于 2^63 的整数。因此，[int] 类中运算符和方法的行为有时在 Dart VM 与编译为 JavaScript 的 Dart 代码之间会有所不同。例如，位运算符在编译为 JavaScript 时会将其操作数截断为 32 位整数。

不能继承（extend）、实现（implement）或混入（mix in）`int` 类。

**另请参阅：**

- [num]：[int] 的父类。
- [内置数字类型](https://dart.dev/language/built-in-types#numbers)
- [数字表示方式](https://dart.dev/resources/language/number-representation)

## 构造函数

### int.fromEnvironment()

```dart
const int.fromEnvironment(
  String name, {
  int defaultValue = 0,
})
```

获取编译配置环境中与 [name] 对应的整数值。

编译配置环境由编译或运行 Dart 程序的外部工具提供，是一个从字符串键到关联字符串值的映射。在同一个程序中，与某个 [name] 关联的字符串值（或未关联任何值）在所有对 [String.fromEnvironment]、`int.fromEnvironment`、[bool.fromEnvironment] 和 [bool.hasEnvironment] 的调用中必须保持一致。可以使用 [String.fromEnvironment] 直接访问这些字符串值。

该构造函数会查找 [name] 对应的字符串值，然后尝试按照与 [int.parse]/[int.tryParse] 相同的语法规则将其解析为整数。也就是说，它接受十进制数字和带 `0x` 前缀的十六进制数字，并接受前导负号。如果在编译配置环境中没有与 [name] 关联的值，或者关联的字符串值无法解析为整数，则该构造函数调用的值为整数 [defaultValue]，默认值为整数零。

其结果实际上与以下代码相同：

```dart template:expression
int.tryParse(const String.fromEnvironment(name, defaultValue: ""))
    ?? defaultValue
```

不同之处在于该构造函数调用可以作为一个常量值。

示例：

```dart
const defaultPort = int.fromEnvironment("defaultPort", defaultValue: 80);
```

若要检查某个值是否存在，请使用 [bool.hasEnvironment]。示例：

```dart
const int? maybeDeclared = bool.hasEnvironment("defaultPort")
    ? int.fromEnvironment("defaultPort")
    : null;
```

在同一个程序中，与某个 [name] 关联的字符串值（或未关联任何值）在所有对 [String.fromEnvironment]、`int.fromEnvironment`、[bool.fromEnvironment] 和 [bool.hasEnvironment] 的调用中必须保持一致。

该构造函数只保证在以 `const` 方式调用时能正常工作。在某些能够在运行时访问编译器选项的平台上，它也可能以非常量方式调用并正常工作，但大多数预先编译（AOT）的平台不具备这些信息。

## 静态方法

### parse()

```dart
int parse(
  String source, {
  int? radix,
})
```

将 [source] 解析为一个可能带符号的整数字面量，并返回其值。

[source] 必须是一个非空的 [radix] 进制数字序列，可以选择性地带有前导正负号（'-' 或 '+'）。

[radix] 的取值范围必须为 2 到 36。所使用的数字首先是十进制数字 0..9，然后是字母 'a'..'z'，对应值为 10 到 35。同时也接受大写字母，其值与对应的小写字母相同。

如果未指定 [radix]，则默认为 10。在这种情况下，[source] 中的数字也可以以 `0x` 开头，此时该数字将被解释为十六进制整数字面量。当 `int` 由 64 位有符号整数实现时，十六进制整数字面量可以表示大于 2<sup>63</sup> 的值，此时该值将被解析为一个*无符号*数字，最终结果为对应的有符号整数值。

对于任意整数 `n` 和有效的进制 `r`，可以保证 `n == int.parse(n.toRadixString(r), radix: r)`。

如果 [source] 字符串不包含有效的整数字面量（可以带有前导符号），则会抛出 [FormatException]。

与其抛出并立即捕获 [FormatException]，不如使用 [tryParse] 来处理可能的解析错误。

示例：

```dart
var value = int.tryParse(text);
if (value == null) {
  // 处理该问题
  // ...
}
```

### tryParse()

```dart
int? tryParse(
  String source, {
  int? radix,
})
```

将 [source] 解析为一个可能带符号的整数字面量。

与 [parse] 类似，但在 [parse] 会抛出 [FormatException] 的情况下，该函数会返回 `null`。

示例：

```dart
print(int.tryParse('2021')); // 2021
print(int.tryParse('1f')); // null
// 来自二进制（二进制，base 2）值。
print(int.tryParse('1100', radix: 2)); // 12
print(int.tryParse('00011111', radix: 2)); // 31
print(int.tryParse('011111100101', radix: 2)); // 2021
// 来自八进制（base 8）值。
print(int.tryParse('14', radix: 8)); // 12
print(int.tryParse('37', radix: 8)); // 31
print(int.tryParse('3745', radix: 8)); // 2021
// 来自十六进制（base 16）值。
print(int.tryParse('c', radix: 16)); // 12
print(int.tryParse('1f', radix: 16)); // 31
print(int.tryParse('7e5', radix: 16)); // 2021
// 来自 35 进制值。
print(int.tryParse('y1', radix: 35)); // 1191 == 34 * 35 + 1
print(int.tryParse('z1', radix: 35)); // null
// 来自 36 进制值。
print(int.tryParse('y1', radix: 36)); // 1225 == 34 * 36 + 1
print(int.tryParse('z1', radix: 36)); // 1261 == 35 * 36 + 1
```

## 属性

### isEven

```dart
bool get isEven
```

当且仅当该整数为偶数时返回 true。

### isOdd

```dart
bool get isOdd
```

当且仅当该整数为奇数时返回 true。

### bitLength

```dart
int get bitLength
```

返回存储该整数所需的最小位数。

该位数不包括符号位，因此它给出了非负（无符号）值的自然长度。对于负值，会先取其补码，再返回第一个与符号位不同的位所在的位置。

若要计算将该值作为有符号数存储所需的位数，请加一，即使用 `x.bitLength + 1`。

```dart
x.bitLength == (-x-1).bitLength;

3.bitLength == 2;     // 00000011
2.bitLength == 2;     // 00000010
1.bitLength == 1;     // 00000001
0.bitLength == 0;     // 00000000
(-1).bitLength == 0;  // 11111111
(-2).bitLength == 1;  // 11111110
(-3).bitLength == 2;  // 11111101
(-4).bitLength == 2;  // 11111100
```

### trailingZeroBitCount

```dart
int get trailingZeroBitCount
```

该整数二进制表示中末尾（最低有效位）连续零位的个数。

在 JavaScript 平台上，仅使用最低有效的 32 位。在原生平台上，直接使用 64 位有符号整数。

末尾零位的个数即该整数二进制表示中最低有效 1 位所在的位置。如果该整数为零，则该值为该平台用于位运算的整数大小（原生平台为 64 位，web 平台为 32 位）。

```dart
1.trailingZeroBitCount;  // 0
2.trailingZeroBitCount;  // 1
8.trailingZeroBitCount;  // 3
0.trailingZeroBitCount;  // 64 on native, 32 on the web
```

### oneBitCount

```dart
int get oneBitCount
```

该整数二进制表示中 `1` 位的个数。

在 JavaScript 平台上，仅使用最低有效的 32 位。在原生平台上，直接使用 64 位有符号整数。

“1 位个数”即该整数二进制表示中数字 `1` 的个数。对于负整数，其 1 位的个数会一直计算到该平台用于位运算的整数大小为止（原生平台为 64 位，web 平台为 32 位）。`n.oneBitCount + (~n).oneBitCount` 的值始终等于该平台用于位运算的整数大小（在 web 平台上，至少当该值最初是一个 32 位整数时是如此）。

```dart
0.oneBitCount;    // 0
1.oneBitCount;    // 1
7.oneBitCount;    // 3
(-1).oneBitCount; // 64 on native, 32 on the web
```

### sign

```dart
int get sign
```

返回该整数的符号。

对于零返回 0，对于小于零的值返回 -1，对于大于零的值返回 +1。

## 方法

### modPow()

```dart
int modPow(int exponent, int modulus)
```

返回该整数的 [exponent] 次幂对 [modulus] 取模的结果。

[exponent] 必须为非负数，[modulus] 必须为正数。

### modInverse()

```dart
int modInverse(int modulus)
```

返回该整数模 [modulus] 的模乘逆元。

[modulus] 必须为正数。

如果不存在模逆元，则视为错误。

### gcd()

```dart
int gcd(int other)
```

返回该整数与 [other] 的最大公约数。

只要其中一个数不为零，结果即为能同时整除 `this` 和 `other` 的数值上最大的整数。

最大公约数与运算顺序无关，因此 `x.gcd(y)` 始终与 `y.gcd(x)` 相同。

对于任意整数 `x`，`x.gcd(x)` 为 `x.abs()`。

如果 `this` 和 `other` 都为零，则结果也为零。

示例：

```dart
print(4.gcd(2)); // 2
print(8.gcd(4)); // 4
print(10.gcd(12)); // 2
print(10.gcd(0)); // 10
print((-2).gcd(-3)); // 1
```

### toUnsigned()

```dart
int toUnsigned(int width)
```

以非负数（即无符号表示）形式返回该整数的最低有效 [width] 位。返回值中所有高于 [width] 的位均为零。

```dart
(-1).toUnsigned(5) == 31   // 11111111  ->  00011111
```

此操作可用于模拟底层语言中的运算。例如，对一个 8 位数值进行递增：

```dart
q = (q + 1).toUnsigned(8);
```

`q` 会从 `0` 一直计数到 `255`，然后环绕回到 `0`。

如果输入值在不发生截断的情况下即可容纳于 [width] 位中，则结果与输入相同。避免 `x` 发生截断所需的最小位宽由 `x.bitLength` 给出，即：

```dart
x == x.toUnsigned(x.bitLength);
```

### toSigned()

```dart
int toSigned(int width)
```

返回该整数的最低有效 [width] 位，并将保留的最高位扩展为符号位。这相当于使用有符号二进制补码表示，将该值截断为 [width] 位。返回值在所有高于 [width] 的位上都具有相同的位值。

```dart
                         //     V--sign bit-V
16.toSigned(5) == -16;   //  00010000 -> 11110000
239.toSigned(5) == 15;   //  11101111 -> 00001111
                         //     ^           ^
```

此操作可用于模拟底层语言中的运算。例如，对一个 8 位有符号数值进行递增：

```dart
q = (q + 1).toSigned(8);
```

`q` 会从 `0` 一直计数到 `127`，然后环绕到 `-128`，再重新计数到 `127`。

如果输入值在不发生截断的情况下即可容纳于 [width] 位中，则结果与输入相同。避免 `x` 发生截断所需的最小位宽为 `x.bitLength + 1`，即：

```dart
x == x.toSigned(x.bitLength + 1);
```

### abs()

```dart
int abs()
```

返回该整数的绝对值。

对于任意整数 `value`，结果与 `value < 0 ? -value : value` 相同。

整数溢出可能导致 `-value` 的结果仍为负数。

### round()

```dart
int round()
```

返回 `this`。

### floor()

```dart
int floor()
```

返回 `this`。

### ceil()

```dart
int ceil()
```

返回 `this`。

### truncate()

```dart
int truncate()
```

返回 `this`。

### roundToDouble()

```dart
double roundToDouble()
```

返回 `this.toDouble()`。

### floorToDouble()

```dart
double floorToDouble()
```

返回 `this.toDouble()`。

### ceilToDouble()

```dart
double ceilToDouble()
```

返回 `this.toDouble()`。

### truncateToDouble()

```dart
double truncateToDouble()
```

返回 `this.toDouble()`。

### toString()

```dart
String toString()
```

返回该整数的字符串表示形式。

返回的字符串可由 [parse] 解析。对于任意 `int` 类型的 `i`，可以保证 `i == int.parse(i.toString())`。

### toRadixString()

```dart
String toRadixString(int radix)
```

将该 [int] 转换为以给定 [radix] 进制表示的字符串。

在字符串表示中，大于 '9' 的数字使用小写字母表示，其中 'a' 表示 10，'z' 表示 35。

[radix] 参数必须是 2 到 36 范围内的整数。

示例：

```dart
// 二进制（base 2）。
print(12.toRadixString(2)); // 1100
print(31.toRadixString(2)); // 11111
print(2021.toRadixString(2)); // 11111100101
print((-12).toRadixString(2)); // -1100
// 八进制（base 8）。
print(12.toRadixString(8)); // 14
print(31.toRadixString(8)); // 37
print(2021.toRadixString(8)); // 3745
// 十六进制（base 16）。
print(12.toRadixString(16)); // c
print(31.toRadixString(16)); // 1f
print(2021.toRadixString(16)); // 7e5
// 36 进制。
print((35 * 36 + 1).toRadixString(36)); // z1
```

## 运算符

### operator &

```dart
int operator &(int other)
```

按位与运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中只有在 `this` 和 [other] 中都置位的位才会被置位。

如果两个操作数均为负数，则结果为负数，否则结果为非负数。

```dart
print((2 & 1).toRadixString(2)); // 0010 & 0001 -> 0000
print((3 & 1).toRadixString(2)); // 0011 & 0001 -> 0001
print((10 & 2).toRadixString(2)); // 1010 & 0010 -> 0010
```

### operator |

```dart
int operator |(int other)
```

按位或运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中在 `this` 或 [other] 任意一方置位的位都会被置位。

如果两个操作数均为非负数，则结果为非负数，否则结果为负数。

示例：

```dart
print((2 | 1).toRadixString(2)); // 0010 | 0001 -> 0011
print((3 | 1).toRadixString(2)); // 0011 | 0001 -> 0011
print((10 | 2).toRadixString(2)); // 1010 | 0010 -> 1010
```

### operator ^

```dart
int operator ^(int other)
```

按位异或运算符。

将 `this` 和 [other] 都视为足够大的二进制补码整数，结果中在 `this` 和 [other] 中恰好有一方置位的位会被置位。

如果两个操作数符号相同，则结果为非负数，否则结果为负数。

示例：

```dart
print((2 ^ 1).toRadixString(2)); //  0010 ^ 0001 -> 0011
print((3 ^ 1).toRadixString(2)); //  0011 ^ 0001 -> 0010
print((10 ^ 2).toRadixString(2)); //  1010 ^ 0010 -> 1000
```

### operator ~

```dart
int operator ~()
```

按位取反运算符。

将 `this` 视为一个足够大的二进制补码整数，结果为将其每一位都取反后得到的数字。

这会将任意整数 `x` 映射为 `-x - 1`。

### operator <<

```dart
int operator <<(int shiftAmount)
```

将该整数的二进制位左移 [shiftAmount] 位。

左移会使该数值变大，相当于将该数乘以 `pow(2, shiftAmount)`。

结果的大小没有限制。可以使用“与”运算符配合合适的掩码来限制中间结果的大小。

如果 [shiftAmount] 为负数，则视为错误。

示例：

```dart
print((3 << 1).toRadixString(2)); // 0011 -> 0110
print((9 << 2).toRadixString(2)); // 1001 -> 100100
print((10 << 3).toRadixString(2)); // 1010 -> 1010000
```

### operator >>

```dart
int operator >>(int shiftAmount)
```

将该整数的二进制位右移 [shiftAmount] 位。

右移会使该数值变小，并舍弃最低有效位，相当于将该数除以 `pow(2, shiftAmount)`（整数除法）。

如果 [shiftAmount] 为负数，则视为错误。

示例：

```dart
print((3 >> 1).toRadixString(2)); // 0011 -> 0001
print((9 >> 2).toRadixString(2)); // 1001 -> 0010
print((10 >> 3).toRadixString(2)); // 1010 -> 0001
print((-6 >> 2).toRadixString); // 111...1010 -> 111...1110 == -2
print((-85 >> 3).toRadixString); // 111...10101011 -> 111...11110101 == -11
```

### operator >>>

```dart
int operator >>>(int shiftAmount)
```

按位无符号右移 [shiftAmount] 位。

最低有效的 [shiftAmount] 位会被舍弃，剩余的位（如果有）向下移动，并在最高有效位一侧移入零位。

[shiftAmount] 必须为非负数。

示例：

```dart
print((3 >>> 1).toRadixString(2)); // 0011 -> 0001
print((9 >>> 2).toRadixString(2)); // 1001 -> 0010
print(((-9) >>> 2).toRadixString(2)); // 111...1011 -> 001...1110 (> 0)
```

### operator unary-

```dart
int operator -()
```

返回该整数的相反数。

对整数取负后的结果符号总是与原数相反，零除外，零的相反数仍为零。
