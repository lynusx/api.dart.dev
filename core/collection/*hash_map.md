# HashMap

```dart
abstract final class HashMap<K, V> implements Map<K, V> {}
```

基于哈希表实现的 [Map]。

[HashMap] 是无序的（不保证迭代顺序）。

`HashMap` 的键必须具有一致的 [Object.==] 和 [Object.hashCode] 实现。这意味着 `==` 运算符必须在键上定义一个稳定的等价关系（自反、对称、传递，并且随时间保持一致），并且对于通过 `==` 判定为相等的对象，其 `hashCode` 必须相同。

遍历映射的键、值或条目（通过 [forEach]）可能以任意顺序进行。迭代顺序仅在映射被修改时才会改变。值的迭代顺序与其关联键的迭代顺序相同，因此并行遍历 [keys] 和 [values] 会得到相互匹配的键值对。

**注意：** 在对映射执行操作期间（例如在 [forEach] 或 [putIfAbsent] 调用期间被调用的函数中），或在遍历映射（[keys]、[values] 或 [entries]）期间，不要修改该映射（添加或移除键）。修改现有键的值（例如使用 [operator[]=] 或 [update]）不会影响映射的结构，也不会破坏迭代过程。

当键处于映射中时，不要以任何会改变其相等性（进而改变其哈希码）的方式修改键。如果映射中某个键的 [Object.hashCode] 发生变化，可能会导致之后对该键的查找失败。

示例：

```dart
final Map<int, String> planets = HashMap(); // 这是一个 HashMap
```

要向映射中添加数据，可使用 [operator[]=]、[addAll] 或 [addEntries]。

```dart continued
planets[3] = 'Earth';
planets.addAll({4: 'Mars'});
final gasGiants = {6: 'Jupiter', 5: 'Saturn'};
planets.addEntries(gasGiants.entries);
print(planets); // fx {5: Saturn, 6: Jupiter, 3: Earth, 4: Mars}
```

要检查映射是否为空，可使用 [isEmpty] 或 [isNotEmpty]。要获取映射条目的数量，可使用 [length]。

```dart continued
final isEmpty = planets.isEmpty; // false
final length = planets.length; // 4
```

[forEach] 用于遍历映射的所有条目。

```dart continued
planets.forEach((key, value) {
  print('$key \t $value');
  // 5        Saturn
  // 4        Mars
  // 3        Earth
  // 6        Jupiter
});
```

要检查映射中是否存在带有特定键的条目，可使用 [containsKey]。

```dart continued
final keyOneExists = planets.containsKey(4); // true
final keyFiveExists = planets.containsKey(1); // false
```

要检查映射中是否存在带有特定值的条目，可使用 [containsValue]。

```dart continued
final marsExists = planets.containsValue('Mars'); // true
final venusExists = planets.containsValue('Venus'); // false
```

要移除带有特定键的条目，可使用 [remove]。

```dart continued
final removeValue = planets.remove(6);
print(removeValue); // Jupiter
print(planets); // fx {4: Mars, 3: Earth, 5: Saturn}
```

要根据键和值同时移除多个条目，可使用 [removeWhere]。

```dart continued
planets.removeWhere((key, value) => key == 5);
print(planets); // fx {3: Earth, 4: Mars}
```

要根据某个键是否已存在条目，有条件地添加或修改该键对应的值，可使用 [putIfAbsent] 或 [update]。

```dart continued
planets.update(4, (v) => 'Saturn');
planets.update(8, (v) => '', ifAbsent: () => 'Neptune');
planets.putIfAbsent(4, () => 'Another Saturn');
print(planets); // fx {4: Saturn, 8: Neptune, 3: Earth}
```

要根据现有的键和值更新所有键对应的值，可使用 [updateAll]。

```dart continued
planets.updateAll((key, value) => 'X');
print(planets); // fx {8: X, 3: X, 4: X}
```

要移除所有条目并清空映射，可使用 [clear]。

```dart continued
planets.clear();
print(planets); // {}
print(planets.isEmpty); // true
```

**另请参阅：**

- [Map]，键/值对集合的通用接口。
- [LinkedHashMap] 按键的插入顺序进行迭代。
- [SplayTreeMap] 按排序顺序迭代键。

## 构造函数

### HashMap()

```dart
HashMap<K, V>({
  bool equals( K, K )?,
  int hashCode( K )?,
  bool isValidKey( dynamic )?,
})
```

创建一个基于哈希表的无序 [Map]。

创建的映射不以任何方式排序。在迭代键或值时，迭代顺序是不确定的，只是在映射未被修改的情况下会保持不变。

如果提供了 [equals]，则使用它来比较映射中的键与新键。如果省略 [equals]，则改用键自身的 [Object.==]。[equals] 函数**不得**修改以它作为相等性判断依据的映射。如果这样做，产生的行为是未定义的。

类似地，如果提供了 [hashCode]，则使用它为键生成哈希值，以便将键放置到映射中。如果省略 [hashCode]，则使用键自身的 [Object.hashCode]。[hashCode] 函数**不得**修改以它作为哈希码依据的映射。如果这样做，产生的行为是未定义的。

所使用的 `equals` 和 `hashCode` 方法应始终保持一致，即如果 `equals(a, b)` 成立，则 `hashCode(a) == hashCode(b)` 也应成立。当对象作为映射中的键时，该对象的哈希值及其相等性判断结果都不应发生变化。如果对象的哈希码或相等性确实发生了变化，产生的行为是未定义的。

如果提供了 [equals] 和 [hashCode] 中的一个，通常也应同时提供另一个。

某些 [equals] 或 [hashCode] 函数可能并不适用于所有对象。如果提供了 [isValidKey]，则用它来检查一个潜在的键——该键不一定是 [K] 的实例，例如 [operator []]、[remove] 和 [containsKey] 的参数类型均为 `Object?`。如果 [isValidKey] 对某个对象返回 `false`，则不会调用 [equals] 和 [hashCode] 函数，并且认为映射中不存在与该对象相等的键。[isValidKey] 函数默认只检测对象是否为 [K] 的实例。

示例：

```dart template:expression
HashMap<int,int>(equals: (int a, int b) => (b - a) % 5 == 0,
                 hashCode: (int e) => e % 5)
```

此示例中的映射不需要传入 `isValidKey` 函数。默认函数只接受 `int` 值，可以安全地传给 `equals` 和 `hashCode` 函数。

如果既未提供 `equals`、`hashCode`，也未提供 `isValidKey`，则默认的 `isValidKey` 会接受所有键。默认的相等性和哈希码运算已知适用于所有对象。

同样，如果 `equals` 为 [identical]，`hashCode` 为 [identityHashCode]，并且省略了 `isValidKey`，则生成的映射基于对象标识（identity），且 `isValidKey` 默认接受所有键。可以直接使用 [HashMap.identity] 创建这样的映射。

### HashMap.identity()

```dart
HashMap<K, V>.identity()
```

创建一个基于标识（identity）的无序映射。

此映射中的键仅在是同一个对象时才被视为相等，完全不使用 [Object.==]。

实际上等价于以下写法的简写：

```dart
HashMap<K, V>(equals: identical, hashCode: identityHashCode)
```

### HashMap.from()

```dart
HashMap<K, V>.from( Map other )
```

创建一个包含 [other] 中所有键/值对的 [HashMap]。

所有键必须是 [K] 的实例，所有值必须是 [V] 的实例。[other] 映射本身可以是任意类型。

```dart
final baseMap = {1: 'A', 2: 'B', 3: 'C'};
final fromBaseMap = HashMap<int, String>.from(baseMap);
print(fromBaseMap); // {1: A, 2: B, 3: C}
```

### HashMap.of()

```dart
HashMap<K, V>.of( Map<K, V> other )
```

创建一个包含 [other] 中所有键/值对的 [HashMap]。示例：

```dart
final baseMap = <int, String>{1: 'A', 2: 'B', 3: 'C'};
final mapOf = HashMap<num, Object>.of(baseMap);
print(mapOf); // {1: A, 2: B, 3: C}
```

### HashMap.fromIterable()

```dart
HashMap<K, V>.fromIterable(
  Iterable iterable, {
  K key( dynamic element )?,
  V value( dynamic element )?,
})
```

创建一个 [HashMap]，其键和值是根据 [iterable] 计算得出的。

对于 [iterable] 中的每个元素，此构造函数会通过分别应用 [key] 和 [value] 来计算出一个键/值对。

键/值对中的键不需要唯一。某个键最后一次出现时的值会直接覆盖之前的值。

如果没有为 [key] 和 [value] 指定值，则默认使用恒等函数。示例：

```dart
final numbers = [11, 12, 13, 14];
final mapFromIterable = HashMap<int, int>.fromIterable(numbers,
    key: (i) => i, value: (i) => i * i);
print(mapFromIterable); // {11: 121, 12: 144, 13: 169, 14: 196}
```

### HashMap.fromIterables()

```dart
HashMap<K, V>.fromIterables(Iterable<K> keys, Iterable<V> values)
```

创建一个 [HashMap]，将给定的 [keys] 与 [values] 关联起来。

此构造函数会遍历 [keys] 和 [values]，将 [keys] 中的每个元素映射到 [values] 中对应的元素。

如果 [keys] 中多次包含同一个对象，则最后一次出现时会覆盖之前的值。

如果两个 [Iterable] 的长度不相同，则会产生错误。示例：

```dart
final keys = ['Mercury', 'Venus', 'Earth', 'Mars'];
final values = [0.06, 0.81, 1, 0.11];
final mapFromIterables = HashMap.fromIterables(keys, values);
print(mapFromIterables);
// {Earth: 1, Mercury: 0.06, Mars: 0.11, Venus: 0.81}
```

### HashMap.fromEntries()

```dart
HashMap<K, V>.fromEntries(Iterable<MapEntry<K, V>> entries)
```

创建一个包含 [entries] 中所有条目的 [HashMap]。

返回一个新的 `HashMap<K, V>`，[entries] 中的所有条目均按迭代顺序被添加进去。

如果多个 [entries] 拥有相同的键，则后出现的条目会覆盖先出现的条目。

示例：

```dart
final numbers = [11, 12, 13, 14];
final map = HashMap.fromEntries(numbers.map((i) => MapEntry(i, i * i)));
print(map); // {11: 121, 12: 144, 13: 169, 14: 196}
```
