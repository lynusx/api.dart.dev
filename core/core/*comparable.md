# Comparator

```dart
typedef Comparator<T> = int Function(T a, T b)
```

泛型比较函数的签名。

比较函数表示一种对象类型上的排序。类型上的全序意味着：对于两个值，它们要么相等，要么一个大于另一个（此时后者必然小于前者）。

[Comparator] 函数通过返回以下值来表示这种全序关系：

- 如果 [a] 小于 [b]，返回负整数；
- 如果 [a] 等于 [b]，返回零；
- 如果 [a] 大于 [b]，返回正整数。

# Comparable

```dart
abstract interface class Comparable<T> {}
```

用于具有内在排序的类型的接口。

[compareTo] 操作定义了对象的全序关系，可用于排序。

[Comparable] 接口应当用于表示类型的自然排序。如果一个类型可以有多种排序方式，且没有一种是明显的自然排序，那么最好不要使用 [Comparable] 接口，而应改为提供单独的 [Comparator]。

建议 [Comparable] 的排序与其 [operator ==] 相等运算符保持一致（即 `a.compareTo(b) == 0` 当且仅当 `a == b`），但这并非强制要求。例如，[double] 和 [DateTime] 的 `compareTo` 方法就与 [operator ==] 运算符不一致。对于 double 而言，`compareTo` 方法比相等运算符更精确；而对于 [DateTime] 而言，则相反，`compareTo` 方法的精度较低。

示例：

```dart
(0.0).compareTo(-0.0);   // => 1
0.0 == -0.0;             // => true
var now = DateTime.now();
var utcNow = now.toUtc();
now == utcNow;           // => false
now.compareTo(utcNow);   // => 0
```

[Comparable] 接口并不意味着一定存在比较运算符 `<`、`<=`、`>` 和 `>=`。只有当排序是一种"小于/大于"式的排序时，即在描述两个元素的顺序时自然会使用"小于"这样的措辞时，才应该定义这些运算符。

如果相等运算符与 [compareTo] 不一致，那么比较运算符应当遵循相等运算符，因而也可能与 [compareTo] 不一致。否则，比较运算符应当与 [compareTo] 方法保持一致，即 `a < b` 当且仅当 `a.compareTo(b) < 0`。

[double] 类定义的比较运算符与相等运算符是兼容的。这些运算符在处理 -0.0 和 NaN 时与 [double.compareTo] 的行为不同。

[DateTime] 类没有比较运算符，取而代之的是命名更为精确的 [DateTime.isBefore] 和 [DateTime.isAfter]，二者均与 [DateTime.compareTo] 保持一致。

## 静态方法

### compare()

```dart
int compare(Comparable a, Comparable b)
```

比较两个可比较对象的 [Comparator]。

它返回 `a.compareTo(b)` 的结果。如果 `a` 与 `b` 的类型不可比较，此调用可能在运行时失败。

该工具函数被用作集合排序的默认比较器，例如在 [List] 的 sort 函数中。

## 方法

### compareTo()

```dart
int compareTo(T other)
```

将此对象与另一个对象进行比较。

返回一个类似 [Comparator] 的值，用于比较 `this` 与 [other]：如果 `this` 排在 [other] 之前，返回负整数；如果 `this` 排在 [other] 之后，返回正整数；如果 `this` 与 [other] 排序相同，则返回零。

[other] 参数必须是与此对象可比较的值。
