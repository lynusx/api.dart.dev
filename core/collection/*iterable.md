# IterableMixin

```dart
typedef IterableMixin<E> = Iterable<E>
```

此 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 混入实现了除 `iterator` 之外的所有 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 成员。

其他所有方法均基于 `iterator` 实现。

---

# IterableBase

```dart
typedef IterableBase<E> = Iterable<E>
```

用于实现 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 的基类。

此类基于 `iterator` 实现了 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 的所有方法，[Iterable.iterator] 除外。

---

# NullableIterableExtensions

```dart
extension NullableIterableExtensions<T extends Object> on Iterable<T?> {}
```

针对元素可为空的可迭代对象的操作。

## 属性

### nonNulls

```dart
Iterable<T> get nonNulls
```

此可迭代对象中的非 `null` 元素。

与此可迭代对象中的元素相同，只是省略了 `null` 值。

---

# IterableExtensions

```dart
extension IterableExtensions<T> on Iterable<T> {}
```

针对可迭代对象的操作。

## 属性

### indexed

```dart
Iterable<(int, T)> get indexed
```

由索引和此可迭代对象中的元素组成的元素对。

这些元素按索引/迭代顺序，依次为 `(0, this.first)` 直到 `(this.length - 1, this.last)`。

### firstOrNull

```dart
T? get firstOrNull
```

此迭代器的第一个元素；如果该可迭代对象为空，则为 `null`。

### lastOrNull

```dart
T? get lastOrNull
```

此可迭代对象的最后一个元素；如果该可迭代对象为空，则为 `null`。

此计算可能效率不高。最后一个值可能需要通过遍历整个可迭代对象并临时存储每个值才能找到。该过程只会遍历一次可迭代对象。如果多次迭代不成问题，对某些可迭代对象而言，采用以下方式可能效率更高：

```dart
var lastOrNull = iterable.isEmpty ? null : iterable.last;
```

### singleOrNull

```dart
T? get singleOrNull
```

此迭代器中唯一的元素；否则为 `null`。

如果该迭代器恰好只有一个元素，则该值即为这个元素。否则，如果迭代器没有元素，或者有两个及以上元素，则该值为 `null`。

## 方法

### elementAtOrNull()

```dart
T? elementAtOrNull(int index)
```

此可迭代对象中位于位置 [index] 处的元素；否则为 `null`。

[index] 从零开始计数，且必须为非负数。

如果该可迭代对象至少有 `index + 1` 个元素，则返回 `elementAt(index)` 的结果；否则返回 `null`。
