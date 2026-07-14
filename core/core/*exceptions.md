# Exception

```dart
abstract interface class Exception {}
```

所有核心库异常都实现的标记接口。

[Exception](https://www.yuque.com/thyname/dart.core/exception) 旨在向用户传达有关失败的信息，以便能够以编程方式处理该错误。它旨在被捕获，并应包含有用的数据字段。

不建议在库代码中直接使用 `Exception("message")` 创建实例，因为这不会为用户提供可捕获的精确类型。在测试或开发过程中使用此类的实例可能是合理的。

对于不打算被捕获的失败，请使用 [Error](https://www.yuque.com/thyname/dart.core/error) 及其子类。

## 构造函数

### Exception()

```dart
Exception([dynamic message])
```

# FormatException

```dart
class FormatException implements Exception {}
```

当字符串或其他数据不具有预期格式，因而无法被解析或处理时抛出的异常。

## 构造函数

### FormatException()

```dart
const FormatException([ String message = "", dynamic source, int? offset ])
```

创建一个新的 `FormatException`，可附带可选的错误 [message]。

也可以选择提供格式有误的实际 [source]，以及检测到问题所在的 [offset]。

## 属性

### message

```dart
String message
```

描述格式错误的消息。

### source

```dart
dynamic source
```

导致该错误的实际源输入。

通常是 [String](https://www.yuque.com/thyname/dart.core/string)，但也可以是其他类型。如果是字符串，其部分内容可能会包含在 [toString] 的消息中。

如果省略或未知，source 为 `null`。

### offset

```dart
int? offset
```

在 [source] 中检测到错误的偏移量。

这是一个从零开始的偏移量，标记了导致创建此异常的格式错误位置。如果 `source` 是字符串，该值应为处于 `0 <= offset <= source.length` 范围内的字符串索引。

如果输入是字符串，[toString] 方法可能会将此偏移量表示为行号和字符位置。该偏移量应位于字符串内部，或位于字符串末尾。

可以省略。如果提供了该值，[source] 也应尽可能一并提供。

## 方法

### toString()

```dart
String toString()
```

返回该格式异常的描述。

该描述始终包含 [message]。

如果提供了 [source] 且它是字符串，描述中将包含源内容（至少一部分）。如果同时提供了 [offset]，所包含的源内容部分将包含该偏移量，并会标注出该偏移位置。

如果源是字符串，且在偏移量之前包含换行符，则描述中只会包含偏移量所在的那一行，并同时给出行号。行号和字符偏移量均从 1 开始。
