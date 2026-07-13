# TypedData

```dart
abstract final class TypedData {}
```

字节序列的类型化视图。

类尝试继承或实现 `TypedData` 是编译时错误。

## 属性

### elementSizeInBytes

```dart
int get elementSizeInBytes
```

此列表中每个元素的表示所占的字节数。

### offsetInBytes

```dart
int get offsetInBytes
```

此视图相对于底层字节缓冲区的偏移量，以字节为单位。

### lengthInBytes

```dart
int get lengthInBytes
```

此视图的长度，以字节为单位。

### buffer

```dart
ByteBuffer get buffer
```

与此对象关联的字节缓冲区。

---

# TypedDataList

```dart
abstract final class TypedDataList<E> implements TypedData, List<E> {}
```

[buffer] 字节上的 [TypedData] 固定长度 [List] 视图。

---

# Endian

```dart
final class Endian {}
```

数字表示的字节序。

数字表示在内存中的字节顺序，[little] 字节序为最低有效字节在前，[big] 字节序（即网络字节序）为最高有效字节在前。

[host] 字节序是底层平台的原生字节序，也是该平台上类型化数据列表（如 [Uint16List]）使用的默认字节序。始终为 [little] 或 [big] 字节序之一。

在使用 [ByteData] 视图访问或更新字节序列时可以指定字节序。若通过较大数字的字节来访问它们，例如通过对使用 [Int64List] 视图写入的同一缓冲区建立的 [Uint8List] 视图进行访问，则可以使用主机字节序。

类尝试继承或实现 `Endian` 是编译时错误。

## 常量

### big

```dart
static const Endian big = Endian._(false);
```

### little

```dart
static const Endian little = Endian._(true);
```

## 静态属性

### host

```dart
Endian host
```

---

# Int8List

```dart
abstract final class Int8List implements _TypedIntList {}
```

8 位有符号整数的固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 8 位，并被解释为取值范围 -128 到 +127 的有符号 8 位二进制补码整数。

类尝试继承或实现 `Int8List` 是编译时错误。

## 构造函数

### Int8List()

```dart
Int8List(int length)
```

创建一个指定长度（以元素为单位）的 [Int8List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 个字节的 [ByteBuffer] 支持。

### Int8List.fromList()

```dart
Int8List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Int8List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 个字节的 [ByteBuffer] 支持。

### Int8List.view()

```dart
Int8List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Int8List] _视图_。

对 [Int8List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Int8List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Int8List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Int8List.sublistView]：

```dart
Int8List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Int8List.sublistView()

```dart
Int8List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Int8List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

## 静态常量

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Int8List asUnmodifiableView()
```

此 [Int8List] 的只读视图。

### sublist()

```dart
Int8List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Int8List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Int8List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int8List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Uint8List

```dart
abstract final class Uint8List implements _TypedIntList {}
```

8 位无符号整数的固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 8 位，并被解释为取值范围 0 到 255 的无符号 8 位整数。

类尝试继承或实现 `Uint8List` 是编译时错误。

## 构造函数

### Uint8List()

```dart
Uint8List(int length)
```

创建一个指定长度（以元素为单位）的 [Uint8List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 个字节的 [ByteBuffer] 支持。

### Uint8List.fromList()

```dart
Uint8List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Uint8List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 个字节的 [ByteBuffer] 支持。

### Uint8List.view()

```dart
Uint8List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Uint8List] _视图_。

对 [Uint8List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Uint8List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Uint8List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Uint8List.sublistView]：

```dart
Uint8List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Uint8List.sublistView()

```dart
Uint8List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Uint8List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Uint8List asUnmodifiableView()
```

此 [Uint8List] 的只读视图。

### sublist()

```dart
Uint8List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Uint8List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Uint8List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint8List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

## 运算符

### operator +

```dart
List<int> operator +(List<int> other)
```

返回此列表与 [other] 的连接结果。

如果 [other] 也是类型化数据列表，则返回的列表将是一个能够同时容纳无符号 8 位整数和 [other] 中元素的类型化数据列表；否则将返回一个普通的整数列表。

---

