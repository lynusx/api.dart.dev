# Iterable

```dart
abstract mixin class Iterable<E> {}
```

一个可以顺序访问其值（或称"元素"）的集合。

可迭代对象的元素通过使用 [iterator] 获取一个 [Iterator](https://www.yuque.com/thyname/dart.core/iterator) 来访问，并使用它逐步遍历这些值。使用迭代器进行遍历是通过调用 [Iterator.moveNext] 完成的，如果调用返回 `true`，则表示迭代器已经移动到下一个元素，该元素可通过 [Iterator.current] 获取。如果调用返回 `false`，则表示没有更多元素。[Iterator.current] 的值只能在最近一次调用 [Iterator.moveNext] 返回 `true` 之后使用。如果在迭代器上第一次调用 [Iterator.moveNext] 之前读取 [Iterator.current]，或者在某次调用返回 false 或抛出错误之后读取，读取 [Iterator.current] 可能会抛出异常，也可能返回一个任意值。

可以从同一个 `Iterable` 创建多个迭代器。每次读取 `iterator` 时，都会返回一个新的迭代器，不同的迭代器可以独立地进行遍历，每个迭代器都能访问可迭代对象的所有元素。同一个可迭代对象的迭代器*应当*以相同的顺序提供相同的值（除非在两次迭代之间修改了底层集合，某些集合允许这样做）。

也可以使用 for-in 循环结构来遍历 `Iterable` 的元素，该结构在底层使用了 `iterator` 获取器。例如，可以遍历 [Map](https://www.yuque.com/thyname/dart.core/map) 的所有键，因为 `Map` 的键是可迭代的。

```dart
var kidsBooks = {'Matilda': 'Roald Dahl',
                 'Green Eggs and Ham': 'Dr Seuss',
                 'Where the Wild Things Are': 'Maurice Sendak'};
for (var book in kidsBooks.keys) {
  print('$book was written by ${kidsBooks[book]}');
}
```

[List](https://www.yuque.com/thyname/dart.core/list) 和 [Set](https://www.yuque.com/thyname/dart.core/set) 类都是 `Iterable`，`dart:collection` 库中的大多数类也是如此。

某些 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 集合是可以修改的。向 `List` 或 `Set` 添加元素会改变其包含的元素，向 `Map` 添加新键会改变 [Map.keys] 的元素。在修改之后创建的迭代器将提供新的元素，并且可能保留也可能不保留现有元素的顺序（例如，向 [HashSet](https://www.yuque.com/thyname/dart.collection/hashset) 添加一个元素后，其顺序可能会完全改变）。

在遍历集合*的过程中*修改集合，通常是*不允许*的。这样做会破坏迭代过程，通常的表现是在下一次调用 [Iterator.moveNext] 时抛出 [ConcurrentModificationError](https://www.yuque.com/thyname/dart.core/concurrentmodificationerror)。[Iterator.current] 获取器的当前值不应受到集合变化的影响，因为 `current` 的值是由上一次调用 [Iterator.moveNext] 设置的。

一些可迭代对象在每次被遍历时都会动态计算其元素，例如 [Iterable.generate] 返回的可迭代对象，或者由 `sync*` 生成器函数返回的可迭代对象。如果计算过程不依赖于其他可能发生变化的对象，那么每次遍历生成的序列应当是相同的。

`Iterable` 的成员（除 `iterator` 本身之外）都是通过查看可迭代对象的元素来工作的。这可以通过遍历 [iterator] 来实现，但某些类可能有更高效的方式来找到结果（例如 [List](https://www.yuque.com/thyname/dart.core/list) 上的 [last] 或 [length]，或者 [Set](https://www.yuque.com/thyname/dart.core/set) 上的 [contains]）。

返回另一个 `Iterable` 的方法（如 [map] 和 [where]）都是*惰性*的——它们只有在返回的可迭代对象本身被遍历时（且仅在必要时）才会遍历原始对象，而不会提前进行。

由于一个可迭代对象可能被遍历多次，不建议在迭代器中包含可检测的副作用。对于 [map] 和 [where] 等方法，返回的可迭代对象会在每次遍历时执行传入的函数，因此这些函数也不应带有副作用。

`Iterable` 声明提供了一个默认实现，可以被继承或混入以实现 `Iterable` 接口。它使用 [iterator] 提供的 [Iterator](https://www.yuque.com/thyname/dart.core/iterator) 实现了除 [iterator] 获取器之外的所有成员。`Iterable` 接口的实现应当在力所能及的情况下为 `Iterable` 的成员提供更高效的实现。

## 构造函数

### Iterable()

```dart
const Iterable<E>()
```

### Iterable.generate()

```dart
Iterable<E>.generate( int count, [ E generator( int index )? ])
```

创建一个动态生成其元素的 `Iterable`。

生成的可迭代对象包含 [count] 个元素，索引为 `n` 处的元素通过调用 `generator(n)` 计算得到。这些值不会被缓存，因此每次迭代都会重新计算这些值。

如果省略 [generator]，则默认使用整数上的恒等函数 `(int x) => x`，因此只有当 [E] 是 `int` 的超类型时才可以省略该参数。

作为一个 `Iterable`，`Iterable.generate(n, generator))` 等价于 `const [0, ..., n - 1].map(generator)`。

### Iterable.withIterator()

```dart
@Since.new("3.8")
Iterable<E>.withIterator( Iterator<E> iteratorFactory() )
```

通过 [Iterator](https://www.yuque.com/thyname/dart.core/iterator) 工厂函数创建一个 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)。

返回的可迭代对象每次读取 [iterator] 时都会调用提供的 [iteratorFactory] 函数来创建一个新的迭代器。[iteratorFactory] 函数每次调用都必须返回一个新的 `Iterator<E>` 实例。

该工厂方法适用于需要通过自定义迭代器创建可迭代对象，或者希望在每次使用时确保获得全新迭代状态的场景。

示例：

```dart
final numbers = Iterable.withIterator(() => [1, 2, 3].iterator);
print(numbers.toList()); // [1, 2, 3]
```

### Iterable.empty()

```dart
const Iterable<E>.empty()
```

创建一个空的可迭代对象。

该空的可迭代对象不包含任何元素，对其进行遍历会立即停止。

## 静态方法

### castFrom()

```dart
Iterable<T> castFrom<S, T>( Iterable<S> source )
```

将 [source] 适配为 `Iterable<T>`。

每当该可迭代对象要产生一个不是 [T] 类型的元素时，访问该元素就会抛出异常。如果 [source] 的所有元素实际上都是 [T] 的实例，或者只访问那些实际上是 [T] 实例的元素，那么生成的可迭代对象就可以作为 `Iterable<T>` 使用。

### iterableToShortString()

```dart
String iterableToShortString( Iterable iterable, [ String leftDelimiter = '(', String rightDelimiter = ')' ])
```

将一个 `Iterable` 转换为类似 [Iterable.toString] 的字符串。

允许使用与 '(' 和 ')' 不同的分隔符。

能够处理循环引用的情况，即某个元素转换为字符串时又会导致 [iterable] 本身被再次转换为字符串。

### iterableToFullString()

```dart
String iterableToFullString( Iterable iterable, [ String leftDelimiter = '(', String rightDelimiter = ')' ])
```

将一个 `Iterable` 转换为字符串。

将每个元素转换为字符串，并用 ", " 分隔这些结果，然后将结果包装在 [leftDelimiter] 和 [rightDelimiter] 之间。

与 [iterableToShortString] 不同，此转换不会省略任何元素，也不会对结果的大小设置任何限制。

能够处理循环引用的情况，即某个元素转换为字符串时又会导致 [iterable] 本身被再次转换为字符串。

## 属性

### iterator

```dart
Iterator<E> get iterator
```

一个新的 `Iterator`，用于遍历此 `Iterable` 的元素。

`Iterable` 类可以指定其元素的迭代顺序（例如 [List](https://www.yuque.com/thyname/dart.core/list) 总是按索引顺序迭代），也可以不作规定（例如基于哈希的 [Set](https://www.yuque.com/thyname/dart.core/set) 可能以任意顺序迭代）。

每次读取 `iterator` 时，都会返回一个新的迭代器，可用于再次遍历所有元素。同一个可迭代对象的多个迭代器可以独立地进行遍历，但只要底层集合没有发生变化，它们应当以相同的顺序返回相同的元素。

修改集合可能导致新创建的迭代器产生不同的元素，也可能改变现有元素的顺序。[List](https://www.yuque.com/thyname/dart.core/list) 精确地规定了其迭代顺序，因此修改列表会以可预测的方式改变迭代顺序。基于哈希的 [Set](https://www.yuque.com/thyname/dart.core/set) 在添加一个新元素后，其迭代顺序可能会完全改变。

在创建新迭代器之后修改底层集合，可能会导致在该迭代器下一次调用 [Iterator.moveNext] 时抛出错误。任何*可修改*的可迭代对象类都应当说明哪些操作会破坏迭代。

### length

```dart
int get length
```

此 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 中的元素个数。

统计所有元素可能需要遍历全部元素，因此可能较慢。一些可迭代对象有更高效的方式来获取元素个数，这些类*必须*重写 `length` 的默认实现。

### isEmpty

```dart
bool get isEmpty
```

此集合是否不包含任何元素。

可以通过检查 `iterator.moveNext()` 是否返回 `false` 来计算。

示例：

```dart
final emptyList = <int>[];
print(emptyList.isEmpty); // true;
print(emptyList.iterator.moveNext()); // false
```

### isNotEmpty

```dart
bool get isNotEmpty
```

此集合是否至少包含一个元素。

可以通过检查 `iterator.moveNext()` 是否返回 `true` 来计算。

示例：

```dart
final numbers = <int>{1, 2, 3};
print(numbers.isNotEmpty); // true;
print(numbers.iterator.moveNext()); // true
```

### first

```dart
E get first
```

第一个元素。

如果 `this` 为空，则抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。否则返回迭代顺序中的第一个元素，等价于 `this.elementAt(0)`。

### last

```dart
E get last
```

最后一个元素。

如果 `this` 为空，则抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。否则可能会遍历所有元素，并返回最后看到的那一个。一些可迭代对象可能有更高效的方式来找到最后一个元素（例如列表可以直接访问最后一个元素，而无需遍历前面的元素）。

### single

```dart
E get single
```

检查此可迭代对象是否只有一个元素，并返回该元素。

如果 `this` 为空或包含多个元素，则抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。此操作不会遍历超过第二个元素。

## 方法

### cast()

```dart
Iterable<R> cast<R>()
```

将此可迭代对象视为 [R] 实例的可迭代对象的视图。

如果此可迭代对象仅包含 [R] 的实例，那么所有操作都能正常工作。如果某个操作尝试访问一个不是 [R] 实例的元素，该访问将抛出异常。

当返回的可迭代对象创建一个依赖于类型 [R] 的新对象时（例如通过 [toList]），该对象将恰好具有类型 [R]。

### followedBy()

```dart
Iterable<E> followedBy(Iterable<E> other)
```

创建此可迭代对象与 [other] 的惰性串联。

返回的可迭代对象将提供与此可迭代对象相同的元素，之后再提供 [other] 的元素，顺序与原始可迭代对象中的顺序相同。

示例：

```dart
var planets = <String>['Earth', 'Jupiter'];
var updated = planets.followedBy(['Mars', 'Venus']);
print(updated); // (Earth, Jupiter, Mars, Venus)
```

### map()

```dart
Iterable<T> map<T>(T toElement(E e))
```

通过 [toElement] 修改后的此可迭代对象的当前元素。

返回一个新的惰性 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)，其元素是通过按迭代顺序对此 `Iterable` 的每个元素调用 `toElement` 创建的。

返回的可迭代对象是惰性的，因此在它本身被遍历之前，不会遍历此可迭代对象的元素；遍历时会逐个应用 [toElement] 来创建元素。转换后的元素不会被缓存。对返回的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 进行多次遍历，将针对每次遍历对每个元素调用一次所提供的 [toElement] 函数。

返回的可迭代对象上的方法可以在不需要结果的元素上省略调用 `toElement`。例如，[elementAt] 可能只调用一次 `toElement`。

等价于：

```dart
Iterable<T> map<T>(T toElement(E e)) sync* {
  for (var value in this) {
    yield toElement(value);
  }
}
```

示例：

```dart import:convert
var products = jsonDecode('''
[
  {"name": "Screwdriver", "price": 42.00},
  {"name": "Wingnut", "price": 0.50}
]
''');
var values = products.map((product) => product['price'] as double);
var totalPrice = values.fold(0.0, (a, b) => a + b); // 42.5.
```

### where()

```dart
Iterable<E> where(bool test(E element))
```

创建一个新的惰性 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)，包含所有满足谓词 [test] 的元素。

匹配的元素在返回的可迭代对象中的顺序与它们在 [iterator] 中的顺序相同。

此方法返回被映射元素的一个视图。只要返回的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 没有被遍历，所提供的函数 [test] 就不会被调用。遍历不会缓存结果，因此对返回的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 进行多次遍历，可能会对同一个元素多次调用所提供的函数 [test]。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.where((x) => x < 5); // (1, 2, 3)
result = numbers.where((x) => x > 5); // (6, 7)
result = numbers.where((x) => x.isEven); // (2, 6)
```

### whereType()

```dart
Iterable<T> whereType<T>()
```

创建一个新的惰性 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)，包含所有类型为 [T] 的元素。

匹配的元素在返回的可迭代对象中的顺序与它们在 [iterator] 中的顺序相同。

此方法返回被映射元素的一个视图。遍历不会缓存结果，因此如果底层元素在两次遍历之间发生变化，多次遍历返回的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 可能会产生不同的结果。

### expand()

```dart
Iterable<T> expand<T>(Iterable<T> toElements(E element))
```

将此 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 的每个元素展开为零个或多个元素。

生成的 Iterable 依次遍历对此对象的每个元素调用 [toElements] 所返回的元素，顺序为迭代顺序。

返回的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 是惰性的，每次遍历返回的可迭代对象时，都会对此可迭代对象的每个元素调用 [toElements]。

示例：

```dart
Iterable<int> count(int n) sync* {
  for (var i = 1; i <= n; i++) {
    yield i;
   }
 }

var numbers = [1, 3, 0, 2];
print(numbers.expand(count)); // (1, 1, 2, 3, 1, 2)
```

等价于：

```dart
Iterable<T> expand<T>(Iterable<T> toElements(E e)) sync* {
  for (var value in this) {
    yield* toElements(value);
  }
}
```

### contains()

```dart
bool contains(Object? element)
```

此集合是否包含一个与 [element] 相等的元素。

此操作会按顺序检查每个元素是否与 [element] 相等，除非存在更高效的方式来查找与 [element] 相等的元素。一旦找到第一个相等的元素，就会停止遍历。

用于判断 [element] 是否与可迭代对象中的某个元素相等的相等性判断，默认使用元素的 [Object.==]。

某些类型的可迭代对象可能对其元素使用不同的相等性判断。例如，[Set](https://www.yuque.com/thyname/dart.core/set) 可能使用自定义的相等性判断（参见 [Set.identity]）作为其 `contains` 使用的判断依据。同样，[Map.keys] 返回的 `Iterable` 的 `contains` 方法也应使用该 `Map` 用于键的相等性判断。

示例：

```dart
final gasPlanets = <int, String>{1: 'Jupiter', 2: 'Saturn'};
final containsOne = gasPlanets.keys.contains(1); // true
final containsFive = gasPlanets.keys.contains(5); // false
final containsJupiter = gasPlanets.values.contains('Jupiter'); // true
final containsMercury = gasPlanets.values.contains('Mercury'); // false
```

### forEach()

```dart
void forEach(void action(E element))
```

按迭代顺序对此可迭代对象的每个元素调用 [action]。

示例：

```dart
final numbers = <int>[1, 2, 6, 7];
numbers.forEach(print);
// 1
// 2
// 6
// 7
```

### reduce()

```dart
E reduce(E combine(E value, E element))
```

通过使用提供的函数迭代地组合集合中的元素，将集合归约为单个值。

该可迭代对象必须至少包含一个元素。如果只有一个元素，则返回该元素。

否则，此方法从迭代器的第一个元素开始，然后按迭代顺序将其与其余元素依次组合，等价于：

```dart
E value = iterable.first;
iterable.skip(1).forEach((element) {
  value = combine(value, element);
});
return value;
```

计算可迭代对象总和的示例：

```dart
final numbers = <double>[10, 2, 5, 0.5];
final result = numbers.reduce((value, element) => value + element);
print(result); // 17.5
```

如果可迭代对象可能为空，请考虑使用 [fold]。

### fold()

```dart
T fold<T>(T initialValue, T combine(T previousValue, E element))
```

通过将集合中的每个元素与一个已有的值进行迭代组合，将集合归约为单个值。

以 [initialValue] 作为初始值，然后遍历元素，并使用 [combine] 函数根据每个元素更新该值，等价于：

```dart
var value = initialValue;
for (E element in this) {
  value = combine(value, element);
}
return value;
```

计算可迭代对象总和的示例：

```dart
final numbers = <double>[10, 2, 5, 0.5];
const initialValue = 100.0;
final result = numbers.fold<double>(
    initialValue, (previousValue, element) => previousValue + element);
print(result); // 117.5
```

### every()

```dart
bool every(bool test(E element))
```

检查此可迭代对象的每个元素是否都满足 [test]。

按迭代顺序检查每个元素，只要有元素使 [test] 返回 `false`，就返回 `false`，否则返回 `true`。如果该可迭代对象为空，则返回 `true`。

示例：

```dart
final planetsByMass = <double, String>{0.06: 'Mercury', 0.81: 'Venus',
  0.11: 'Mars'};
// 检查所有键是否都小于 1。
final every = planetsByMass.keys.every((key) => key < 1.0); // true
```

### join()

```dart
String join([String separator = ""])
```

将每个元素转换为 [String](https://www.yuque.com/thyname/dart.core/string) 并将这些字符串连接起来。

遍历此可迭代对象的元素，通过调用 [Object.toString] 将每个元素转换为一个 [String](https://www.yuque.com/thyname/dart.core/string)，然后将这些字符串连接起来，各元素之间插入 [separator] 字符串。

示例：

```dart
final planetsByMass = <double, String>{0.06: 'Mercury', 0.81: 'Venus',
  0.11: 'Mars'};
final joinedNames = planetsByMass.values.join('-'); // Mercury-Venus-Mars
```

### any()

```dart
bool any(bool test(E element))
```

检查此可迭代对象的任意一个元素是否满足 [test]。

按迭代顺序检查每个元素，只要有元素使 [test] 返回 `true`，就返回 `true`，否则返回 `false`。如果该可迭代对象为空，则返回 `false`。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.any((element) => element >= 5); // true;
result = numbers.any((element) => element >= 10); // false;
```

### toList()

```dart
List<E> toList({bool growable = true})
```

创建一个包含此 [Iterable](https://www.yuque.com/thyname/dart.core/iterable) 元素的 [List](https://www.yuque.com/thyname/dart.core/list)。

元素按迭代顺序排列。如果 [growable] 为 false，则返回的列表是定长列表。

示例：

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Mars'};
final keysList = planets.keys.toList(growable: false); // [1, 2, 3]
final valuesList =
    planets.values.toList(growable: false); // [Mercury, Venus, Mars]
```

### toSet()

```dart
Set<E> toSet()
```

创建一个包含与此可迭代对象相同元素的 [Set](https://www.yuque.com/thyname/dart.core/set)。

如果该可迭代对象中某个元素出现多次，或者包含一个或多个相等的元素，则该集合可能包含比该可迭代对象更少的元素。集合中元素的顺序不保证与该可迭代对象中的顺序相同。

示例：

```dart
final planets = <int, String>{1: 'Mercury', 2: 'Venus', 3: 'Mars'};
final valueSet = planets.values.toSet(); // {Mercury, Venus, Mars}
```

### take()

```dart
Iterable<E> take(int count)
```

创建一个惰性可迭代对象，包含此可迭代对象的前 [count] 个元素。

如果 `this` 包含的元素少于 `count` 个，则返回的 `Iterable` 可能包含少于 `count` 个元素。

可以通过步进 [iterator]，直到看到 [count] 个元素为止来计算这些元素。

`count` 不能为负数。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
final result = numbers.take(4); // (1, 2, 3, 5)
final takeAll = numbers.take(100); // (1, 2, 3, 5, 6, 7)
```

### takeWhile()

```dart
Iterable<E> takeWhile(bool test(E value))
```

创建一个惰性可迭代对象，包含满足 [test] 的前导元素。

过滤操作是惰性进行的。返回的可迭代对象每个新的迭代器都会从头开始遍历 `this` 的元素。

可以通过步进 [iterator]，直到找到一个使 `test(element)` 为 false 的元素为止来计算这些元素。此时，返回的可迭代对象停止遍历（其 `moveNext()` 返回 false）。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.takeWhile((x) => x < 5); // (1, 2, 3)
result = numbers.takeWhile((x) => x != 3); // (1, 2)
result = numbers.takeWhile((x) => x != 4); // (1, 2, 3, 5, 6, 7)
result = numbers.takeWhile((x) => x.isOdd); // (1)
```

### skip()

```dart
Iterable<E> skip(int count)
```

创建一个 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)，提供除前 [count] 个元素之外的所有元素。

当返回的可迭代对象被遍历时，它会开始遍历 `this`，首先跳过最初的 [count] 个元素。如果 `this` 的元素少于 `count` 个，则生成的 Iterable 为空。之后，剩余的元素将按此可迭代对象中相同的顺序进行遍历。

某些可迭代对象可能能够在不先遍历前面元素的情况下找到后面的元素，例如在遍历 [List](https://www.yuque.com/thyname/dart.core/list) 时。这类可迭代对象允许直接忽略被跳过的最初元素。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
final result = numbers.skip(4); // (6, 7)
final skipAll = numbers.skip(100); // () - 没有元素。
```

[count] 不能为负数。

### skipWhile()

```dart
Iterable<E> skipWhile(bool test(E value))
```

创建一个 `Iterable`，跳过满足 [test] 的前导元素。

过滤操作是惰性进行的。返回的可迭代对象的每个新 [Iterator](https://www.yuque.com/thyname/dart.core/iterator) 都会遍历 `this` 的所有元素。

返回的可迭代对象通过遍历此可迭代对象来提供元素，但会跳过所有 `test(element)` 返回 true 的初始元素。如果所有元素都满足 `test`，则生成的可迭代对象为空；否则，它会按原始顺序遍历剩余的元素，从第一个使 `test(element)` 返回 `false` 的元素开始。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.skipWhile((x) => x < 5); // (5, 6, 7)
result = numbers.skipWhile((x) => x != 3); // (3, 5, 6, 7)
result = numbers.skipWhile((x) => x != 4); // ()
result = numbers.skipWhile((x) => x.isOdd); // (2, 3, 5, 6, 7)
```

### firstWhere()

```dart
E firstWhere(bool test(E element), {E orElse()})
```

第一个满足给定谓词 [test] 的元素。

遍历元素并返回第一个满足 [test] 的元素。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.firstWhere((element) => element < 5); // 1
result = numbers.firstWhere((element) => element > 5); // 6
result =
    numbers.firstWhere((element) => element > 10, orElse: () => -1); // -1
```

如果没有元素满足 [test]，则返回调用 [orElse] 函数的结果。如果省略 [orElse]，则默认抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。一旦找到第一个匹配的元素，就会停止遍历。

### lastWhere()

```dart
E lastWhere(bool test(E element), {E orElse()})
```

最后一个满足给定谓词 [test] 的元素。

能够直接访问其元素的可迭代对象可以按任意顺序检查其元素（例如，列表可能从最后一个元素开始检查，然后向列表开头方向移动）。默认实现按迭代顺序遍历元素，对每个元素检查 `test(element)`，最终返回最后一个匹配的元素。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
var result = numbers.lastWhere((element) => element < 5); // 3
result = numbers.lastWhere((element) => element > 5); // 7
result = numbers.lastWhere((element) => element > 10,
    orElse: () => -1); // -1
```

如果没有元素满足 [test]，则返回调用 [orElse] 函数的结果。如果省略 [orElse]，则默认抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。

### singleWhere()

```dart
E singleWhere(bool test(E element), {E orElse()})
```

满足 [test] 的单个元素。

检查各元素，判断 `test(element)` 是否返回 true。如果恰好有一个元素满足 [test]，则返回该元素。如果找到多个匹配的元素，则抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。如果没有找到匹配的元素，则返回 [orElse] 的结果。如果省略 [orElse]，则默认抛出 [StateError](https://www.yuque.com/thyname/dart.core/stateerror)。

示例：

```dart
final numbers = <int>[2, 2, 10];
var result = numbers.singleWhere((element) => element > 5); // 10
```

当没有找到匹配的元素时，将返回调用 [orElse] 的结果。

```dart continued
result = numbers.singleWhere((element) => element == 1,
    orElse: () => -1); // -1
```

不能有多个匹配的元素。

```dart continued
result = numbers.singleWhere((element) => element == 2); // 抛出错误。
```

### elementAt()

```dart
E elementAt(int index)
```

返回第 [index] 个元素。

[index] 必须是非负数，且小于 [length]。索引 0 表示第一个元素（因此 `iterable.elementAt(0)` 等价于 `iterable.first`）。

可能会按迭代顺序遍历元素，忽略前 [index] 个元素，然后返回下一个元素。一些可迭代对象可能有更高效的方式来找到该元素。

示例：

```dart
final numbers = <int>[1, 2, 3, 5, 6, 7];
final elementAt = numbers.elementAt(4); // 6
```

### toString()

```dart
String toString()
```

返回 `this` 中（部分）元素的字符串表示。

各元素通过其自身的 `toString` 结果来表示。

默认表示形式总是包含前三个元素。如果可迭代对象中的元素少于一百个，还会包含最后两个元素。

如果生成的字符串不超过 80 个字符，则会从可迭代对象的开头包含更多元素。

如果已知某些元素不会出现在输出中，转换过程可能会省略对这些元素调用 `toString`，并且可能在遍历一百个元素后停止遍历。
