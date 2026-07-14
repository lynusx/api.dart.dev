# Map

```dart
abstract interface class Map<K, V> {}
```

一个键/值对的集合，可以通过其关联的键来检索值。

映射中的键数量是有限的，且每个键都恰好关联一个值。

映射及其键和值都可以被迭代。迭代顺序由映射的具体类型决定。示例：

- 普通的 [HashMap](https://www.yuque.com/thyname/dart.collection/hashmap) 是无序的（不保证任何顺序）；
- [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap) 按键的插入顺序迭代；
- 而像 [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap) 这样的有序映射按键的排序顺序迭代。

通常不允许在对映射执行操作期间修改映射（添加或删除键），例如在 [forEach] 调用期间调用的函数中。在迭代键或值时修改映射也可能破坏迭代。

通常不允许在键位于映射中时修改键的相等性（因此也不能修改其哈希码）。某些特殊子类型可能更宽松，此时应记录该行为。

键的相等性必须是一种等价关系。如果存储在映射中的键与用于查找的键在是否相等上不一致（即相等性不具有 _对称性_），则查找行为是未指定的。

## 构造函数

### Map()

```dart
Map<K, V>()
```

创建一个空的 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap)。

此构造函数等价于非 const 的映射字面量 `<K, V>{}`。

`LinkedHashMap` 要求键实现兼容的 `operator==` 和 `hashCode`。它按键的插入顺序迭代。

### Map.from()

```dart
Map<K, V>.from( Map other )
```

创建一个与 [other] 具有相同键和值的 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap)。

所有键必须是 [K] 的实例，所有值必须是 [V] 的实例。与 [Map.of] 不同，[other] 映射本身可以是任意类型，键和值的类型会在运行时进行检查（且可能失败）。

尽可能优先使用 [Map.of]；仅在需要创建一个比原映射类型更精确的新映射，且已知所有键和值都具有这些更精确的类型时，才使用 `Map.from`。

`LinkedHashMap` 要求键实现兼容的 `operator==` 和 `hashCode`。它按键的插入顺序迭代。

```dart
final planets = <num, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth', 4: 'Mars'};
final mapFrom = Map<int, String>.from(planets);
print(mapFrom); // {1: Mercury, 2: Venus, 3: Earth, 4: Mars}
```

### Map.of()

```dart
Map<K, V>.of( Map<K, V> other )
```

创建一个与 [other] 具有相同键和值的 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap)。

`LinkedHashMap` 要求键实现兼容的 `operator==` 和 `hashCode`，并且允许 `null` 作为键。它按键的插入顺序迭代。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
final mapOf = Map<num, String>.of(planets);
print(mapOf); // {1: Mercury, 2: Venus, 3: Earth}
```

### Map.unmodifiable()

```dart
Map<K, V>.unmodifiable( Map other )
```

创建一个包含 [other] 中所有条目的不可修改的基于哈希的映射。

所有键必须是 [K] 的实例，所有值必须是 [V] 的实例。[other] 映射本身可以是任意类型。

该映射要求键实现兼容的 `operator==` 和 `hashCode`。创建的映射按 [other] 提供的顺序以固定顺序迭代键。

生成的映射行为与 [Map.from] 的结果相同，只是此构造函数返回的映射不可修改。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
final unmodifiableMap = Map.unmodifiable(planets);
unmodifiableMap[4] = 'Mars'; // Throws
```

### Map.unmodifiableOf()

```dart
Map<K, V>.unmodifiableOf(Map<K, V> other)
```

创建一个包含 [other] 中所有条目的不可修改的基于哈希的映射。

该映射要求键实现兼容的 `operator==` 和 `hashCode`。创建的映射按 [other] 提供的顺序以固定顺序迭代键。

