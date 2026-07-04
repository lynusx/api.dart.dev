# Set

```dart
abstract interface class Set<E> implements Iterable<E>, _SetIterable<E> {}
```

一个对象集合，其中每个对象最多只能出现一次。

也就是说，对于元素类型的每个对象，该对象要么被视为在集合中，要么被视为 _不_ 在集合中。

Set 的实现可能会将某些元素视为不可区分。在对集合执行任何操作时，这些元素都被视为相同。

[Set] 的默认实现 [LinkedHashSet] 会将根据 [Object.==] 和 [Object.hashCode] 相等的对象视为不可区分。

遍历集合元素的顺序可能是无序的，也可能以某种方式排序。示例：

- [HashSet] 是无序的，这意味着其迭代顺序是未指定的，
- [LinkedHashSet] 按元素的插入顺序迭代，
- 而像 [SplayTreeSet] 这样的有序集合按元素的排序顺序迭代。

通常不允许在对集合执行操作期间修改集合（添加或删除元素），例如在调用 [forEach] 或 [containsAll] 期间。也不允许在迭代集合本身或由该集合支持的任何 [Iterable]（如 [where] 和 [map] 等方法返回的对象）时修改集合。

通常不允许在元素位于集合中时修改元素的相等性（因此也不能修改其哈希码）。某些特殊子类型可能更宽松，此时应记录该行为。

## 构造函数

### Set()

```dart
Set<E>()
```

创建一个空的 [Set]。

创建的 [Set] 是一个普通的 [LinkedHashSet]。因此，它会将使用 [operator ==] 判断相等的元素视为不可区分，并要求它们具有兼容的 [Object.hashCode] 实现。

此集合等价于通过 `LinkedHashSet<E>()` 创建的集合。

### Set.identity()

```dart
Set<E>.identity()
```

创建一个空的恒等 [Set]。

创建的 [Set] 是一个使用恒等作为相等关系的 [LinkedHashSet]。

此集合等价于通过 `LinkedHashSet<E>.identity()` 创建的集合。

### Set.from()

```dart
Set<E>.from( Iterable elements )
```

创建一个包含所有 [elements] 的 [Set]。

所有 [elements] 都应该是 [E] 的实例。`elements` 这一可迭代对象本身可以是任意类型，因此此构造函数可用于对 `Set` 进行向下转型，例如：

```dart
Set<SuperType> superSet = ...;
Set<SubType> subSet =
    Set<SubType>.from(superSet.where((e) => e is SubType));
```

创建的 [Set] 是一个 [LinkedHashSet]。因此，它会将使用 [operator ==] 判断相等的元素视为不可区分，并要求它们具有兼容的 [Object.hashCode] 实现。

此集合等价于通过 `LinkedHashSet<E>.from(elements)` 创建的集合。

```dart
final numbers = <num>{10, 20, 30};
final setFrom = Set<int>.from(numbers);
print(setFrom); // {10, 20, 30}
```

### Set.of()

```dart
Set<E>.of( Iterable<E> elements )
```

根据 [elements] 创建一个 [Set]。

创建的 [Set] 是一个 [LinkedHashSet]。因此，它会将使用 [operator ==] 判断相等的元素视为不可区分，并要求它们具有兼容的 [Object.hashCode] 实现。

此集合等价于通过 `LinkedHashSet<E>.of(elements)` 创建的集合。

```dart
final baseSet = <int>{1, 2, 3};
final setOf = Set<num>.of(baseSet);
print(setOf); // {1, 2, 3}
```

### Set.unmodifiable()

```dart
Set<E>.unmodifiable( Iterable<E> elements )
```

根据 [elements] 创建一个不可修改的 [Set]。

新集合的行为与 [Set.of] 的结果相同，只是此构造函数返回的集合不可修改。

```dart
final characters = <String>{'A', 'B', 'C'};
final unmodifiableSet = Set.unmodifiable(characters);
```

## 静态方法

### castFrom()

```dart
Set<T> castFrom<S, T>(
  Set<S> source, {
  Set<R> newSet<R>()?,
})
```

将 [source] 适配为 `Set<T>`。

