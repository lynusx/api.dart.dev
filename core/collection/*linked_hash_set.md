# LinkedHashSet

```dart
abstract final class LinkedHashSet<E> implements Set<E> {}
```

[LinkedHashSet] 是一种基于哈希表实现的 [Set]。

[Set] 的默认实现即为 [LinkedHashSet]。

`LinkedHashSet` 还会记录元素的插入顺序，迭代按照先插入先迭代的顺序进行。

`LinkedHashSet` 的元素必须具备一致的 [Object.==] 和 [Object.hashCode] 实现。这意味着 `==` 运算符必须在元素上定义一个稳定的等价关系（自反、对称、传递，且随时间保持一致），并且对于被 `==` 判定为相等的对象，`hashCode` 必须相同。

元素的迭代按照元素插入的顺序进行。晚于另一元素添加的元素会在迭代中较晚出现。添加一个已存在于集合中的元素不会改变其在迭代顺序中的位置，但先移除该元素再重新添加，会使其成为迭代中的最后一个元素。

只要对象的哈希码分布良好，`HashSet` 上大多数简单操作都能在（可能是均摊的）常数时间内完成，包括 [add]、[contains]、[remove] 和 [length]。

**注意：** 在对集合执行某项操作期间（例如在 [forEach] 或 [containsAll] 调用期间调用的函数中，或在迭代集合期间），不要修改该集合（添加或移除元素）。

当元素位于集合中时，不要以改变其相等性（从而改变其哈希码）的方式修改该元素。某些特殊类型的集合可能在相等性方面更为宽松，此时它们应在文档中说明其不同的行为和限制。

示例：

```dart
final planets = <String>{}; // LinkedHashSet
```

要向集合中添加数据，可使用 [add] 或 [addAll]。

```dart continued
final uranusAdded = planets.add('Uranus'); // true
planets.addAll({'Venus', 'Mars', 'Earth', 'Jupiter'});
print(planets); // {Uranus, Venus, Mars, Earth, Jupiter}
```

要检查集合是否为空，可使用 [isEmpty] 或 [isNotEmpty]。要获取集合中元素的数量，可使用 [length]。

```dart continued
print(planets.isEmpty); // false
print(planets.length); // 5
```

要检查集合中是否存在某个特定值的元素，可使用 [contains]。

```dart continued
final marsExists = planets.contains('Mars'); // true
```

[forEach] 方法会对集合中的每个元素调用一个函数。

```dart continued
planets.forEach(print);
// Uranus
// Venus
// Mars
// Earth
// Jupiter
```

要复制该集合，可使用 [toSet]。

```dart continued
final copySet = planets.toSet();
print(copySet); // {Uranus, Venus, Mars, Earth, Jupiter}
```

要移除某个元素，可使用 [remove]。

```dart continued
final removedValue = planets.remove('Mars'); // Mars
print(planets); // {Uranus, Venus, Earth, Jupiter}
```

要同时移除多个元素，可使用 [removeWhere] 或 [removeAll]。

```dart continued
planets.removeWhere((element) => element.startsWith('E'));
print(planets); // {Uranus, Venus, Jupiter}
```

要移除集合中所有不满足条件的元素，可使用 [retainWhere]。

````dart continued
planets.retainWhere((element) => element.contains('Jupiter'));
print(planets); // {Jupiter}
```dart continued
要移除所有元素并清空集合，可使用 [clear]。
```dart continued
planets.clear();
print(planets.isEmpty); // true
print(planets); // {}
````

**另请参阅：**

- [Set] 是每个对象只能出现一次的集合的通用接口。
- [HashSet] 中对象的迭代顺序不保证。
- [SplayTreeSet] 按排序顺序迭代对象。

## 构造函数

### LinkedHashSet()

```dart
LinkedHashSet<E>({
  bool equals( E, E )?,
  int hashCode( E )?,
  bool isValidKey( dynamic )?,
})
```

使用提供的 [equals] 和 [hashCode] 创建一个按插入顺序排列的哈希集合。

提供的 [equals] 必须定义一个稳定的等价关系，[hashCode] 必须与 [equals] 保持一致。

如果提供了 [equals] 和 [hashCode] 中的一个，通常也应同时提供另一个。

某些 [equals] 或 [hashCode] 函数可能并不适用于所有对象。如果提供了 [isValidKey]，则用它来检查某个不一定是 [E] 实例的潜在元素，例如传递给 [contains] 的参数，其类型为 `Object?`。如果 [isValidKey] 对某个对象返回 `false`，则不会调用 [equals] 和 [hashCode] 函数，并且认为映射中不存在与该键相等的键。[isValidKey] 函数默认只测试对象是否为 [E] 的实例，这意味着：

```dart template:expression
LinkedHashSet<int>(equals: (int e1, int e2) => (e1 - e2) % 5 == 0,
                   hashCode: (int e) => e % 5);
```

不需要 `isValidKey` 参数，因为它默认只接受 `int` 类型的值，而这类值同时被 `equals` 和 `hashCode` 接受。

如果 `equals`、`hashCode` 和 `isValidKey` 都未提供，则默认的 `isValidKey` 会接受所有值。此时假定默认的相等性和哈希码操作对所有对象都适用。

同样，如果 `equals` 为 [identical]，`hashCode` 为 [identityHashCode]，且省略了 `isValidKey`，则得到的集合是基于标识（identity）的，此时 `isValidKey` 默认接受所有键。这样的映射可以直接使用 [LinkedHashSet.identity] 创建。

### LinkedHashSet.identity()

```dart
LinkedHashSet<E>.identity()
```

创建一个按插入顺序排列、基于标识（identity）的集合。

实际上等价于以下简写形式：

```dart
LinkedHashSet<E>(equals: identical, hashCode: identityHashCode)
```

### LinkedHashSet.from()

```dart
LinkedHashSet<E>.from(Iterable elements)
```

创建一个包含所有 [elements] 的链式哈希集合。

按照 `LinkedHashSet<E>()` 的方式创建一个链式哈希集合，并按 `elements` 被迭代的顺序将其中每个元素添加到该集合中。

[elements] 中的所有元素都应为 [E] 的实例。`elements` 这一可迭代对象本身可以是任意元素类型，因此该构造函数可用于对某个 `Set` 进行向下转型（down-cast），例如：

```dart
Set<SuperType> superSet = ...;
Iterable<SuperType> tmp = superSet.where((e) => e is SubType);
Set<SubType> subSet = LinkedHashSet<SubType>.from(tmp);
```

示例：

```dart
final numbers = <num>[10, 20, 30];
final setFrom = LinkedHashSet<int>.from(numbers);
print(setFrom); // {10, 20, 30}
```

### LinkedHashSet.of()

```dart
LinkedHashSet<E>.of(Iterable<E> elements)
```

根据 [elements] 创建一个链式哈希集合。

按照 `LinkedHashSet<E>()` 的方式创建一个链式哈希集合，并按 `elements` 被迭代的顺序将其中每个元素添加到该集合中。示例：

```dart
final baseSet = <int>{1, 2, 3};
final setOf = LinkedHashSet<num>.of(baseSet);
print(setOf); // {1, 2, 3}
```

## 属性

### iterator

```dart
Iterator<E> get iterator
```

提供一个按插入顺序迭代元素的迭代器。

## 方法

### forEach()

```dart
void forEach(void action(E element))
```

对集合中的每个元素执行一个函数。

元素按插入顺序进行迭代。
