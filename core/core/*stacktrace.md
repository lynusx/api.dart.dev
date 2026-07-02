# StackTrace

```dart
abstract interface class StackTrace {}
```

所有堆栈跟踪对象都实现的接口。

[StackTrace] 用于向用户传达触发异常的调用序列信息。

这些对象由运行时创建，无法以编程方式创建它们。

## 构造函数

### StackTrace.fromString()

```dart
StackTrace.fromString(String stackTraceString)
```

根据 [stackTraceString] 创建一个 `StackTrace` 对象。

创建的堆栈跟踪对象的 `toString` 方法将返回 `stackTraceString`。

`stackTraceString` 可以是其他堆栈跟踪返回的字符串，也可以是任意字符串。如果该字符串看起来不像堆栈跟踪，解析堆栈跟踪的代码很可能会失败，因此应谨慎使用伪造的堆栈跟踪。

## 静态属性

### empty

```dart
_StringStackTrace const empty
```

一个不包含任何信息的堆栈跟踪对象。

在需要堆栈跟踪但用户未提供的情况下，该堆栈跟踪对象用作默认值。

### current

```dart
StackTrace get current
```

返回当前堆栈跟踪的表示形式。

这与执行以下操作的效果类似：

```dart
try { throw 0; } catch (_, stack) { return stack; }
```

该获取器会在可能的情况下不抛出异常地实现这一效果。

## 方法

### toString()

```dart
String toString()
```

返回该堆栈跟踪的 [String] 表示形式。

该字符串表示从抛出异常的位置到当前调用序列顶部的完整堆栈跟踪。

其字符串表示的确切格式尚未最终确定。
