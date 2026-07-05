# MapBase

```dart
abstract mixin class MapBase<K, V> implements Map<K, V> {}
```

Base class for implementing a [Map].

This class has a basic implementation of all but five of the members of [Map]. A basic `Map` class can be implemented by extending this class and implementing `keys`, `operator[]`, `operator[]=`, `remove` and `clear`. The remaining operations are implemented in terms of these five.

The `keys` iterable should have efficient [Iterable.length] and [Iterable.contains] operations, and it should catch concurrent modifications of the keys while iterating.

A more efficient implementation is usually possible by overriding some of the other members as well.

### MapBase()

```dart
const MapBase<K, V>()
```

## 属性

* keys
* entries
* length
* isEmpty
* isNotEmpty
* values

## 方法

* remove()
* clear()
* cast()
* forEach()
* addAll()
* containsValue()
* putIfAbsent()
* update()
* updateAll()
* map()
* addEntries()
* removeWhere()
* containsKey()
* mapToString()

## 运算符

* operator []
* operator []=

---



# MapMixin

```dart
typedef MapMixin<K, V> = MapBase<K, V>
```

Mixin implementing a [Map].

This mixin has a basic implementation of all but five of the members of [Map]. A basic `Map` class can be implemented by mixin in this class and implementing `keys`, `operator[]`, `operator[]=`, `remove` and `clear`. The remaining operations are implemented in terms of these five.

The `keys` iterable should have efficient [Iterable.length] and [Iterable.contains] operations, and it should catch concurrent modifications of the keys while iterating.

A more efficient implementation is usually possible by overriding some of the other members as well.

---



# UnmodifiableMapBase

```dart
abstract class UnmodifiableMapBase<K, V> = MapBase<K, V> with _UnmodifiableMapMixin<K, V>;
```

Basic implementation of an unmodifiable [Map].

This class has a basic implementation of all but two of the members of an unmodifiable [Map]. A simple unmodifiable `Map` class can be implemented by extending this class and implementing `keys` and `operator[]`.

Modifying operations throw when used. The remaining non-modifying operations are implemented in terms of `keys` and `operator[]`.

The `keys` iterable should have efficient [Iterable.length] and [Iterable.contains] operations, and it should catch concurrent modifications of the keys while iterating.

A more efficient implementation is usually possible by overriding some of the other members as well.

---



# MapView

```dart
class MapView<K, V> implements Map<K, V> {}
```

Wrapper around a class that implements [Map] that only exposes `Map` members.

A simple wrapper that delegates all `Map` members to the map provided in the constructor.

Base for delegating map implementations like [UnmodifiableMapView].

## 构造函数

### MapView()

```dart
const MapView<K, V>(Map<K, V> map)
```

Creates a view which forwards operations to [map].

## 属性

* isEmpty
* isNotEmpty
* length
* keys
* values
* entries

## 方法

* cast()
* addAll()
* clear()
* putIfAbsent()
* containsKey()
* containsValue()
* forEach()
* remove()
* addEntries()
* map()
* update()
* updateAll()
* removeWhere()

## 运算符

* operator []
* operator []=



---



# UnmodifiableMapView

```dart
class UnmodifiableMapView<K, V> extends MapView<K, V> with _UnmodifiableMapMixin<K, V> {}
```

View of a [Map] that disallow modifying the map.

A wrapper around a `Map` that forwards all members to the map provided in the constructor, except for operations that modify the map. Modifying operations throw instead.

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

* cast()
