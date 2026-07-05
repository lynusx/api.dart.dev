# Queue

```dart
abstract interface class Queue<E> implements Iterable<E>, _QueueIterable<E> {}
```

A [Queue] is a collection that can be manipulated at both ends. One can iterate over the elements of a queue through [forEach] or with an [Iterator].

It is generally not allowed to modify the queue (add or remove entries) while an operation in the queue is being performed, for example during a call to [forEach]. Modifying the queue while it is being iterated will most likely break the iteration. This goes both for using the [iterator] directly, or for iterating an `Iterable` returned by a method like [map] or [where].

Example:

```dart
final queue = Queue<int>(); // ListQueue() by default
print(queue.runtimeType); // ListQueue

// Adding items to queue
queue.addAll([1, 2, 3]);
queue.addFirst(0);
queue.addLast(10);
print(queue); // {0, 1, 2, 3, 10}

// Removing items from queue
queue.removeFirst();
queue.removeLast();
print(queue); // {1, 2, 3}
```

## 构造函数

### Queue()

```dart
Queue()
```

Creates a queue.

### Queue.from()

```dart
Queue<E>.from(Iterable elements)
```

Creates a queue containing all [elements].

The element order in the queue is as if the elements were added using [addLast] in the order provided by [elements].iterator.

All the [elements] should be instances of [E]. The `elements` iterable itself may have any element type, so this constructor can be used to down-cast a `Queue`, for example as:

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    Queue<SubType>.from(superQueue.whereType<SubType>());
```

### Queue.of()

```dart
Queue.of(Iterable<E> elements)
```

Creates a queue from [elements].

The element order in the queue is as if the elements were added using [addLast] in the order provided by [elements].iterator.

## 静态方法

### castFrom()

```dart
Queue<T> castFrom<S, T>(Queue<S> source)
```

Adapts [source] to be a `Queue<T>`.

Any time the queue would produce an element that is not a [T], the element access will throw.

When a [T] value is stored into the adapted queue, the operation will throw unless the value is also an instance of [S].

If all accessed elements of [source] are actually instances of [T], and if all elements stored into the returned queue are actually instances of [S], then the returned queue can be used as a `Queue<T>`.

Methods which accept `Object?` as argument, like [contains] and [remove], will pass the argument directly to this queue's method without any checks.

## 方法

### cast()

```dart
Queue<R> cast<R>()
```

Provides a view of this queue as a queue of [R] instances, if necessary.

If this queue contains only instances of [R], all read operations will work correctly. If any operation tries to access an element that is not an instance of [R], the access will throw instead.

Elements added to the queue (e.g., by using [addFirst] or [addAll]) must be instances of [R] to be valid arguments to the adding function, and they must also be instances of [E] to be accepted by this queue as well.

Methods which accept `Object?` as argument, like [contains] and [remove], will pass the argument directly to the this queue's method without any checks. That means that you can do `queueOfStrings.cast<int>().remove("a")` successfully, even if it looks like it shouldn't have any effect.

### removeFirst()

```dart
E removeFirst()
```

Removes and returns the first element of this queue.

The queue must not be empty when this method is called.

### removeLast()

```dart
E removeLast()
```

Removes and returns the last element of the queue.

The queue must not be empty when this method is called.

### addFirst()

```dart
void addFirst(E value)
```

Adds [value] at the beginning of the queue.

### addLast()

```dart
void addLast(E value)
```

Adds [value] at the end of the queue.

### add()

```dart
void add(E value)
```

Adds [value] at the end of the queue.

### remove()

```dart
bool remove(Object? value)
```

Removes a single instance of [value] from the queue.

Returns `true` if a value was removed, or `false` if the queue contained no element equal to [value].

### addAll()

```dart
void addAll(Iterable<E> iterable)
```

Adds all elements of [iterable] at the end of the queue. The length of the queue is extended by the length of [iterable].

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

Removes all elements matched by [test] from the queue.

The `test` function must not throw or modify the queue.

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

Removes all elements not matched by [test] from the queue.

The `test` function must not throw or modify the queue.

### clear()

```dart
void clear()
```

Removes all elements in the queue. The size of the queue becomes zero.

---



# DoubleLinkedQueue

```dart
final class DoubleLinkedQueue<E> extends Iterable<E> implements Queue<E> {}
```

A [Queue] implementation based on a double-linked list.

Allows constant time add, remove-at-ends and peek operations.

## 构造函数

### DoubleLinkedQueue()

```dart
DoubleLinkedQueue<E>()
```

### DoubleLinkedQueue.from()

```dart
DoubleLinkedQueue<E>.from(Iterable elements)
```

Creates a double-linked queue containing all [elements].

The element order in the queue is as if the elements were added using [addLast] in the order provided by [elements].iterator.

All the [elements] should be instances of [E]. The [elements] iterable itself may have any element type, so this constructor can be used to down-cast a [Queue], for example as:

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    DoubleLinkedQueue<SubType>.from(superQueue.whereType<SubType>());
```

### DoubleLinkedQueue.of()

```dart
DoubleLinkedQueue<E>.of(Iterable<E> elements)
```

Creates a double-linked queue from [elements].

The element order in the queue is as if the elements were added using [addLast] in the order provided by [elements].iterator.

## 属性

* first
* last
* single
* iterator
* length
* isEmpty

## 方法

* cast()
* addLast()
* addFirst()
* add()
* addAll()
* removeLast()
* removeFirst()
* remove()
* removeWhere()
* retainWhere()
* clear()
* toString()

