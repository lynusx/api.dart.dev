# Type

```dart
abstract interface class Type {}
```

类型的运行时表示。

Type 对象表示类型。可以通过以下几种方式创建一个 Type 对象：

- 通过 _类型字面量_，即将类型名称作为表达式，像 `Type type = int;` 这样；或者将类型变量作为表达式，像 `Type type = T;` 这样。
- 通过读取对象的运行时类型，像 `Type type = o.runtimeType;` 这样。
- 通过 `dart:mirrors`。

Type 对象旨在作为使用 `dart:mirrors` 的入口点。仅支持与其他 Type 对象进行相等性比较，以及将其转换为字符串用于调试这两种操作。

## 属性

### hashCode

```dart
int get hashCode
```

与 [operator==] 兼容的该类型的哈希码。

## 方法

### toString()

```dart
String toString()
```

返回表示底层类型的字符串。

该字符串仅用于在调试时向读者提供信息。其格式没有任何保证，[Type](https://www.yuque.com/thyname/dart.core/type) 实例返回的字符串值完全由具体实现决定。

该字符串应当保持一致，因此 _相同_ 类型的 `Type` 对象在整个程序运行期间返回相同的字符串。该字符串可能包含也可能不包含与该类型声明的源代码名称相对应的部分（如果该类型确实有源代码名称的话），有些通过 `dart:mirrors` 可访问的类型可能没有源代码名称。

## 运算符

### operator ==

```dart
bool operator ==(Object other)
```

判断 [other] 是否为表示等价类型的 [Type](https://www.yuque.com/thyname/dart.core/type) 实例。

语言规范规定了哪些类型被视为等价类型。如果两个类型是等价的，则可以保证它们互为彼此的子类型；但也存在互为子类型但并不等价的类型（例如 `dynamic` 和 `void`，或者 `FutureOr<Object>` 和 `Object`）。