生成的映射行为与 [Map.of] 的结果相同，只是此构造函数返回的映射不可修改。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
final unmodifiableMap = Map.unmodifiableOf(planets);
unmodifiableMap[4] = 'Mars'; // Throws
```

### Map.identity()

```dart
Map<K, V>.identity()
```

使用默认实现 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap) 创建一个恒等映射（identity map）。

恒等映射使用 [identical](https://www.yuque.com/thyname/dart.core/identical) 判断相等性，使用 [identityHashCode](https://www.yuque.com/thyname/dart.core/identityhashcode) 计算键的哈希码，而不是使用键本身的 [Object.==] 和 [Object.hashCode]。

该映射按键的插入顺序迭代。

### Map.fromIterable()

```dart
Map<K, V>.fromIterable(
  Iterable iterable, {
  K key( dynamic element )?,
  V value( dynamic element )?,
})
```

创建一个 Map 实例，其键和值根据 [iterable] 计算得出。

对于 [iterable] 的每个元素，分别应用 [key] 和 [value] 函数计算出一个键/值对。

等价于映射字面量：

```dart
<K, V>{for (var v in iterable) key(v): value(v)}
```

通常优先使用该字面量形式，因为它可以提供更精确的类型。

以下示例根据一个整数列表创建了一个新映射。`map` 的键是 `list` 中的值转换为字符串后的结果，`map` 的值是 `list` 中值的平方：

```dart
final numbers = <int>[1, 2, 3];
final map = Map<String, int>.fromIterable(numbers,
    key: (item) => item.toString(),
    value: (item) => item * item);
print(map); // {1: 1, 2: 4, 3: 9}
```

如果未为 [key] 和 [value] 指定函数，则默认使用恒等函数。此时，可迭代对象的元素必须可以赋值给创建的映射的键或值类型。

在以下示例中，`map` 的键和对应的值直接来自 `list` 的值：

```dart
final numbers = <int>[1, 2, 3];
final map = Map.fromIterable(numbers);
print(map); // {1: 1, 2: 2, 3: 3}
```

由源 [iterable] 计算出的键不要求唯一。后出现的键的值会覆盖之前出现的同一键的值。

创建的映射是 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap)。`LinkedHashMap` 要求键实现兼容的 `operator==` 和 `hashCode`。它按键的插入顺序迭代。

### Map.fromIterables()

```dart
Map<K, V>.fromIterables( Iterable<K> keys, Iterable<V> values )
```

创建一个将给定的 [keys] 与给定的 [values] 相关联的映射。

映射的构造过程会同时遍历 [keys] 和 [values]，并为每一对键和值向映射中添加一个条目。

```dart
final rings = <bool>[false, false, true, true];
final planets = <String>{'Earth', 'Mars', 'Jupiter', 'Saturn'};
final map = Map<String, bool>.fromIterables(planets, rings);
print(map); // {Earth: false, Mars: false, Jupiter: true, Saturn: true}
```

如果 [keys] 中多次包含同一对象，则最后一次出现的值会覆盖之前的值。

两个 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 必须具有相同的长度。

创建的映射是 [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap)。`LinkedHashMap` 要求键实现兼容的 `operator==` 和 `hashCode`。它按键的插入顺序迭代。

### Map.fromEntries()

```dart
Map<K, V>.fromEntries( Iterable<MapEntry<K, V>> entries )
```

创建一个新映射并添加所有条目。

返回一个新的 `Map<K, V>`，其中 [entries] 的所有条目均已按迭代顺序添加。

如果多个 [entries] 具有相同的键，后出现的会覆盖先出现的值。

等价于映射字面量：

```dart
<K, V>{for (var e in entries) e.key: e.value}
```

示例：

```dart
final moonCount = <String, int>{'Mercury': 0, 'Venus': 0, 'Earth': 1,
  'Mars': 2, 'Jupiter': 79, 'Saturn': 82, 'Uranus': 27, 'Neptune': 14};
