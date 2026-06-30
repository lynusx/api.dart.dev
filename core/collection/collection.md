Classes and utilities that supplement the collection support in dart:core.

To use this library in your code:

```dart
import 'dart:collection';
```

## Map

A finite mapping from unique keys to their associated values. Allows efficient lookup of the value associated with a key, if any, and iterating through the individual keys and values of the map. The [Map] interface has a number of implementations, including the following:

- [HashMap] is unordered, the order of iteration is not guaranteed.
- [LinkedHashMap] iterates in key insertion order.
- [SplayTreeMap] iterates the keys in sorted order.
- [UnmodifiableMapView] is a wrapper, an unmodifiable [Map] view of another `Map`.

## Set

A collection of objects in which each object can occur only once. The [Set] interface has a number of implementations, including the following:

- [HashSet] does not guarantee the order of the objects in the iterations.
- [LinkedHashSet] iterates the objects in insertion order.
- [SplayTreeSet] iterates the objects in sorted order.
- [UnmodifiableSetView] is a wrapper, an unmodifiable [Set] view of another `Set`.

## Queue

A queue is a sequence of elements that is intended to be modified, by adding or removing elements, only at its ends. Dart queues are _double ended_ queues, which means that they can be accessed equally from either end, and can therefore be used to implement both stack and queue behavior.

- [Queue] is the general interface for queues.
- [ListQueue] is a list-based queue. Default implementation for [Queue].
- [DoubleLinkedQueue] is a queue implementation based on a double-linked list.

## List

An indexable sequence of objects. Objects can be accessed using their position, index, in the sequence. [List] is also called an "array" in other programming languages.

- [UnmodifiableListView] is a wrapper, an unmodifiable [List] view of another `List`.

## LinkedList

[LinkedList] is a specialized double-linked list of elements that extends [LinkedListEntry]. Each element knows its own place in the linked list, as well as which list it is in. {@category Core}
