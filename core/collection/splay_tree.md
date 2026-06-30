# SplayTreeMap

```dart
final class SplayTreeMap<K, V> extends _SplayTree<K, _SplayTreeMapNode<K, V>> with MapMixin<K, V> {}
```

A [Map] of objects that can be ordered relative to each other.

The map is based on a self-balancing binary tree. It allows most single-entry operations in amortized logarithmic time.

Keys of the map are compared using the `compare` function passed in the constructor, both for ordering and for equality. If the map contains only the key `a`, then `map.containsKey(b)` will return `true` if and only if `compare(a, b) == 0`, and the value of `a == b` is not even checked. If the compare function is omitted, the objects are assumed to be [Comparable], and are compared using their [Comparable.compareTo] method. Non-comparable objects (including `null`) will not work as keys in that case.

To allow calling [operator []], [remove] or [containsKey] with objects that are not supported by the `compare` function, an extra `isValidKey` predicate function can be supplied. This function is tested before using the `compare` function on an argument value that may not be a [K] value. If omitted, the `isValidKey` function defaults to testing if the value is a [K].

**Notice:** Do not modify a map (add or remove keys) while an operation is being performed on that map, for example in functions called during a [forEach] or [putIfAbsent] call, or while iterating the map ([keys], [values] or [entries]).

Example:

```dart
final planetsByMass = SplayTreeMap<double, String>((a, b) => a.compareTo(b));
```

To add data to a map, use [operator[]=], [addAll] or [addEntries].

```
planetsByMass[0.06] = 'Mercury';
planetsByMass
    .addAll({0.81: 'Venus', 1.0: 'Earth', 0.11: 'Mars', 317.83: 'Jupiter'});
```

To check if the map is empty, use [isEmpty] or [isNotEmpty]. To find the number of map entries, use [length].

```
print(planetsByMass.isEmpty); // false
print(planetsByMass.length); // 5
```

The [forEach] method calls a function for each key/value entry of the map.

```
planetsByMass.forEach((key, value) {
  print('$key \t $value');
  // 0.06    Mercury
  // 0.11    Mars
  // 0.81    Venus
  // 1.0     Earth
  // 317.83  Jupiter
});
```

To check whether the map has an entry with a specific key, use [containsKey].

```
final keyOneExists = planetsByMass.containsKey(1.0); // true
final keyFiveExists = planetsByMass.containsKey(5); // false
```

To check whether the map has an entry with a specific value, use [containsValue].

```
final earthExists = planetsByMass.containsValue('Earth'); // true
final plutoExists = planetsByMass.containsValue('Pluto'); // false
```

To remove an entry with a specific key, use [remove].

```
final removedValue = planetsByMass.remove(1.0);
print(removedValue); // Earth
```

To remove multiple entries at the same time, based on their keys and values, use [removeWhere].

```
planetsByMass.removeWhere((key, value) => key <= 1);
print(planetsByMass); // {317.83: Jupiter}
```

To conditionally add or modify a value for a specific key, depending on whether there already is an entry with that key, use [putIfAbsent] or [update].

```
planetsByMass.update(1, (v) => '', ifAbsent: () => 'Earth');
planetsByMass.putIfAbsent(317.83, () => 'Another Jupiter');
print(planetsByMass); // {1.0: Earth, 317.83: Jupiter}
```

To update the values of all keys, based on the existing key and value, use [updateAll].

```
planetsByMass.updateAll((key, value) => 'X');
print(planetsByMass); // {1.0: X, 317.83: X}
```

To remove all entries and empty the map, use [clear].

```
planetsByMass.clear();
print(planetsByMass.isEmpty); // false
print(planetsByMass); // {}
```

**See also:**

- [Map], the general interface of key/value pair collections.
- [HashMap] is unordered (the order of iteration is not guaranteed).
- [LinkedHashMap] iterates in key insertion order.

### SplayTreeMap()

