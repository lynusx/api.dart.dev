# Record

```dart
abstract final class Record {}
```

一个记录（record）值。

`Record` 类是所有*记录类型*的超类型，但它本身不是任何对象实例的运行时类型*（它是一个抽象类）*。所有实现 `Record` 的对象，其运行时类型都是某个记录类型。

一个记录值由记录类型描述，由若干*字段*组成，每个字段可以是位置字段或命名字段。

记录值和记录类型的写法与参数列表及简化的函数类型参数列表类似（不允许也不需要 `required` 修饰符，因为记录字段永远不是可选的）。示例：

```dart
(int, String, {bool isValid}) triple = (1, "one", isValid: true);
```

这在语法上类似于

```dart
typedef F = void Function(int, String, {bool isValid});
void callIt(F f) => f(1, "one", isValid: true);
```

每个记录及记录类型都有一个*形状*（shape），由位置字段的数量和命名字段的名称决定。例如：

```dart continued
(double value, String name, {String isValid}) another = (
     3.14, "Pi", isValid: "real");
```

这是另一个具有相同*形状*（两个位置字段，一个名为 `isValid` 的命名字段）但类型不同的记录声明。写在位置字段上的名称完全是为了文档说明目的，它们对程序没有任何影响*（这与函数类型中位置参数上的名称相同，例如 `typedef F = int Function(int value);` 中的标识符 `value` 没有任何作用）*。

记录值主要通过模式（pattern）进行解构，例如：

```dart continued
switch (triple) {
  case (int value, String name, isValid: bool ok): // ....
}
```

各个字段也可以使用命名获取器（getter）访问，位置字段使用 `$1`、`$2` 等，命名字段则使用字段名本身。

```dart continued
int value = triple.$1;
String name = triple.$2;
bool ok = triple.isValid;
```

因此，以下标识符不能用作命名字段的名称：

- [Object] 成员的名称：`hashCode`、`runtimeType`、`toString` 和 `noSuchMethod`。
- 同一记录中位置获取器的名称，因此 `(0, $1: 0)` 是无效的，但 `(0, $2: 0)` 是有效的，因为在*该*记录形状中不存在带有获取器 `$2` 的位置字段。_（这仍然会造成混淆，实践中应避免。）_
- 此外，不允许使用以下划线 `_` 开头的名称。字段名称不能是库私有的。

记录对象的运行时类型是一个记录类型，因此是 [Record] 的子类型，并传递性地成为 [Object] 及其超类型的子类型。

记录值不具有持久的 [identical] 行为。对一个记录对象的引用*随时*可能变为对另一个具有相同形状和字段值的记录对象的引用。

除此之外，一个记录类型只能是另一个具有相同形状的记录类型的子类型，且仅当前一个记录类型的字段类型是另一个记录类型对应字段类型的子类型时才成立。也就是说，`(int, String, {bool isValid})` 是 `(num, String, {Object isValid})` 的子类型，因为它们具有相同的形状，且字段类型逐一对应地是子类型关系。不同形状的记录类型彼此无关。

## 属性

### runtimeType

```dart
Type get runtimeType
```

表示记录运行时类型的 `Type` 对象。

记录的运行时类型由该记录的*形状*（位置字段的数量和命名字段的名称）以及每个字段的运行时类型定义。（记录的运行时类型不依赖于其字段值的 `runtimeType` 获取器，该获取器可能已被重写覆盖 [Object.runtimeType]。）

一个记录类型的 `Type` 对象仅当与另一个记录类型的 `Type` 对象具有相同形状，且对应字段具有相同类型时，两者才相等。

### hashCode

```dart
int get hashCode
```

与 `==` 兼容的哈希码。

由于 [operator==] 是根据记录字段值的 `==` 运算符定义的，因此哈希码也是根据字段值的 [Object.hashCode] 计算得出的。

访问字段值 `hashCode` 的顺序没有保证。这些值如何组合是未指定的，但在单次程序运行中会保持一致。

## 方法

### toString()

```dart
String toString()
```

创建记录的字符串表示形式。

该字符串表示形式仅用于调试，在开发模式和生产模式下可能有所不同。生产模式下没有保证的格式。

在开发模式下，该字符串将尽量表示为一个带括号、以逗号分隔的字段表示列表，其中位置字段的表示为该值的 `toString`，命名字段的表示则为 `someName:` 后跟该值的 `toString`（`someName` 为字段名）。

## 运算符

### operator ==

```dart
bool operator ==(Object other)
```

检查 [other] 是否与此记录具有相同的形状且字段相等。

一条记录仅当与另一条具有相同*形状*的记录进行比较，且所有字段的值都根据其 `==` 与 [other] 中对应字段的值相等时，才与该记录相等。

字段值相等性的检查顺序没有保证，是否在发现不相等的对应字段后继续检查其余字段也是未指定的。甚至不保证该顺序在单次程序运行中保持一致。

与往常一样，对于那些破坏了相等性约定的对象（例如不等于自身的 [double.nan]）需格外小心。例如

```dart
var pair = ("a", double.nan);
if (pair != pair) print("Oops");
```

将打印 "Oops"，因为 `pair == pair` 的定义等价于 `"a" == "a" & double.nan == double.nan`，其结果为 false。