final map = Map.fromEntries(moonCount.entries);
```

## 静态方法

### castFrom()

```dart
Map<K2, V2> castFrom<K, V, K2, V2>(Map<K, V> source)
```

将 [source] 适配为 `Map<K2, V2>`。

每当该集合会生成一个不是 [K2] 或 [V2] 的键或值时，访问操作都会抛出异常。

每当试图向适配后的映射中添加 [K2] 类型的键或 [V2] 类型的值时，存储操作都会抛出异常，除非该键同时也是 [K] 的实例，且该值同时也是 [V] 的实例。

如果 [source] 中所有被访问的条目都具有 [K2] 类型的键和 [V2] 类型的值，并且添加到返回的映射中的所有条目都具有 [K] 类型的键和 [V] 类型的值，那么返回的映射就可以作为 `Map<K2, V2>` 使用。

接受 `Object?` 作为参数的方法，如 [containsKey]、[remove] 和 [operator []]，会将参数直接传递给该映射的相应方法，而不进行任何检查。

## 属性

### entries

```dart
Iterable<MapEntry<K, V>> get entries
```

此 [Map](https://www.yuque.com/thyname/dart.core/map) 的映射条目。

### keys

```dart
Iterable<K> get keys
```

此 [Map](https://www.yuque.com/thyname/dart.core/map) 的键。

返回的可迭代对象具有高效的 `length` 和 `contains` 操作，分别基于映射的 [length] 和 [containsKey]。

迭代顺序由具体的 `Map` 实现决定，但在映射发生变化之间必须保持一致。

在迭代键的过程中修改映射可能会破坏迭代。

### values

```dart
Iterable<V> get values
```

此 [Map](https://www.yuque.com/thyname/dart.core/map) 的值。

值按其对应键的顺序进行迭代。这意味着并行迭代 [keys] 和 [values] 将得到匹配的键值对。

返回的可迭代对象具有基于映射 [length] 的高效 `length` 方法。其 [Iterable.contains] 方法基于 `==` 比较。

在迭代值的过程中修改映射可能会破坏迭代。

### length

```dart
int get length
```

映射中键/值对的数量。

### isEmpty

```dart
bool get isEmpty
```

映射中是否不存在任何键/值对。

### isNotEmpty

```dart
bool get isNotEmpty
```

映射中是否至少存在一个键/值对。

## 方法

### cast()

```dart
Map<RK, RV> cast<RK, RV>()
```

在必要时，提供此映射作为具有 [RK] 键和 [RV] 值实例的视图。

如果此映射已经是 `Map<RK, RV>`，则原样返回。

如果此集合只包含 [RK] 类型的键和 [RV] 类型的值，所有读取操作都能正常工作。如果某个操作暴露出非 [RK] 类型的键或非 [RV] 类型的值，该操作将抛出异常。

添加到映射中的条目必须同时对 `Map<K, V>` 和 `Map<RK, RV>` 有效。

接受 `Object?` 作为参数的方法，如 [containsKey]、[remove] 和 [operator []]，会将参数直接传递给该映射的相应方法，而不进行任何检查。这意味着即使看起来不应该有任何效果，`mapWithStringKeys.cast<int,int>().remove("a")` 也能成功执行。

### containsValue()

```dart
bool containsValue(Object? value)
```

此映射是否包含给定的 [value]。

如果映射中的任意值根据 `==` 运算符与 `value` 相等，则返回 true。

```dart
final moonCount = <String, int>{'Mercury': 0, 'Venus': 0, 'Earth': 1,
  'Mars': 2, 'Jupiter': 79, 'Saturn': 82, 'Uranus': 27, 'Neptune': 14};
final moons3 = moonCount.containsValue(3); // false
final moons82 = moonCount.containsValue(82); // true
```

### containsKey()

```dart
bool containsKey(Object? key)
```

此映射是否包含给定的 [key]。

如果映射中的任意键根据映射所使用的相等性与 `key` 相等，则返回 true。

```dart
final moonCount = <String, int>{'Mercury': 0, 'Venus': 0, 'Earth': 1,
  'Mars': 2, 'Jupiter': 79, 'Saturn': 82, 'Uranus': 27, 'Neptune': 14};
