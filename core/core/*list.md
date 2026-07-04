# List

```dart
abstract interface class List<E> implements Iterable<E>, _ListIterable<E> {}
```

具有长度的可索引对象集合。

此类的子类实现不同类型的列表。最常见的列表类型有：

- **固定长度列表**

​ 尝试使用可更改列表长度的操作时会发生错误。

- **可增长列表**

​ 完整实现了此类中定义的 API。

通过 `[]` 创建的默认可增长列表会维护一个内部缓冲区，并在必要时扩展该缓冲区。这保证了一系列 [add] 操作都能以均摊常数时间执行。直接设置 length 可能需要与新长度成正比的时间，并可能改变内部容量，从而导致后续的 add 操作需要立即扩大缓冲区容量。其他列表实现的性能表现可能有所不同。

固定长度列表示例：

```dart
final fixedLengthList = List<int>.filled(5, 0); // Creates fixed-length list.
print(fixedLengthList); // [0, 0, 0, 0, 0]
fixedLengthList[0] = 87;
fixedLengthList.setAll(1, [1, 2, 3]);
print(fixedLengthList); // [87, 1, 2, 3, 0]
// Fixed length list length can't be changed or increased
fixedLengthList.length = 0;  // Throws
fixedLengthList.add(499);    // Throws
```

可增长列表示例：

```dart
final growableList = <String>['A', 'B']; // Creates growable list.
```

要向可增长列表添加数据，可使用 [operator[]=]、[add] 或 [addAll]。

```dart
growableList[0] = 'G';
print(growableList); // [G, B]
growableList.add('X');
growableList.addAll({'C', 'B'});
print(growableList); // [G, B, X, C, B]
```

要检查某元素是否在列表中，以及它所在的位置，可使用 [indexOf] 或 [lastIndexOf]。

```dart
final indexA = growableList.indexOf('A'); // -1 (not in the list)
final firstIndexB = growableList.indexOf('B'); // 1
final lastIndexB = growableList.lastIndexOf('B'); // 4
```

要从可增长列表中移除元素，可使用 [remove]、[removeAt]、[removeLast]、[removeRange] 或 [removeWhere]。

```dart
growableList.remove('C');
growableList.removeLast();
print(growableList); // [G, B, X]
```

要在列表中的某个位置插入元素，可使用 [insert] 或 [insertAll]。

```dart
growableList.insert(1, 'New');
print(growableList); // [G, New, B, X]
```

要替换列表中某个范围的元素，可使用 [fillRange]、[replaceRange] 或 [setRange]。

```dart
growableList.replaceRange(0, 2, ['AB', 'A']);
print(growableList); // [AB, A, B, X]
growableList.fillRange(2, 4, 'F');
print(growableList); // [AB, A, F, F]
```

要对列表元素进行排序，可使用 [sort]。

```dart
growableList.sort((a, b) => a.compareTo(b));
print(growableList); // [A, AB, F, F]
```

要随机打乱此列表的元素顺序，可使用 [shuffle]。

```dart
growableList.shuffle();
print(growableList); // e.g. [AB, F, A, F]
```

要查找第一个满足某个断言的元素，或在没有元素满足时返回默认值，可使用 [firstWhere]。

```dart
bool isVowel(String char) => char.length == 1 && "AEIOU".contains(char);
final firstVowel = growableList.firstWhere(isVowel, orElse: () => ''); // ''
```

还有类似的 [lastWhere] 和 [singleWhere] 方法。

列表是一个 [Iterable]，支持其全部方法，包括 [where]、[map]、[whereType] 和 [toList]。

列表属于 [Iterable]。迭代按索引顺序遍历各个值。改变元素的值不会影响迭代，但在迭代步骤之间改变有效索引——即改变列表的长度——会引发 [ConcurrentModificationError]。这意味着只有可增长列表才会抛出 ConcurrentModificationError。如果长度只是暂时改变，并在继续迭代前恢复，迭代器可能检测不到这种变化。

通常不允许在对列表执行某项操作期间（例如调用 [forEach] 或 [sort] 时）修改列表的长度（添加或删除元素）。在迭代列表期间改变其长度——无论是直接迭代该列表，还是通过迭代一个以该列表为后备存储的 [Iterable]——都会破坏迭代过程。

## 构造函数

### List.filled()

```dart
List<E>.filled(
  int length,
  E fill, {
  bool growable = false,
})
```

创建一个具有给定长度的列表，并在每个位置填充 [fill]。

[length] 必须是非负整数。

示例：

```dart
final zeroList = List<int>.filled(3, 0, growable: true); // [0, 0, 0]
```

如果 [growable] 为 false（默认值），创建的列表为固定长度；如果为 true，则为可增长列表。如果列表是可增长的，增加其 [length] 并**不会**用 [fill] 初始化新增的元素。列表创建并填充完成后，与使用 `[]` 或其他 [List] 构造函数创建的可增长列表或固定长度列表没有区别。

所创建列表的所有元素共享同一个 [fill] 值。

```dart
final shared = List.filled(3, []);
shared[0].add(499);
print(shared);  // [[499], [499], [499]]
```

可以使用 [List.generate] 创建一个固定长度的列表，并在每个位置生成一个新对象。

```dart
final unique = List.generate(3, (_) => []);
unique[0].add(499);
print(unique); // [[499], [], []]
```

### List.empty()

```dart
List<E>.empty({
	bool growable = false,
})
```

创建一个新的空列表。

如果 [growable] 为 `false`（默认值），该列表是长度为零的固定长度列表。如果 [growable] 为 `true`，该列表是可增长的，等价于 `<E>[]`。

```dart
final growableList = List.empty(growable: true); // []
growableList.add(1); // [1]

final fixedLengthList = List.empty(growable: false);
fixedLengthList.add(1); // error
```

### List.from()

```dart
List<E>.from(
  Iterable elements, {
  bool growable = true,
})
```

创建一个包含所有 [elements] 的列表。

[elements] 的 [Iterator] 决定元素的顺序。

所有 [elements] 都应是 [E] 的实例。

示例：

```dart
final numbers = <num>[1, 2, 3];
final listFrom = List<int>.from(numbers);
print(listFrom); // [1, 2, 3]
```

`elements` 这个可迭代对象本身可以是任意元素类型，因此该构造函数可用于对 `List` 进行向下转型，例如：

```dart import:convert
const jsonArray = '''
  [{"text": "foo", "value": 1, "status": true},
   {"text": "bar", "value": 2, "status": false}]
''';
final List<dynamic> dynamicList = jsonDecode(jsonArray);
final List<Map<String, dynamic>> fooData =
    List.from(dynamicList.where((x) => x is Map && x['text'] == 'foo'));
print(fooData); // [{text: foo, value: 1, status: true}]
```

当 [growable] 为 true 时，该构造函数创建一个可增长列表；否则返回一个固定长度列表。

### List.of()

```dart
List<E>.of(
  Iterable<E> elements, {
  bool growable = true,
})
```

根据 [elements] 创建一个列表。

[elements] 的 [Iterator] 决定元素的顺序。

当 [growable] 为 true 时，该构造函数创建一个可增长列表；否则返回一个固定长度列表。

```dart
final numbers = <int>[1, 2, 3];
final listOf = List<num>.of(numbers);
print(listOf); // [1, 2, 3]
```

### List.generate()

```dart
List<E>.generate(
  int length,
  E generator( int index ), {
  bool growable = true,
})
```

生成一个值列表。

创建一个具有 [length] 个位置的列表，并按从 `0` 到 `length - 1` 递增的顺序，为每个索引调用 [generator] 生成的值填充该列表。

```dart
final growableList =
    List<int>.generate(3, (int index) => index * index, growable: true);
print(growableList); // [0, 1, 4]

final fixedLengthList =
    List<int>.generate(3, (int index) => index * index, growable: false);
print(fixedLengthList); // [0, 1, 4]
```

如果 [growable] 设为 false，创建的列表为固定长度。

[length] 必须为非负数。

### List.unmodifiable()

```dart
List<E>.unmodifiable( Iterable elements )
```

创建一个包含所有 [elements] 的不可修改列表。

[elements] 的 [Iterator] 决定元素的顺序。

不可修改列表的长度和元素都不能更改。如果元素本身是不可变的，那么生成的列表也是不可变的。

```dart
final numbers = <int>[1, 2, 3];
final unmodifiableList = List.unmodifiable(numbers); // [1, 2, 3]
unmodifiableList[1] = 87; // Throws.
```

### List.unmodifiableOf()

```dart
List<E>.unmodifiableOf(Iterable<E> elements)
```

创建一个包含所有 [elements] 的不可修改列表。

[elements] 的 [Iterator] 决定元素的顺序。

不可修改列表的长度和元素都不能更改。如果元素本身是不可变的，那么生成的列表也是不可变的。

```dart
final numbers = <int>[1, 2, 3];
final unmodifiableList = List.unmodifiable(numbers); // [1, 2, 3]
unmodifiableList[1] = 87; // Throws.
```

## 静态方法

### castFrom()

```dart
List<T> castFrom<S, T>(List<S> source)
```

将 [source] 适配为 `List<T>`。

每当该列表要生成一个不是 [T] 的元素时，该元素的访问操作都会抛出异常。

每当尝试向适配后的列表中存入一个 [T] 值时，除非该值同时也是 [S] 的实例，否则存入操作会抛出异常。

如果 [source] 中所有被访问的元素实际上都是 [T] 的实例，并且所有存入返回列表的元素实际上都是 [S] 的实例，那么返回的列表就可以当作 `List<T>` 使用。

接受 `Object?` 作为参数的方法，如 [contains] 和 [remove]，会将参数直接传递给此列表的相应方法，而不做任何检查。

### copyRange()

```dart
void copyRange<T>(
  List<T> target,
  int at,
  List<T> source, [
  int? start,
  int? end,
])
```

将一个列表中的某个范围复制到另一个列表中。

这是一个可用于实现 [setRange] 等方法的工具函数。

从 [start] 到 [end] 的范围必须是 [source] 的有效范围，并且从位置 [at] 开始必须有足够的空间容纳 `end - start` 个元素。如果省略 [start]，默认为零。如果省略 [end]，默认为 [source].length。

如果 [source] 与 [target] 是同一个列表，会遵循重叠的源范围与目标范围规则，使目标范围最终包含源范围的初始内容。否则，元素复制的顺序不作保证。

### writeIterable()

```dart
void writeIterable<T>(List<T> target, int at, Iterable<T> source)
```

将一个可迭代对象的元素写入列表。

这是一个可用于实现 [setAll] 等方法的工具函数。

[source] 的元素从位置 [at] 开始被写入 [target]。写完 [target] 的最后一个位置后，[source] 中不得再包含更多元素。

如果 source 是一个列表，使用 [copyRange] 函数可能更高效。

## 属性

### first

```dart
E get first
```

第一个元素。

如果 `this` 为空，则抛出 [StateError](https://api.flutter.dev/flutter/dart-core/StateError-class.html)。否则按迭代顺序返回第一个元素，等价于 `this.elementAt(0)`。

---

```dart
void set first(E value)
```

列表的第一个元素。

访问列表的第一个元素时，列表必须非空。

与 [Iterable] 不同，列表的第一个元素可以被修改。`list.first` 等价于 `list[0]`，无论是取值还是赋值都是如此。

```dart
final numbers = <int>[1, 2, 3];
print(numbers.first); // 1
numbers.first = 10;
print(numbers.first); // 10
numbers.clear();
numbers.first; // Throws.
```

### last

```dart
E get last
```

最后一个元素。

如果 `this` 为空，则抛出 [StateError](https://api.flutter.dev/flutter/dart-core/StateError-class.html)。否则可能会遍历元素并返回最后遇到的一个。某些可迭代对象可能有更高效的方式获取最后一个元素（例如列表可以直接访问最后一个元素，而无需遍历前面的元素）。

---

```dart
void set last(E value)
```

列表的最后一个元素。

访问列表的最后一个元素时，列表必须非空。

与 [Iterable] 不同，列表的最后一个元素可以被修改。`list.last` 等价于 `theList[theList.length - 1]`，无论是取值还是赋值都是如此。

```dart
final numbers = <int>[1, 2, 3];
print(numbers.last); // 3
numbers.last = 10;
print(numbers.last); // 10
numbers.clear();
numbers.last; // Throws.
```

### length

```dart
int get length
```

此列表中对象的数量。

列表的有效索引是 `0` 到 `length - 1`。

```dart
final numbers = <int>[1, 2, 3];
print(numbers.length); // 3
```

---

```dart
set length(int newLength)
```

设置 `length` 会改变列表中元素的数量。

列表必须是可增长的。如果 [newLength] 大于当前长度，新增的条目会被初始化为 `null`，因此当元素类型 [E] 为非空类型时，[newLength] 不得大于当前长度。

```dart
final maybeNumbers = <int?>[1, null, 3];
maybeNumbers.length = 5;
print(maybeNumbers); // [1, null, 3, null, null]
maybeNumbers.length = 2;
print(maybeNumbers); // [1, null]

final numbers = <int>[1, 2, 3];
numbers.length = 1;
print(numbers); // [1]
numbers.length = 5; // Throws, cannot add `null`s.
```

### reversed

```dart
Iterable<E> get reversed
```

此列表中对象按逆序排列所得的 [Iterable]。

```dart
final numbers = <String>['two', 'three', 'four'];
final reverseOrder = numbers.reversed;
print(reverseOrder.toList()); // [four, three, two]
```

## 方法

### cast()

```dart
List<R> cast<R>()
```

返回此列表作为 [R] 实例列表的一个视图。

如果此列表只包含 [R] 的实例，所有读取操作都能正常工作。如果任何操作尝试读取一个不是 [R] 实例的元素，该访问操作将抛出异常。

添加到列表中的元素（例如通过 [add] 或 [addAll]）必须是 [R] 的实例才是添加函数的有效参数，同时它们也必须是 [E] 的实例才能被此列表接受。

接受 `Object?` 作为参数的方法，如 [contains] 和 [remove]，会将参数直接传递给此列表的相应方法，而不做任何检查。这意味着即便看起来不应产生任何效果，`listOfStrings.cast<int>().remove("a")` 也能成功执行。

通常实现为 `List.castFrom<E, R>(this)`。

### add()

```dart
void add(E value)
```

将 [value] 添加到此列表末尾，长度增加一。

列表必须是可增长的。

```dart
final numbers = <int>[1, 2, 3];
numbers.add(4);
print(numbers); // [1, 2, 3, 4]
```

### addAll()

```dart
void addAll(Iterable<E> iterable)
```

将 [iterable] 中的所有对象追加到此列表末尾。

将列表的长度扩展 [iterable] 中对象的数量。列表必须是可增长的。

```dart
final numbers = <int>[1, 2, 3];
numbers.addAll([4, 5, 6]);
print(numbers); // [1, 2, 3, 4, 5, 6]
```

### sort()

```dart
void sort([int compare(E a, E b)])
```

按照 [compare] 函数指定的顺序对此列表进行排序。

[compare] 函数必须作为一个 [Comparator]。

```dart
final numbers = <String>['two', 'three', 'four'];
// Sort from shortest to longest.
numbers.sort((a, b) => a.length.compareTo(b.length));
print(numbers); // [two, four, three]
```

如果省略 [compare]，默认的 [List] 实现会使用 [Comparable.compare]。

```dart
final numbers = <int>[13, 2, -11, 0];
numbers.sort();
print(numbers); // [-11, 0, 2, 13]
```

在这种情况下，列表中的元素彼此之间必须是可比较的（[Comparable]）。

[Comparator] 可能会将不同的对象比较为相等（返回零），即使它们是不同的对象。排序函数不保证稳定，因此比较结果相等的不同对象在结果中可能以任意顺序出现：

```dart
final numbers = <String>['one', 'two', 'three', 'four'];
numbers.sort((a, b) => a.length.compareTo(b.length));
print(numbers); // [one, two, four, three] OR [two, one, four, three]
```

### shuffle()

```dart
void shuffle([Random? random])
```

随机打乱此列表元素的顺序。

```dart
final numbers = <int>[1, 2, 3, 4, 5];
numbers.shuffle();
print(numbers); // [1, 3, 4, 5, 2] OR some other random result.
```

### indexOf()

```dart
int indexOf(E element, [int start = 0])
```

[element] 在此列表中第一次出现的索引。

从索引 [start] 开始搜索直到列表末尾。第一次遇到满足 `o == element` 的对象 `o` 时，返回 `o` 的索引。

```dart
final notes = <String>['do', 're', 'mi', 're'];
print(notes.indexOf('re')); // 1

final indexWithStart = notes.indexOf('re', 2); // 3
```

如果未找到 [element]，返回 -1。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final index = notes.indexOf('fa'); // -1
```

### indexWhere()

```dart
int indexWhere(bool test(E element), [int start = 0])
```

列表中第一个满足给定 [test] 的元素的索引。

从索引 [start] 开始搜索直到列表末尾。第一次遇到满足 `test(o)` 为真的对象 `o` 时，返回 `o` 的索引。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final first = notes.indexWhere((note) => note.startsWith('r')); // 1
final second = notes.indexWhere((note) => note.startsWith('r'), 2); // 3
```

如果未找到 [element]，返回 -1。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final index = notes.indexWhere((note) => note.startsWith('k')); // -1
```

### lastIndexWhere()

```dart
int lastIndexWhere(bool test(E element), [int? start])
```

列表中最后一个满足给定 [test] 的元素的索引。

从索引 [start] 开始向 0 搜索。第一次遇到满足 `test(o)` 为真的对象 `o` 时，返回 `o` 的索引。如果省略 [start]，默认为列表的 [length]。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final first = notes.lastIndexWhere((note) => note.startsWith('r')); // 3
final second = notes.lastIndexWhere((note) => note.startsWith('r'),
    2); // 1
```

如果未找到 [element]，返回 -1。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final index = notes.lastIndexWhere((note) => note.startsWith('k'));
print(index); // -1
```

### lastIndexOf()

```dart
int lastIndexOf(E element, [int? start])
```

[element] 在此列表中最后一次出现的索引。

从索引 [start] 向 0 反向搜索列表。

第一次遇到满足 `o == element` 的对象 `o` 时，返回 `o` 的索引。

```dart
final notes = <String>['do', 're', 'mi', 're'];
const startIndex = 2;
final index = notes.lastIndexOf('re', startIndex); // 1
```

如果未提供 [start]，此方法从列表末尾开始搜索。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final index = notes.lastIndexOf('re'); // 3
```

如果未找到 [element]，返回 -1。

```dart
final notes = <String>['do', 're', 'mi', 're'];
final index = notes.lastIndexOf('fa'); // -1
```

### clear()

```dart
void clear()
```

移除此列表中的所有对象；列表长度变为零。

列表必须是可增长的。

```dart
final numbers = <int>[1, 2, 3];
numbers.clear();
print(numbers.length); // 0
print(numbers); // []
```

### insert()

```dart
void insert(int index, E element)
```

在此列表的 [index] 位置插入 [element]。

这会使列表长度增加一，并将该索引及之后的所有对象向列表末尾方向移动。

列表必须是可增长的。[index] 值必须为非负数，且不大于 [length]。

```dart
final numbers = <int>[1, 2, 3, 4];
const index = 2;
numbers.insert(index, 10);
print(numbers); // [1, 2, 10, 3, 4]
```

### insertAll()

```dart
void insertAll(int index, Iterable<E> iterable)
```

在此列表的 [index] 位置插入 [iterable] 中的所有对象。

这会使列表长度增加 [iterable] 的长度，并将后面的所有对象向列表末尾方向移动。

列表必须是可增长的。[index] 值必须为非负数，且不大于 [length]。

```dart
final numbers = <int>[1, 2, 3, 4];
final insertItems = [10, 11];
numbers.insertAll(2, insertItems);
print(numbers); // [1, 2, 10, 11, 3, 4]
```

### setAll()

```dart
void setAll(int index, Iterable<E> iterable)
```

用 [iterable] 中的对象覆写元素。

[iterable] 的元素被写入此列表，从位置 [index] 开始。此操作不会增加列表的长度。

[index] 必须为非负数，且不大于 [length]。

[iterable] 的元素数量不得超过从 [index] 到 [length] 所能容纳的数量。

如果 `iterable` 基于此列表，其值可能在 `setAll` 操作*期间*发生变化。

```dart
final list = <String>['a', 'b', 'c', 'd'];
list.setAll(1, ['bee', 'sea']);
print(list); // [a, bee, sea, d]
```

### remove()

```dart
bool remove(Object? value)
```

从此列表中移除第一次出现的 [value]。

如果 [value] 曾在列表中，返回 true，否则返回 false。列表必须是可增长的。

```dart
final parts = <String>['head', 'shoulders', 'knees', 'toes'];
final retVal = parts.remove('head'); // true
print(parts); // [shoulders, knees, toes]
```

如果 [value] 不在列表中，该方法不产生任何效果。

```dart
final parts = <String>['shoulders', 'knees', 'toes'];
// Note: 'head' has already been removed.
final retVal = parts.remove('head'); // false
print(parts); // [shoulders, knees, toes]
```

### removeAt()

```dart
E removeAt(int index)
```

从此列表中移除位于位置 [index] 的对象。

此方法将 `this` 的长度减一，并将其后所有对象向前移动一位。

返回被移除的值。

[index] 必须在 `0 ≤ index < length` 范围内。列表必须是可增长的。

```dart
final parts = <String>['head', 'shoulder', 'knees', 'toes'];
final retVal = parts.removeAt(2); // knees
print(parts); // [head, shoulder, toes]
```

### removeLast()

```dart
E removeLast()
```

移除并返回此列表中的最后一个对象。

列表必须是可增长且非空的。

```dart
final parts = <String>['head', 'shoulder', 'knees', 'toes'];
final retVal = parts.removeLast(); // toes
print(parts); // [head, shoulder, knees]
```

### removeWhere()

```dart
void removeWhere(bool test(E element))
```

从此列表中移除所有满足 [test] 的对象。

如果 `test(o)` 为真，则对象 `o` 满足 [test]。

```dart
final numbers = <String>['one', 'two', 'three', 'four'];
numbers.removeWhere((item) => item.length == 3);
print(numbers); // [three, four]
```

列表必须是可增长的。

### retainWhere()

```dart
void retainWhere(bool test(E element))
```

从此列表中移除所有不满足 [test] 的对象。

如果 `test(o)` 为真，则对象 `o` 满足 [test]。

```dart
final numbers = <String>['one', 'two', 'three', 'four'];
numbers.retainWhere((item) => item.length == 3);
print(numbers); // [one, two]
```

列表必须是可增长的。

### sublist()

```dart
List<E> sublist(int start, [int? end])
```

返回一个包含 [start] 与 [end] 之间元素的新列表。

新列表是一个 `List<E>`，包含此列表中位置大于或等于 [start] 且小于 [end] 的元素，顺序与它们在此列表中出现的顺序相同。

```dart
final colors = <String>['red', 'green', 'blue', 'orange', 'pink'];
print(colors.sublist(1, 3)); // [green, blue]
```

如果省略 [end]，默认为此列表的 [length]。

```dart
final colors = <String>['red', 'green', 'blue', 'orange', 'pink'];
print(colors.sublist(3)); // [orange, pink]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ [length]。如果 `end` 等于 `start`，则返回的列表为空。

### getRange()

```dart
Iterable<E> getRange(int start, int end)
```

创建一个遍历某个范围内元素的 [Iterable]。

返回的可迭代对象遍历此列表中位置大于或等于 [start] 且小于 [end] 的元素。

调用时提供的范围 [start] 和 [end] 必须有效。若 0 ≤ `start` ≤ `end` ≤ [length]，则从 [start] 到 [end] 的范围有效。空范围（`end == start`）也是有效的。

返回的 [Iterable] 的行为类似于 `skip(start).take(end - start)`。也就是说，它*不会*因为此列表大小改变而中断，只是如果提前到达列表末尾（即 `end`，甚至 `start`，大于 [length]），会提前结束。

```dart
final colors = <String>['red', 'green', 'blue', 'orange', 'pink'];
final firstRange = colors.getRange(0, 3);
print(firstRange.join(', ')); // red, green, blue

final secondRange = colors.getRange(2, 5);
print(secondRange.join(', ')); // blue, orange, pink
```

### setRange()

```dart
void setRange(
  int start,
  int end,
  Iterable<E> iterable, [
  int skipCount = 0,
])
```

将 [iterable] 中的部分元素写入此列表的某个范围。

将 [iterable] 中的对象（先跳过 [skipCount] 个对象）复制到此列表从 [start]（含）到 [end]（不含）的范围内。

```dart
final list1 = <int>[1, 2, 3, 4];
final list2 = <int>[5, 6, 7, 8, 9];
// Copies the 4th and 5th items in list2 as the 2nd and 3rd items
// of list1.
const skipCount = 3;
list1.setRange(1, 3, list2, skipCount);
print(list1); // [1, 8, 9, 4]
```

给定的范围 [start] 和 [end] 必须有效。若 0 ≤ `start` ≤ `end` ≤ [length]，则从 [start] 到 [end] 的范围有效。空范围（`end == start`）也是有效的。

在跳过 [skipCount] 个对象后，[iterable] 必须有足够的对象来填充从 `start` 到 `end` 的范围。

如果 `iterable` 就是此列表本身，该操作会正确地将原本位于 `skipCount` 到 `skipCount + (end - start)` 范围内的元素复制到 `start` 到 `end` 的范围，即使这两个范围存在重叠。

如果 `iterable` 以其他方式依赖于此列表，则不作任何保证。

### removeRange()

```dart
void removeRange(int start, int end)
```

从列表中移除一个范围的元素。

移除列表中位置大于或等于 [start] 且小于 [end] 的元素。这会使列表的长度减少 `end - start`。

给定的范围 [start] 和 [end] 必须有效。若 0 ≤ `start` ≤ `end` ≤ [length]，则从 [start] 到 [end] 的范围有效。空范围（`end == start`）也是有效的。

列表必须是可增长的。

```dart
final numbers = <int>[1, 2, 3, 4, 5];
numbers.removeRange(1, 4);
print(numbers); // [1, 5]
```

### fillRange()

```dart
void fillRange(int start, int end, [E? fillValue])
```

用 [fillValue] 覆写一个范围的元素。

将位置大于或等于 [start] 且小于 [end] 的元素设置为 [fillValue]。

给定的范围 [start] 和 [end] 必须有效。若 0 ≤ `start` ≤ `end` ≤ [length]，则从 [start] 到 [end] 的范围有效。空范围（`end == start`）也是有效的。

如果元素类型不可为空，则必须提供 [fillValue]，且不得为 `null`。

示例：

```dart
final words = List.filled(5, 'old');
print(words); // [old, old, old, old, old]
words.fillRange(1, 3, 'new');
print(words); // [old, new, new, old, old]
```

### replaceRange()

```dart
void replaceRange(int start, int end, Iterable<E> replacements)
```

用 [replacements] 中的元素替换一个范围的元素。

移除从 [start] 到 [end] 范围内的对象，然后在 [start] 处插入 [replacements] 中的元素。

```dart
final numbers = <int>[1, 2, 3, 4, 5];
final replacements = [6, 7];
numbers.replaceRange(1, 4, replacements);
print(numbers); // [1, 6, 7, 5]
```

给定的范围 [start] 和 [end] 必须有效。若 0 ≤ `start` ≤ `end` ≤ [length]，则从 [start] 到 [end] 的范围有效。空范围（`end == start`）也是有效的。

操作 `list.replaceRange(start, end, replacements)` 大致等价于：

```dart
final numbers = <int>[1, 2, 3, 4, 5];
numbers.removeRange(1, 4);
final replacements = [6, 7];
numbers.insertAll(1, replacements);
print(numbers); // [1, 6, 7, 5]
```

但可能效率更高。

列表必须是可增长的。此方法不适用于固定长度列表，即使 [replacements] 的元素数量与被替换范围的元素数量相同也是如此。这种情况下应改用 [setRange]。

### asMap()

```dart
Map<int, E> asMap()
```

此列表的一个不可修改的 [Map] 视图。

该映射使用此列表的索引作为键，对应的对象作为值。`Map.keys` 这个 [Iterable] 按数字顺序迭代此列表的索引。

```dart
var words = <String>['fee', 'fi', 'fo', 'fum'];
var map = words.asMap();  // {0: fee, 1: fi, 2: fo, 3: fum}
map.keys.toList(); // [0, 1, 2, 3]
```

## 运算符

### operator []

```dart
E operator [](int index)
```

列表中给定 [index] 处的对象。

[index] 必须是此列表的有效索引，即 `index` 必须为非负数且小于 [length]。

### operator []=

```dart
void operator []=(int index, E value)
```

将列表中给定 [index] 处的值设置为 [value]。

[index] 必须是此列表的有效索引，即 `index` 必须为非负数且小于 [length]。

### operator +

```dart
List<E> operator +(List<E> other)
```

返回此列表与 [other] 拼接后的结果。

返回一个新列表，包含此列表的元素，后接 [other] 的元素。

默认行为是返回一个普通的可增长列表。某些列表类型可能选择返回与自身类型相同的列表（参见 [Uint8List.+]）。

### operator ==

```dart
bool operator ==(Object other)
```

此列表是否等于 [other]。

默认情况下，列表只与自身相等。即使 [other] 也是一个列表，该相等性比较也不会比较两个列表的元素。
