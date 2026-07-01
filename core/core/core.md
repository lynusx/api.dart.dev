为每个 Dart 程序提供内置类型、集合和其他核心功能。

此库会自动导入。

此库中的部分类（如 [String] 和 [num]）支持 Dart 的内置数据类型。另一些类（如 [List] 和 [Map]）则提供用于管理对象集合的数据结构。还有一些类表示常用的数据类型，例如 URI、日期和时间以及错误。

## 数字与布尔值

[int] 和 [double] 分别支持 Dart 内置的数值数据类型：整数和双精度浮点数。[bool] 类型的对象值为 true 或 false。这些类型的变量可以通过字面量构造：

```dart
int meaningOfLife = 42;
double valueOfPi  = 3.141592;
bool visible      = true;
```

## 字符串与正则表达式

[String] 是不可变的，表示一个字符序列。

```dart
String shakespeareQuote = "All the world's a stage, ...";
```

[StringBuffer] 提供了一种高效构造字符串的方式。

```dart
var moreShakespeare = StringBuffer();
moreShakespeare.write('And all the men and women ');
moreShakespeare.write('merely players; ...');
```

[String] 和 [StringBuffer] 类实现了字符串拆分、拼接以及其他字符串操作功能。

```dart
bool isPalindrome(String text) => text == text.split('').reversed.join();
```

[RegExp] 实现了 Dart 正则表达式，它提供了一套用于在文本中匹配模式的语法。例如，以下正则表达式匹配包含一个或多个数字的子字符串：

```dart
var numbers = RegExp(r'\d+');
```

Dart 正则表达式与 JavaScript 正则表达式具有相同的语法和语义。有关 JavaScript 正则表达式的规范，请参阅 <http://ecma-international.org/ecma-262/5.1/#sec-15.10>。

## 集合

`dart:core` 库提供了基本的集合类型，例如 [List]、[Map] 和 [Set]。

[List] 是具有长度的有序对象集合，有时也称为数组。当需要通过索引访问对象时，应使用 [List]。

```dart
var superheroes = ['Batman', 'Superman', 'Harry Potter'];
```

[Set] 是唯一对象的无序集合，无法通过索引（位置）高效获取元素。向集合中添加已存在的元素不会产生任何效果。

```dart
var villains = {'Joker'};
print(villains.length); // 1
villains.addAll(['Joker', 'Lex Luthor', 'Voldemort']);
print(villains.length); // 3
```

[Map] 是键值对的无序集合，其中每个键只能出现一次。Map 有时也称为关联数组，因为它将键与值关联起来以便于检索。当需要通过唯一标识符访问对象时，应使用 [Map]。

```dart
var sidekicks = {'Batman': 'Robin',
                 'Superman': 'Lois Lane',
                 'Harry Potter': 'Ron and Hermione'};
```

除了这些类之外，`dart:core` 还包含 [Iterable]，这是一个定义了对象集合常用功能的接口。例如，对集合中的每个元素执行函数、对每个元素应用测试、检索对象以及确定元素数量等能力。

[List] 和 [Set] 都实现了 [Iterable]，[Map] 也将其用于其键和值。

如需了解其他类型的集合，请参阅 `dart:collection` 库。

## 日期与时间

使用 [DateTime] 表示时间点，使用 [Duration] 表示时间跨度。

可以通过构造函数创建 [DateTime] 对象，也可以通过解析格式正确的字符串来创建。

```dart
var now = DateTime.now();
var berlinWallFell = DateTime(1989, 11, 9);
var moonLanding = DateTime.parse("1969-07-20");
```

通过指定各个时间单位来创建 [Duration] 对象。

```dart
var timeRemaining = const Duration(hours: 56, minutes: 14);
```

除 [DateTime] 和 [Duration] 外，`dart:core` 还包含用于测量经过时间的 [Stopwatch] 类。

## Uri

[Uri] 对象表示统一资源标识符（uniform resource identifier），用于标识一个资源，例如 Web 上的资源。

```dart
var dartlang = Uri.parse('http://dartlang.org/');
```

## 错误

[Error] 类表示运行时发生的错误。该类的子类表示特定类型的错误。

## 其他文档

有关如何使用内置类型的更多信息，请参阅 [内置类型](https://dart.dev/language/built-in-types)。

此外，请查看 [`dart:core` 简介](https://dart.dev/libraries/dart-core)，以了解此库中类型的更多内容。

[Dart 语言规范](https://dart.dev/resources/language/spec) 提供了技术细节。
