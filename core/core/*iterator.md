# Iterator

```dart
abstract interface class Iterator<E> {}
```

用于从一个对象中逐个获取项的接口。

for-in 结构在底层透明地使用 `Iterator` 来判断迭代是否结束，并获取每一项（即*元素*）。

如果在迭代过程中修改了被遍历的对象，其行为是未指定的。

`Iterator` 初始时位于第一个元素之前。在访问第一个元素之前，必须先使用 [moveNext] 将迭代器向前移动，使其指向第一个元素。如果没有剩余元素，则 [moveNext] 返回 false，此后对 [moveNext] 的所有调用也都将返回 false。

不得在调用 [moveNext] 之前，或者在某次调用 [moveNext] 返回 false 之后访问 [current] 的值。

`Iterator` 的典型用法如下：

```dart
var it = obj.iterator;
while (it.moveNext()) {
  use(it.current);
}
```

**另请参阅：** [`dart:core` 简介](https://dart.dev/libraries/dart-core)中的[迭代](https://dart.dev/libraries/dart-core#iteration)。

## 属性

### current

```dart
E get current
```

当前元素。

如果迭代器尚未移动到第一个元素（即尚未调用过 [moveNext]），或者迭代器已经移动到 [Iterable] 的最后一个元素之后（即 [moveNext] 已返回 false），则 [current] 的值是未指定的。此时，[Iterator] 既可以抛出异常，也可以返回一个特定于该迭代器的默认值。

`current` 获取器应当保持其值，直到下一次调用 [moveNext] 为止，即使底层集合发生了变化也是如此。在成功调用 `moveNext` 之后，用户不需要缓存当前值，而是可以持续从迭代器中读取该值。

```dart
final colors = ['blue', 'yellow', 'red'];
var colorsIterator = colors.iterator;
while (colorsIterator.moveNext()) {
  print(colorsIterator.current);
}
```

该示例的输出为：

```
blue
yellow
red
```

## 方法

### moveNext()

```dart
bool moveNext()
```

将迭代器向前移动到迭代过程中的下一个元素。

应当在读取 [current] 之前调用此方法。如果调用 `moveNext` 返回 `true`，那么在再次调用 `moveNext` 之前，[current] 将包含迭代过程中的下一个元素。如果调用返回 `false`，则表示没有更多元素，此时不应再使用 [current]。

在 [moveNext] 已经返回 `false` 之后再次调用它是安全的，但此后必须继续返回 `false`，且不能产生任何其他效果。

调用 `moveNext` 可能因多种原因而抛出异常，包括对底层集合的并发修改。如果发生这种情况，迭代器可能处于不一致的状态，此后迭代器的任何行为都是未指定的，包括读取 [current] 的结果。

```dart
final colors = ['blue', 'yellow', 'red'];
final colorsIterator = colors.iterator;
print(colorsIterator.moveNext()); // true
print(colorsIterator.moveNext()); // true
print(colorsIterator.moveNext()); // true
print(colorsIterator.moveNext()); // false
```
