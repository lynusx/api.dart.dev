# Invocation

```dart
abstract class Invocation {}
```

对某个对象上成员调用的表示。

这是当对象不支持所尝试调用的成员时，传递给 [Object.noSuchMethod] 的对象类型。

## 构造函数

### Invocation()

```dart
Invocation()
```

### Invocation.method()

```dart
Invocation.method(
  Symbol memberName,
  Iterable<Object?>? positionalArguments, [
  Map<Symbol, Object?>? namedArguments
])
```

创建一个对应于方法调用的 invocation。

该方法调用没有类型参数。如果省略了命名参数，则默认为没有命名参数。

### Invocation.genericMethod()

```dart
Invocation.genericMethod(
  Symbol memberName,
  Iterable<Type>? typeArguments,
  Iterable<Object?>? positionalArguments, [
  Map<Symbol, Object?>? namedArguments,
])
```

创建一个对应于泛型方法调用的 invocation。

如果 [typeArguments] 为 `null` 或为空，则该构造函数等价于使用其余参数调用 [Invocation.method]。所有类型参数都必须非空。

如果省略了命名参数，则默认为没有命名参数。

### Invocation.getter()

```dart
Invocation.getter(Symbol name)
```

创建一个对应于获取器（getter）调用的 invocation。

### Invocation.setter()

```dart
Invocation.setter(Symbol memberName, Object? argument)
```

创建一个对应于设置器（setter）调用的 invocation。

该构造函数可以接受任意 [Symbol](https://www.yuque.com/thyname/dart.core/symbol) 作为 [memberName]，但请注意，_实际的 setter 名称_ 以 `=` 结尾，因此 `object.member = value` 对应的 invocation 为：

```dart
Invocation.setter(const Symbol("member="), value)
```

## 属性

### memberName

```dart
Symbol get memberName
```

被调用成员的名称。

### typeArguments

```dart
List<Type> get typeArguments
```

调用的类型参数的一个不可修改视图。

如果该成员是获取器、设置器或运算符，则类型参数列表始终为空。

### positionalArguments

```dart
List get positionalArguments
```

调用的位置参数的一个不可修改视图。

如果该成员是获取器，则位置参数列表始终为空。

### namedArguments

```dart
Map<Symbol, dynamic> get namedArguments
```

调用的命名参数的一个不可修改视图。

如果该成员是获取器、设置器或运算符，则命名参数映射始终为空。

### isMethod

```dart
bool get isMethod
```

该调用是否为方法调用。

### isGetter

```dart
bool get isGetter
```

该调用是否为获取器调用。如果是，则以上三种参数列表均为空。

### isSetter

```dart
bool get isSetter
```

该调用是否为设置器调用。

如果是，则 [positionalArguments] 恰好包含一个位置参数，[namedArguments] 为空，且 typeArguments 为空。

### isAccessor

```dart
bool get isAccessor
```

该调用是否为获取器或设置器调用。
