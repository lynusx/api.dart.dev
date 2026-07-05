# Queue

```dart
abstract interface class Queue<E> implements Iterable<E>, _QueueIterable<E> {}
```

[Queue] 是一个可以在两端进行操作的集合。可以通过 [forEach] 或使用 [Iterator] 迭代队列中的元素。

通常不允许在队列执行操作期间修改队列（添加或删除条目），例如在调用 [forEach] 期间进行修改。在迭代队列时修改队列很可能会破坏迭代过程。无论是直接使用 [iterator]，还是迭代通过 [map] 或 [where] 等方法返回的 `Iterable`，都是如此。

示例：

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

创建一个队列。

### Queue.from()

```dart
Queue<E>.from(Iterable elements)
```

创建一个包含所有 [elements] 的队列。

队列中元素的顺序如同按照 [elements].iterator 提供的顺序使用 [addLast] 添加一样。

所有 [elements] 都应该是 [E] 的实例。`elements` 可迭代对象本身可以是任意元素类型，因此该构造函数可用于对 `Queue` 进行向下转型，例如：

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    Queue<SubType>.from(superQueue.whereType<SubType>());
```

### Queue.of()

```dart
Queue.of(Iterable<E> elements)
```

根据 [elements] 创建一个队列。

队列中元素的顺序如同按照 [elements].iterator 提供的顺序使用 [addLast] 添加一样。

## 静态方法

### castFrom()

```dart
Queue<T> castFrom<S, T>(Queue<S> source)
```

将 [source] 适配为 `Queue<T>`。

每当队列要产生一个不是 [T] 类型的元素时，该元素访问将抛出异常。

当一个 [T] 类型的值被存储到适配后的队列中时，除非该值同时也是 [S] 的实例，否则该操作将抛出异常。

如果 [source] 中被访问的所有元素实际上都是 [T] 的实例，并且存储到返回队列中的所有元素实际上都是 [S] 的实例，那么返回的队列可以用作 `Queue<T>`。

接受 `Object?` 作为参数的方法，如 [contains] 和 [remove]，会将参数直接传递给该队列的方法，而不进行任何检查。

## 方法

### cast()

```dart
Queue<R> cast<R>()
```

在必要时，提供该队列作为 [R] 实例队列的视图。

如果该队列只包含 [R] 的实例，则所有读取操作都能正常工作。如果任何操作尝试访问一个不是 [R] 实例的元素，则该访问将抛出异常。

添加到队列中的元素（例如，通过使用 [addFirst] 或 [addAll]）必须是 [R] 的实例才能作为添加函数的有效参数，同时也必须是 [E] 的实例才能被该队列接受。

接受 `Object?` 作为参数的方法，如 [contains] 和 [remove]，会将参数直接传递给该队列的方法，而不进行任何检查。这意味着即使看起来不应该有任何效果，你也可以成功执行 `queueOfStrings.cast<int>().remove("a")`。

### removeFirst()

```dart
E removeFirst()
```

移除并返回该队列的第一个元素。

调用该方法时队列不能为空。

### removeLast()

```dart
E removeLast()
```

移除并返回该队列的最后一个元素。

调用该方法时队列不能为空。

### addFirst()

```dart
void addFirst(E value)
```

在队列开头添加 [value]。

### addLast()

```dart
void addLast(E value)
```

在队列末尾添加 [value]。

### add()

```dart
void add(E value)
```

在队列末尾添加 [value]。

### remove()

```dart
bool remove(Object? value)
```

从队列中移除单个 [value] 实例。

如果移除了某个值，则返回 `true`；如果队列中不包含与 [value] 相等的元素，则返回 `false`。

### addAll()

```dart
void addAll(Iterable<E> iterable)
```

将 [iterable] 中的所有元素添加到队列末尾。队列的长度会按照 [iterable] 的长度进行扩展。

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

从队列中移除所有与 [test] 匹配的元素。

`test` 函数不能抛出异常或修改队列。

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

从队列中移除所有与 [test] 不匹配的元素。

`test` 函数不能抛出异常或修改队列。

### clear()

```dart
void clear()
```

移除队列中的所有元素。队列的大小变为零。

---

# DoubleLinkedQueue

```dart
final class DoubleLinkedQueue<E> extends Iterable<E> implements Queue<E> {}
```

一个基于双向链表实现的 [Queue]。

支持常数时间的添加、两端移除和查看（peek）操作。

## 构造函数

### DoubleLinkedQueue()

```dart
DoubleLinkedQueue<E>()
```

### DoubleLinkedQueue.from()

```dart
DoubleLinkedQueue<E>.from(Iterable elements)
```

创建一个包含所有 [elements] 的双向链表队列。

队列中元素的顺序如同按照 [elements].iterator 提供的顺序使用 [addLast] 添加一样。

所有 [elements] 都应该是 [E] 的实例。[elements] 可迭代对象本身可以是任意元素类型，因此该构造函数可用于对 [Queue] 进行向下转型，例如：

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    DoubleLinkedQueue<SubType>.from(superQueue.whereType<SubType>());
```

