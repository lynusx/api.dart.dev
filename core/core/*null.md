# Null

```dart
final class Null {}
```

保留字 `null` 表示该类的唯一实例对象。

`Null` 类是唯一不实现 `Object` 的类。试图继承或实现 [Null] 类是编译时错误。

该语言提供了一些用于处理 `null` 值的专用运算符，例如：

```dart
e1!       // Throws if e1 is null.
e2 ?? e3  // Same as e2, unless e2 is null, then use value of e3
x ??= e4  // Same as x unless x is null, then same as `x = e4`.
e5?.foo() // call `foo` on e5, unless e5 is null.
[...? e6] // spreads e6 into the list literal, unless e6 is null.
```

## 属性

### hashCode

```dart
int get hashCode
```

此对象的哈希码。

哈希码是一个表示对象状态的单一整数，该状态会影响 [operator ==](https://api.dart.dev/dart-core/Object/operator_equals.html) 比较的结果。

所有对象都有哈希码。[Object](https://api.dart.dev/dart-core/Object-class.html) 默认实现的哈希码仅表示对象的标识，这与默认的 [operator ==](https://api.dart.dev/dart-core/Object/operator_equals.html) 实现方式相同，即仅当两个对象是同一个对象时才认为它们相等（参见 [identityHashCode](https://api.dart.dev/dart-core/identityHashCode.html)）。

如果重写了 [operator ==](https://api.dart.dev/dart-core/Object/operator_equals.html) 以改用对象状态进行比较，则哈希码也必须相应更改以反映该状态，否则该对象将无法用于基于哈希的数据结构，如默认的 [Set](https://api.dart.dev/dart-core/Set-class.html) 和 [Map](https://api.dart.dev/dart-core/Map-class.html) 实现。

根据 [operator ==](https://api.dart.dev/dart-core/Object/operator_equals.html) 相等的对象，其哈希码必须相同。对象的哈希码应仅在对象以影响相等性的方式发生变化时才改变。对哈希码没有其他要求。它们在同一程序的不同执行之间不需要保持一致，也没有分布上的保证。

不相等的对象允许具有相同的哈希码。技术上甚至允许所有实例都具有相同的哈希码，但如果冲突发生得过于频繁，可能会降低基于哈希的数据结构（如 [HashSet](https://api.dart.dev/dart-collection/HashSet-class.html) 或 [HashMap](https://api.dart.dev/dart-collection/HashMap-class.html)）的效率。

如果子类重写了 [hashCode](https://api.dart.dev/dart-core/Null/hashCode.html)，则也应重写 [operator ==](https://api.dart.dev/dart-core/Object/operator_equals.html) 运算符，以保持一致性。

## 方法

### toString()

```dart
String toString()
```

返回字符串 `"null"`。
