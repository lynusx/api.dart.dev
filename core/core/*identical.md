# identical()

```dart
bool identical(Object? a, Object? b)
```

检查两个对象引用是否指向同一个对象。

Dart 中的 _值_，即存储在变量中的内容，都是 _对象引用_。同一个对象可以有多个引用。

Dart 对象具有标识（identity），这将其与其他对象区分开来，即使这些对象具有完全相同的状态。`identical` 函数用于判断两个对象引用是否指向 _同一个_ 对象。

如果 `identical` 调用返回 `true`，则可以确定这两个参数之间没有任何可区分的方法。如果返回 `false`，则只能说明这两个参数已知不是同一个对象。

非常量的生成性（非工厂）构造函数调用，或对非常量的列表、集合、映射字面量求值，总会创建一个 _新_ 对象，该对象与任何已存在的对象都不相同。

_常量规范化_（Constant canonicalization）确保两个创建具有相同状态对象的编译时常量表达式，其求值结果引用同一个规范化实例。例如：

```dart
print(identical(const <int>[1], const <int>[1])); // true
```

整数和浮点数比较特殊，它们完全不允许创建新实例。如果两个整数相等，它们也必然是相同的（identical）。如果两个浮点数具有相同的二进制表示，它们也是相同的（在 Web 平台上，[double.nan] 和 `-0.0` 存在一些特殊情况）。

[Record](记录) 值不具有 _持久性_ 标识。这使得编译器可以将记录拆分为各个部分，并在之后重新构建，而无需担心创建出具有相同标识的对象。一个记录 _可能_ 与另一个具有相同形状（shape）的记录相同——如果所有对应字段都相同，但也可能不相同；不过它永远不会与其他任何东西相同。

示例：

```dart
var o = new Object();
var isIdentical = identical(o, new Object()); // false, different objects.
isIdentical = identical(o, o); // true, same object.
isIdentical = identical(const Object(), const Object()); // true, const canonicalizes.
isIdentical = identical([1], [1]); // false, different new objects.
isIdentical = identical(const [1], const [1]); // true.
isIdentical = identical(const [1], const [2]); // false.
isIdentical = identical(2, 1 + 1); // true, integers canonicalize.

var pair = (1, "a"); // Create a record.
isIdentical = identical(pair, pair); // true or false, can be either.

var pair2 = (1, "a"); // Create another(?) record.
isIdentical = identical(pair, pair2); // true or false, can be either.

isIdentical = identical(pair, (2, "a")); // false, not identical values.
isIdentical = identical(pair, (1, "a", more: true)); // false, wrong shape.
```

# identityHashCode()

```dart
int identityHashCode(Object? object)
```

[object] 的标识哈希码（identity hash code）。

返回即使 `hashCode` 已被重写，原始的 [Object.hashCode] 在该对象上也会返回的值。

该哈希码与 [identical] 兼容，这意味着对于任何 _非记录_ 对象，只要传入相同的参数，在同一次程序执行过程中始终保证返回相同的结果。

[Record](记录) 的标识哈希码是未定义的，因为记录不保证具有持久性标识。记录值的标识及其标识哈希码可能随时发生变化。

```dart
var identitySet = HashSet(equals: identical, hashCode: identityHashCode);
var dt1 = DateTime.now();
var dt2 = DateTime.fromMicrosecondsSinceEpoch(dt1.microsecondsSinceEpoch);
assert(dt1 == dt2);
identitySet.add(dt1);
print(identitySet.contains(dt1)); // true
print(identitySet.contains(dt2)); // false
```
