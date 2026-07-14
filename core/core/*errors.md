# Error

```dart
class Error {}
```

程序失败时抛出的错误对象。

`Error` 对象表示程序员本应避免的程序失败。

例如：使用无效参数调用函数，或使用错误数量的参数调用函数，或在不允许调用的时机调用它。

调用者不应该预期或捕获这些错误——如果它们发生，则说明程序出现了错误，终止程序可能是最安全的应对方式。

在决定某个函数是否应该抛出错误时，应清楚地描述发生该错误的条件，且这些条件应当是可检测和可预测的，这样使用该函数的程序员就能够避免触发该错误。

这类描述通常使用"必须"（must）或"不得"（must not）等词语来描述条件，如果你在函数文档中看到这样的词语，那么不满足该要求很可能会导致抛出错误。

示例（来自 [String.contains]）：

```plaintext
`startIndex` must not be negative or greater than `length`.
```

在这种情况下，如果 `startIndex` 为负数或过大，则会抛出错误。

如果这些条件在调用函数之前无法被检测到，那么被调用的函数就不应该抛出 `Error`。它仍然可能抛出，但调用者将不得不捕获抛出的值，这实际上使其成为一种替代结果而非错误。在这种情况下，我们认为抛出的对象是一个*异常*（exception）而非错误（error）。抛出的对象可以选择实现 [Exception](https://www.yuque.com/thyname/dart.core/exception)，以表明它表示的是一种异常但非错误的情况，但实现 [Exception](https://www.yuque.com/thyname/dart.core/exception) 除了具有文档说明作用外，没有其他效果。

在 Dart 中，所有非 `null` 的值都可以被抛出。_继承_（extending）自 `Error` 类的对象会被特殊处理：首次被抛出时，抛出点的堆栈跟踪会被记录并存储在该错误对象中，可以使用 [stackTrace] 获取器来获取它。仅仅*实现*（implements）`Error` 而不继承它的错误对象，不会自动存储堆栈跟踪。

错误对象也用于表示系统级的失败，例如栈溢出或内存不足的情况，这些情况同样不应由用户捕获或处理。

由于错误对象并非为了被捕获而创建，因此子类之间无需进行区分。相反，创建子类是为了便于创建一组具有一致错误消息的相关错误。例如，[String.contains] 方法在其 `startIndex` 不在 `0..length` 范围内时会使用 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)，该错误可以通过 `RangeError.range(startIndex, 0, length)` 轻松创建。捕获 [Error](https://www.yuque.com/thyname/dart.core/error) 的特定子类并非其设计初衷，除了在测试自己的代码时，不应在其他场景中这样做。

## 构造函数

### Error()

```dart
Error()
```

## 静态方法

### safeToString()

```dart
String safeToString(Object? object)
```

安全地将某个值转换为 [String](https://www.yuque.com/thyname/dart.core/string) 描述。

该转换保证不会抛出异常，因此除了特定的已知可信类型外，它不会调用对象自身的 toString 方法。

### throwWithStackTrace()

```dart
@Since.new("2.16")
Never throwWithStackTrace( Object error, StackTrace stackTrace )
```

抛出 `error`，并关联堆栈跟踪 `stackTrace`。

其行为类似于：在抛出时刻，若 [StackTrace.current](https://api.dart.dev/dart-core/StackTrace/current.html) 恰好为 `stackTrace`，此时执行 `throw error`。

与普通的 `throw` 一样，如果 `error` 继承自 [Error](https://www.yuque.com/thyname/dart.core/error) 且此前尚未被抛出过，则其 [Error.stackTrace](https://api.dart.dev/dart-core/Error/stackTrace.html) 属性将被设置为 `stackTrace`。

此函数不保证保留 `stackTrace` 的对象标识（identity）。被 `try`/`catch` 捕获到的该错误的 [StackTrace](https://www.yuque.com/thyname/dart.core/stacktrace) 对象，或被设置为该 `error` 的 [Error.stackTrace](https://api.dart.dev/dart-core/Error/stackTrace.html) 的对象，可能与作为参数提供的 `stackTrace` 对象并非同一对象，但根据 [StackTrace.toString](https://api.dart.dev/dart-core/StackTrace/toString.html)，二者内容相同。

## 方法

### stackTrace

```dart
StackTrace? get stackTrace
```

该错误首次被抛出时的堆栈跟踪。

*继承*自 `Error` 的类，在首次被 `throw` 表达式抛出时，会自动填充堆栈跟踪。

---

# AssertionError

```dart
class AssertionError extends Error {}
```

当 assert 语句失败时，由运行时系统抛出的错误。

## 构造函数

### AssertionError()

```dart
AssertionError([Object? message])
```

使用提供的 [message] 创建一个断言错误。

## 属性

### message

```dart
Object? message
```

描述该断言错误的消息。

---

# TypeError

```dart
class TypeError extends Error {}
```

当发生动态类型错误时，由运行时系统抛出的错误。

---

# ArgumentError

```dart
class ArgumentError extends Error {}
```

当函数接收到不可接受的参数时抛出的错误。

方法应当为其所接受的参数记录限制条件，例如：某个整数参数是否必须为非空（non-nullable）、某个字符串参数是否必须非空（non-empty），或者某个 `dynamic` 类型的参数是否必须实际属于几种可接受类型之一。

用户应当能够预测哪些参数会导致抛出错误，从而避免使用这些参数进行调用。

在错误中附带提供不可接受的值几乎总是一个好主意，这有助于用户找出问题所在，因此推荐使用 [ArgumentError.value] 构造函数。仅当由于某种原因无法提供该值时，才使用 [ArgumentError.new]。

## 构造函数

### ArgumentError()

```dart
ArgumentError([ dynamic message, @Since.new("2.14") String? name ])
```

创建一个错误，并使用 [message] 描述参数存在的问题。

现有代码中，`message` 可能被用来保存无效值。如果 `message` 不是 [String](https://www.yuque.com/thyname/dart.core/string) 类型，则会被视为一个值而非消息文本。

如果提供了 [name]，它应为接收到无效参数的形参名称。

推荐改用 [ArgumentError.value]，以便同时保留并记录该无效值。

### ArgumentError.value()

```dart
ArgumentError.value(dynamic value, [String? name, dynamic message])
```

创建一个包含无效值 [value] 的错误。

消息由 [message] 参数拼接 [name] 参数（如果提供）以及该值构建而成。例如：

```plaintext
Invalid argument (foo): null
```

`name` 应与函数的参数名一致，但如果该函数是某接口的实现方法，且其参数名与接口方法的参数名不同，那么使用接口方法中的参数名可能更为有用（或者直接重命名参数以使其保持一致）。

### ArgumentError.notNull()

```dart
ArgumentError.notNull([String? name])
```

为一个不得为 `null` 但实际为 `null` 的参数创建一个参数错误。

## 属性

### invalidValue

```dart
dynamic invalidValue
```

无效值。

### name

```dart
String? name
```

无效参数的名称（如果可用）。

### message

```dart
dynamic message
```

描述该问题的消息。

## 静态方法

### checkNotNull()

```dart
T checkNotNull<T>(T? argument, [String? name])
```

如果 [argument] 为 `null`，则抛出异常。

如果提供了 [name]，则会在错误消息中用作参数名称。

如果 [argument] 不为 null，则返回该参数。

---

# RangeError

```dart
class RangeError extends ArgumentError {}
```

当参数值超出可接受范围时抛出的错误。

## 构造函数

### RangeError()

```dart
RangeError(dynamic message)
```

使用给定的 [message] 创建一个新的 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)。

### RangeError.value()

```dart
RangeError.value(num value, [String? name, String? message])
```

针对给定的 [value]，创建一个带消息的新 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)。

可选的 [name] 可以指定具有无效值的参数名称，[message] 可以覆盖默认的错误描述。

### RangeError.range()

```dart
RangeError.range(
  num invalidValue,
  int? minValue,
  int? maxValue, [
  String? name,
  String? message,
])
```

针对超出有效范围的某个值，创建一个新的 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)。

允许的范围为从 [minValue] 到 [maxValue]（含边界值）。如果 `minValue` 或 `maxValue` 为 `null`，则该方向上的范围视为无限。

对于从 0 到某对象长度（末端不含）的范围，请使用 [RangeError.index]。

可选的 [name] 可以指定具有无效值的参数名称，[message] 可以覆盖默认的错误描述。

### RangeError.index()

```dart
RangeError.index(
  int index,
  dynamic indexable, [
  String? name,
  String? message,
  int? length,
])
```

创建一个新的 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)，说明 [index] 不是 [indexable] 的有效索引。

可选的 [name] 可以指定具有无效值的参数名称，[message] 可以覆盖默认的错误描述。

[length] 是发生错误时 [indexable] 的长度。如果省略 `length`，则默认为 `indexable.length`。

## 静态方法

### checkValueInInterval()

```dart
int checkValueInInterval(
  int value,
  int minValue,
  int maxValue, [
  String? name,
  String? message,
])
```

检查整数 [value] 是否位于指定区间内。

如果 [value] 不在该区间内，则抛出异常。该区间为从 [minValue] 到 [maxValue]（均含边界值）。

如果提供了 [name] 或 [message]，它们将分别用作所抛出错误的参数名称和消息文本。

如果 [value] 在该区间内，则返回该值。

### checkValidIndex()

```dart
int checkValidIndex(
  int index,
  dynamic indexable, [
  String? name,
  int? length,
  String? message,
])
```

检查 [index] 是否为某个可索引对象的有效索引。

如果 [index] 不是 [indexable] 的有效索引，则抛出异常。

可索引（indexable）对象是指具有 `length` 属性，并且具有索引运算符 `[]`（当 `0 <= index < length` 时接受该索引）的对象。

如果提供了 [name] 或 [message]，它们将分别用作所抛出错误的参数名称和消息文本。如果省略 [name]，则默认为 `"index"`。

如果提供了 [length]，则使用它作为可索引对象的长度，否则该长度通过 `indexable.length` 求得。

如果 [index] 是有效索引，则返回该索引。

### checkValidRange()

```dart
int checkValidRange(
  int start,
  int? end,
  int length, [
  String? startName,
  String? endName,
  String? message,
])
```

检查某个范围是否表示某个可索引对象的一个切片。

如果该范围对于长度为给定 [length] 的可索引对象无效，则抛出异常。对于长度为 [length] 的可索引对象，一个范围有效的条件是 `0 <= [start] <= [end] <= [length]`。`end` 为 `null` 时视为等同于 `length`。

[startName] 和 [endName] 分别默认为 `"start"` 和 `"end"`。

返回实际的 `end` 值：如果 `end` 为 `null`，则返回 `length`；否则返回 `end` 本身。

### checkNotNegative()

```dart
int checkNotNegative(int value, [String? name, String? message])
```

检查某个整数值是否为非负数。

如果该值为负数，则抛出异常。

如果提供了 [name] 或 [message]，它们将分别用作所抛出错误的参数名称和消息文本。如果省略 [name]，则默认为 `index`。

如果 [value] 不为负数，则返回该值。

## 属性

### start

```dart
num? start
```

[value] 允许取的最小值。

### end

```dart
num? end
```

[value] 允许取的最大值。

### invalidValue

```dart
num? get invalidValue
```

无效值。

---

# IndexError

```dart
class IndexError extends ArgumentError implements RangeError {}
```

一种特殊的 [RangeError](https://www.yuque.com/thyname/dart.core/rangeerror)，用于索引不在 `0..indexable.length-1` 范围内的情况。

同时包含该可索引对象、发生错误时该对象的长度，以及无效索引本身。

## 构造函数

### IndexError.withLength()

```dart
@Since.new("2.19")
  IndexError.withLength(
  int invalidValue,
  int length, {
  Object? indexable,
  String? name,
  String? message,
})
```

创建一个新的 [IndexError](https://www.yuque.com/thyname/dart.core/indexerror)，说明 [invalidValue] 不是 [indexable] 的有效索引。

[length] 是发生错误时 [indexable] 的长度。

该消息将作为该错误字符串表示的一部分使用。

## 静态方法

### check()

```dart
@Since.new("2.19")
  int check(
  int index,
  int length, {
  Object? indexable,
  String? name,
  String? message,
})
```

检查 [index] 是否为某个可索引对象的有效索引。

如果 [index] 不是有效索引，则抛出异常。

可索引（indexable）对象是指具有 `length` 属性，并且具有索引运算符 `[]`（当 `0 <= index < length` 时接受该索引）的对象。

[length] 是该可索引对象的长度。

[indexable]（如果提供）即为该可索引对象。

[name] 是索引值对应的参数名称，默认值为 "index"；如果该无效索引并非一个参数，可以将其设置为 null，以便在错误字符串中省略名称。

[message]（如果提供）将包含在错误字符串中。

如果 [index] 是有效索引，则返回该索引。

## 属性

### indexable

```dart
Object? indexable
```

`invalidValue` 不是其有效索引的那个可索引对象。

例如可以是 [List](https://www.yuque.com/thyname/dart.core/list) 或 [String](https://www.yuque.com/thyname/dart.core/string)，二者都具有基于索引的操作。

### length

```dart
int length
```

发生错误时 [indexable] 的长度。

### invalidValue

```dart
int get invalidValue
```

### start

```dart
int get start => 0;
```

### end

```dart
int get end => length - 1;
```

---

# NoSuchMethodError

```dart
class NoSuchMethodError extends Error {}
```

当函数或方法调用无效时抛出的错误。

当动态函数或方法调用向被调用的函数提供了无效的类型参数或参数列表时抛出。对于非动态调用，静态类型检查会阻止此类无效参数的出现。

也由 [Object.noSuchMethod] 的默认实现抛出。

## 构造函数

### NoSuchMethodError.withInvocation()

```dart
NoSuchMethodError.withInvocation(Object? receiver, Invocation invocation)
```

创建一个与失败的方法调用相对应的 [NoSuchMethodError](https://www.yuque.com/thyname/dart.core/nosuchmethoderror)。

[receiver] 是该方法调用的接收者，即尝试调用该方法所针对的对象。

[invocation] 表示该失败的方法调用，其值不应为 `null`。

---

# UnsupportedError

```dart
class UnsupportedError extends Error {}
```

该对象不允许执行此操作。

当某个实例无法实现其签名中的某个方法时，会抛出此 [Error](https://www.yuque.com/thyname/dart.core/error)。例如，它被用于集合的不可修改（unmodifiable）版本，当有人调用某个修改性方法时。

## 构造函数

### UnsupportedError()

```dart
UnsupportedError(String? message)
```

---

# UnimplementedError

```dart
class UnimplementedError extends Error implements UnsupportedError {}
```

由尚未实现的操作抛出。

此 [Error](https://www.yuque.com/thyname/dart.core/error) 由尚未完成、尚未实现其所需全部功能的代码抛出。

如果某个类并不打算实现该功能，则应改为抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。此错误仅用于开发阶段。

## 构造函数

### UnimplementedError()

```dart
UnimplementedError([String? message])
```

## 属性

### message

```dart
String? message
```

---

# StateError

```dart
class StateError extends Error {}
```

对象的当前状态不允许执行该操作。

当某个特定对象当前处于不支持所请求操作的状态，但其他类似对象可能支持该操作，或者该对象本身之后可以切换到支持该操作的状态时，应使用此错误。

示例：对当前为空的列表调用 `list.first`。如果该操作从未被该对象或该类支持，请考虑改用 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

这是一种通用错误，用于表示多种不同的错误操作。消息内容应当具有描述性。

## 构造函数

### StateError()

```dart
StateError(String message)
```

## 属性

### message

```dart
String message
```

---

# ConcurrentModificationError

```dart
class ConcurrentModificationError extends Error {}
```

当集合在迭代过程中被修改时发生的错误。

对于某些集合，某些修改操作在迭代期间可能是被允许的，因此每个集合（[Iterable](https://www.yuque.com/thyname/dart.core/iterable) 或类似的值集合）都应声明在迭代期间允许哪些操作。

## 构造函数

### ConcurrentModificationError()

```dart
ConcurrentModificationError([Object? modifiedObject])
```

## 属性

### modifiedObject

```dart
Object? modifiedObject
```

以不兼容方式被修改的对象。

---

# OutOfMemoryError

```dart
final class OutOfMemoryError implements Error {}
```

平台可在内存不足的情况下使用的错误。

## 构造函数

### OutOfMemoryError()

```dart
OutOfMemoryError()
```

### stackTrace

```dart
StackTrace? get stackTrace
```

---

# StackOverflowError

```dart
final class StackOverflowError implements Error {}
```

平台可在栈溢出的情况下使用的错误。

## 构造函数

### StackOverflowError()

```dart
StackOverflowError()
```

### stackTrace

```dart
StackTrace? get stackTrace
```
