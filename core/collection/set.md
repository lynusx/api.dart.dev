Base implementations of [Set].

# SetBase

```dart
abstract mixin class SetBase<E> implements Set<E> {}
```

Base implementation of [Set].

This class provides a base implementation of a `Set` that depends only on the abstract members: [add], [contains], [lookup], [remove], [iterator], [length] and [toSet].

Some of the methods assume that `toSet` creates a modifiable set. If using this base class for an unmodifiable set, where `toSet` should return an unmodifiable set, it's necessary to reimplement [retainAll], [union], [intersection] and [difference].

Implementations of `Set` using this base should consider also implementing `clear` in constant time. The default implementation works by removing every element.

### SetBase()

```dart
SetBase()
```

### add()

```dart
bool add(E value)
```

### contains()

```dart
bool contains(Object? element)
```

### lookup()

```dart
E? lookup(Object? element)
```

### remove()

```dart
bool remove(Object? value)
```

### iterator

```dart
Iterator<E> get iterator
```

### toSet()

```dart
Set<E> toSet()
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

### cast()

```dart
Set<R> cast<R>()
```

### followedBy()

```dart
Iterable<E> followedBy(Iterable<E> other)
```

### whereType()

```dart
Iterable<T> whereType<T>()
```

### clear()

```dart
void clear()
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

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

### containsAll()

```dart
bool containsAll(Iterable<Object?> other)
```

### union()

```dart
Set<E> union(Set<E> other)
```

### intersection()

```dart
Set<E> intersection(Set<Object?> other)
```

### difference()

```dart
Set<E> difference(Set<Object?> other)
```

### toList()

```dart
List<E> toList({bool growable = true})
```

### map()

```dart
Iterable<T> map<T>(T f(E element))
```

### single

```dart
E get single
```

### toString()

```dart
String toString()
```

### where()

```dart
Iterable<E> where(bool f(E element))
```

### expand()

```dart
Iterable<T> expand<T>(Iterable<T> f(E element))
```

### forEach()

```dart
void forEach(void f(E element))
```

### reduce()

```dart
E reduce(E combine(E value, E element))
```

### fold()

```dart
T fold<T>(T initialValue, T combine(T previousValue, E element))
```

### every()

```dart
bool every(bool f(E element))
```

### join()

```dart
String join([String separator = ""])
```

### any()

```dart
bool any(bool test(E element))
```

### take()

```dart
Iterable<E> take(int n)
```

### takeWhile()

```dart
Iterable<E> takeWhile(bool test(E value))
```

### skip()

```dart
Iterable<E> skip(int n)
```

### skipWhile()

```dart
Iterable<E> skipWhile(bool test(E value))
```

### first

```dart
E get first
```

### last

```dart
E get last
```

### firstWhere()

```dart
E firstWhere(bool test(E value), {E Function()? orElse})
```

### lastWhere()

```dart
E lastWhere(bool test(E value), {E Function()? orElse})
```

### singleWhere()

```dart
E singleWhere(bool test(E value), {E Function()? orElse})
```

### elementAt()

```dart
E elementAt(int index)
```

### setToString()

```dart
String setToString(Set set)
```

Converts a [Set] to a [String].

Converts [set] to a string by converting each element to a string (by calling [Object.toString]), joining them with ", ", and wrapping the result in "{" and "}".

Handles circular references where converting one of the elements to a string ends up converting [set] to a string again.

# SetMixin

```dart
typedef SetMixin<E> = SetBase<E>
```

Mixin implementation of [Set].

This class provides a base implementation of a `Set` that depends only on the abstract members: [add], [contains], [lookup], [remove], [iterator], [length] and [toSet].

Some of the methods assume that `toSet` creates a modifiable set. If using this mixin for an unmodifiable set, where `toSet` should return an unmodifiable set, it's necessary to reimplement [retainAll], [union], [intersection] and [difference].

Implementations of `Set` using this mixin should consider also implementing `clear` in constant time. The default implementation works by removing every element.

# UnmodifiableSetView

```dart
class UnmodifiableSetView<E> extends SetBase<E> with _UnmodifiableSetMixin<E> {}
```

An unmodifiable [Set] view of another [Set].

Methods that could change the set, such as [add] and [remove], must not be called.

```dart
final baseSet = <String>{'Mars', 'Mercury', 'Earth', 'Venus'};
final unmodifiableSetView = UnmodifiableSetView(baseSet);

// Remove an element from the original set.
baseSet.remove('Venus');
print(unmodifiableSetView); // {Mars, Mercury, Earth}

unmodifiableSetView.remove('Earth'); // Throws.
```

### UnmodifiableSetView()

```dart
UnmodifiableSetView(Set<E> source)
```

Creates an [UnmodifiableSetView] of [source].

### contains()

```dart
bool contains(Object? element)
```

### lookup()

```dart
E? lookup(Object? element)
```

### length

```dart
int get length
```

### iterator

```dart
Iterator<E> get iterator
```

### toSet()

```dart
Set<E> toSet()
```
