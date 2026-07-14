# SplayTreeMap

```dart
final class SplayTreeMap<K, V> extends _SplayTree<K, _SplayTreeMapNode<K, V>> with MapMixin<K, V> {}
```

一个可以按彼此顺序排列的对象组成的 [Map](https://www.yuque.com/thyname/dart.core/map)。

该映射基于自平衡二叉树实现，支持大多数单条目操作，均摊时间复杂度为对数级。

映射的键使用构造函数中传入的 `compare` 函数进行比较，该函数同时用于排序和判断相等性。如果映射中只包含键 `a`，那么 `map.containsKey(b)` 仅当 `compare(a, b) == 0` 时才返回 `true`，甚至不会检查 `a == b` 的值。如果省略 compare 函数，则假定对象是可比较的（[Comparable](https://www.yuque.com/thyname/dart.core/comparable)），并使用其 [Comparable.compareTo] 方法进行比较。在这种情况下，不可比较的对象（包括 `null`）不能用作键。

为了允许使用 `compare` 函数不支持的对象调用 [operator []]、[remove] 或 [containsKey]，可以提供一个额外的 `isValidKey` 判定函数。在对可能不是 [K] 类型的参数值使用 `compare` 函数之前，会先测试该函数。如果省略，`isValidKey` 函数默认测试该值是否为 [K] 类型。

**注意：** 在对映射执行操作期间，不要修改该映射（添加或删除键），例如在 [forEach] 或 [putIfAbsent] 调用期间被调用的函数中，或者在遍历映射（[keys]、[values] 或 [entries]）时。

示例：

```dart
final planetsByMass = SplayTreeMap<double, String>((a, b) => a.compareTo(b));
```

要向映射添加数据，请使用 [operator[]=]、[addAll] 或 [addEntries]。

```dart
planetsByMass[0.06] = 'Mercury';
planetsByMass
    .addAll({0.81: 'Venus', 1.0: 'Earth', 0.11: 'Mars', 317.83: 'Jupiter'});
```

要检查映射是否为空，请使用 [isEmpty] 或 [isNotEmpty]。要获取映射条目的数量，请使用 [length]。

```dart
print(planetsByMass.isEmpty); // false
print(planetsByMass.length); // 5
```

[forEach] 方法会为映射中的每个键/值条目调用一个函数。

```dart
planetsByMass.forEach((key, value) {
  print('$key \t $value');
  // 0.06    Mercury
  // 0.11    Mars
  // 0.81    Venus
  // 1.0     Earth
  // 317.83  Jupiter
});
```

要检查映射是否包含具有特定键的条目，请使用 [containsKey]。

```dart
final keyOneExists = planetsByMass.containsKey(1.0); // true
final keyFiveExists = planetsByMass.containsKey(5); // false
```

要检查映射是否包含具有特定值的条目，请使用 [containsValue]。

```dart
final earthExists = planetsByMass.containsValue('Earth'); // true
final plutoExists = planetsByMass.containsValue('Pluto'); // false
```

要删除具有特定键的条目，请使用 [remove]。

```dart
final removedValue = planetsByMass.remove(1.0);
print(removedValue); // Earth
```

要根据键和值同时删除多个条目，请使用 [removeWhere]。

```dart
planetsByMass.removeWhere((key, value) => key <= 1);
print(planetsByMass); // {317.83: Jupiter}
```

要根据特定键是否已存在条目，有条件地添加或修改该键的值，请使用 [putIfAbsent] 或 [update]。

```dart
planetsByMass.update(1, (v) => '', ifAbsent: () => 'Earth');
planetsByMass.putIfAbsent(317.83, () => 'Another Jupiter');
print(planetsByMass); // {1.0: Earth, 317.83: Jupiter}
```

要根据现有的键和值更新所有键的值，请使用 [updateAll]。

```dart
planetsByMass.updateAll((key, value) => 'X');
print(planetsByMass); // {1.0: X, 317.83: X}
```

要删除所有条目并清空映射，请使用 [clear]。

```dart
planetsByMass.clear();
print(planetsByMass.isEmpty); // false
print(planetsByMass); // {}
```

**另请参阅：**

- [Map](https://www.yuque.com/thyname/dart.core/map)，键/值对集合的通用接口。
- [HashMap](https://www.yuque.com/thyname/dart.collection/hashmap) 是无序的（不保证迭代顺序）。
- [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap) 按键的插入顺序进行迭代。

## 构造函数

### SplayTreeMap()

```dart
SplayTreeMap<K, V>([
  int Function(K, K)? compare,
  bool Function(dynamic)? isValidKey
])
```

### SplayTreeMap.from()

```dart
SplayTreeMap<K, V>.from(
  Map<Object?, Object?> other, [
  int Function(K, K)? compare,
  bool Function(dynamic)? isValidKey,
])
```

创建一个包含 [other] 中所有键/值对的 [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap)。

所有键必须是 [K] 的实例，所有值必须是 [V] 的实例。[other] 映射本身可以是任意类型。示例：

```dart
final baseMap = <int, Object>{3: 'C', 1: 'A', 2: 'B'};
final fromBaseMap = SplayTreeMap<int, String>.from(baseMap);
print(fromBaseMap); // {1: A, 2: B, 3: C}
```

### SplayTreeMap.of()

```dart
SplayTreeMap<K, V>.of(
  Map<K, V> other, [
  int Function(K, K)? compare,
  bool Function(dynamic)? isValidKey,
])
```

创建一个包含 [other] 中所有键/值对的 [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap)。示例：

```dart
final baseMap = <int, String>{3: 'A', 2: 'B', 1: 'C', 4: 'D'};
final mapOf = SplayTreeMap<num, Object>.of(baseMap);
print(mapOf); // {1: C, 2: B, 3: A, 4: D}
```

### SplayTreeMap.fromIterable()

```dart
SplayTreeMap<K, V>.fromIterable(
  Iterable<dynamic> iterable, {
  K Function(dynamic)? key,
  V Function(dynamic)? value,
  int Function(K, K)? compare,
  bool Function(dynamic)? isValidKey,
})
```

创建一个 [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap)，其键和值根据 [iterable] 计算得出。

对于 [iterable] 中的每个元素，此构造函数通过分别应用 [key] 和 [value] 来计算一个键/值对。

键/值对中的键不需要唯一。键的最后一次出现将直接覆盖之前的值。

如果没有为 [key] 和 [value] 指定函数，则默认使用可迭代对象的值本身。示例：

```dart
final numbers = [12, 11, 14, 13];
final mapFromIterable =
    SplayTreeMap<int, int>.fromIterable(numbers,
        key: (i) => i, value: (i) => i * i);
print(mapFromIterable); // {11: 121, 12: 144, 13: 169, 14: 196}
```

### SplayTreeMap.fromIterables()

```dart
SplayTreeMap<K, V>.fromIterables(
  Iterable<K> keys,
  Iterable<V> values, [
  int Function(K, K)? compare,
  bool Function(dynamic)? isValidKey,
])
```

创建一个将给定的 [keys] 与 [values] 相关联的 [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap)。

此构造函数遍历 [keys] 和 [values]，将 [keys] 中的每个元素映射到 [values] 中对应的元素。

如果 [keys] 中多次包含同一个对象，则最后一次出现会覆盖之前的值。

如果两个 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 的长度不一致，则会产生错误。示例：

```dart
final keys = ['1', '2', '3', '4'];
final values = ['A', 'B', 'C', 'D'];
final mapFromIterables = SplayTreeMap.fromIterables(keys, values);
print(mapFromIterables); // {1: A, 2: B, 3: C, 4: D}
```

## 属性

- isEmpty
- isNotEmpty
- length
- keys
- values
- entries

## 运算符

- operator []
- operator []=
-

## 方法

- remove()
- putIfAbsent()
- update()
- updateAll()
- addAll()
- forEach()
- clear()
- containsKey()
- containsValue()

### firstKey()

```dart
K? firstKey()
```

映射中的第一个键。

如果映射为空，则返回 `null`。

### lastKey()

```dart
K? lastKey()
```

映射中的最后一个键。

如果映射为空，则返回 `null`。

### lastKeyBefore()

```dart
K? lastKeyBefore(K key)
```

映射中严格小于 [key] 的最后一个键。

如果未找到这样的键，则返回 `null`。

### firstKeyAfter()

```dart
K? firstKeyAfter(K key)
```

获取映射中严格大于 [key] 的第一个键。如果未找到这样的键，则返回 `null`。

---

# SplayTreeSet

```dart
final class SplayTreeSet<E> extends _SplayTree<E, _SplayTreeSetNode<E>> with Iterable<E>, SetMixin<E> {}
```

一个可以按彼此顺序排列的对象组成的 [Set](https://www.yuque.com/thyname/dart.core/set)。

该集合基于自平衡二叉树实现，支持大多数操作，均摊时间复杂度为对数级。

集合中的元素使用构造函数中传入的 `compare` 函数进行比较，该函数同时用于排序和判断相等性。如果集合中只包含对象 `a`，那么 `set.contains(b)` 仅当 `compare(a, b) == 0` 时才返回 `true`，甚至不会检查 `a == b` 的值。如果省略 compare 函数，则假定对象是可比较的（[Comparable](https://www.yuque.com/thyname/dart.core/comparable)），并使用其 [Comparable.compareTo] 方法进行比较。在这种情况下，不可比较的对象（包括 `null`）不能用作元素。

**注意：** 在对集合执行操作期间，不要修改该集合（添加或删除元素），例如在 [forEach] 或 [containsAll] 调用期间被调用的函数中，或者在遍历集合时。

当元素处于集合中时，不要以改变其相等性（从而改变其哈希码）的方式修改元素。某些特殊类型的集合在相等性方面可能更为宽松，在这种情况下，它们应当记录其不同的行为和限制。

示例：

```dart
final planets = SplayTreeSet<String>((a, b) => a.compareTo(b));
```

要向集合添加数据，请使用 [add] 或 [addAll]。

```dart
planets.add('Neptune');
planets.addAll({'Venus', 'Mars', 'Earth', 'Jupiter'});
print(planets); // {Earth, Jupiter, Mars, Neptune, Venus}
```

要检查集合是否为空，请使用 [isEmpty] 或 [isNotEmpty]。要获取集合中元素的数量，请使用 [length]。

```dart
final isEmpty = planets.isEmpty; // false
final length = planets.length; // 5
```

要检查集合是否包含特定元素，请使用 [contains]。

```dart
final marsExists = planets.contains('Mars'); // true
```

要通过索引获取元素值，请使用 [elementAt]。

```dart
final elementAt = planets.elementAt(1);
print(elementAt); // Jupiter
```

要创建集合的副本，请使用 [toSet]。

```dart
final copySet = planets.toSet(); // a `SplayTreeSet` with the same ordering.
print(copySet); // {Earth, Jupiter, Mars, Neptune, Venus}
```

要删除一个元素，请使用 [remove]。

```dart
final removedValue = planets.remove('Mars'); // true
print(planets); // {Earth, Jupiter, Neptune, Venus}
```

要同时删除多个元素，请使用 [removeWhere]。

```dart
planets.removeWhere((element) => element.startsWith('J'));
print(planets); // {Earth, Neptune, Venus}
```

要删除此集合中所有不满足条件的元素，请使用 [retainWhere]。

```dart
planets.retainWhere((element) => element.contains('Earth'));
print(planets); // {Earth}
```

要删除所有元素并清空集合，请使用 [clear]。

```dart
planets.clear();
print(planets.isEmpty); // true
print(planets); // {}
```

**另请参阅：**

- [Set](https://www.yuque.com/thyname/dart.core/set) 是对象集合的基类。
- [HashSet](https://www.yuque.com/thyname/dart.collection/hashset) 不保证迭代中对象的顺序。
- [LinkedHashSet](https://www.yuque.com/thyname/dart.collection/linkedhashset) 按插入顺序存储对象。

## 构造函数

### SplayTreeSet()

```dart
SplayTreeSet([
  int Function(E key1, E key2)? compare,
  bool Function(dynamic potentialKey)? isValidKey,
])
```

使用给定的 compare 函数创建一个新的 [SplayTreeSet](https://www.yuque.com/thyname/dart.collection/splaytreeset)。

如果省略 [compare] 函数，则默认使用 [Comparable.compare]，此时元素必须是可比较的。

提供的 `compare` 函数可能并不适用于所有对象，甚至可能不适用于所有 `E` 实例。

对于向集合添加元素的操作，用户不应传入与 compare 函数不兼容的对象。

[contains]、[remove]、[lookup]、[removeAll] 或 [retainAll] 等方法的类型被定义为可以接受任意对象，[isValidKey] 测试可用于在将这些对象交给 `compare` 函数之前对其进行过滤。

如果提供了 [isValidKey]，则在上述方法中，只有满足 `isValidKey(other)` 的值才会使用 `compare` 方法进行比较。如果 `isValidKey` 函数对某个对象返回 false，则认为该对象不在集合中。

如果省略，`isValidKey` 函数默认检查类型参数：`other is E`。

### SplayTreeSet.from()

```dart
factory SplayTreeSet.from(
  Iterable elements, [
  int Function(E key1, E key2)? compare,
  bool Function(dynamic potentialKey)? isValidKey,
])
```

创建一个包含所有 [elements] 的 [SplayTreeSet](https://www.yuque.com/thyname/dart.collection/splaytreeset)。

该集合的工作方式如同通过 `SplayTreeSet<E>(compare, isValidKey)` 创建一样。

所有 [elements] 都应该是 [E] 的实例，并且是 [compare] 的有效参数。`elements` 可迭代对象本身可以具有任意元素类型，因此该构造函数可用于对 `Set` 进行向下转型，例如：

```dart
Set<SuperType> superSet = ...;
Set<SubType> subSet =
    SplayTreeSet<SubType>.from(superSet.whereType<SubType>());
```

示例：

```dart
final numbers = <num>[20, 30, 10];
final setFrom = SplayTreeSet<int>.from(numbers);
print(setFrom); // {10, 20, 30}
```

### SplayTreeSet.of()

```dart
factory SplayTreeSet.of(
  Iterable<E> elements, [
  int Function(E key1, E key2)? compare,
  bool Function(dynamic potentialKey)? isValidKey,
])
```

根据 [elements] 创建一个 [SplayTreeSet](https://www.yuque.com/thyname/dart.collection/splaytreeset)。

该集合的工作方式如同通过 `new SplayTreeSet<E>(compare, isValidKey)` 创建一样。

所有 [elements] 都应该是 [compare] 函数的有效参数。示例：

```dart
final baseSet = <int>{1, 2, 3};
final setOf = SplayTreeSet<num>.of(baseSet);
print(setOf); // {1, 2, 3}
```

## 属性

- iterator
- length
- isEmpty
- isNotEmpty
- first
- last
- single

## 方法

- cast()
- contains()
- add()
- remove()
- addAll()
- removeAll()
- retainAll()
- lookup()
- intersection()
- difference()
- union()
- clear()
- toSet()
- toString()
