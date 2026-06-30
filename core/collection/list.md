# ListBase

```dart
abstract mixin class ListBase<E> implements List<E> {}
```

Abstract implementation of a list.

`ListBase` can be used as a base class for implementing the `List` interface.

This class implements all read operations using only the `length` and `operator[]` and members. It implements write operations using those and `add`, `length=` and `operator[]=` Classes using this base class should implement those five operations.

**NOTICE**: For backwards compatibility reasons, there is a default implementation of `add` which only works for lists with a nullable element type. For list with a non-nullable element type, the `add` method must be implemented.

**NOTICE**: Forwarding just the four `length` and `[]` read/write operations to a normal growable [List] (as created by a `[]` literal) will give very bad performance for `add` and `addAll` operations of `ListBase`. These operations are implemented by increasing the length of the list by one for each `add` operation, and repeatedly increasing the length of a growable list is not efficient. To avoid this, override 'add' and 'addAll' to also forward directly to the growable list, or, if possible, use `DelegatingList` from "package:collection/collection.dart" instead of a `ListMixin`.

### ListBase()

```dart
ListBase()
```

### iterator

```dart
Iterator<E> get iterator
```

### elementAt()

```dart
E elementAt(int index)
```

### followedBy()

```dart
Iterable<E> followedBy(Iterable<E> other)
```

### forEach()

```dart
void forEach(void action(E element))
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

### first

```dart
void set first(E value)
```

### last

```dart
E get last
```

### last

```dart
void set last(E value)
```

### single

```dart
E get single
```

### contains()

```dart
bool contains(Object? element)
```

### every()

```dart
bool every(bool test(E element))
```

### any()

```dart
bool any(bool test(E element))
```

### firstWhere()

```dart
E firstWhere(bool test(E element), {E Function()? orElse})
```

### lastWhere()

```dart
E lastWhere(bool test(E element), {E Function()? orElse})
```

### singleWhere()

```dart
E singleWhere(bool test(E element), {E Function()? orElse})
```

### join()

```dart
String join([String separator = ""])
```

### where()

```dart
Iterable<E> where(bool test(E element))
```

### whereType()

```dart
Iterable<T> whereType<T>()
```

### map()

```dart
Iterable<T> map<T>(T f(E element))
```

### expand()

```dart
Iterable<T> expand<T>(Iterable<T> f(E element))
```

### reduce()

```dart
E reduce(E combine(E previousValue, E element))
```

### fold()

```dart
T fold<T>(T initialValue, T combine(T previousValue, E element))
```

### skip()

```dart
Iterable<E> skip(int count)
```

### skipWhile()

```dart
Iterable<E> skipWhile(bool test(E element))
```

### take()

```dart
Iterable<E> take(int count)
```

### takeWhile()

```dart
Iterable<E> takeWhile(bool test(E element))
```

### toList()

```dart
List<E> toList({bool growable = true})
```

### toSet()

```dart
Set<E> toSet()
```

### add()

```dart
void add(E element)
```

### addAll()

```dart
void addAll(Iterable<E> iterable)
```

### remove()

```dart
bool remove(Object? element)
```

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

### clear()

```dart
void clear()
```

### cast()

```dart
List<R> cast<R>()
```

### removeLast()

```dart
E removeLast()
```

### sort()

```dart
void sort([int Function(E a, E b)? compare])
```

### shuffle()

```dart
void shuffle([Random? random])
```

### asMap()

```dart
Map<int, E> asMap()
```

### operator +()

```dart
List<E> operator +(List<E> other)
```

### sublist()

```dart
List<E> sublist(int start, [int? end])
```

### getRange()

```dart
Iterable<E> getRange(int start, int end)
```

### removeRange()

```dart
void removeRange(int start, int end)
```

### fillRange()

```dart
void fillRange(int start, int end, [E? fill])
```

### setRange()

```dart
void setRange(int start, int end, Iterable<E> iterable, [int skipCount = 0])
```

### replaceRange()

```dart
void replaceRange(int start, int end, Iterable<E> newContents)
```

### indexOf()

```dart
int indexOf(Object? element, [int start = 0])
```

### indexWhere()

```dart
int indexWhere(bool test(E element), [int start = 0])
```

### lastIndexOf()

```dart
int lastIndexOf(Object? element, [int? start])
```

### lastIndexWhere()

```dart
int lastIndexWhere(bool test(E element), [int? start])
```

### insert()

```dart
void insert(int index, E element)
```

### removeAt()

```dart
E removeAt(int index)
```

### insertAll()

```dart
void insertAll(int index, Iterable<E> iterable)
```

### setAll()

```dart
void setAll(int index, Iterable<E> iterable)
```

### reversed

```dart
Iterable<E> get reversed
```

### toString()

```dart
String toString()
```

### listToString()

```dart
String listToString(List<Object?> list)
```

Converts a [List] to a [String].

Converts [list] to a string by converting each element to a string (by calling [Object.toString]), joining them with ", ", and wrapping the result in `[` and `]`.

Handles circular references where converting one of the elements to a string ends up converting [list] to a string again.

# ListMixin

```dart
typedef ListMixin<E> = ListBase<E>
```

Base mixin implementation of a [List] class.

`ListMixin` can be used as a mixin to make a class implement the `List` interface.

This mixin implements all read operations using only the `length` and `operator[]` and members. It implements write operations using those and `add`, `length=` and `operator[]=`. Classes using this mixin should implement those five operations.

**NOTICE**: For backwards compatibility reasons, there is a default implementation of `add` which only works for lists with a nullable element type. For lists with a non-nullable element type, the `add` method must be implemented.

**NOTICE**: Forwarding just the four `length` and `[]` read/write operations to a normal growable [List] (as created by a `[]` literal) will give very bad performance for `add` and `addAll` operations of `ListMixin`. These operations are implemented by increasing the length of the list by one for each `add` operation, and repeatedly increasing the length of a growable list is not efficient. To avoid this, override 'add' and 'addAll' to also forward directly to the growable list, or, if possible, use `DelegatingList` from "package:collection/collection.dart" instead of a `ListMixin`.