final containsUranus = moonCount.containsKey('Uranus'); // true
final containsPluto = moonCount.containsKey('Pluto'); // false
```

### map()

```dart
Map<K2, V2> map<K2, V2>(MapEntry<K2, V2> convert(K key, V value))
```

返回一个新映射，其中此映射的所有条目都通过给定的 [convert] 函数进行转换。

### addEntries()

```dart
void addEntries(Iterable<MapEntry<K, V>> newEntries)
```

将 [newEntries] 的所有键/值对添加到此映射中。

如果 [newEntries] 中的某个键已存在于此映射中，则对应的值会被覆盖。

该操作等价于对 [newEntries] 中的每个 [MapEntry](https://www.yuque.com/thyname/dart.core/mapentry) 执行 `this[entry.key] = entry.value`。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus',
  3: 'Earth', 4: 'Mars'};
final gasGiants = <int, String>{5: 'Jupiter', 6: 'Saturn'};
final iceGiants = <int, String>{7: 'Uranus', 8: 'Neptune'};
planets.addEntries(gasGiants.entries);
planets.addEntries(iceGiants.entries);
print(planets);
// {1: Mercury, 2: Venus, 3: Earth, 4: Mars, 5: Jupiter, 6: Saturn,
//  7: Uranus, 8: Neptune}
```

### update()

```dart
V update(
  K key,
  V update( V value ), {
  V ifAbsent()?,
})
```

更新指定 [key] 对应的值。

返回与该键关联的新值。

如果该键存在，则使用当前值调用 [update]，并将新值存入映射。

如果该键不存在且提供了 [ifAbsent]，则调用 [ifAbsent]，并将返回值作为该键对应的值添加到映射中。

如果该键不存在，则必须提供 [ifAbsent]。

```dart
final planetsFromSun = <int, String>{1: 'Mercury', 2: 'unknown',
  3: 'Earth'};
// Update value for known key value 2.
planetsFromSun.update(2, (value) => 'Venus');
print(planetsFromSun); // {1: Mercury, 2: Venus, 3: Earth}

final largestPlanets = <int, String>{1: 'Jupiter', 2: 'Saturn',
  3: 'Neptune'};
// Key value 8 is missing from list, add it using [ifAbsent].
largestPlanets.update(8, (value) => 'New', ifAbsent: () => 'Mercury');
print(largestPlanets); // {1: Jupiter, 2: Saturn, 3: Neptune, 8: Mercury}
```

### updateAll()

```dart
void updateAll(V update(K key, V value))
```

更新所有值。

遍历映射中的所有条目，并使用调用 [update] 的结果更新它们。

```dart
final terrestrial = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
terrestrial.updateAll((key, value) => value.toUpperCase());
print(terrestrial); // {1: MERCURY, 2: VENUS, 3: EARTH}
```

### removeWhere()

```dart
void removeWhere(bool test(K key, V value))
```

删除此映射中所有满足给定 [test] 的条目。

```dart
final terrestrial = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
terrestrial.removeWhere((key, value) => value.startsWith('E'));
print(terrestrial); // {1: Mercury, 2: Venus}
```

### putIfAbsent()

```dart
V putIfAbsent(K key, V ifAbsent())
```

查找 [key] 对应的值，如果不存在则添加一个新条目。

如果存在与 [key] 关联的值，则返回该值。否则调用 [ifAbsent] 获取一个新值，将 [key] 与该值相关联，然后返回该新值。

也就是说，如果该键当前在映射中，`map.putIfAbsent(key, ifAbsent)` 等价于 `map[key]`。如果该键当前不在映射中，则等价于 `map[key] = ifAbsent()`（但不保证实际调用了 `[]` 和 `[]=` 运算符来实现这一效果）。

```dart
final diameters = <num, String>{1.0: 'Earth'};
final otherDiameters = <double, String>{0.383: 'Mercury', 0.949: 'Venus'};

for (final item in otherDiameters.entries) {
  diameters.putIfAbsent(item.key, () => item.value);
}
print(diameters); // {1.0: Earth, 0.383: Mercury, 0.949: Venus}

// If the key already exists, the current value is returned.
final result = diameters.putIfAbsent(0.383, () => 'Random');
print(result); // Mercury
print(diameters); // {1.0: Earth, 0.383: Mercury, 0.949: Venus}
```