# Uint8ClampedList

```dart
abstract final class Uint8ClampedList implements _TypedIntList {}
```

8 位无符号整数的固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被限制在无符号 8 位值范围内，即所有低于零的值都存储为零，所有高于 255 的值都存储为 255。

类尝试继承或实现 `Uint8ClampedList` 是编译时错误。

## 构造函数

### Uint8ClampedList()

```dart
Uint8ClampedList(int length)
```

创建一个指定长度（以元素为单位）的 [Uint8ClampedList]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 个字节的 [ByteBuffer] 支持。

### Uint8ClampedList.fromList()

```dart
Uint8ClampedList.fromList(List<int> elements)
```

创建一个与 [elements] 列表大小相同的 [Uint8ClampedList]，并在需要时对值进行限制后复制。

复制值时会根据需要进行限制以适配列表，方式与存储值时的限制方式相同。

该列表由一个恰好包含 `elements.length` 个字节的 [ByteBuffer] 支持。

### Uint8ClampedList.view()

```dart
Uint8ClampedList.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

在指定的字节 [buffer] 中创建指定区域的 [Uint8ClampedList] _视图_。

对 [Uint8List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Uint8ClampedList.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Uint8ClampedList.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Uint8ClampedList.sublistView]：

```dart
Uint8ClampedList.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Uint8ClampedList.sublistView()

```dart
@Since.new("3.3")
Uint8ClampedList.sublistView( TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Uint8ClampedList] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 1;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Uint8ClampedList asUnmodifiableView()
```

此 [Uint8ClampedList] 的只读视图。

### sublist()

```dart
Uint8ClampedList sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Uint8ClampedList`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Uint8ClampedList.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint8ClampedList
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Int16List

```dart
abstract final class Int16List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 16 位有符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 16 位，并被解释为取值范围 -32768 到 +32767 的有符号 16 位二进制补码整数。

类尝试继承或实现 `Int16List` 是编译时错误。

## 构造函数

### Int16List()

```dart
Int16List(int length)
```

创建一个指定长度（以元素为单位）的 [Int16List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 2 个字节的 [ByteBuffer] 支持。

### Int16List.fromList()

```dart
Int16List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Int16List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 2 个字节的 [ByteBuffer] 支持。

### Int16List.view()

```dart
Int16List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Int16List] _视图_。

对 [Int16List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Int16List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Int16List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Int16List.sublistView]：

```dart
Int16List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Int16List.sublistView()

```dart
Int16List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Int16List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 2 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 2;
```

## 方法

### asUnmodifiableView()

```dart
Int16List asUnmodifiableView()
```

此 [Int16List] 的只读视图。

### sublist()

```dart
Int16List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Int16List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Int16List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int16List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Uint16List

```dart
abstract final class Uint16List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 16 位无符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 16 位，并被解释为取值范围 0 到 65535 的无符号 16 位整数。

类尝试继承或实现 `Uint16List` 是编译时错误。

## 构造函数

### Uint16List()

```dart
Uint16List(int length)
```

