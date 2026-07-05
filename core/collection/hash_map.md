# HashMap

```dart
abstract final class HashMap<K, V> implements Map<K, V> {}
```

A hash-table based implementation of [Map].

The [HashMap] is unordered (the order of iteration is not guaranteed).

The keys of a `HashMap` must have consistent [Object.==] and [Object.hashCode] implementations. This means that the `==` operator must define a stable equivalence relation on the keys (reflexive, symmetric, transitive, and consistent over time), and that `hashCode` must be the same for objects that are considered equal by `==`.

Iterating the map's keys, values or entries (through [forEach]) may happen in any order. The iteration order only changes when the map is modified. Values are iterated in the same order as their associated keys, so iterating the [keys] and [values] in parallel will give matching key and value pairs.

**Notice:** Do not modify a map (add or remove keys) while an operation is being performed on that map, for example in functions called during a [forEach] or [putIfAbsent] call, or while iterating the map ([keys], [values] or [entries]). Changing the value of an existing key, for example using [operator[]=] or [update], does not affect the map's structure and will not break iteration.

Do not modify keys in any way which changes their equality (and thus their hash code) while they are in the map. If a map key's [Object.hashCode] changes, it may cause future lookups for that key to fail.

Example:

```dart
final Map<int, String> planets = HashMap(); // Is a HashMap
```

To add data to a map, use [operator[]=], [addAll] or [addEntries].

```dart continued
planets[3] = 'Earth';
planets.addAll({4: 'Mars'});
final gasGiants = {6: 'Jupiter', 5: 'Saturn'};
planets.addEntries(gasGiants.entries);
print(planets); // fx {5: Saturn, 6: Jupiter, 3: Earth, 4: Mars}
```

To check if the map is empty, use [isEmpty] or [isNotEmpty]. To find the number of map entries, use [length].

```dart continued
final isEmpty = planets.isEmpty; // false
final length = planets.length; // 4
```

The [forEach] iterates through all entries of a map.

```dart continued
planets.forEach((key, value) {
  print('$key \t $value');
  // 5        Saturn
  // 4        Mars
  // 3        Earth
  // 6        Jupiter
});
```

To check whether the map has an entry with a specific key, use [containsKey].

```dart continued
final keyOneExists = planets.containsKey(4); // true
final keyFiveExists = planets.containsKey(1); // false
```

To check whether the map has an entry with a specific value, use [containsValue].

```dart continued
final marsExists = planets.containsValue('Mars'); // true
final venusExists = planets.containsValue('Venus'); // false
```

To remove an entry with a specific key, use [remove].

```dart continued
final removeValue = planets.remove(6);
print(removeValue); // Jupiter
print(planets); // fx {4: Mars, 3: Earth, 5: Saturn}
```

To remove multiple entries at the same time, based on their keys and values, use [removeWhere].

```dart continued
planets.removeWhere((key, value) => key == 5);
print(planets); // fx {3: Earth, 4: Mars}
```

To conditionally add or modify a value for a specific key, depending on whether there already is an entry with that key, use [putIfAbsent] or [update].

```dart continued
planets.update(4, (v) => 'Saturn');
planets.update(8, (v) => '', ifAbsent: () => 'Neptune');
planets.putIfAbsent(4, () => 'Another Saturn');
print(planets); // fx {4: Saturn, 8: Neptune, 3: Earth}
```

To update the values of all keys, based on the existing key and value, use [updateAll].

```dart continued
planets.updateAll((key, value) => 'X');
print(planets); // fx {8: X, 3: X, 4: X}
```

To remove all entries and empty the map, use [clear].

```dart continued
planets.clear();
print(planets); // {}
print(planets.isEmpty); // true
```

**See also:**

- [Map], the general interface of key/value pair collections.
- [LinkedHashMap] iterates in key insertion order.
- [SplayTreeMap] iterates the keys in sorted order.



## 构造函数

### HashMap()

```dart
HashMap<K, V>({
  bool equals( K, K )?,
  int hashCode( K )?,
  bool isValidKey( dynamic )?,
})
```

Creates an unordered hash-table based [Map].

