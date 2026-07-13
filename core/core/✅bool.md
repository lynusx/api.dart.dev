# bool

```dart
final class bool {}
```

保留字 `true` 和 `false` 表示该类仅有的两个实例对象。

类若试图继承（extend）或实现（implement）bool 是编译时错误。

## 构造函数

### bool.fromEnvironment()

```dart
const bool.fromEnvironment(
  String name, {
  bool defaultValue = false,
})
```

编译配置环境中 `name` 对应的布尔值。

编译配置环境由编译或运行 Dart 程序的外围工具提供。该环境是一组字符串键到其关联字符串值的映射。在单个程序中，与 `name` 关联的字符串值（或值的缺失）在对 [String.fromEnvironment]、[int.fromEnvironment]、`bool.fromEnvironment` 和 [bool.hasEnvironment] 的所有调用中必须保持一致。可以使用 [String.fromEnvironment] 直接访问该字符串值。

该构造函数会将与 `name` 关联的字符串值解析为布尔值，其解析方式等同于调用 [`bool.tryParse(value)`][bool.tryParse]，即只接受字符串 `"true"` 和 `"false"`。

如果编译配置环境中不存在与 `name` 关联的值，或者关联的字符串值既不是 `"true"` 也不是 `"false"`，则该构造函数调用的结果为 `defaultValue` 的布尔值，其默认值为 `false`。

结果等价于：

```dart template:expression
(const String.fromEnvironment(name) == "true")
    || ((const String.fromEnvironment(name) != "false") && defaultValue)
```

示例：

```dart
const bool loggingEnabled = bool.fromEnvironment("logging");
```

若需检查某个值是否存在，可使用 [bool.hasEnvironment]。示例：

```dart
const bool? yesNoMaybe = bool.hasEnvironment("optionalFlag")
    ? bool.fromEnvironment("optionalFlag")
    : null;
```

如需接受除 `"true"` 或 `"false"` 之外的其他字符串，请直接使用 [String.fromEnvironment] 构造函数。示例：

```dart
const isLoggingOn = (const String.fromEnvironment("logging") == "on");
```

该构造函数只保证在以 `const` 方式调用时正常工作。在某些能够在运行时访问编译器选项的平台上，非常量调用也可能有效，但大多数提前编译（AOT）平台不具备此信息。

### bool.hasEnvironment()

```dart
bool.hasEnvironment(String name)
```

`name` 是否已在编译配置环境中声明。

编译配置环境由编译或运行 Dart 程序的外围工具提供。该环境是一组字符串键到其关联字符串值的映射。在单个程序中，与 `name` 关联的字符串值（或值的缺失）在对 [String.fromEnvironment]、[int.fromEnvironment]、`bool.fromEnvironment` 和 [bool.hasEnvironment] 的所有调用中必须保持一致。

如果 `name` 在编译配置环境中存在关联值，该构造函数的求值结果为 `true`；否则为 `false`。若存在关联值，则可以通过 `const String.fromEnvironment(name)` 访问该值。否则，已知 `String.fromEnvironment(name, defaultValue: someString)` 会求值为给定的 `defaultValue`。

[String.fromEnvironment]、[int.fromEnvironment] 和 [bool.fromEnvironment] 构造函数始终产生构造函数所要求的 [String](https://www.yuque.com/thyname/dart.core/string)、[int](https://www.yuque.com/thyname/dart.core/int) 或 [bool](https://www.yuque.com/thyname/dart.core/bool) 类型的结果。在大多数情况下，某个 `name` 在配置环境中缺少关联值仅意味着代码应回退到默认行为，而相同类型的默认值通常能完美地表示这种情况。

在某些情况下，使用不同类型的值（通常是 `null`）能更好地表示"未做选择"这一状态。此时，可以先使用该构造函数检查是否存在某个值，然后再使用其他 `fromEnvironment` 构造函数。示例：

```dart
const int? indentOverride = bool.hasEnvironment("indent-override")
    ? int.fromEnvironment("indent-override")
    : null;
void indentLines(List<String> lines, int indentation) {
  int actualIndentation = indentOverride ?? indentation;
  // ... 对 lines 执行某些操作
}
```

这种模式允许编译配置为程序提供一个覆盖值，同时也允许不提供该值，程序可以区分"显式提供了值"和"未提供值"这两种情况。

另一个使用场景是：仅当所需的值可用时才执行额外操作。示例：

```dart
const Logger? logger = bool.hasEnvironment("logging-id")
    ? Logger(id: String.fromEnvironment("logging-id"))
    : null;
```

该构造函数只保证在以 `const` 方式调用时正常工作。在某些能够在运行时访问编译器选项的平台上，非常量调用也可能有效，但大多数提前编译（AOT）平台不具备此信息。

## 静态方法

### parse()

```dart
@Since.new("3.0")
bool parse(
	String source, {
	bool caseSensitive = true,
})
```

将 `source` 解析为布尔字面量，可选择是否区分大小写。

如果 `caseSensitive` 为 `true`（默认值），则唯一可接受的输入是字符串 `"true"` 和 `"false"`，分别返回结果 `true` 和 `false`。

如果 `caseSensitive` 为 `false`，则单词 `"true"` 和 `"false"` 的任意大小写组合（ASCII 字母）都会被接受，如同先将输入转换为小写后再处理。

如果 `source` 字符串不包含有效的布尔字面量，则抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

与其抛出异常后立即捕获 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)，不如使用 [tryParse] 来处理可能出现的解析错误。