创建一个指定长度（以元素为单位）的 [Uint16List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 2 个字节的 [ByteBuffer] 支持。

### Uint16List.fromList()

```dart
Uint16List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Uint16List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 2 个字节的 [ByteBuffer] 支持。

### Uint16List.view()

```dart
Uint16List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

在指定的字节缓冲区中创建指定区域的 [Uint16List] _视图_。

对 [Uint16List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Uint16List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Uint16List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Uint16List.sublistView]：

```dart
Uint16List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Uint16List.sublistView()

```dart
Uint16List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Uint16List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 2 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 2;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Uint16List asUnmodifiableView()
```

此 [Uint16List] 的只读视图。

### sublist()

```dart
Uint16List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Uint16List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Uint16List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint16List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Int32List

```dart
abstract final class Int32List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 32 位有符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 32 位，并被解释为取值范围 -2147483648 到 2147483647 的有符号 32 位二进制补码整数。

类尝试继承或实现 `Int32List` 是编译时错误。

## 构造函数

### Int32List()

```dart
Int32List(int length)
```

创建一个指定长度（以元素为单位）的 [Int32List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 4 个字节的 [ByteBuffer] 支持。

### Int32List.fromList()

```dart
Int32List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Int32List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 4 个字节的 [ByteBuffer] 支持。

### Int32List.view()

```dart
Int32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Int32List] _视图_。

对 [Int32List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Int32List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Int32List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Int32List.sublistView]：

```dart
Int32List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Int32List.sublistView()

```dart
Int32List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Int32List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 4 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 4;
```

## 方法

### asUnmodifiableView()

```dart
@Since.new("3.3")
Int32List asUnmodifiableView()
```

此 [Int32List] 的只读视图。

### sublist()

```dart
Int32List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Int32List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Int32List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int32List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Uint32List

```dart
abstract final class Uint32List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 32 位无符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 32 位，并被解释为取值范围 0 到 4294967295 的无符号 32 位整数。

类尝试继承或实现 `Uint32List` 是编译时错误。

## 构造函数

### Uint32List()

```dart
Uint32List(int length)
```

创建一个指定长度（以元素为单位）的 [Uint32List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 4 个字节的 [ByteBuffer] 支持。

### Uint32List.fromList()

```dart
Uint32List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Uint32List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 4 个字节的 [ByteBuffer] 支持。

### Uint32List.view()

```dart
Uint32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

在指定的字节缓冲区中创建指定区域的 [Uint32List] _视图_。

对 [Uint32List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Uint32List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Uint32List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Uint32List.sublistView]：

```dart
Uint32List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Uint32List.sublistView()

```dart
Uint32List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Uint32List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 4 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 4;
```

## 方法

### asUnmodifiableView()

```dart
Uint32List asUnmodifiableView()
```

此 [Uint32List] 的只读视图。

### sublist()

```dart
Uint32List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Uint32List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Uint32List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint32List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Int64List

```dart
abstract final class Int64List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 64 位有符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 64 位，并被解释为取值范围 -9223372036854775808 到 +9223372036854775807 的有符号 64 位二进制补码整数。

类尝试继承或实现 `Int64List` 是编译时错误。

## 构造函数

### Int64List()

```dart
Int64List(int length)
```

创建一个指定长度（以元素为单位）的 [Int64List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 8 个字节的 [ByteBuffer] 支持。

### Int64List.fromList()

```dart
Int64List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Int64List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 8 个字节的 [ByteBuffer] 支持。

### Int64List.view()

```dart
Int64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Int64List] _视图_。

对 [Int64List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Int64List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Int64List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Int64List.sublistView]：

```dart
Int64List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Int64List.sublistView()

```dart
Int64List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Int64List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 8 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 8;
```

## 方法

### asUnmodifiableView()

```dart
Int64List asUnmodifiableView()
```

此 [Int64List] 的只读视图。

### sublist()

```dart
Int64List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Int64List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Int64List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Int64List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Uint64List

```dart
abstract final class Uint64List implements _TypedIntList {}
```

可作为 [TypedData] 查看的 64 位无符号整数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

列表中存储的整数会被截断为其低 64 位，并被解释为取值范围 0 到 18446744073709551615 的无符号 64 位整数。

类尝试继承或实现 `Uint64List` 是编译时错误。

## 构造函数

### Uint64List()

```dart
Uint64List(int length)
```

创建一个指定长度（以元素为单位）的 [Uint64List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 8 个字节的 [ByteBuffer] 支持。

### Uint64List.fromList()

```dart
Uint64List.fromList(List<int> elements)
```

创建一个与 [elements] 列表长度相同的 [Uint64List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 8 个字节的 [ByteBuffer] 支持。

### Uint64List.view()

```dart
Uint64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

在指定的字节缓冲区中创建指定区域的 [Uint64List] _视图_。

对 [Uint64List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Uint64List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Uint64List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Uint64List.sublistView]：

```dart
Uint64List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Uint64List.sublistView()

```dart
Uint64List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Uint64List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 8 的整数倍。

## 静态属性

### bytesPerElement

```dart
static const int bytesPerElement = 8;
```

## 方法

### asUnmodifiableView()

```dart
Uint64List asUnmodifiableView()
```

此 [Uint64List] 的只读视图。

### sublist()

```dart
Uint64List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Uint64List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Uint64List.fromList([0, 1, 2, 3, 4]);
print(numbers.sublist(1, 3)); // [1, 2]
print(numbers.sublist(1, 3).runtimeType); // Uint64List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1, 2, 3, 4]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

---

# Float32List

```dart
abstract final class Float32List implements _TypedFloatList {}
```

可作为 [TypedData] 查看的 IEEE 754 单精度二进制浮点数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

存储在列表中的 double 值会被转换为最接近的单精度值。读取的值会被转换为具有相同值的 double 值。

类尝试继承或实现 `Float32List` 是编译时错误。

## 构造函数

### Float32List()

```dart
Float32List(int length)
```

创建一个指定长度（以元素为单位）的 [Float32List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 4 个字节的 [ByteBuffer] 支持。

### Float32List.fromList()

```dart
Float32List.fromList(List<double> elements)
```

创建一个与 [elements] 列表长度相同的 [Float32List]，并复制其中的元素。

复制值时会将其截断以适配列表，方式与存储值时的截断方式相同。

该列表由一个恰好包含 `elements.length` 乘以 4 个字节的 [ByteBuffer] 支持。

### Float32List.view()

```dart
Float32List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Float32List] _视图_。

对 [Float32List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Float32List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Float32List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Float32List.sublistView]：

```dart
Float32List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Float32List.sublistView()

```dart
Float32List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Float32List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 4 的整数倍。

### asUnmodifiableView()

```dart
Float32List asUnmodifiableView()
```

此 [Float32List] 的只读视图。

### sublist()

```dart
Float32List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Float32List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Float32List.fromList([0.0, 1.0, 2.0, 3.0, 4.0]);
print(numbers.sublist(1, 3)); // [1.0, 2.0]
print(numbers.sublist(1, 3).runtimeType); // Float32List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1.0, 2.0, 3.0, 4.0]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

### bytesPerElement

```dart
int bytesPerElement
```

# Float64List

```dart
abstract final class Float64List implements _TypedFloatList {}
```

可作为 [TypedData] 查看的 IEEE 754 双精度二进制浮点数固定长度列表。

对于长列表，此实现比默认的 [List] 实现在空间和时间上都可能更高效。

类尝试继承或实现 `Float64List` 是编译时错误。

### Float64List()

```dart
Float64List(int length)
```

创建一个指定长度（以元素为单位）的 [Float64List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 8 个字节的 [ByteBuffer] 支持。

### Float64List.fromList()

```dart
Float64List.fromList(List<double> elements)
```

创建一个与 [elements] 列表长度相同的 [Float64List]，并复制其中的元素。

该列表由一个恰好包含 `elements.length` 乘以 8 个字节的 [ByteBuffer] 支持。

### Float64List.view()

```dart
Float64List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Float64List] _视图_。

