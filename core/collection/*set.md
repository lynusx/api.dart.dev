# SetBase

```dart
abstract mixin class SetBase<E> implements Set<E> {}
```

[Set](https://www.yuque.com/thyname/dart.core/set) 的基础实现。

此类提供了一个 `Set` 的基础实现，该实现仅依赖于以下抽象成员：[add]、[contains]、[lookup]、[remove]、[iterator]、[length] 和 [toSet]。

部分方法假定 `toSet` 会创建一个可修改的集合。如果将此基类用于不可修改的集合（此时 `toSet` 应返回一个不可修改的集合），则需要重新实现 [retainAll]、[union]、[intersection] 和 [difference]。

使用此基类实现 `Set` 时，应考虑同时以常量时间实现 `clear`。默认实现的方式是逐个移除所有元素。

## 构造函数

### SetBase()

```dart
const SetBase<E>()
```

## 属性

- iterator
- length
- isEmpty
- isNotEmpty
- first
- last

## 方法

- add()
- contains()
- lookup()
- remove()
- toSet()
- cast()
- followedBy()
- whereType()
- clear()
- addAll()
- removeAll()
- retainAll()
- removeWhere()
- retainWhere()
- containsAll()
- union()
- intersection()
- difference()
- toList()
- map()
- single
- toString()
- where()
- expand()
- forEach()
- reduce()
- fold()
- every()
- join()
- any()
- take()
- takeWhile()
- skip()
- skipWhile()
- firstWhere()
- lastWhere()
- singleWhere()
- elementAt()

### setToString()

```dart
String setToString(Set set)
```

将 [Set](https://www.yuque.com/thyname/dart.core/set) 转换为 [String](https://www.yuque.com/thyname/dart.core/string)。

通过将 [set] 中的每个元素转换为字符串（调用 [Object.toString]），并用 ", " 连接这些字符串，再用 "{" 和 "}" 包裹结果，从而将 [set] 转换为字符串。

处理循环引用的情况，即某个元素转换为字符串的过程中又反过来需要将 [set] 转换为字符串。

---

# SetMixin

```dart
typedef SetMixin<E> = SetBase<E>
```

[Set](https://www.yuque.com/thyname/dart.core/set) 的混入实现。

此类提供了一个 `Set` 的基础实现，该实现仅依赖于以下抽象成员：[add]、[contains]、[lookup]、[remove]、[iterator]、[length] 和 [toSet]。

部分方法假定 `toSet` 会创建一个可修改的集合。如果将此混入用于不可修改的集合（此时 `toSet` 应返回一个不可修改的集合），则需要重新实现 [retainAll]、[union]、[intersection] 和 [difference]。

使用此混入实现 `Set` 时，应考虑同时以常量时间实现 `clear`。默认实现的方式是逐个移除所有元素。

---

# UnmodifiableSetView

```dart
class UnmodifiableSetView<E> extends SetBase<E> with _UnmodifiableSetMixin<E> {}
```

另一个 [Set](https://www.yuque.com/thyname/dart.core/set) 的不可修改视图。

不得调用可能改变集合的方法，例如 [add] 和 [remove]。

```dart
final baseSet = <String>{'Mars', 'Mercury', 'Earth', 'Venus'};
final unmodifiableSetView = UnmodifiableSetView(baseSet);

// Remove an element from the original set.
baseSet.remove('Venus');
print(unmodifiableSetView); // {Mars, Mercury, Earth}

unmodifiableSetView.remove('Earth'); // Throws.
```

## 构造函数

### UnmodifiableSetView()

```dart
UnmodifiableSetView<E>(Set<E> source)
```

创建 [source] 的一个 [UnmodifiableSetView](https://www.yuque.com/thyname/dart.collection/unmodifiablesetview)。

## 属性

- length
- iterator

## 方法

- contains()
- lookup()
- toSet()
