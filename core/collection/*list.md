# ListBase

```dart
abstract mixin class ListBase<E> implements List<E> {}
```

`List` 的抽象实现。

`ListBase` 可以用作实现 `List` 接口的基类。

该类仅使用 `length` 和 `operator[]` 成员实现了所有读取操作。它使用这些成员以及 `add`、`length=` 和 `operator[]=` 实现了写入操作。使用该基类的类应实现这五个操作。

**注意**：出于向后兼容的原因，`add` 提供了一个默认实现，但该实现仅适用于元素类型可为空的列表。对于元素类型非空的列表，必须实现 `add` 方法。

**注意**：如果仅将 `length` 和 `[]` 这四个读/写操作转发给一个普通的可增长 [List](https://www.yuque.com/thyname/dart.core/list)（如通过 `[]` 字面量创建的列表），那么 `add` 和 `addAll` 操作的性能会非常差。这些操作的实现方式是每次 `add` 操作都将列表长度增加一，而反复增加可增长列表的长度效率并不高。为避免这种情况，请重写 `add` 和 `addAll`，使其也直接转发给该可增长列表；或者，如果可行，请使用 "package:collection/collection.dart" 中的 `DelegatingList`，而不是 `ListMixin`。

## 构造函数

### ListBase()

```dart
const ListBase<E>()
```

## 属性

- iterator
- isEmpty
- isNotEmpty
- first
- last
- single

## 方法

- elementAt()
- followedBy()
- forEach()
- contains()
- every()
- any()
- firstWhere()
- lastWhere()
- singleWhere()
- join()
- where()
- whereType()
- map()
- expand()
- reduce()
- fold()
- skip()
- skipWhile()
- take()
- takeWhile()
- toList()
- toSet()
- add()
- addAll()
- remove()
- removeWhere()
- retainWhere()
- clear()
- cast()
- removeLast()
- sort()
- shuffle()
- asMap()
- sublist()
- getRange()
- removeRange()
- fillRange()
- setRange()
- replaceRange()
- indexOf()
- indexWhere()
- lastIndexOf()
- lastIndexWhere()
- insert()
- removeAt()
- insertAll()
- setAll()
- reversed

### listToString()

```dart
String listToString(List<Object?> list)
```

将 [List](https://www.yuque.com/thyname/dart.core/list) 转换为 [String](https://www.yuque.com/thyname/dart.core/string)。

通过对 [list] 中的每个元素调用 [Object.toString] 转换为字符串，再用 ", " 连接这些字符串，并用 `[` 和 `]` 包裹结果，从而将 [list] 转换为字符串。

能够处理循环引用的情况，即某个元素转换为字符串时又反过来将 [list] 转换为字符串。

## 运算符

- operator +

---

# ListMixin

```dart
typedef ListMixin<E> = ListBase<E>
```

[List](https://www.yuque.com/thyname/dart.core/list) 类的基础混入实现。

`ListMixin` 可以用作混入，使一个类实现 `List` 接口。

该混入仅使用 `length` 和 `operator[]` 成员实现了所有读取操作。它使用这些成员以及 `add`、`length=` 和 `operator[]=` 实现了写入操作。使用该混入的类应实现这五个操作。

**注意**：出于向后兼容的原因，`add` 提供了一个默认实现，但该实现仅适用于元素类型可为空的列表。对于元素类型非空的列表，必须实现 `add` 方法。

**注意**：如果仅将 `length` 和 `[]` 这四个读/写操作转发给一个普通的可增长 [List](https://www.yuque.com/thyname/dart.core/list)（如通过 `[]` 字面量创建的列表），那么 `add` 和 `addAll` 操作的性能会非常差。这些操作的实现方式是每次 `add` 操作都将列表长度增加一，而反复增加可增长列表的长度效率并不高。为避免这种情况，请重写 `add` 和 `addAll`，使其也直接转发给该可增长列表；或者，如果可行，请使用 "package:collection/collection.dart" 中的 `DelegatingList`，而不是 `ListMixin`。
