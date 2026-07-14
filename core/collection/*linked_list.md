# LinkedList

```dart
base class LinkedList<E extends LinkedListEntry<E>> extends Iterable<E> {}
```

一个特殊的双向链表，其元素扩展自 [LinkedListEntry](https://www.yuque.com/thyname/dart.collection/linkedlistentry)。

这不是一个通用的数据结构。它只接受*扩展*自 [LinkedListEntry](https://www.yuque.com/thyname/dart.collection/linkedlistentry) 类的元素。关于允许在两端进行常量时间添加和删除操作的通用集合，请参阅 [Queue](https://www.yuque.com/thyname/dart.collection/queue) 的实现。

这不是 [List](https://www.yuque.com/thyname/dart.core/list) 的实现。尽管名称如此，该类并未实现 [List](https://www.yuque.com/thyname/dart.core/list) 接口。它不支持通过索引进行常量时间查找。

由于元素本身包含了该链表的链接信息，因此每个元素同时只能存在于一个链表中。要将元素添加到另一个链表，必须先将其从当前链表中移除（如果存在的话）。出于同样的原因，[remove] 和 [contains] 方法基于*标识*进行判断，即使 [LinkedListEntry](https://www.yuque.com/thyname/dart.collection/linkedlistentry) 重写了 [Object.==] 也是如此。

作为回报，每个元素都知道自己在链表中的位置，以及它所属的链表。这使得在只有元素本身的情况下，[LinkedListEntry.insertAfter]、[LinkedListEntry.insertBefore] 和 [LinkedListEntry.unlink] 操作可以在常量时间内完成。

`LinkedList` 还允许在两端进行常量时间的添加和删除操作，并提供常量时间的 length 获取器。

示例：

```dart
final class EntryItem extends LinkedListEntry<EntryItem> {
  final int id;
  final String text;
  EntryItem(this.id, this.text);

  @override
  String toString() {
    return '$id : $text';
  }
}

void main() {
  final linkedList = LinkedList<EntryItem>();
  linkedList
      .addAll([EntryItem(1, 'A'), EntryItem(2, 'B'), EntryItem(3, 'C')]);
  print(linkedList.first); // 1 : A
  print(linkedList.last); // 3 : C

  // Add new item after first item.
  linkedList.first.insertAfter(EntryItem(15, 'E'));
  // Add new item before last item.
  linkedList.last.insertBefore(EntryItem(10, 'D'));
  // Iterate items.
  for (var entry in linkedList) {
    print(entry);
    // 1 : A
    // 15 : E
    // 2 : B
    // 10 : D
    // 3 : C
  }

  // Remove item using index from list.
  linkedList.elementAt(2).unlink();
  print(linkedList); // (1 : A, 15 : E, 10 : D, 3 : C)
  // Remove first item.
  linkedList.first.unlink();
  print(linkedList); // (15 : E, 10 : D, 3 : C)
  // Remove last item from list.
  linkedList.remove(linkedList.last);
  print(linkedList); // (15 : E, 10 : D)
  // Remove all items.
  linkedList.clear();
  print(linkedList.length); // 0
  print(linkedList.isEmpty); // true
}
```

## 构造函数

### LinkedList()

```dart
LinkedList<E extends LinkedListEntry<E>>()
```

构造一个新的空链表。

## 属性

- iterator
- length
- first
- last
- single
- isEmpty

## 方法

### addFirst()

```dart
void addFirst(E entry)
```

将 [entry] 添加到链表的开头。

### add()

```dart
void add(E entry)
```

将 [entry] 添加到链表的末尾。

### addAll()

```dart
void addAll(Iterable<E> entries)
```

将 [entries] 添加到链表的末尾。

### remove()

```dart
bool remove(E entry)
```

从链表中移除 [entry]。

如果 [entry] 不在该链表中，则返回 false 并且不执行任何操作。

如果该元素在链表中，这等价于调用 `entry.unlink()`。

### contains()

```dart
bool contains(Object? entry)
```

判断 [entry] 是否是属于该链表的 [LinkedListEntry](https://www.yuque.com/thyname/dart.collection/linkedlistentry)。

如果 [entry] 的 [LinkedListEntry.list] 是该链表，则认为它属于该链表。

### clear()

```dart
void clear()
```

移除该链表中的所有元素。

### forEach()

```dart
void forEach(void action(E entry))
```

对链表中的每个元素调用 [action]。

如果 [action] 修改了该链表，则会出错。

---

# LinkedListEntry

```dart
abstract base mixin class LinkedListEntry<E extends LinkedListEntry<E>> {}
```

可以作为 [LinkedList](https://www.yuque.com/thyname/dart.collection/linkedlist) 元素的对象。

`LinkedList` 的所有元素都必须扩展该类。该类提供了将元素连接在一起的内部链接，以及对该元素当前所属链表的引用。

一个元素同一时间最多只能存在于一个链表中。当元素在链表中时，[list] 属性指向该链表；否则，`list` 属性为 `null`。

元素被创建时，尚未加入任何链表。

## 属性

### list

```dart
LinkedList<E>? get list
```

包含该元素的链表。

如果该元素当前不在任何链表中，则值为 `null`。

### next

```dart
E? get next
```

该元素在链表中的后继元素。

如果链表中没有后继元素，或该元素当前不在任何链表中，则值为 `null`。

### previous

```dart
E? get previous
```

该元素在链表中的前驱元素。

如果链表中没有前驱元素，或该元素当前不在任何链表中，则值为 `null`。

## 方法

### unlink()

```dart
void unlink()
```

将该元素从其所属的链表中断开连接。

调用该方法时，该元素必须当前处于某个链表中。

### insertAfter()

```dart
void insertAfter(E entry)
```

在该元素所属链表中，将一个元素插入到该元素之后。

调用该方法时，该元素必须处于某个链表中。[entry] 不能已处于任何链表中。

### insertBefore()

```dart
void insertBefore(E entry)
```

在该元素所属链表中，将一个元素插入到该元素之前。

调用该方法时，该元素必须处于某个链表中。[entry] 不能已处于任何链表中。