对 [Float64List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Float64List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Float64List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Float64List.sublistView]：

```dart
Float64List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Float64List.sublistView()

```dart
Float64List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Float64List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 8 的整数倍。

### asUnmodifiableView()

```dart
Float64List asUnmodifiableView()
```

此 [Float64List] 的只读视图。

### sublist()

```dart
Float64List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Float64List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Float64List.fromList([0.0, 1.0, 2.0, 3.0, 4.0]);
print(numbers.sublist(1, 3)); // [1.0, 2.0]
print(numbers.sublist(1, 3).runtimeType); // Float64List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(1)); // [1.0, 2.0, 3.0, 4.0]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

### bytesPerElement

```dart
int bytesPerElement
```

# Float32x4List

```dart
abstract final class Float32x4List implements TypedDataList<Float32x4>, TypedData {}
```

可作为 [TypedData] 查看的 [Float32x4] 数值固定长度列表。

对于长列表，此实现将比默认的 [List] 实现在空间和时间上都更高效。

类尝试继承或实现 `Float32x4List` 是编译时错误。

### Float32x4List()

```dart
Float32x4List(int length)
```

创建一个指定长度（以元素为单位）的 [Float32x4List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 16 个字节的 [ByteBuffer] 支持。

### Float32x4List.fromList()

```dart
Float32x4List.fromList(List<Float32x4> elements)
```

创建一个与 [elements] 列表长度相同的 [Float32x4List]，并复制其中的元素。

该列表由一个恰好包含 `elements.length` 乘以 16 个字节的 [ByteBuffer] 支持。

### Float32x4List.view()

```dart
Float32x4List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Float32x4List] _视图_。