The created map is not ordered in any way. When iterating the keys or values, the iteration order is unspecified except that it will stay the same as long as the map isn't changed.

If [equals] is provided, it is used to compare the keys in the map with new keys. If [equals] is omitted, the key's own [Object.==] is used instead. The [equals] function _must not_ change the map it's used as an equality for. If it does, the resulting behavior is unspecified.

Similarly, if [hashCode] is provided, it is used to produce a hash value for keys in order to place them in the map. If [hashCode] is omitted, the key's own [Object.hashCode] is used. The [hashCode] function _must not_ change the map it's used as a hash code for. If it does, the resulting behavior is unspecified.

The used `equals` and `hashCode` method should always be consistent, so that if `equals(a, b)`, then `hashCode(a) == hashCode(b)`. The hash of an object, or what it compares equal to, should not change while the object is a key in the map. If the hash code or equality of an object does change, the resulting behavior is unspecified.

If you supply one of [equals] and [hashCode], you should generally also supply the other.

Some [equals] or [hashCode] functions might not work for all objects. If [isValidKey] is supplied, it's used to check a potential key which is not necessarily an instance of [K], like the arguments to [operator []], [remove] and [containsKey], which are typed as `Object?`. If [isValidKey] returns `false`, for an object, the [equals] and [hashCode] functions are not called, and no key equal to that object is assumed to be in the map. The [isValidKey] function defaults to just testing if the object is an instance of [K].

Example:

```dart template:expression
HashMap<int,int>(equals: (int a, int b) => (b - a) % 5 == 0,
                 hashCode: (int e) => e % 5)
```

This example map does not need an `isValidKey` function to be passed. The default function accepts precisely `int` values, which can safely be passed to both the `equals` and `hashCode` functions.

If neither `equals`, `hashCode`, nor `isValidKey` is provided, the default `isValidKey` instead accepts all keys. The default equality and hash code operations are known to work on all objects.

Likewise, if `equals` is [identical], `hashCode` is [identityHashCode] and `isValidKey` is omitted, the resulting map is identity based, and the `isValidKey` defaults to accepting all keys. Such a map can be created directly using [HashMap.identity].

### HashMap.identity()

```dart
HashMap<K, V>.identity()
```

Creates an unordered identity-based map.

Keys of this map are considered equal only to the same object, and do not use [Object.==] at all.

Effectively shorthand for:

```dart
HashMap<K, V>(equals: identical, hashCode: identityHashCode)
```

### HashMap.from()

```dart
HashMap<K, V>.from( Map other )
```

Creates a [HashMap] that contains all key/value pairs of [other].

The keys must all be instances of [K] and the values of [V]. The [other] map itself can have any type.

```dart
final baseMap = {1: 'A', 2: 'B', 3: 'C'};
final fromBaseMap = HashMap<int, String>.from(baseMap);
print(fromBaseMap); // {1: A, 2: B, 3: C}
```

### HashMap.of()

```dart
HashMap<K, V>.of( Map<K, V> other )
```

Creates a [HashMap] that contains all key/value pairs of [other]. Example:

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

Creates a [HashMap] where the keys and values are computed from the [iterable].

For each element of the [iterable] this constructor computes a key/value pair, by applying [key] and [value] respectively.

The keys of the key/value pairs do not need to be unique. The last occurrence of a key will simply overwrite any previous value.

If no values are specified for [key] and [value], the default is the identity function. Example:

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

Creates a [HashMap] associating the given [keys] to [values].

This constructor iterates over [keys] and [values] and maps each element of [keys] to the corresponding element of [values].

If [keys] contains the same object multiple times, the last occurrence overwrites the previous value.

It is an error if the two [Iterable]s don't have the same length. Example:

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

Creates a [HashMap] containing the entries of [entries].

Returns a new `HashMap<K, V>` where all entries of [entries] have been added in iteration order.

If multiple [entries] have the same key, later occurrences overwrite the earlier ones.

Example:

```dart
final numbers = [11, 12, 13, 14];
final map = HashMap.fromEntries(numbers.map((i) => MapEntry(i, i * i)));
print(map); // {11: 121, 12: 144, 13: 169, 14: 196}
```
