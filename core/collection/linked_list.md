# LinkedList

```dart
base class LinkedList<E extends LinkedListEntry<E>> extends Iterable<E> {}
```

A specialized double-linked list of elements that extends [LinkedListEntry].

This is not a generic data structure. It only accepts elements that _extend_ the [LinkedListEntry] class. See the [Queue] implementations for generic collections that allow constant time adding and removing at the ends.

This is not a [List] implementation. Despite its name, this class does not implement the [List] interface. It does not allow constant time lookup by index.

Because the elements themselves contain the links of this linked list, each element can be in only one list at a time. To add an element to another list, it must first be removed from its current list (if any). For the same reason, the [remove] and [contains] methods are based on _identity_, even if the [LinkedListEntry] chooses to override [Object.==].

In return, each element knows its own place in the linked list, as well as which list it is in. This allows constant time [LinkedListEntry.insertAfter], [LinkedListEntry.insertBefore] and [LinkedListEntry.unlink] operations when all you have is the element.

A `LinkedList` also allows constant time adding and removing at either end, and a constant time length getter.

Example:

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

Constructs a new empty linked list.

## 属性

* iterator
* length
* first
* last
* single
* isEmpty

## 方法

### addFirst()

```dart
void addFirst(E entry)
```

Adds [entry] to the beginning of the linked list.

### add()

```dart
void add(E entry)
```

Adds [entry] to the end of the linked list.

### addAll()

```dart
void addAll(Iterable<E> entries)
```

Adds [entries] to the end of the linked list.

### remove()

```dart
bool remove(E entry)
```

Removes [entry] from the linked list.

Returns false and does nothing if [entry] is not in this linked list.

This is equivalent to calling `entry.unlink()` if the entry is in this list.

### contains()

```dart
bool contains(Object? entry)
```

Whether [entry] is a [LinkedListEntry] belonging to this list.

The [entry] is considered as belonging to this list if its [LinkedListEntry.list] is this list.

### clear()

```dart
void clear()
```

Remove all elements from this linked list.

### forEach()

```dart
void forEach(void action(E entry))
```

Call [action] with each entry in this linked list.

It's an error if [action] modifies the linked list.

---



# LinkedListEntry

```dart
abstract base mixin class LinkedListEntry<E extends LinkedListEntry<E>> {}
```

An object that can be an element in a [LinkedList].

All elements of a `LinkedList` must extend this class. The class provides the internal links that link elements together in the `LinkedList`, and a reference to the linked list itself that an element is currently part of.

An entry can be in at most one linked list at a time. While an entry is in a linked list, the [list] property points to that linked list, and otherwise the `list` property is `null`.

When created, an entry is not in any linked list.

## 属性

### list

```dart
LinkedList<E>? get list
```

The linked list containing this element.

The value is `null` if this entry is not currently in any list.

### next

```dart
E? get next
```

The successor of this element in its linked list.

The value is `null` if there is no successor in the linked list, or if this entry is not currently in any list.

### previous

```dart
E? get previous
```

The predecessor of this element in its linked list.

The value is `null` if there is no predecessor in the linked list, or if this entry is not currently in any list.

## 方法

### unlink()

```dart
void unlink()
```

Unlink the element from its linked list.

The entry must currently be in a linked list when this method is called.

### insertAfter()

```dart
void insertAfter(E entry)
```

Insert an element after this element in this element's linked list.

This entry must be in a linked list when this method is called. The [entry] must not be in a linked list.

### insertBefore()

```dart
void insertBefore(E entry)
```

Insert an element before this element in this element's linked list.

This entry must be in a linked list when this method is called. The [entry] must not be in a linked list.
