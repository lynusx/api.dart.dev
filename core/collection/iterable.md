# IterableMixin

```dart
typedef IterableMixin<E> = Iterable<E>
```

This [Iterable] mixin implements all [Iterable] members except `iterator`.

All other methods are implemented in terms of `iterator`.

---



# IterableBase

```dart
typedef IterableBase<E> = Iterable<E>
```

Base class for implementing [Iterable].

This class implements all methods of [Iterable], except [Iterable.iterator], in terms of `iterator`.

---



# NullableIterableExtensions

```dart
extension NullableIterableExtensions<T extends Object> on Iterable<T?> {}
```

Operations on iterables with nullable elements.

## 属性

### nonNulls

```dart
Iterable<T> get nonNulls
```

The non-`null` elements of this iterable.

The same elements as this iterable, except that `null` values are omitted.

---



# IterableExtensions

```dart
extension IterableExtensions<T> on Iterable<T> {}
```

Operations on iterables.

## 属性

### indexed

```dart
Iterable<(int, T)> get indexed
```

Pairs of elements of the indices and elements of this iterable.

The elements are `(0, this.first)` through `(this.length - 1, this.last)`, in index/iteration order.

### firstOrNull

```dart
T? get firstOrNull
```

The first element of this iterator, or `null` if the iterable is empty.

### lastOrNull

```dart
T? get lastOrNull
```

The last element of this iterable, or `null` if the iterable is empty.

This computation may not be efficient. The last value is potentially found by iterating the entire iterable and temporarily storing every value. The process only iterates the iterable once. If iterating more than once is not a problem, it may be more efficient for some iterables to do:

```dart
var lastOrNull = iterable.isEmpty ? null : iterable.last;
```

### singleOrNull

```dart
T? get singleOrNull
```

The single element of this iterator, or `null`.

If the iterator has precisely one element, this is that element. Otherwise, if the iterator has zero elements, or it has two or more, the value is `null`.

## 方法

### elementAtOrNull()

```dart
T? elementAtOrNull(int index)
```

The element at position [index] of this iterable, or `null`.

The [index] is zero based, and must be non-negative.

Returns the result of `elementAt(index)` if the iterable has at least `index + 1` elements, and `null` otherwise.
