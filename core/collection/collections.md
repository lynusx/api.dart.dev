# UnmodifiableListView

```dart
class UnmodifiableListView<E> extends UnmodifiableListBase<E> {}
```

An unmodifiable [List] view of another List.

The source of the elements may be a [List] or any [Iterable] with efficient [Iterable.length] and [Iterable.elementAt].

```dart
final numbers = <int>[10, 20, 30];
final unmodifiableListView = UnmodifiableListView(numbers);

// Insert new elements into the original list.
numbers.addAll([40, 50]);
print(unmodifiableListView); // [10, 20, 30, 40, 50]

unmodifiableListView.remove(20); // Throws.
```

### UnmodifiableListView()

```dart
UnmodifiableListView(Iterable<E> source)
```

Creates an unmodifiable list backed by [source].

The [source] of the elements may be a [List] or any [Iterable] with efficient [Iterable.length] and [Iterable.elementAt].

### cast()

```dart
List<R> cast<R>()
```

### length

```dart
int get length
```

### operator []()

```dart
E operator [](int index)
```
