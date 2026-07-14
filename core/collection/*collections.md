# UnmodifiableListView

```dart
class UnmodifiableListView<E> extends UnmodifiableListBase<E> {}
```

另一个 List 的不可修改 [List](https://www.yuque.com/thyname/dart.core/list) 视图。

元素的来源可以是 [List](https://www.yuque.com/thyname/dart.core/list)，也可以是任何具有高效 [Iterable.length] 和 [Iterable.elementAt] 实现的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)。

```dart
final numbers = <int>[10, 20, 30];
final unmodifiableListView = UnmodifiableListView(numbers);

// 向原始列表中插入新元素。
numbers.addAll([40, 50]);
print(unmodifiableListView); // [10, 20, 30, 40, 50]

unmodifiableListView.remove(20); // 抛出异常。
```

## 构造函数

### UnmodifiableListView()

```dart
UnmodifiableListView<E>(Iterable<E> source)
```

创建一个由 [source] 支持的不可修改列表。

元素的 [source] 可以是 [List](https://www.yuque.com/thyname/dart.core/list)，也可以是任何具有高效 [Iterable.length] 和 [Iterable.elementAt] 实现的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)。

## 属性

- length

## 方法

- cast()

## 运算符

- operator []