[ifAbsent] 函数允许修改映射，如果这样做，其行为与等价的 `map[key] = ifAbsent()` 相同。

### addAll()

```dart
void addAll(Map<K, V> other)
```

将 [other] 的所有键/值对添加到此映射中。

如果 [other] 中的某个键已存在于此映射中，则其值会被覆盖。

该操作等价于对 other 中的每个键及其关联的值执行 `this[key] = value`。它会遍历 [other]，因此 [other] 在迭代期间不能发生变化。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Earth'};
planets.addAll({5: 'Jupiter', 6: 'Saturn'});
print(planets); // {1: Mercury, 2: Earth, 5: Jupiter, 6: Saturn}
```

### remove()

```dart
V? remove(Object? key)
```

从映射中删除 [key] 及其关联的值（如果存在）。

返回该值在被删除之前与 `key` 关联的值。如果 `key` 不在映射中，则返回 `null`。

注意，某些映射允许将 `null` 作为值，因此返回 `null` 并不总是意味着该键不存在。

```dart
final terrestrial = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
final removedValue = terrestrial.remove(2); // Venus
print(terrestrial); // {1: Mercury, 3: Earth}
```

### clear()

```dart
void clear()
```

删除映射中的所有条目。

之后，映射变为空。

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Earth'};
planets.clear(); // {}
```

### forEach()

```dart
void forEach(void action(K key, V value))
```

对映射中的每个键/值对应用 [action]。

调用 `action` 时不得向映射中添加或删除键。

```dart
final planetsByMass = <num, String>{0.81: 'Venus', 1: 'Earth',
  0.11: 'Mars', 17.15: 'Neptune'};

planetsByMass.forEach((key, value) {
  print('$key: $value');
  // 0.81: Venus
  // 1: Earth
  // 0.11: Mars
  // 17.15: Neptune
});
```

## 运算符

### operator []

```dart
V? operator [](Object? key)
```

给定 [key] 对应的值；如果 [key] 不在映射中，则为 `null`。

某些映射允许将 `null` 作为值。对于这类映射，使用此运算符进行查找无法区分某个键不在映射中，还是该键存在但其值为 `null`。如果需要区分这两种情况，可以使用 [containsKey] 或 [putIfAbsent] 等方法。

### operator []=

```dart
void operator []=(K key, V value)
```

将 [key] 与给定的 [value] 相关联。

如果该键已存在于映射中，则更改其关联的值。否则将该键/值对添加到映射中。

---

# MapEntry

```dart
final class MapEntry<K, V> {}
```

表示 [Map](https://www.yuque.com/thyname/dart.core/map) 中一个条目的键/值对。

[Map](https://www.yuque.com/thyname/dart.core/map) 接口包含多种可以基于条目对象来检查或修改映射的方法。

```dart
final map = {'1': 'A', '2': 'B'};
map.addEntries([
 MapEntry('3', 'C'),
 MapEntry('4', 'D'),
]);
print(map); // {1: A, 2: B, 3: C, 4: D}
```

不要扩展或实现 `MapEntry` 类。如果 Dart 语言引入值类型，`MapEntry` 类将被改为此类值类型，届时很可能不再允许被类扩展或实现。

## 构造函数

### MapEntry()

```dart
MapEntry(K key, V value)
```

创建一个具有 [key] 和 [value] 的条目。

## 属性

### key

```dart
K key
```

条目的键。

```dart
final map = {'theKey': 'theValue'}; // Map<String, String>
var entry = map.entries.first; // MapEntry<String, String>
print(entry.key); // 'theKey'
```

### value

```dart
V value
```

在映射中与 [key] 关联的值。

```dart
final map = {'theKey': 'theValue'}; // Map<String, String>
var entry = map.entries.first; // MapEntry<String, String>
print(entry.value); // 'theValue'
```

## 方法

### toString()

```dart
String toString()
```

仅用于调试的字符串表示形式。

不保证长期稳定不变。