对 [Float32x4List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Float32x4List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Float32x4List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Float32x4List.sublistView]：

```dart
Float32x4List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Float32x4List.sublistView()

```dart
Float32x4List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Float32x4List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 16 的整数倍。

### asUnmodifiableView()

```dart
Float32x4List asUnmodifiableView()
```

此 [Float32x4List] 的只读视图。

### operator +()

```dart
List<Float32x4> operator +(List<Float32x4> other)
```

此列表与 [other] 的连接结果。

如果 [other] 也是 [Float32x4List]，则结果是一个新的 [Float32x4List]；否则结果是一个普通的可增长 `List<Float32x4>`。

### sublist()

```dart
Float32x4List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Float32x4List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Float32x4List.fromList([
  Float32x4(0, 1, 2, 3),
  Float32x4(1, 2, 3, 4),
  Float32x4(2, 3, 4, 5),
  Float32x4(3, 4, 5, 6),
  Float32x4(4, 5, 6, 7),
]);
print(numbers.sublist(1, 2)); // [Float32x4(1, 2, 3, 4)]
print(numbers.sublist(1, 2).runtimeType); // Float32x4List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(4)); // [Float32x4(4, 5, 6, 7)]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

### bytesPerElement

```dart
int bytesPerElement
```

# Int32x4List

```dart
abstract final class Int32x4List implements TypedDataList<Int32x4>, TypedData {}
```

可作为 [TypedData] 查看的 [Int32x4] 数值固定长度列表。

对于长列表，此实现将比默认的 [List] 实现在空间和时间上都更高效。

### Int32x4List()

```dart
Int32x4List(int length)
```

创建一个指定长度（以元素为单位）的 [Int32x4List]，其所有元素初始值均为零。

该列表由一个恰好包含 [length] 乘以 16 个字节的 [ByteBuffer] 支持。

### Int32x4List.fromList()

```dart
Int32x4List.fromList(List<Int32x4> elements)
```

创建一个与 [elements] 列表长度相同的 [Int32x4List]，并复制其中的元素。

该列表由一个恰好包含 `elements.length` 乘以 16 个字节的 [ByteBuffer] 支持。

### Int32x4List.view()

```dart
Int32x4List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Int32x4List] _视图_。

对 [Int32x4List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Int32x4List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Int32x4List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Int32x4List.sublistView]：

```dart
Int32x4List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Int32x4List.sublistView()

```dart
Int32x4List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Int32x4List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 16 的整数倍。

### asUnmodifiableView()

```dart
Int32x4List asUnmodifiableView()
```

此 [Int32x4List] 的只读视图。

### operator +()

```dart
List<Int32x4> operator +(List<Int32x4> other)
```

此列表与 [other] 的连接结果。

如果 [other] 也是 [Int32x4List]，则结果是一个新的 [Int32x4List]；否则结果是一个普通的可增长 `List<Int32x4>`。

### sublist()

