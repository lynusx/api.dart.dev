# Enum

```dart
abstract interface class Enum {}
```

一个枚举值。

该类由所有使用 `enum` 声明引入的类型和值实现。非平台类不能扩展或混入此类。具体类不能实现该接口。

用于命名 `enum` 值的标识符可通过 `enum` 值上的 [EnumName.name] 扩展属性作为 [String](https://www.yuque.com/thyname/dart.core/string) 获取。

## 静态方法

### compareByIndex()

```dart
@Since.new("2.15")
int compareByIndex<T extends Enum>( T value1, T value2 )
```

按 [index] 比较两个枚举值。

这是一个用于枚举类型的通用 [Comparator](https://www.yuque.com/thyname/dart.core/comparator) 函数，它根据枚举值的 [index] 值对枚举值进行排序，该值对应于 `enum` 声明中枚举元素声明的源码顺序。

示例：

```dart
enum Season { spring, summer, autumn, winter }

void main() {
  var relationByIndex =
      Enum.compareByIndex(Season.spring, Season.summer); // < 0
  relationByIndex =
      Enum.compareByIndex(Season.summer, Season.spring); // > 0
  relationByIndex =
      Enum.compareByIndex(Season.spring, Season.winter); // < 0
  relationByIndex =
      Enum.compareByIndex(Season.winter, Season.spring); // > 0
}
```

### compareByName()

```dart
@Since.new("2.15")
int compareByName<T extends Enum>( T value1, T value2 )
```

按名称比较枚举值。

枚举值的 [EnumName.name] 是表示该枚举值声明时所用源码名称的字符串。

此 [Comparator](https://www.yuque.com/thyname/dart.core/comparator) 通过比较两个枚举值的名称来进行比较，可用于按名称对枚举值进行排序。比较使用 [String.compareTo]，因此区分大小写。

示例：

```dart
enum Season { spring, summer, autumn, winter }

void main() {
  var seasons = [...Season.values]..sort(Enum.compareByName);
  print(seasons);
  // [Season.autumn, Season.spring, Season.summer, Season.winter]
}
```

## 属性

### index

```dart
int get index
```

枚举值的数字标识符。

单个枚举中的值从零开始连续编号，直到比值的数量少 1 的数字。这也是该值在枚举类型的静态 `values` 列表中的索引。

# EnumName

```dart
extension EnumName on Enum {}
```

访问枚举值名称的方式。

之所以将该方法声明为扩展方法而非实例方法，是为了允许枚举值拥有名为 `name` 的值。

### name

```dart
String get name
```

枚举值的名称。

该名称是包含用于声明该枚举值的源码标识符的字符串。

例如，给定如下声明：

```dart
enum MyEnum {
  value1,
  value2
}
```

`MyEnum.value1.name` 的结果为字符串 `"value1"`。

# EnumByName

```dart
extension EnumByName<T extends Enum> on Iterable<T> {}
```

按名称访问枚举值。

这是枚举值集合上的扩展，用于枚举类型的 `values` 列表，允许通过名称查找值。

由于枚举类通常较小，[byName] 的查找是通过线性遍历所有值并比较其名称与所提供名称来实现的。如果需要更高效的查找（例如查找操作非常频繁），可以考虑改用映射：

```dart
static myEnumNameMap = MyEnum.values.asNameMap();
```

然后使用该映射进行查找。

### byName()

```dart
T byName(String name)
```

在此列表中查找名称为 [name] 的枚举值。

遍历该集合，查找 [EnumName.name] 所报告的名称为 [name] 的枚举。返回具有给定名称的第一个值。必须能找到这样的值。

### asNameMap()

```dart
Map<String, T> asNameMap()
```

创建一个从枚举值名称到值本身的映射。

调用此方法的集合应包含名称各不相同的枚举，例如枚举类的 `values` 列表。每个名称在创建的映射中只能对应一个值，因此如果两个或多个枚举值具有相同的名称（无论是同一个值，还是不同枚举类型的值），返回的映射中最多只会包含其中一个。