如果提供了 [newSet]，它将用于创建 [toSet]、[union] 返回的新集合，也用于 [intersection] 和 [difference]。如果省略 [newSet]，则默认使用 [Set] 的默认构造函数创建新集合，此时 [intersection] 和 [difference] 会返回对源集合调用相同方法所得结果的适配版本。

每当该集合会生成一个不是 [T] 的元素时，元素访问都会抛出异常。

每当试图向适配后的集合中添加一个 [T] 类型的值时，存储操作都会抛出异常，除非该值同时也是 [S] 的实例。

如果 [source] 中所有被访问的元素实际上都是 [T] 的实例，并且添加到返回的集合中的所有元素实际上都是 [S] 的实例，那么返回的集合就可以作为 `Set<T>` 使用。

接受一个或多个 `Object?` 作为参数的方法，如 [contains]、[remove] 和 [removeAll]，会将参数直接传递给该集合的相应方法，而不进行任何检查。

## 属性

### iterator

```dart
Iterator<E> get iterator
```

用于遍历此集合元素的迭代器。

迭代顺序由具体的 `Set` 实现决定，但在集合发生变化之间必须保持一致。

## 方法

### cast()

```dart
Set<R> cast<R>()
```

提供此集合作为 [R] 实例集合的视图。

如果此集合只包含 [R] 的实例，所有读取操作都能正常工作。如果某个操作试图访问一个不是 [R] 实例的元素，该访问将抛出异常。

添加到集合中的元素（例如通过 [add] 或 [addAll]）必须是 [R] 的实例才能作为添加函数的有效参数，同时也必须是 [E] 的实例才能被此集合接受。

接受一个或多个 `Object?` 作为参数的方法，如 [contains]、[remove] 和 [removeAll]，会将参数直接传递给该集合的相应方法，而不进行任何检查。这意味着即使看起来不应该有任何效果，`setOfStrings.cast<int>().remove("a")` 也能成功执行。

### contains()

```dart
bool contains(Object? value)
```

[value] 是否在集合中。

```dart
final characters = <String>{'A', 'B', 'C'};
final containsB = characters.contains('B'); // true
final containsD = characters.contains('D'); // false
```

### add()

```dart
bool add(E value)
```

将 [value] 添加到集合中。

如果 [value]（或与其相等的值）尚未在集合中，则返回 `true`。否则返回 `false`，且集合不发生变化。

示例：

```dart
final dateTimes = <DateTime>{};
final time1 = DateTime.fromMillisecondsSinceEpoch(0);
final time2 = DateTime.fromMillisecondsSinceEpoch(0);
// time1 and time2 are equal, but not identical.
assert(time1 == time2);
assert(!identical(time1, time2));
final time1Added = dateTimes.add(time1);
print(time1Added); // true
// A value equal to time2 exists already in the set, and the call to
// add doesn't change the set.
final time2Added = dateTimes.add(time2);
print(time2Added); // false

print(dateTimes); // {1970-01-01 02:00:00.000}
assert(dateTimes.length == 1);
assert(identical(time1, dateTimes.first));
print(dateTimes.length);
```

### addAll()

```dart
void addAll(Iterable<E> elements)
```

将所有 [elements] 添加到此集合中。

等价于使用 [add] 逐个添加 [elements] 中的每个元素，但某些集合实现可能会对此进行优化。

```dart
final characters = <String>{'A', 'B'};
characters.addAll({'A', 'B', 'C'});
print(characters); // {A, B, C}
```

### remove()

```dart
bool remove(Object? value)
```

从集合中删除 [value]。

如果 [value] 在集合中，则返回 `true`，否则返回 `false`。如果 [value] 不在集合中，此方法不产生任何效果。

```dart
final characters = <String>{'A', 'B', 'C'};
final didRemoveB = characters.remove('B'); // true
final didRemoveD = characters.remove('D'); // false
print(characters); // {A, C}
```

### lookup()

```dart
E? lookup(Object? object)
```

如果集合中存在与 [object] 相等的对象，则返回该对象。

像 [contains] 一样检查 [object] 是否在集合中，如果存在，则返回集合中的该对象，否则返回 `null`。

