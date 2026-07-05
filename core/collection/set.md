# SetBase

```dart
abstract mixin class SetBase<E> implements Set<E> {}
```

Base implementation of [Set].

This class provides a base implementation of a `Set` that depends only on the abstract members: [add], [contains], [lookup], [remove], [iterator], [length] and [toSet].

Some of the methods assume that `toSet` creates a modifiable set. If using this base class for an unmodifiable set, where `toSet` should return an unmodifiable set, it's necessary to reimplement [retainAll], [union], [intersection] and [difference].

Implementations of `Set` using this base should consider also implementing `clear` in constant time. The default implementation works by removing every element.

## 构造函数

### SetBase()

```dart
const SetBase<E>()
```

## 属性

* iterator
* length
* isEmpty
* isNotEmpty
* first
* last

## 方法

* add()
* contains()
* lookup()
* remove()
* toSet()
* cast()
* followedBy()
* whereType()
* clear()
* addAll()
* removeAll()
* retainAll()
* removeWhere()
* retainWhere()
* containsAll()
* union()
* intersection()
* difference()
* toList()
* map()
* single
* toString()
* where()
* expand()
* forEach()
* reduce()
* fold()
* every()
* join()
* any()
* take()
* takeWhile()
* skip()
* skipWhile()
* firstWhere()
* lastWhere()
* singleWhere()
* elementAt()

### setToString()

```dart
String setToString(Set set)
```

Converts a [Set] to a [String].

Converts [set] to a string by converting each element to a string (by calling [Object.toString]), joining them with ", ", and wrapping the result in "{" and "}".

Handles circular references where converting one of the elements to a string ends up converting [set] to a string again.

---



# SetMixin

```dart
typedef SetMixin<E> = SetBase<E>
```

Mixin implementation of [Set].

This class provides a base implementation of a `Set` that depends only on the abstract members: [add], [contains], [lookup], [remove], [iterator], [length] and [toSet].

Some of the methods assume that `toSet` creates a modifiable set. If using this mixin for an unmodifiable set, where `toSet` should return an unmodifiable set, it's necessary to reimplement [retainAll], [union], [intersection] and [difference].

Implementations of `Set` using this mixin should consider also implementing `clear` in constant time. The default implementation works by removing every element.

---



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

## 构造函数

### UnmodifiableSetView()

```dart
UnmodifiableSetView<E>(Set<E> source)
```

Creates an [UnmodifiableSetView] of [source].

## 属性

* length
* iterator

## 方法

* contains()
* lookup()
* toSet()