```dart
Int32x4List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Int32x4List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Int32x4List.fromList([
  Int32x4(0, 1, 2, 3),
  Int32x4(1, 2, 3, 4),
  Int32x4(2, 3, 4, 5),
  Int32x4(3, 4, 5, 6),
  Int32x4(4, 5, 6, 7),
]);
print(numbers.sublist(1, 2)); // [Int32x4(1, 2, 3, 4)]
print(numbers.sublist(1, 2).runtimeType); // Int32x4List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(3)); // [Int32x4(3, 4, 5, 6), Int32x4(4, 5, 6, 7)]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

### bytesPerElement

```dart
int bytesPerElement
```

# Float64x2List

```dart
abstract final class Float64x2List implements TypedDataList<Float64x2>, TypedData {}
```

可作为 [TypedData] 查看的 [Float64x2] 数值固定长度列表。

对于长列表，此实现将比默认的 [List] 实现在空间和时间上都更高效。

类尝试继承或实现 `Float64x2List` 是编译时错误。

### Float64x2List()

```dart
Float64x2List(int length)
```

创建一个指定长度（以元素为单位）的 [Float64x2List]，其所有元素的所有分量（lane）均设为零。

该列表由一个恰好包含 [length] 乘以 16 个字节的 [ByteBuffer] 支持。

### Float64x2List.fromList()

```dart
Float64x2List.fromList(List<Float64x2> elements)
```

创建一个与 [elements] 列表长度相同的 [Float64x2List]，并复制其中的元素。

该列表由一个恰好包含 `elements.length` 乘以 16 个字节的 [ByteBuffer] 支持。

### operator +()

```dart
List<Float64x2> operator +(List<Float64x2> other)
```

此列表与 [other] 的连接结果。

如果 [other] 也是 [Float64x2List]，则结果是一个新的 [Float64x2List]；否则结果是一个普通的可增长 `List<Float64x2>`。

### Float64x2List.view()

```dart
Float64x2List.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [Float64x2List] _视图_。

对 [Float64x2List] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供 length，则视图会延伸至字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + ([length] \* [bytesPerElement]) 必须小于或等于 [buffer] 的长度。

[offsetInBytes] 必须是 [bytesPerElement] 的整数倍。

请注意，从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是对更大缓冲区的视图，其 [TypedData.offsetInBytes] 大于零。仅执行 `Float64x2List.view(other.buffer, 0, count)` 可能无法指向你期望的字节位置。因此你可能需要执行：

```dart
Float64x2List.view(other.buffer, other.offsetInBytes, count)
```

或者，使用包含此计算的 [Float64x2List.sublistView]：

```dart
Float64x2List.sublistView(other, 0, count);
```

（第三个参数是结束索引而非长度，因此如果起始位置大于零，也无需相应地减少 count）。

### Float64x2List.sublistView()

```dart
Float64x2List.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的某个元素范围上创建一个 [Float64x2List] 视图。

在 `data.buffer` 上创建一个视图，该视图对应于 [data] 中从 [start] 到 [end] 的元素所在的字节范围。如果 [data] 是类型化数据列表（如 [Uint16List]），则该视图基于索引从 [start] 到 [end] 的元素所占的字节。如果 [data] 是 [ByteData]，则将其视为字节列表处理。

若提供了 [start] 和 [end]，二者必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，与类型化数据列表的 [List.length] 相同。

如果省略，则 [start] 默认为零，[end] 默认为 _elementCount_。

所查看字节范围的起始和结束索引必须是 16 的整数倍。

### asUnmodifiableView()

```dart
Float64x2List asUnmodifiableView()
```

此 [Float64x2List] 的只读视图。

### sublist()

```dart
Float64x2List sublist(int start, [int? end])
```

创建一个包含 [start] 和 [end] 之间元素的新列表。

新列表是一个 `Float64x2List`，按此列表中出现的顺序，包含位置大于等于 [start] 且小于 [end] 的元素。

```dart
var numbers = Float64x2List.fromList([
  Float64x2(0, 1),
  Float64x2(1, 2),
  Float64x2(2, 3),
  Float64x2(3, 4),
  Float64x2(4, 5),
]);
print(numbers.sublist(1, 3)); // [Float64x2(1, 2), Float64x2(2, 3)]
print(numbers.sublist(1, 3).runtimeType); // Float64x2List
```

如果省略 [end]，则默认为此列表的 [length]。

```dart
print(numbers.sublist(3)); // [Float64x2(3, 4), Float64x2(4, 5)]
```

`start` 和 `end` 的位置必须满足关系 0 ≤ `start` ≤ `end` ≤ `this.length`。如果 `end` 等于 `start`，则返回的列表为空。

### bytesPerElement

```dart
int bytesPerElement
```
