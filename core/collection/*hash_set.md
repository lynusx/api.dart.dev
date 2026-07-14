# HashSet

```dart
abstract final class HashSet<E> implements Set<E> {}
```

一种基于哈希表实现的无序 [Set](https://www.yuque.com/thyname/dart.core/set)。

`HashSet` 的元素必须具有一致的相等性和 hashCode 实现。这意味着相等性运算必须在元素上定义一个稳定的等价关系（自反、对称、传递，并且随时间保持一致），并且 hashCode 必须与相等性保持一致，使得被判定为相等的对象具有相同的 hashCode。

只要对象的哈希码分布良好，`HashSet` 上大多数简单操作都能在（可能是均摊的）常数时间内完成，包括 [add]、[contains]、[remove] 和 [length]。

**集合的迭代顺序未作规定，取决于所提供元素的哈希码。** 不过该顺序是稳定的：只要集合未被修改，多次遍历同一集合会得到相同的顺序。

**注意：** 在对集合执行操作期间（例如在 [forEach] 或 [containsAll] 调用期间被调用的函数中），或在遍历集合期间，不要修改该集合（添加或移除元素）。

当元素处于集合中时，不要以任何会改变其相等性（进而改变其哈希码）的方式修改元素。某些特殊类型的集合在相等性方面可能更为宽松，在这种情况下，应记录其不同的行为和限制。

示例：

```dart
final letters = HashSet<String>();
```

要向集合中添加数据，可使用 [add] 或 [addAll]。

```dart continued
letters.add('A');
letters.addAll({'B', 'C', 'D'});
```

要检查集合是否为空，可使用 [isEmpty] 或 [isNotEmpty]。要获取集合中元素的数量，可使用 [length]。

```dart continued
print(letters.isEmpty); // false
print(letters.length); // 4
print(letters); // fx {A, D, C, B}
```

要检查集合中是否存在具有特定值的元素，可使用 [contains]。

```dart continued
final bExists = letters.contains('B'); // true
```

[forEach] 方法会针对集合中的每个元素调用一次函数。

```dart continued
letters.forEach(print);
// A
// D
// C
// B
```

要复制集合，可使用 [toSet]。

```dart continued
final anotherSet = letters.toSet();
print(anotherSet); // fx {A, C, D, B}
```

要移除一个元素，可使用 [remove]。

```dart continued
final removedValue = letters.remove('A'); // true
print(letters); // fx {B, C, D}
```

要同时移除多个元素，可使用 [removeWhere] 或 [removeAll]。

```dart continued
letters.removeWhere((element) => element.startsWith('B'));
print(letters); // fx {D, C}
```

要移除集合中所有不满足条件的元素，可使用 [retainWhere]。

```dart continued
letters.retainWhere((element) => element.contains('C'));
print(letters); // {C}
```

要移除所有元素并清空集合，可使用 [clear]。

```dart continued
letters.clear();
print(letters.isEmpty); // true
print(letters); // {}
```

**另请参阅：**

- [Set](https://www.yuque.com/thyname/dart.core/set) 是集合的通用接口，其中每个对象只能出现一次。
- [LinkedHashSet](https://www.yuque.com/thyname/dart.collection/linkedhashset) 按插入顺序存储对象。
- [SplayTreeSet](https://www.yuque.com/thyname/dart.collection/splaytreeset) 按排序顺序迭代对象。

## 构造函数

### HashSet()

```dart
HashSet<E>({
  bool equals( E, E )?,
  int hashCode( E )?,
  bool isValidKey( dynamic )?,
})
```

使用提供的 [equals] 作为相等性判断依据来创建一个哈希集合。

提供的 [equals] 必须定义一个稳定的等价关系，并且 [hashCode] 必须与 [equals] 保持一致。

如果省略了 [equals] 或 [hashCode]，则集合使用元素自身固有的 [Object.==] 和 [Object.hashCode]。

如果提供了 [equals] 和 [hashCode] 中的一个，通常也应同时提供另一个。

某些 [equals] 或 [hashCode] 函数可能并不适用于所有对象。如果提供了 [isValidKey]，则用它来检查一个潜在的元素——该元素不一定是 [E] 的实例，例如 [contains] 的参数类型为 `Object?`。如果 [isValidKey] 对某个对象返回 `false`，则不会调用 [equals] 和 [hashCode] 函数，并且认为映射中不存在与该对象相等的键。[isValidKey] 函数默认只检测对象是否为 [E] 的实例，这意味着：

```dart template:expression
HashSet<int>(equals: (int e1, int e2) => (e1 - e2) % 5 == 0,
             hashCode: (int e) => e % 5)
```

不需要传入 `isValidKey` 参数，因为它默认只接受 `int` 值，而 `equals` 和 `hashCode` 均能接受这类值。

如果既未提供 `equals`、`hashCode`，也未提供 `isValidKey`，则默认的 `isValidKey` 会接受所有值。默认的相等性和哈希码运算被假定适用于所有对象。

同样，如果 `equals` 为 [identical](https://www.yuque.com/thyname/dart.core/identical)，`hashCode` 为 [identityHashCode](https://www.yuque.com/thyname/dart.core/identityhashcode)，并且省略了 `isValidKey`，则生成的集合基于对象标识（identity），且 `isValidKey` 默认接受所有键。可以直接使用 [HashSet.identity] 创建这样的集合。

### HashSet.identity()

```dart
HashSet<E>.identity()
```

创建一个基于标识（identity）的无序集合。

实际上等价于以下写法的简写：

```dart
HashSet<E>(equals: identical, hashCode: identityHashCode)
```

### HashSet.from()

```dart
HashSet<E>.from( Iterable elements )
```

创建一个包含所有 [elements] 的哈希集合。

按照 `HashSet<E>()` 的方式创建一个哈希集合，并将给定的所有 [elements] 添加进去。元素按顺序添加。如果 [elements] 中存在两个相等但不相同（not identical）的条目，则结果集合中保留的是第一个。

所有 [elements] 都应是 [E] 的实例。`elements` 这个可迭代对象本身可以具有任意元素类型，因此该构造函数可用于对 `Set` 进行向下转型，例如：

```dart
Set<SuperType> superSet = ...;
Set<SubType> subSet =
    HashSet<SubType>.from(superSet.whereType<SubType>());
```

示例：

```dart
final numbers = <num>[10, 20, 30];
final hashSetFrom = HashSet<int>.from(numbers);
print(hashSetFrom); // fx {20, 10, 30}
```

### HashSet.of()

```dart
HashSet<E>.of( Iterable<E> elements )
```

创建一个包含所有 [elements] 的哈希集合。

按照 `HashSet<E>()` 的方式创建一个哈希集合，并将给定的所有 [elements] 添加进去。元素按顺序添加。如果 [elements] 中存在两个相等但不相同（not identical）的条目，则结果集合中保留的是第一个。示例：

```dart
final baseSet = <int>{1, 2, 3};
final hashSetOf = HashSet<num>.of(baseSet);
print(hashSetOf); // fx {3, 1, 2}
```

## 属性

### iterator

```dart
Iterator<E> get iterator
```

提供一个用于遍历此集合中元素的迭代器。

迭代顺序未作规定，但在集合未发生变化期间保持一致。