### DoubleLinkedQueue.of()

```dart
DoubleLinkedQueue<E>.of(Iterable<E> elements)
```

根据 [elements] 创建一个双向链表队列。

队列中元素的顺序如同按照 [elements].iterator 提供的顺序使用 [addLast] 添加一样。

## 属性

- first
- last
- single
- iterator
- length
- isEmpty

## 方法

- cast()
- addLast()
- addFirst()
- add()
- addAll()
- removeLast()
- removeFirst()
- remove()
- removeWhere()
- retainWhere()
- clear()
- toString()

### firstEntry()

```dart
DoubleLinkedQueueEntry<E>? firstEntry()
```

队列中第一个元素对应的条目（entry）对象。

队列中的每个元素都有一个关联的 [DoubleLinkedQueueEntry]。

返回队列中第一个元素对应的条目对象，如果队列为空则返回 `null`。

这些条目对象也可以通过 [lastEntry] 访问，并可以使用 [DoubleLinkedQueueEntry.nextEntry] 和 [DoubleLinkedQueueEntry.previousEntry] 进行遍历。

### lastEntry()

```dart
DoubleLinkedQueueEntry<E>? lastEntry()
```

队列中最后一个元素对应的条目对象。

队列中的每个元素都有一个关联的 [DoubleLinkedQueueEntry]。

返回队列中最后一个元素对应的条目对象，如果队列为空则返回 `null`。

这些条目对象也可以通过 [firstEntry] 访问，并可以使用 [DoubleLinkedQueueEntry.nextEntry] 和 [DoubleLinkedQueueEntry.previousEntry] 进行遍历。

### forEachEntry()

```dart
void forEachEntry(void action(DoubleLinkedQueueEntry<E> element))
```

对该双向链表队列的每个条目对象调用 [action]。

队列中的每个元素都有一个关联的 [DoubleLinkedQueueEntry]。该方法从第一个到最后一个遍历条目对象，并依次对每个对象调用 [action]。

这些条目对象也可以通过 [firstEntry] 和 [lastEntry] 访问，并可以使用 [DoubleLinkedQueueEntry.nextEntry()] 和 [DoubleLinkedQueueEntry.previousEntry()] 进行遍历。

[action] 函数可以使用 [DoubleLinkedQueueEntry] 上的方法来移除该条目，或者在该条目之前或之后插入元素。如果当前条目被移除，迭代将从调用 [action] 时紧跟在当前条目之后的条目继续进行。在当前元素被移除之前插入到其之后的任何元素都不会被本次迭代访问到。

---

# ListQueue

```dart
final class ListQueue<E> extends ListIterable<E> implements Queue<E> {}
```

基于列表实现的 [Queue]。

