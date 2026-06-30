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
MapBase()
```

### keys

```dart
Iterable<K> get keys
```

### operator []()

```dart
V? operator [](Object? key)
```

### operator []=()

```dart
operator []=(K key, V value)
```

### remove()

```dart
V? remove(Object? key)
```

### clear()

```dart
void clear()
```

### cast()

```dart
Map<RK, RV> cast<RK, RV>()
```

### forEach()

```dart
void forEach(void action(K key, V value))
```

### addAll()

```dart
void addAll(Map<K, V> other)
```

### containsValue()

```dart
bool containsValue(Object? value)
```

### putIfAbsent()

```dart
V putIfAbsent(K key, V ifAbsent())
```

### update()

```dart
V update(K key, V update(V value), {V Function()? ifAbsent})
```

### updateAll()

```dart
void updateAll(V update(K key, V value))
```

### entries

```dart
Iterable<MapEntry<K, V>> get entries
```

### map()

```dart
Map<K2, V2> map<K2, V2>(MapEntry<K2, V2> transform(K key, V value))
```

### addEntries()

```dart
void addEntries(Iterable<MapEntry<K, V>> newEntries)
```

### removeWhere()

```dart
void removeWhere(bool test(K key, V value))
```

### containsKey()

```dart
bool containsKey(Object? key)
```

### length

```dart
int get length
```

### isEmpty

```dart
bool get isEmpty
```

### isNotEmpty

```dart
bool get isNotEmpty
```

### values

```dart
Iterable<V> get values
```

### toString()

```dart
String toString()
```

### mapToString()

```dart
String mapToString(Map<Object?, Object?> m)
```

# MapMixin

```dart
typedef MapMixin<K, V> = MapBase<K, V>
```

Mixin implementing a [Map].

This mixin has a basic implementation of all but five of the members of [Map]. A basic `Map` class can be implemented by mixin in this class and implementing `keys`, `operator[]`, `operator[]=`, `remove` and `clear`. The remaining operations are implemented in terms of these five.

The `keys` iterable should have efficient [Iterable.length] and [Iterable.contains] operations, and it should catch concurrent modifications of the keys while iterating.

A more efficient implementation is usually possible by overriding some of the other members as well.

# UnmodifiableMapBase

```dart
abstract class UnmodifiableMapBase<K, V> = MapBase<K, V> with _UnmodifiableMapMixin<K, V>;
```

Basic implementation of an unmodifiable [Map].

This class has a basic implementation of all but two of the members of an unmodifiable [Map]. A simple unmodifiable `Map` class can be implemented by extending this class and implementing `keys` and `operator[]`.

Modifying operations throw when used. The remaining non-modifying operations are implemented in terms of `keys` and `operator[]`.

The `keys` iterable should have efficient [Iterable.length] and [Iterable.contains] operations, and it should catch concurrent modifications of the keys while iterating.

A more efficient implementation is usually possible by overriding some of the other members as well.

# MapView

```dart
class MapView<K, V> implements Map<K, V> {}
```

Wrapper around a class that implements [Map] that only exposes `Map` members.

A simple wrapper that delegates all `Map` members to the map provided in the constructor.

Base for delegating map implementations like [UnmodifiableMapView].

### MapView()

```dart
MapView(Map<K, V> map)
```

Creates a view which forwards operations to [map].

### cast()

```dart
Map<RK, RV> cast<RK, RV>()
```

### operator []()

```dart
V? operator [](Object? key)
```

### operator []=()

```dart
void operator []=(K key, V value)
```

### addAll()

```dart
void addAll(Map<K, V> other)
```

### clear()

```dart
void clear()
```

### putIfAbsent()

```dart
V putIfAbsent(K key, V ifAbsent())
```

### containsKey()

```dart
bool containsKey(Object? key)
```

### containsValue()

```dart
bool containsValue(Object? value)
```

### forEach()

```dart
void forEach(void action(K key, V value))
```

### isEmpty

```dart
bool get isEmpty
```

### isNotEmpty

```dart
bool get isNotEmpty
```

### length

```dart
int get length
```

### keys

```dart
Iterable<K> get keys
```

### remove()

```dart
V? remove(Object? key)
```

### toString()

```dart
String toString()
```

### values

```dart
Iterable<V> get values
```

### entries

```dart
Iterable<MapEntry<K, V>> get entries
```

### addEntries()

```dart
void addEntries(Iterable<MapEntry<K, V>> entries)
```

### map()

```dart
Map<K2, V2> map<K2, V2>(MapEntry<K2, V2> transform(K key, V value))
```

### update()

```dart
V update(K key, V update(V value), {V Function()? ifAbsent})
```

### updateAll()

```dart
void updateAll(V update(K key, V value))
```

### removeWhere()

```dart
void removeWhere(bool test(K key, V value))
```

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

### UnmodifiableMapView()

```dart
UnmodifiableMapView(Map<K, V> map)
```

### cast()

```dart
Map<RK, RV> cast<RK, RV>()
```
