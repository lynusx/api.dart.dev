# MapBase

```dart
abstract mixin class MapBase<K, V> implements Map<K, V> {}
```

实现 [Map](https://www.yuque.com/thyname/dart.core/map) 的基类。

该类基本实现了 [Map](https://www.yuque.com/thyname/dart.core/map) 除五个成员之外的所有成员。通过扩展该类并实现 `keys`、`operator[]`、`operator[]=`、`remove` 和 `clear`，即可实现一个基本的 `Map` 类。其余操作均基于这五个成员实现。

`keys` 可迭代对象应具有高效的 [Iterable.length] 和 [Iterable.contains] 操作，并且应在迭代过程中捕获对 keys 的并发修改。

通常可以通过重写其他一些成员来实现更高效的实现。

### MapBase()

```dart
const MapBase<K, V>()
```

## 属性

- keys
- entries
- length
- isEmpty
- isNotEmpty
- values

## 方法

- remove()
- clear()
- cast()
- forEach()
- addAll()
- containsValue()
- putIfAbsent()
- update()
- updateAll()
- map()
- addEntries()
- removeWhere()
- containsKey()
- mapToString()

## 运算符

- operator []
- operator []=

---

# MapMixin

```dart
typedef MapMixin<K, V> = MapBase<K, V>
```

实现 [Map](https://www.yuque.com/thyname/dart.core/map) 的混入。

该混入基本实现了 [Map](https://www.yuque.com/thyname/dart.core/map) 除五个成员之外的所有成员。通过混入该类并实现 `keys`、`operator[]`、`operator[]=`、`remove` 和 `clear`，即可实现一个基本的 `Map` 类。其余操作均基于这五个成员实现。

`keys` 可迭代对象应具有高效的 [Iterable.length] 和 [Iterable.contains] 操作，并且应在迭代过程中捕获对 keys 的并发修改。

通常可以通过重写其他一些成员来实现更高效的实现。

---

# UnmodifiableMapBase

```dart
abstract class UnmodifiableMapBase<K, V> = MapBase<K, V> with _UnmodifiableMapMixin<K, V>;
```

不可变 [Map](https://www.yuque.com/thyname/dart.core/map) 的基本实现。

该类基本实现了不可变 [Map](https://www.yuque.com/thyname/dart.core/map) 除两个成员之外的所有成员。通过扩展该类并实现 `keys` 和 `operator[]`，即可实现一个简单的不可变 `Map` 类。

修改操作在使用时会抛出异常。其余非修改操作均基于 `keys` 和 `operator[]` 实现。

`keys` 可迭代对象应具有高效的 [Iterable.length] 和 [Iterable.contains] 操作，并且应在迭代过程中捕获对 keys 的并发修改。

通常可以通过重写其他一些成员来实现更高效的实现。

---

# MapView

```dart
class MapView<K, V> implements Map<K, V> {}
```

对实现 [Map](https://www.yuque.com/thyname/dart.core/map) 的类进行包装，仅暴露 `Map` 的成员。

一个简单的包装器，将所有 `Map` 成员委托给构造函数中提供的映射。

是 [UnmodifiableMapView](https://www.yuque.com/thyname/dart.collection/unmodifiablemapview) 等委托式映射实现的基础。

## 构造函数

### MapView()

```dart
const MapView<K, V>(Map<K, V> map)
```

创建一个将操作转发给 [map] 的视图。

## 属性

- isEmpty
- isNotEmpty
- length
- keys
- values
- entries

## 方法

- cast()
- addAll()
- clear()
- putIfAbsent()
- containsKey()
- containsValue()
- forEach()
- remove()
- addEntries()
- map()
- update()
- updateAll()
- removeWhere()

## 运算符

- operator []
- operator []=

---

# UnmodifiableMapView

```dart
class UnmodifiableMapView<K, V> extends MapView<K, V> with _UnmodifiableMapMixin<K, V> {}
```

禁止修改的 [Map](https://www.yuque.com/thyname/dart.core/map) 视图。

一个 `Map` 的包装器，将所有成员转发给构造函数中提供的映射，但会修改映射的操作除外。修改操作会抛出异常。

```dart
final baseMap = <int, String>{1: 'Mars', 2: 'Mercury', 3: 'Venus'};
final unmodifiableMapView = UnmodifiableMapView(baseMap);

// Remove an entry from the original map.
baseMap.remove(3);
print(unmodifiableMapView); // {1: Mars, 2: Mercury}

unmodifiableMapView.remove(1); // Throws.
```

## 构造函数

### UnmodifiableMapView()

```dart
UnmodifiableMapView(Map<K, V> map)
```

## 方法

- cast()
