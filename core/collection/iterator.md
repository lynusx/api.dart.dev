# HasNextIterator

```dart
final class HasNextIterator<E> {}
```

Wrapper for [Iterator] providing the pre-Dart 1.0 iterator interface.

This class should not be used in new code.

The [HasNextIterator] class wraps an [Iterator] and provides methods to iterate over an object using [hasNext] and [next].

The [HasNextIterator] does not implement the [Iterator] interface.

This class was intended for migration to the current [Iterator] interface, from an earlier interface using [hasNext] and [next]. The API change happened in the Dart 1.0 release. Any other use of this class should be migrated to using the current API directly, e.g., using a separate variable to cache the [Iterator.moveNext] result, so that [hasNext] can be checked multiple times.

### HasNextIterator()

```dart
HasNextIterator(Iterator<E> iterator)
```

### hasNext

```dart
bool get hasNext
```

Whether the iterator has a next element.

Should be checked to be `true` before calling [next].

### next()

```dart
E next()
```

Provides the next element of the iterable, and moves past it.

Must only be used when [hasNext] is `true`. The value of [hasNext] can change after calling this method.