### firstEntry()

```dart
DoubleLinkedQueueEntry<E>? firstEntry()
```

The entry object of the first element in the queue.

Each element of the queue has an associated [DoubleLinkedQueueEntry].

Returns the entry object corresponding to the first element of the queue, or `null` if the queue is empty.

The entry objects can also be accessed using [lastEntry], and they can be iterated using [DoubleLinkedQueueEntry.nextEntry] and [DoubleLinkedQueueEntry.previousEntry].

### lastEntry()

```dart
DoubleLinkedQueueEntry<E>? lastEntry()
```

The entry object of the last element in the queue.

Each element of the queue has an associated [DoubleLinkedQueueEntry].

Returns the entry object corresponding to the last element of the queue, or `null` if the queue is empty.

The entry objects can also be accessed using [firstEntry], and they can be iterated using [DoubleLinkedQueueEntry.nextEntry] and [DoubleLinkedQueueEntry.previousEntry].

### forEachEntry()

```dart
void forEachEntry(void action(DoubleLinkedQueueEntry<E> element))
```

Calls [action] for each entry object of this double-linked queue.

Each element of the queue has an associated [DoubleLinkedQueueEntry]. This method iterates the entry objects from first to last and calls [action] with each object in turn.

The entry objects can also be accessed using [firstEntry] and [lastEntry], and iterated using [DoubleLinkedQueueEntry.nextEntry()] and [DoubleLinkedQueueEntry.previousEntry()].

The [action] function can use methods on [DoubleLinkedQueueEntry] to remove the entry or it can insert elements before or after the entry. If the current entry is removed, iteration continues with the entry that was following the current entry when [action] was called. Any elements inserted after the current element before it is removed will not be visited by the iteration.

---



# ListQueue

```dart
final class ListQueue<E> extends ListIterable<E> implements Queue<E> {}
```

List based [Queue].

Keeps a cyclic buffer of elements, and grows to a larger buffer when it fills up. This guarantees constant time peek and remove operations, and amortized constant time add operations.

The structure is efficient for any queue or stack usage.

Example:

```dart
final queue = ListQueue<int>();
```

To add objects to a queue, use [add], [addAll], [addFirst] or[addLast].

```dart continued
queue.add(5);
queue.addFirst(0);
queue.addLast(10);
queue.addAll([1, 2, 3]);
print(queue); // {0, 5, 10, 1, 2, 3}
```

To check if the queue is empty, use [isEmpty] or [isNotEmpty]. To find the number of queue entries, use [length].

```dart continued
final isEmpty = queue.isEmpty; // false
final queueSize = queue.length; // 6
```

To get first or last item from queue, use [first] or [last].

```dart continued
final first = queue.first; // 0
final last = queue.last; // 3
```

To get item value using index, use [elementAt].

```dart continued
final itemAt = queue.elementAt(2); // 10
```

To convert queue to list, call [toList].

```dart continued
final numbers = queue.toList();
print(numbers); // [0, 5, 10, 1, 2, 3]
```

To remove item from queue, call [remove], [removeFirst] or [removeLast].

```dart continued
queue.remove(10);
queue.removeFirst();
queue.removeLast();
print(queue); // {5, 1, 2}
```

To remove multiple elements at the same time, use [removeWhere].

```dart continued
queue.removeWhere((element) => element == 1);
print(queue); // {5, 2}
```

To remove all elements in this queue that do not meet a condition, use [retainWhere].

```dart continued
queue.retainWhere((element) => element < 4);
print(queue); // {2}
```

To remove all items and empty the set, use [clear].

```dart continued
queue.clear();
print(queue.isEmpty); // true
print(queue); // {}
```

## 构造函数

### ListQueue()

```dart
ListQueue<E>([int? initialCapacity])
```

Create an empty queue.

If [initialCapacity] is given, prepare the queue for at least that many elements.

### ListQueue.from()

```dart
ListQueue<E>.from(Iterable elements)
```

Create a `ListQueue` containing all [elements].

The elements are added to the queue, as by [addLast], in the order given by `elements.iterator`.

All the [elements] should be instances of [E]. The `elements` iterable itself may have any element type, so this constructor can be used to down-cast a `Queue`, for example as:

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    ListQueue<SubType>.from(superQueue.whereType<SubType>());
```

Example:

```dart
final numbers = <num>[10, 20, 30];
final queue = ListQueue<int>.from(numbers);
print(queue); // {10, 20, 30}
```

### ListQueue.of()

```dart
ListQueue<E>.of(Iterable<E> elements)
```

Create a `ListQueue` from [elements].

The elements are added to the queue, as by [addLast], in the order given by `elements.iterator`. Example:

```dart
final baseQueue = ListQueue.of([1.0, 2.0, 3.0]); // A ListQueue<double>
final numQueue = ListQueue<num>.of(baseQueue);
print(numQueue); // {1.0, 2.0, 3.0}
```

## 属性

* iterator
* isEmpty
* length
* first
* last
* single



## 方法

* cast()
* forEach()
* elementAt()
* toList()
* add()
* addAll()
* remove()
* clear()
* addLast()
* addFirst()
* removeFirst()
* removeLast()
* toString()

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

Remove all elements matched by [test].

This method is inefficient since it works by repeatedly removing single elements, each of which can take linear time.

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

Remove all elements not matched by [test].

This method is inefficient since it works by repeatedly removing single elements, each of which can take linear time.
