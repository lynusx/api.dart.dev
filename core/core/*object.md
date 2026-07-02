# Object

```dart
class Object {}
```

所有 Dart 对象（`null` 除外）的基类。

因为 `Object` 是非空 Dart 类层次结构的根，所以除 `Null` 之外的其他所有 Dart 类都是 `Object` 的子类。

定义类时，应考虑重写 [toString] 方法，使其返回描述该类实例的字符串。你可能还需要定义 [hashCode] 和 [operator ==]，具体说明请参阅 [`dart:core` 简介](https://dart.dev/libraries/dart-core) 中的 [实现映射键](https://dart.dev/libraries/dart-core#implementing-map-keys) 一节。

## 构造函数

### Object()

```dart
Object()
```

创建一个新的 [Object] 实例。

[Object] 实例没有实际意义的状态，仅通过其标识才有用。一个 [Object] 实例只与自身相等。

## 静态方法

### hash()

```dart
@Since.new("2.14")
int hash(
  Object? object1,
  Object? object2, [
  Object? object3 = sentinelValue,
  Object? object4 = sentinelValue,
  Object? object5 = sentinelValue,
  Object? object6 = sentinelValue,
  Object? object7 = sentinelValue,
  Object? object8 = sentinelValue,
  Object? object9 = sentinelValue,
  Object? object10 = sentinelValue,
  Object? object11 = sentinelValue,
  Object? object12 = sentinelValue,
  Object? object13 = sentinelValue,
  Object? object14 = sentinelValue,
  Object? object15 = sentinelValue,
  Object? object16 = sentinelValue,
  Object? object17 = sentinelValue,
  Object? object18 = sentinelValue,
  Object? object19 = sentinelValue,
  Object? object20 = sentinelValue,
])
```

为多个对象创建一个组合哈希码。

该哈希码通过对每个参数的 [Object.hashCode] 进行数值组合计算得出，计算对象为所有实际传入的参数，即使参数为 `null` 也是如此。

示例：

```dart
class SomeObject {
  final Object a, b, c;
  SomeObject(this.a, this.b, this.c);
  bool operator ==(Object other) =>
      other is SomeObject && a == other.a && b == other.b && c == other.c;
  int get hashCode => Object.hash(a, b, c);
}
```

在单次程序运行期间，使用相同参数多次调用此函数时，计算得到的值将保持一致。

此函数生成的哈希值*不*保证在同一程序的不同运行之间，或在同一程序不同 isolate 中运行的代码之间保持稳定。所使用的具体算法在不同平台之间，或平台库的不同版本之间可能有所不同，并且可能依赖于每次程序运行时都会变化的值。

当使用一个按相同顺序包含此函数实际参数的集合来调用 [hashAll] 函数时，将得到与此函数相同的结果。

### hashAll()

```dart
@Since.new("2.14")
int hashAll( Iterable<Object?> objects )
```

为一系列对象创建一个组合哈希码。

该哈希码通过按迭代顺序对 [objects] 中每个元素的 [Object.hashCode] 进行数值组合计算得出，计算对象为 [objects] 中的所有元素，即使元素为 `null` 也是如此。

`hashAll([o])` 的结果并不等于 `o.hashCode`。

示例：

```dart
class SomeObject {
  final List<String> path;
  SomeObject(this.path);
  bool operator ==(Object other) {
    if (other is SomeObject) {
      if (path.length != other.path.length) return false;
      for (int i = 0; i < path.length; i++) {
        if (path[i] != other.path[i]) return false;
      }
      return true;
    }
    return false;
  }

  int get hashCode => Object.hashAll(path);
}
```

在单次程序运行期间，使用具有相同哈希码且顺序相同的对象再次调用此函数时，计算得到的值将保持一致。

此函数生成的哈希值*不*保证在同一程序的不同运行之间，或在同一程序不同 isolate 中运行的代码之间保持稳定。所使用的具体算法在不同平台之间，或平台库的不同版本之间可能有所不同，并且可能依赖于每次程序运行时都会变化的值。

### hashAllUnordered()

```dart
@Since.new("2.14")
int hashAllUnordered( Iterable<Object?> objects )
```

为一组对象创建一个组合哈希码。

该哈希码通过以与顺序无关的方式对 [objects] 中每个元素的 [Object.hashCode] 进行数值组合计算得出，计算对象为 [objects] 中的所有元素，即使元素为 `null` 也是如此。

`hashAllUnordered({o})` 的结果并不等于 `o.hashCode`。

示例：

```dart
bool setEquals<T>(Set<T> set1, Set<T> set2) {
  var hashCode1 = Object.hashAllUnordered(set1);
  var hashCode2 = Object.hashAllUnordered(set2);
  if (hashCode1 != hashCode2) return false;
  // Compare elements ...
}
```

在单次程序运行期间，使用具有相同哈希码的对象再次调用此函数时，计算得到的值将保持一致，即使对象的顺序不一定相同。

此函数生成的哈希值*不*保证在同一程序的不同运行之间保持稳定。所使用的具体算法在不同平台之间，或平台库的不同版本之间可能有所不同，并且可能依赖于每次程序运行时都会变化的值。

## 属性

### hashCode

```dart
int get hashCode
```

此对象的哈希码。

哈希码是一个单一整数，表示影响 [operator ==] 比较结果的对象状态。

所有对象都有哈希码。[Object] 实现的默认哈希码仅表示对象的标识，这与默认的 [operator ==] 实现方式相同——只有在对象完全相同（identical）时才认为它们相等（参见 [identityHashCode]）。

如果重写 [operator ==] 以改用对象状态进行比较，则哈希码也必须相应更改以表示该状态，否则该对象将无法用于基于哈希的数据结构，例如默认的 [Set] 和 [Map] 实现。

根据 [operator ==] 判定为相等的对象，其哈希码必须相同。对象的哈希码只应在对象发生影响相等性的变化时才改变。除此之外，对哈希码没有其他要求：它们在同一程序的不同运行之间无需保持一致，也没有分布方面的保证。

不相等的对象允许拥有相同的哈希码。从技术上讲，甚至允许所有实例都拥有相同的哈希码，但如果冲突发生得过于频繁，可能会降低 [HashSet] 或 [HashMap] 等基于哈希的数据结构的效率。

如果子类重写了 [hashCode]，则也应重写 [operator ==] 运算符，以保持一致性。

### runtimeType

```dart
Type get runtimeType
```

该对象运行时类型的表示。

## 方法

### toString()

```dart
String toString()
```

此对象的字符串表示形式。

有些类具有默认的文本表示形式，通常与静态 `parse` 函数（如 [int.parse]）配对使用。这些类会将该文本表示形式作为其字符串表示。

其他类则没有程序会关心的、有意义的文本表示形式。此类类通常会重写 `toString`，以便在检查对象时提供有用的信息，主要用于调试或日志记录。

### noSuchMethod()

```dart
dynamic noSuchMethod( Invocation invocation )
```

当访问不存在的方法或属性时调用。

动态成员调用可能会尝试调用接收对象上不存在的成员。示例：

```dart
dynamic object = 1;
object.add(42); // Statically allowed, run-time error
```

这段无效代码会调用整数 `1` 的 `noSuchMethod` 方法，并传入一个表示 `.add(42)` 调用及其参数的 [Invocation]（随后会抛出异常）。

类可以重写 [noSuchMethod]，为此类无效的动态调用提供自定义行为。

拥有非默认 [noSuchMethod] 实现的类也可以省略其接口成员的实现。示例：

```dart
class MockList<T> implements List<T> {
  noSuchMethod(Invocation invocation) {
    log(invocation);
    super.noSuchMethod(invocation); // Will throw.
  }
}
void main() {
  MockList().add(42);
}
```

尽管 `MockList` 类没有为任何 `List` 接口方法提供具体实现，这段代码在编译时也不会产生任何警告或错误。对 `List` 方法的调用会被转发到 `noSuchMethod`，因此这段代码会 `log` 一个类似于 `Invocation.method(#add, [42])` 的调用，随后抛出异常。

如果 `noSuchMethod` 返回了一个值，该值将成为原始调用的结果。如果该值的类型不是原始调用可以返回的类型，则会在调用处发生类型错误。

默认行为是抛出一个 [NoSuchMethodError]。

## 运算符

### operator ==()

```dart
bool operator ==(Object other)
```

相等运算符。

对于所有 [Object]，默认行为是当且仅当该对象与 [other] 是同一个对象时才返回 true。

重写此方法可为类指定不同的相等关系。重写后的方法仍必须是一个等价关系，即必须满足：

- **完全性（Total）**：必须对所有参数都返回一个布尔值，绝不应抛出异常。

- **自反性（Reflexive）**：对于所有对象 `o`，`o == o` 必须为 true。

- **对称性（Symmetric）**：对于所有对象 `o1` 和 `o2`，`o1 == o2` 与 `o2 == o1` 必须同时为 true 或同时为 false。

- **传递性（Transitive）**：对于所有对象 `o1`、`o2` 和 `o3`，如果 `o1 == o2` 和 `o2 == o3` 均为 true，则 `o1 == o3` 也必须为 true。

该方法还应随时间保持一致，即两个对象是否相等，只应在至少一个对象被修改时才发生变化。

如果子类重写了相等运算符，也应重写 [hashCode] 方法，以保持一致性。
