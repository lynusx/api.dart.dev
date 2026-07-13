# Deprecated

```dart
class Deprecated {}
```

标注 `@Deprecated('migration')` 将某个功能标记为已弃用。

标注 [deprecated](https://www.yuque.com/thyname/dart.core/deprecated-) 是弃用某功能的简写形式，弃用后没有指定"下一版本"的具体时间，也没有迁移说明。

一个功能可以是 API 的任何部分，从整个库到单个参数。

`@Deprecated` 标注的目的是告知当前正在使用该功能的开发者，即使该功能目前仍能正常工作，他们也应尽快停止在代码中使用该功能。

弃用是一种早期警告，表明被弃用的功能计划在稍后的时间被移除，该时间可能在 `message` 中指定。已弃用的功能不应再被使用，使用该功能的代码将在未来的某个时刻出错。如果现有代码使用了该功能，应重写该代码以不再使用被弃用的功能。

已弃用的功能应在 `message` 中说明如何实现相同的效果，以便开发者知道如何重写代码。

`@Deprecated` 标注适用于库、顶层声明（变量、获取器、设置器、函数、类、混入、扩展和类型定义）、类级别声明（无论是否为静态的变量、获取器、设置器、方法、运算符或构造函数）、命名可选参数以及尾部可选位置参数。

弃用会传递性地应用于被弃用功能的各个部分：

- 如果一个库被弃用，那么它的每个成员也都被弃用。
- 如果一个类被弃用，那么它的每个成员也都被弃用。
- 如果一个变量被弃用，那么它隐式的获取器和设置器也被弃用。

如果某个功能在父类中被弃用，它不会在子类中自动被弃用。将某个成员从父类中移除但保留在子类中是合理的做法，因此需要能够仅在父类中弃用该成员。

处理 Dart 源代码的工具可能会在以下情况报告：

- 代码导入了一个已弃用的库。
- 代码导出了一个已弃用的库，或非弃用库中的某个已弃用成员。
- 代码静态引用了一个已弃用的声明。
- 代码使用了某个静态类型已知对象的成员，且该成员在其静态类型的接口上已被弃用。
- 代码调用某方法时传入的参数对应于对象静态类型上已弃用的可选参数。

如果被弃用功能的使用发生在库、类或方法本身也已被弃用的情况下，工具不应就此提示用户。已弃用的功能预期会使用其他已弃用的功能。

## 构造函数

### Deprecated()

```dart
const Deprecated( String? message )
```

创建一个弃用标注，用于指定被标注功能的迁移路径和到期时间。

`message` 将作为警告的一部分显示。该消息应面向使用被标注功能的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.implement()

```dart
Deprecated.implement([String? message])
```

创建一个用于弃用实现某个类或混入的标注。

该标注可用于当前接口可被实现的 `class` 或 `mixin` 声明（即未标记为 `final`、`sealed` 或 `base`），但该实现能力将在后续版本中被移除。

任何 `implements` 该接口的现有类、混入或枚举声明都会收到警告，提示此类用法已被弃用。不影响 `extends` 或混入（mix in）该被标注类或混入的类（参见 [Deprecated.extend]、[Deprecated.mixin] 和 [Deprecated.subclass]）。

该标注不会被子类继承。如果某个公共子类也将变得不可实现——当被标注声明变为 `final` 或 `base`（但不是 `sealed`）时会发生这种情况——则该子类也应弃用实现能力。

`message`（如果提供）将作为警告的一部分显示。该消息应面向拥有实现类的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.extend()

```dart
const Deprecated.extend([ String? message ])
```

创建一个用于弃用继承某个类的标注。

该标注可用于当前可被继承的 `class` 声明（即未标记为 `final`、`sealed` 或 `interface`），但该继承能力将在后续版本中被移除。

任何 `extends` 该类的现有类声明都会收到警告，提示此类用法已被弃用。不影响实现或混入该被标注类的类（参见 [Deprecated.implement]、[Deprecated.mixin] 和 [Deprecated.subclass]）。

该标注不会被子类继承。如果某个公共子类也将变得不可继承——当被标注声明变为 `final` 或 `interface`（但不是 `sealed`）时会发生这种情况——则该子类也应弃用可继承性。

`message`（如果提供）将作为警告的一部分显示。该消息应面向拥有继承类的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.subclass()

```dart
const Deprecated.subclass([ String? message ])
```

创建一个用于弃用子类化（实现或继承）某个类的标注。

该标注可用于当前可被子类化的 `class` 和 `mixin` 声明（即未标记为 `final` 或 `sealed`），但该子类化能力将在后续版本中被移除。

任何 `extends` 或 `implements` 该被标注类或混入的现有类声明都会收到警告，提示此类用法已被弃用。不影响混入该被标注类的类（参见 [Deprecated.extend]、[Deprecated.implement] 和 [Deprecated.mixin]）。

该标注不会被子类继承。如果某个公共子类也将变得不可子类化——当被标注声明变为 `final`（但不是 `sealed`）时会发生这种情况——则该子类也应弃用子类化能力。

`message`（如果提供）将作为警告的一部分显示。该消息应面向拥有子类化类的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.instantiate()

```dart
const Deprecated.instantiate([ String? message ])
```

创建一个用于弃用实例化某个类的标注。

该标注可用于当前可被实例化的 `class` 声明（即未标记为 `abstract` 或 `sealed`），但该实例化能力将在后续版本中被移除。

任何实例化被标注类的现有代码都会收到警告，提示此类用法已被弃用。

`message`（如果提供）将作为警告的一部分显示。该消息应面向拥有实例化代码的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.mixin()

```dart
const Deprecated.mixin([ String? message ])
```

创建一个用于弃用混入某个类的标注。

该标注可用于当前可被混入的 `class` 声明（即标记为 `mixin`），但该混入能力将在后续版本中被移除。

任何混入该被标注类的现有类声明都会收到警告，提示此类用法已被弃用。不影响继承或实现该被标注类的类（参见 [Deprecated.extend]、[Deprecated.implement] 和 [Deprecated.subclass]）。

`message`（如果提供）将作为警告的一部分显示。该消息应面向拥有混入该被标注类的类的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

### Deprecated.optional()

```dart
const Deprecated.optional([ String? message ])
```

创建一个用于弃用省略被标注参数实参的标注。

该标注可用于方法、构造函数或顶层函数的可选参数，表明该参数将在后续版本中变为必需参数。

任何未为被标注参数传值的函数调用都会收到警告，提示此类省略已被弃用。

该标注不会在方法重写中被继承。

`message`（如果提供）将作为警告的一部分显示。该消息应面向调用带有被标注参数的函数的开发者，应推荐替代方案（如果有的话），并说明该功能预计何时被移除（如果早于或晚于下一个主版本）。

## 属性

### message

```dart
String? message
```

在用户使用已弃用功能时提供给用户的消息。

该消息应说明如果有替代方案，如何迁移离开该功能，以及该被弃用功能预计何时被移除。

## 方法

### toString()

```dart
String toString()
```

该对象的字符串表示形式。

某些类具有默认的文本表示形式，通常与静态的 `parse` 函数（如 [int.parse](https://api.dart.dev/dart-core/int/parse.html)）成对出现。这些类会将文本表示形式作为其字符串表示形式返回。

其他类没有程序关心的有意义文本表示形式。此类类通常会重写 `toString`，以便在检查对象时提供有用信息，主要用于调试或日志记录。

---

# deprecated

```dart
Deprecated const deprecated
```

将某个功能标记为 [Deprecated](https://www.yuque.com/thyname/dart.core/deprecated)，直到下一版本发布。

---

# override

```dart
Object const override
```

用于重写接口成员的实例成员上的标注。

标注不会影响 Dart 程序的语义。该标注被 Dart 分析器识别，使分析器能够针对某些原本有效的程序中可能存在的问题提供提示或警告。因此，该标注的含义由 Dart 分析器定义。

`@override` 标注表达了某个声明"应当"重写某个接口方法的意图，而这一点从声明本身是无法看出的。这一额外信息使分析器能够在该意图未被满足时发出警告——即某个成员本意是重写父类成员或实现接口成员，但实际未能做到。如果成员名称拼写错误，或父类重命名了该成员，就可能出现这种情况。

`@override` 标注适用于实例方法、实例获取器、实例设置器和实例变量（字段）。当应用于实例变量时，表示该变量隐式的获取器和设置器（如果有）被标记为重写。它对变量本身没有影响。

可以使用更多的 [lint 规则](https://dart.dev/lints) 来基于 `@override` 标注启用更多警告。

---

# pragma

```dart
final class pragma {}
```

给工具的提示。

处理 Dart 程序的工具可以接受以 `pragma` 标注形式添加在声明上的提示，用于指导其行为。每个工具决定接受哪些提示、这些提示的含义，以及它们是否以及如何应用于被标注实体的子部分。

识别 pragma 提示的工具应选择一个 pragma 前缀来标识该工具。它们应将任何 `name` 以其前缀加 `:` 开头的提示识别为面向该工具的提示。带有其他工具前缀的提示应被忽略（除非与该其他工具兼容是一个目标）。

如果该工具在自身前缀前加上未加前缀的名称时也能识别该名称，那么工具也可以识别未加前缀的名称。

如果提示可以参数化，还可以添加一个额外的 `options` 对象。

例如：

```dart template:top
@pragma('Tool:pragma-name', [param1, param2, ...])
class Foo { }

@pragma('OtherTool:other-pragma')
void foo() { }
```

这里，类 `Foo` 被标注了工具 Tool 特定的 pragma 'pragma-name'，函数 `foo` 被标注了 OtherTool 特定的 pragma 'other-pragma'。

## 构造函数

### pragma()

```dart
pragma(String name, [Object? options])
```

创建一个名为 `name`、带有可选 `options` 的提示。

## 属性

### name

```dart
String name
```

该提示的名称。

一个被一个或多个工具识别的字符串，或者是在该字符串前加上工具标识符和冒号后得到的字符串，后者仅被该特定工具识别。

### options

```dart
Object? options
```

用于参数化该提示的可选附加数据。