维护一个元素的循环缓冲区，当缓冲区填满时会扩展为更大的缓冲区。这保证了查看（peek）和移除操作具有常数时间复杂度，添加操作具有均摊常数时间复杂度。

该数据结构对于任何队列或栈的使用场景都是高效的。

示例：

```dart
final queue = ListQueue<int>();
```

要向队列中添加对象，请使用 [add]、[addAll]、[addFirst] 或 [addLast]。

```dart continued
queue.add(5);
queue.addFirst(0);
queue.addLast(10);
queue.addAll([1, 2, 3]);
print(queue); // {0, 5, 10, 1, 2, 3}
```

要检查队列是否为空，请使用 [isEmpty] 或 [isNotEmpty]。要获取队列中条目的数量，请使用 [length]。

```dart continued
final isEmpty = queue.isEmpty; // false
final queueSize = queue.length; // 6
```

要获取队列中的第一个或最后一个元素，请使用 [first] 或 [last]。

```dart continued
final first = queue.first; // 0
final last = queue.last; // 3
```

要通过索引获取元素值，请使用 [elementAt]。

```dart continued
final itemAt = queue.elementAt(2); // 10
```

要将队列转换为列表，请调用 [toList]。

```dart continued
final numbers = queue.toList();
print(numbers); // [0, 5, 10, 1, 2, 3]
```

要从队列中移除元素，请调用 [remove]、[removeFirst] 或 [removeLast]。

```dart continued
queue.remove(10);
queue.removeFirst();
queue.removeLast();
print(queue); // {5, 1, 2}
```

要同时移除多个元素，请使用 [removeWhere]。

```dart continued
queue.removeWhere((element) => element == 1);
print(queue); // {5, 2}
```

要移除该队列中所有不满足条件的元素，请使用 [retainWhere]。

```dart continued
queue.retainWhere((element) => element < 4);
print(queue); // {2}
```

要移除所有元素并清空该集合，请使用 [clear]。

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

创建一个空队列。

如果给定了 [initialCapacity]，则为队列预留至少能容纳该数量元素的空间。

### ListQueue.from()

```dart
ListQueue<E>.from(Iterable elements)
```

创建一个包含所有 [elements] 的 `ListQueue`。

元素按照 `elements.iterator` 给出的顺序，如同通过 [addLast] 一样被添加到队列中。

所有 [elements] 都应该是 [E] 的实例。`elements` 可迭代对象本身可以是任意元素类型，因此该构造函数可用于对 `Queue` 进行向下转型，例如：

```dart
Queue<SuperType> superQueue = ...;
Queue<SubType> subQueue =
    ListQueue<SubType>.from(superQueue.whereType<SubType>());
```

示例：

```dart
final numbers = <num>[10, 20, 30];
final queue = ListQueue<int>.from(numbers);
print(queue); // {10, 20, 30}
```

### ListQueue.of()

```dart
ListQueue<E>.of(Iterable<E> elements)
```

根据 [elements] 创建一个 `ListQueue`。

元素按照 `elements.iterator` 给出的顺序，如同通过 [addLast] 一样被添加到队列中。示例：

```dart
final baseQueue = ListQueue.of([1.0, 2.0, 3.0]); // A ListQueue<double>
final numQueue = ListQueue<num>.of(baseQueue);
print(numQueue); // {1.0, 2.0, 3.0}
```

## 属性

- iterator
- isEmpty
- length
- first
- last
- single

## 方法

- cast()
- forEach()
- elementAt()
- toList()
- add()
- addAll()
- remove()
- clear()
- addLast()
- addFirst()
- removeFirst()
- removeLast()
- toString()

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

移除所有与 [test] 匹配的元素。

该方法效率较低，因为它通过重复移除单个元素来实现，而每次移除都可能花费线性时间。

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

移除所有与 [test] 不匹配的元素。

该方法效率较低，因为它通过重复移除单个元素来实现，而每次移除都可能花费线性时间。