```dart
SplayTreeMap([int Function(K key1, K key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

### SplayTreeMap.from()

```dart
SplayTreeMap.from(Map<Object?, Object?> other, [int Function(K key1, K key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Creates a [SplayTreeMap] that contains all key/value pairs of [other].

The keys must all be instances of [K] and the values of [V]. The [other] map itself can have any type. Example:

```dart
final baseMap = <int, Object>{3: 'C', 1: 'A', 2: 'B'};
final fromBaseMap = SplayTreeMap<int, String>.from(baseMap);
print(fromBaseMap); // {1: A, 2: B, 3: C}
```

### SplayTreeMap.of()

```dart
SplayTreeMap.of(Map<K, V> other, [int Function(K key1, K key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Creates a [SplayTreeMap] that contains all key/value pairs of [other]. Example:

```dart
final baseMap = <int, String>{3: 'A', 2: 'B', 1: 'C', 4: 'D'};
final mapOf = SplayTreeMap<num, Object>.of(baseMap);
print(mapOf); // {1: C, 2: B, 3: A, 4: D}
```

### SplayTreeMap.fromIterable()

```dart
SplayTreeMap.fromIterable(Iterable iterable, {K Function(dynamic element)? key, V Function(dynamic element)? value, int Function(K key1, K key2)? compare, bool Function(dynamic potentialKey)? isValidKey})
```

Creates a [SplayTreeMap] where the keys and values are computed from the [iterable].

For each element of the [iterable] this constructor computes a key/value pair, by applying [key] and [value] respectively.

The keys of the key/value pairs do not need to be unique. The last occurrence of a key will simply overwrite any previous value.

If no functions are specified for [key] and [value], the default is to use the iterable value itself. Example:

```dart
final numbers = [12, 11, 14, 13];
final mapFromIterable =
    SplayTreeMap<int, int>.fromIterable(numbers,
        key: (i) => i, value: (i) => i * i);
print(mapFromIterable); // {11: 121, 12: 144, 13: 169, 14: 196}
```

### SplayTreeMap.fromIterables()

```dart
SplayTreeMap.fromIterables(Iterable<K> keys, Iterable<V> values, [int Function(K key1, K key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Creates a [SplayTreeMap] associating the given [keys] to [values].

This constructor iterates over [keys] and [values] and maps each element of [keys] to the corresponding element of [values].

If [keys] contains the same object multiple times, the last occurrence overwrites the previous value.

It is an error if the two [Iterable]s don't have the same length. Example:

```dart
final keys = ['1', '2', '3', '4'];
final values = ['A', 'B', 'C', 'D'];
final mapFromIterables = SplayTreeMap.fromIterables(keys, values);
print(mapFromIterables); // {1: A, 2: B, 3: C, 4: D}
```

### operator []()

```dart
V? operator [](Object? key)
```

### remove()

```dart
V? remove(Object? key)
```

### operator []=()

```dart
void operator []=(K key, V value)
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

### addAll()

```dart
void addAll(Map<K, V> other)
```

### isEmpty

```dart
bool get isEmpty
```

### isNotEmpty

```dart
bool get isNotEmpty
```

### forEach()

```dart
void forEach(void f(K key, V value))
```

### length

```dart
int get length
```

### clear()

```dart
void clear()
```

### containsKey()

```dart
bool containsKey(Object? key)
```

### containsValue()

```dart
bool containsValue(Object? value)
```

### keys

```dart
Iterable<K> get keys
```

### values

```dart
Iterable<V> get values
```

### entries

```dart
Iterable<MapEntry<K, V>> get entries
```

### firstKey()

```dart
K? firstKey()
```

The first key in the map.

Returns `null` if the map is empty.

### lastKey()

```dart
K? lastKey()
```

The last key in the map.

Returns `null` if the map is empty.

### lastKeyBefore()

```dart
K? lastKeyBefore(K key)
```

The last key in the map that is strictly smaller than [key].

Returns `null` if such a key was not found.

### firstKeyAfter()

```dart
K? firstKeyAfter(K key)
```

Get the first key in the map that is strictly larger than [key]. Returns `null` if such a key was not found.

# SplayTreeSet

```dart
final class SplayTreeSet<E> extends _SplayTree<E, _SplayTreeSetNode<E>> with Iterable<E>, SetMixin<E> {}
```

A [Set] of objects that can be ordered relative to each other.

The set is based on a self-balancing binary tree. It allows most operations in amortized logarithmic time.

Elements of the set are compared using the `compare` function passed in the constructor, both for ordering and for equality. If the set contains only an object `a`, then `set.contains(b)` will return `true` if and only if `compare(a, b) == 0`, and the value of `a == b` is not even checked. If the compare function is omitted, the objects are assumed to be [Comparable], and are compared using their [Comparable.compareTo] method. Non-comparable objects (including `null`) will not work as an element in that case.

**Note:** Do not modify a set (add or remove elements) while an operation is being performed on that set, for example in functions called during a [forEach] or [containsAll] call, or while iterating the set.

Do not modify elements in a way which changes their equality (and thus their hash code) while they are in the set. Some specialized kinds of sets may be more permissive with regards to equality, in which case they should document their different behavior and restrictions.

Example:

```dart
final planets = SplayTreeSet<String>((a, b) => a.compareTo(b));
```

To add data to a set, use [add] or [addAll].

```
planets.add('Neptune');
planets.addAll({'Venus', 'Mars', 'Earth', 'Jupiter'});
print(planets); // {Earth, Jupiter, Mars, Neptune, Venus}
```

To check if the set is empty, use [isEmpty] or [isNotEmpty]. To find the number of elements in the set, use [length].

```
final isEmpty = planets.isEmpty; // false
final length = planets.length; // 5
```

To check whether the set contains a specific element, use [contains].

```
final marsExists = planets.contains('Mars'); // true
```

To get element value using index, use [elementAt].

```
final elementAt = planets.elementAt(1);
print(elementAt); // Jupiter
```

To make a copy of set, use [toSet].

```
final copySet = planets.toSet(); // a `SplayTreeSet` with the same ordering.
print(copySet); // {Earth, Jupiter, Mars, Neptune, Venus}
```

To remove an element, use [remove].

```
final removedValue = planets.remove('Mars'); // true
print(planets); // {Earth, Jupiter, Neptune, Venus}
```

To remove multiple elements at the same time, use [removeWhere].

```
planets.removeWhere((element) => element.startsWith('J'));
print(planets); // {Earth, Neptune, Venus}
```

To removes all elements in this set that do not meet a condition, use [retainWhere].

```
planets.retainWhere((element) => element.contains('Earth'));
print(planets); // {Earth}
```

To remove all elements and empty the set, use [clear].

```
planets.clear();
print(planets.isEmpty); // true
print(planets); // {}
```

**See also:**

- [Set] is a base-class for collection of objects.
- [HashSet] the order of the objects in the iterations is not guaranteed.
- [LinkedHashSet] objects stored based on insertion order.

### SplayTreeSet()

```dart
SplayTreeSet([int Function(E key1, E key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Create a new [SplayTreeSet] with the given compare function.

If the [compare] function is omitted, it defaults to [Comparable.compare], and the elements must be comparable.

A provided `compare` function may not work on all objects. It may not even work on all `E` instances.

For operations that add elements to the set, the user is supposed to not pass in objects that don't work with the compare function.

The methods [contains], [remove], [lookup], [removeAll] or [retainAll] are typed to accept any object(s), and the [isValidKey] test can used to filter those objects before handing them to the `compare` function.

If [isValidKey] is provided, only values satisfying `isValidKey(other)` are compared using the `compare` method in the methods mentioned above. If the `isValidKey` function returns false for an object, it is assumed to not be in the set.

If omitted, the `isValidKey` function defaults to checking against the type parameter: `other is E`.

### SplayTreeSet.from()

```dart
SplayTreeSet.from(Iterable elements, [int Function(E key1, E key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Creates a [SplayTreeSet] that contains all [elements].

The set works as if created by `SplayTreeSet<E>(compare, isValidKey)`.

All the [elements] should be instances of [E] and valid arguments to [compare]. The `elements` iterable itself may have any element type, so this constructor can be used to down-cast a `Set`, for example as:

```dart
Set<SuperType> superSet = ...;
Set<SubType> subSet =
    SplayTreeSet<SubType>.from(superSet.whereType<SubType>());
```

Example:

```dart
final numbers = <num>[20, 30, 10];
final setFrom = SplayTreeSet<int>.from(numbers);
print(setFrom); // {10, 20, 30}
```

### SplayTreeSet.of()

```dart
SplayTreeSet.of(Iterable<E> elements, [int Function(E key1, E key2)? compare, bool Function(dynamic potentialKey)? isValidKey])
```

Creates a [SplayTreeSet] from [elements].

The set works as if created by `new SplayTreeSet<E>(compare, isValidKey)`.

All the [elements] should be valid as arguments to the [compare] function. Example:

```dart
final baseSet = <int>{1, 2, 3};
final setOf = SplayTreeSet<num>.of(baseSet);
print(setOf); // {1, 2, 3}
```

### cast()

```dart
Set<R> cast<R>()
```

### iterator

```dart
Iterator<E> get iterator
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

### first

```dart
E get first
```

### last

```dart
E get last
```

### single

```dart
E get single
```

### contains()

```dart
bool contains(Object? element)
```

### add()

```dart
bool add(E element)
```

### remove()

```dart
bool remove(Object? object)
```

### addAll()

```dart
void addAll(Iterable<E> elements)
```

### removeAll()

```dart
void removeAll(Iterable<Object?> elements)
```

### retainAll()

```dart
void retainAll(Iterable<Object?> elements)
```

### lookup()

```dart
E? lookup(Object? object)
```

### intersection()

```dart
Set<E> intersection(Set<Object?> other)
```

### difference()

```dart
Set<E> difference(Set<Object?> other)
```

### union()

```dart
Set<E> union(Set<E> other)
```

### clear()

```dart
void clear()
```

### toSet()

```dart
Set<E> toSet()
```

### toString()

```dart
String toString()
```