示例：

```dart
print(bool.parse('true')); // true
print(bool.parse('false')); // false
print(bool.parse('TRUE')); // 抛出 FormatException
print(bool.parse('TRUE', caseSensitive: false)); // true
print(bool.parse('FALSE', caseSensitive: false)); // false
print(bool.parse('NO')); // 抛出 FormatException
print(bool.parse('YES')); // 抛出 FormatException
print(bool.parse('0')); // 抛出 FormatException
print(bool.parse('1')); // 抛出 FormatException
```

### tryParse()

```dart
@Since.new("3.0")
bool? tryParse(
	String source, {
	bool caseSensitive = true,
})
```

将 `source` 解析为布尔字面量，可选择是否区分大小写。

如果 `caseSensitive` 为 `true`（默认值），则唯一可接受的输入是字符串 `"true"` 和 `"false"`，分别返回结果 `true` 和 `false`。

如果 `caseSensitive` 为 `false`，则单词 `"true"` 和 `"false"` 的任意大小写组合（ASCII 字母）都会被接受，如同先将输入转换为小写后再处理。

如果 `source` 字符串不包含有效的布尔字面量，则返回 `null`。

如果可以假定输入有效，可使用 [bool.parse] 以避免处理可能为 `null` 的结果。

示例：

```dart
print(bool.tryParse('true')); // true
print(bool.tryParse('false')); // false
print(bool.tryParse('TRUE')); // null
print(bool.tryParse('TRUE', caseSensitive: false)); // true
print(bool.tryParse('FALSE', caseSensitive: false)); // false
print(bool.tryParse('NO')); // null
print(bool.tryParse('YES')); // null
print(bool.tryParse('0')); // null
print(bool.tryParse('1')); // null
```

## 属性

### hashCode

该对象的哈希码（hash code）。

哈希码是一个表示对象状态的单一整数，该状态会影响 [operator ==](https://api.flutter.dev/flutter/dart-core/Object/operator_equals.html) 的比较结果。

所有对象都有哈希码。[Object](https://www.yuque.com/thyname/dart.core/object) 实现的默认哈希码仅表示对象的标识（identity），这与默认的 [operator ==](https://api.flutter.dev/flutter/dart-core/Object/operator_equals.html) 实现方式相同，即仅当两个对象是同一个对象（identical）时才认为它们相等（参见 [identityHashCode](https://www.yuque.com/thyname/dart.core/identityhashcode)）。

如果重写 [operator ==](https://api.flutter.dev/flutter/dart-core/Object/operator_equals.html) 以改为基于对象状态进行比较，则也必须相应地修改哈希码以表示该状态，否则该对象将无法在基于哈希的数据结构（如默认的 [Set](https://www.yuque.com/thyname/dart.core/set) 和 [Map](https://www.yuque.com/thyname/dart.core/map) 实现）中使用。

根据 [operator ==](https://api.flutter.dev/flutter/dart-core/Object/operator_equals.html)，彼此相等的对象其哈希码必须相同。对象的哈希码应仅在该对象以影响相等性的方式发生变化时才改变。除此之外，对哈希码没有其他要求。它们无需在同一程序的多次运行之间保持一致，也不保证任何分布特性。

允许不相等的对象拥有相同的哈希码。技术上甚至允许所有实例都拥有相同的哈希码，但如果冲突发生得过于频繁，可能会降低基于哈希的数据结构（如 [HashSet](https://www.yuque.com/thyname/dart.collection/hashset) 或 [HashMap](https://www.yuque.com/thyname/dart.collection/hashmap)）的效率。

如果子类重写了 [hashCode](https://api.flutter.dev/flutter/dart-core/bool/hashCode.html)，则也应同时重写 [operator ==](https://api.flutter.dev/flutter/dart-core/Object/operator_equals.html) 运算符以保持一致性。

```dart
int get hashCode
```

## 方法

### toString()

```dart
String toString()
```

对于 `true` 返回 `"true"`，对于 `false` 返回 `"false"`。

## 运算符

### operator &

```dart
bool operator &(bool other)
```

this 和 `other` 的逻辑合取（"与"运算）。

当 this 和 `other` 都为 `true` 时返回 `true`，否则返回 `false`。

### operator |

```dart
bool operator |(bool other)
```

this 和 `other` 的逻辑析取（"或"运算，即"包含或"）。

当 this 或 `other` 中至少有一个为 `true` 时返回 `true`，否则返回 `false`。

### operator ^

```dart
bool operator ^(bool other)
```

this 和 `other` 的逻辑异或（"排斥或"运算）。

返回 this 和 `other` 是否既非同时为 `true` 也非同时为 `false`。
