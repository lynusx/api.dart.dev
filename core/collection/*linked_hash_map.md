# LinkedHashMap

```dart
abstract final class LinkedHashMap<K, V> implements Map<K, V> {}
```

一个按插入顺序排列的 [Map]，预期具备常数时间的查找性能。

非常量的映射字面量（如 `{"a": 42, "b": 7}`）就是一个 `LinkedHashMap`。

[keys]、[values] 和 [entries] 按键的插入顺序进行迭代。

该映射使用哈希表来查找条目，因此键必须具备合适的 [Object.operator==] 和 [Object.hashCode] 实现。如果哈希码分布不均，映射操作的性能可能会受到影响。

键的插入顺序会被记住，键按照其插入映射的顺序进行迭代。值和条目按照其对应键的顺序进行迭代。当某个键已存在于映射中时，更改该键的值不会改变迭代顺序；但如果先移除该键再重新添加，则该键会成为迭代顺序中的最后一个。

**注意：** 在对映射执行某项操作期间（例如在 [forEach] 或 [putIfAbsent] 调用期间调用的函数中，或在迭代映射（[keys]、[values] 或 [entries]）期间），不要修改该映射（添加或移除键）。

`LinkedHashMap` 的键必须具备一致的 [Object.==] 和 [Object.hashCode] 实现。这意味着 `==` 运算符必须在键上定义一个稳定的等价关系（自反、对称、传递，且随时间保持一致），并且对于被 `==` 判定为相等的对象，`hashCode` 必须相同。

示例：

```dart
final planetsByDiameter = {0.949: 'Venus'}; // 一个新的 LinkedHashMap
```

要向映射中添加数据，可使用 [operator[]=]、[addAll] 或 [addEntries]。

```dart continued
planetsByDiameter[1] = 'Earth';
planetsByDiameter.addAll({0.532: 'Mars', 11.209: 'Jupiter'});
```

要检查映射是否为空，可使用 [isEmpty] 或 [isNotEmpty]。要获取映射条目的数量，可使用 [length]。

```dart continued
print(planetsByDiameter.isEmpty); // false
print(planetsByDiameter.length); // 4
print(planetsByDiameter);
// {0.949: Venus, 1.0: Earth, 0.532: Mars, 11.209: Jupiter}
```

[forEach] 方法会为映射中的每个键/值条目调用一个函数。

```dart continued
planetsByDiameter.forEach((key, value) {
  print('$key \t $value');
  // 0.949    Venus
  // 1.0      Earth
  // 0.532    Mars
  // 11.209   Jupiter
});
```

要检查映射中是否存在具有特定键的条目，可使用 [containsKey]。

```dart continued
final keyOneExists = planetsByDiameter.containsKey(1); // true
final keyFiveExists = planetsByDiameter.containsKey(5); // false
```

要检查映射中是否存在具有特定值的条目，可使用 [containsValue]。

```dart continued
final earthExists = planetsByDiameter.containsValue('Earth'); // true
final saturnExists =  planetsByDiameter.containsValue('Saturn'); // false
```

要移除具有特定键的条目，可使用 [remove]。

```dart continued
final removedValue = planetsByDiameter.remove(1);
print(removedValue); // Earth
print(planetsByDiameter); // {0.949: Venus, 0.532: Mars, 11.209: Jupiter}
```

要根据键和值同时移除多个条目，可使用 [removeWhere]。

```dart continued
planetsByDiameter.removeWhere((key, value) => key == 0.949);
print(planetsByDiameter); // {0.532: Mars, 11.209: Jupiter}
```

要根据映射中是否已存在具有该键的条目，来有条件地添加或修改某个键对应的值，可使用 [putIfAbsent] 或 [update]。

```dart continued
planetsByDiameter.update(0.949, (v) => 'Venus', ifAbsent: () => 'Venus');
planetsByDiameter.putIfAbsent(0.532, () => "Another Mars if needed");
print(planetsByDiameter); // {0.532: Mars, 11.209: Jupiter, 0.949: Venus}
```

要基于现有的键和值更新所有键的值，可使用 [updateAll]。

```dart continued
planetsByDiameter.updateAll((key, value) => 'X');
print(planetsByDiameter); // {0.532: X, 11.209: X, 0.949: X}
```

要移除所有条目并清空映射，可使用 [clear]。

```dart continued
planetsByDiameter.clear();
print(planetsByDiameter); // {}
print(planetsByDiameter.isEmpty); // true
```

**另请参阅：**

- [Map]，键/值对集合的通用接口。
- [HashMap] 是无序的（迭代顺序不保证）。
- [SplayTreeMap] 按排序顺序迭代键。

## 构造函数

### LinkedHashMap()

```dart
LinkedHashMap<K, V>({
  bool equals( K, K )?,
  int hashCode( K )?,
  bool isValidKey( dynamic )?,
})
```

创建一个基于哈希表、按插入顺序排列的 [Map]。

如果提供了 [equals]，则使用它来比较表中的键与新键。如果省略 [equals]，则改用键自身的 [Object.==]。[equals] 函数**禁止**修改使用它作为等价判断依据的映射。如果发生了修改，其结果行为是未指定的。

类似地，如果提供了 [hashCode]，则使用它来为键生成哈希值，以便将其放入哈希表中。如果省略，则使用键自身的 [Object.hashCode]。[hashCode] 函数**禁止**修改使用它作为哈希码依据的映射。如果发生了修改，其结果行为是未指定的。

所使用的 `equals` 和 `hashCode` 方法应始终保持一致，即如果 `equals(a, b)` 成立，则 `hashCode(a) == hashCode(b)` 也应成立。对象的哈希值，或其判定相等的对象，在该对象位于表中期间不应发生变化。如果对象的哈希码或相等性确实发生了变化，则结果行为是未指定的。

如果提供了 [equals] 或 [hashCode] 中的一个，通常也应同时提供另一个。

某些 [equals] 或 [hashCode] 函数可能并不适用于所有对象。如果提供了 [isValidKey]，则用它来检查某个不一定是 [K] 实例的潜在键，例如传递给 [operator []]、[remove] 和 [containsKey] 的参数，这些参数的类型都是 `Object?`。如果 [isValidKey] 对某个对象返回 `false`，则不会调用 [equals] 和 [hashCode] 函数，并且认为映射中不存在与该对象相等的键。[isValidKey] 函数默认只测试对象是否为 [K] 的实例。

示例：

```dart template:expression
LikedHashMap<int,int>(equals: (int a, int b) => (b - a) % 5 == 0,
                      hashCode: (int e) => e % 5)
```

该示例映射不需要传入 `isValidKey` 函数。默认函数只接受 `int` 类型的值，而这类值可以安全地传给 `equals` 和 `hashCode` 函数。

如果 `equals`、`hashCode` 和 `isValidKey` 都未提供，则默认的 `isValidKey` 会接受所有键。此时假定默认的相等性和哈希码操作对所有对象都适用。

同样，如果 `equals` 为 [identical]，`hashCode` 为 [identityHashCode]，且省略了 `isValidKey`，则得到的映射是基于标识（identity）的，此时 `isValidKey` 默认接受所有键。这样的映射可以直接使用 [LinkedHashMap.identity] 创建。

### LinkedHashMap.identity()

```dart
LinkedHashMap<K, V>.identity()
```

创建一个按插入顺序排列、基于标识（identity）的映射。

实际上等价于以下简写形式：

```dart template:expression
LinkedHashMap<K, V>(equals: identical,
                    hashCode: identityHashCode)
```

### LinkedHashMap.from()

```dart
LinkedHashMap<K, V>.from(Map other )
```

创建一个包含 [other] 中所有键值对的 [LinkedHashMap]。

所有键都必须是 [K] 的实例，所有值都必须是 [V] 的实例。[other] 映射本身可以是任意类型。示例：

```dart
final baseMap = <num, Object>{1: 'A', 2: 'B', 3: 'C'};
final fromBaseMap = LinkedHashMap<int, String>.from(baseMap);
print(fromBaseMap); // {1: A, 2: B, 3: C}
```

### LinkedHashMap.of()

```dart
LinkedHashMap<K, V>.of(Map<K, V> other )
```

创建一个包含 [other] 中所有键值对的 [LinkedHashMap]。示例：

```dart
final baseMap = <int, String>{3: 'A', 2: 'B', 1: 'C', 4: 'D'};
final mapOf = LinkedHashMap<num, Object>.of(baseMap);
print(mapOf); // {3: A, 2: B, 1: C, 4: D}
```

### LinkedHashMap.fromIterable()

```dart
LinkedHashMap<K, V>.fromIterable(
  Iterable iterable, {
  K key( dynamic element )?,
  V value( dynamic element )?,
})
```

创建一个 [LinkedHashMap]，其键和值由 [iterable] 计算得出。

对于 [iterable] 中的每个元素，此构造函数会分别应用 [key] 和 [value] 来计算出一个键/值对。

键/值对中的键不要求唯一。如果出现重复的键，后出现的键会直接覆盖之前的值。

如果未为 [key] 和 [value] 指定函数，则两者默认均为恒等函数（identity function）。示例：

```dart
final numbers = [11, 12, 13, 14];
final mapFromIterable =
    LinkedHashMap.fromIterable(numbers, key: (i) => i, value: (i) => i * i);
print(mapFromIterable); // {11: 121, 12: 144, 13: 169, 14: 196}
```

### LinkedHashMap.fromIterables()

```dart
LinkedHashMap<K, V>.fromIterables(Iterable<K> keys, Iterable<V> values)
```

创建一个将给定的 [keys] 关联到 [values] 的 [LinkedHashMap]。

此构造函数会遍历 [keys] 和 [values]，将 [keys] 中的每个元素映射到 [values] 中对应位置的元素。

如果 [keys] 中包含多次相同的对象，则后出现的会覆盖之前的值。

两个 [Iterable] 的长度必须相同，否则会产生错误。示例：

```dart
final values = [0.06, 0.81, 1, 0.11];
final keys = ['Mercury', 'Venus', 'Earth', 'Mars'];
final mapFromIterables = LinkedHashMap.fromIterables(keys, values);
print(mapFromIterables);
// {Mercury: 0.06, Venus: 0.81, Earth: 1, Mars: 0.11}
```

### LinkedHashMap.fromEntries()

```dart
LinkedHashMap<K, V>.fromEntries(Iterable<MapEntry<K, V>> entries)
```

创建一个包含 [entries] 中所有条目的 [LinkedHashMap]。

返回一个新的 `LinkedHashMap<K, V>`，[entries] 中的所有条目会按迭代顺序被添加到其中。

如果 [entries] 中存在多个具有相同键的条目，后出现的会覆盖先出现的。示例：

```dart
final numbers = [11, 12, 13, 14];
final map = LinkedHashMap.fromEntries(numbers.map((i) => MapEntry(i, i * i)));
print(map); // {11: 121, 12: 144, 13: 169, 14: 196}
```
