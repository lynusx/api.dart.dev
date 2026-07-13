补充 dart:core 中集合支持的类和工具。

要在代码中使用此库：

```dart
import 'dart:collection';
```

## Map

一种从唯一键到其关联值的有限映射。允许高效地查找与键关联的值（如果存在），并遍历映射中的各个键和值。[Map](https://www.yuque.com/thyname/dart.core/map) 接口有多种实现，包括以下几种：

- [HashMap](https://www.yuque.com/thyname/dart.collection/hashmap) 是无序的，不保证迭代顺序。
- [LinkedHashMap](https://www.yuque.com/thyname/dart.collection/linkedhashmap) 按键的插入顺序进行迭代。
- [SplayTreeMap](https://www.yuque.com/thyname/dart.collection/splaytreemap) 按排序顺序迭代键。
- [UnmodifiableMapView](https://www.yuque.com/thyname/dart.collection/unmodifiablemapview) 是一个包装器，是另一个 `Map` 的不可修改 [Map](https://www.yuque.com/thyname/dart.core/map) 视图。

## Set

一种对象集合，其中每个对象只能出现一次。[Set](https://www.yuque.com/thyname/dart.core/set) 接口有多种实现，包括以下几种：

- [HashSet](https://www.yuque.com/thyname/dart.collection/hashset) 不保证迭代时对象的顺序。
- [LinkedHashSet](https://www.yuque.com/thyname/dart.collection/linkedhashset) 按插入顺序迭代对象。
- [SplayTreeSet](https://www.yuque.com/thyname/dart.collection/splaytreeset) 按排序顺序迭代对象。
- [UnmodifiableSetView](https://www.yuque.com/thyname/dart.collection/unmodifiablesetview) 是一个包装器，是另一个 `Set` 的不可修改 [Set](https://www.yuque.com/thyname/dart.core/set) 视图。

## Queue

队列是一系列元素，只能在其两端通过添加或移除元素来修改。Dart 中的队列是**双端**队列，这意味着可以从任意一端进行访问，因此既可以实现栈的行为，也可以实现队列的行为。

- [Queue](https://www.yuque.com/thyname/dart.collection/queue) 是队列的通用接口。
- [ListQueue](https://www.yuque.com/thyname/dart.collection/listqueue) 是基于列表实现的队列，是 [Queue](https://www.yuque.com/thyname/dart.collection/queue) 的默认实现。
- [DoubleLinkedQueue](https://www.yuque.com/thyname/dart.collection/doublelinkedqueue) 是基于双向链表实现的队列。

## List

可通过索引访问的对象序列。可以使用对象在序列中的位置（索引）来访问对象。[List](https://www.yuque.com/thyname/dart.core/list) 在其他编程语言中也被称为“数组”。

- [UnmodifiableListView](https://www.yuque.com/thyname/dart.collection/unmodifiablelistview) 是一个包装器，是另一个 `List` 的不可修改 [List](https://www.yuque.com/thyname/dart.core/list) 视图。

## LinkedList

[LinkedList](https://www.yuque.com/thyname/dart.collection/linkedlist) 是一种特殊的双向链表，其元素继承自 [LinkedListEntry](https://www.yuque.com/thyname/dart.collection/linkedlistentry)。每个元素都知道自己在链表中的位置，以及自己所属的链表。