如果集合使用的相等关系不是恒等关系，那么返回的对象可能与 [object] 不 _完全相同_。某些集合实现可能无法实现此方法。如果 [contains] 方法是通过计算得出的，而不是基于实际的对象实例，那么可能不存在代表该集合元素的特定对象实例。

```dart
final characters = <String>{'A', 'B', 'C'};
final containsB = characters.lookup('B');
print(containsB); // B
final containsD = characters.lookup('D');
print(containsD); // null
```

### removeAll()

```dart
void removeAll(Iterable<Object?> elements)
```

从此集合中删除 [elements] 中的每个元素。

```dart
final characters = <String>{'A', 'B', 'C'};
characters.removeAll({'A', 'B', 'X'});
print(characters); // {C}
```

### retainAll()

```dart
void retainAll(Iterable<Object?> elements)
```

删除此集合中所有不属于 [elements] 中元素的元素。

对 [elements] 中的每个元素检查此集合中是否存在与之相等的元素（根据 `this.contains`），如果存在，则保留集合中相等的元素；不与 [elements] 中任何元素相等的元素将被删除。

```dart
final characters = <String>{'A', 'B', 'C'};
characters.retainAll({'A', 'B', 'X'});
print(characters); // {A, B}
```

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

删除此集合中所有满足 [test] 的元素。

```dart
final characters = <String>{'A', 'B', 'C'};
characters.removeWhere((element) => element.startsWith('B'));
print(characters); // {A, C}
```

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

删除此集合中所有不满足 [test] 的元素。

```dart
final characters = <String>{'A', 'B', 'C'};
characters.retainWhere(
    (element) => element.startsWith('B') || element.startsWith('C'));
print(characters); // {B, C}
```

### containsAll()

```dart
bool containsAll(Iterable<Object?> other)
```

此集合是否包含 [other] 中的所有元素。

```dart
final characters = <String>{'A', 'B', 'C'};
final containsAB = characters.containsAll({'A', 'B'});
print(containsAB); // true
final containsAD = characters.containsAll({'A', 'D'});
print(containsAD); // false
```

### intersection()

```dart
Set<E> intersection(Set<Object?> other)
```

创建一个新集合，该集合是此集合与 [other] 的交集。

也就是说，返回的集合包含此 [Set] 中根据 `other.contains` 判断也属于 [other] 的所有元素。

```dart
final characters1 = <String>{'A', 'B', 'C'};
final characters2 = <String>{'A', 'E', 'F'};
final intersectionSet = characters1.intersection(characters2);
print(intersectionSet); // {A}
```

### union()

```dart
Set<E> union(Set<E> other)
```

创建一个新集合，包含此集合与 [other] 的所有元素。

也就是说，返回的集合包含此 [Set] 的所有元素以及 [other] 的所有元素。

```dart
final characters1 = <String>{'A', 'B', 'C'};
final characters2 = <String>{'A', 'E', 'F'};
final unionSet1 = characters1.union(characters2);
print(unionSet1); // {A, B, C, E, F}
final unionSet2 = characters2.union(characters1);
print(unionSet2); // {A, E, F, B, C}
```

### difference()

```dart
Set<E> difference(Set<Object?> other)
```

创建一个新集合，包含此集合中不属于 [other] 的元素。

也就是说，返回的集合包含此 [Set] 中根据 `other.contains` 判断不属于 [other] 的所有元素。

```dart
final characters1 = <String>{'A', 'B', 'C'};
final characters2 = <String>{'A', 'E', 'F'};
final differenceSet1 = characters1.difference(characters2);
print(differenceSet1); // {B, C}
final differenceSet2 = characters2.difference(characters1);
print(differenceSet2); // {E, F}
```

### clear()

```dart
void clear()
```

删除集合中的所有元素。

```dart
final characters = <String>{'A', 'B', 'C'};
characters.clear(); // {}
```

### toSet()

```dart
Set<E> toSet()
```

创建一个与此 `Set` 具有相同元素和行为的 [Set]。

返回的集合在添加和删除元素方面的行为与此集合相同。它最初包含相同的元素。如果此集合指定了元素的顺序，返回的集合也将具有相同的顺序。
