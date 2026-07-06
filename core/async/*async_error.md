# AsyncError

```dart
final class AsyncError implements Error {}
```

一个错误及其堆栈跟踪。

用于将错误和堆栈跟踪作为单个值处理，例如由 [Zone.errorCallback] 返回时。

## 构造函数

### AsyncError()

```dart
AsyncError(Object error, StackTrace? stackTrace)
```

## 静态方法

### defaultStackTrace()

```dart
static StackTrace defaultStackTrace(Object error)
```

错误的默认堆栈跟踪。

如果 [error] 是一个 [Error] 且具有 [Error.stackTrace]，则返回该堆栈跟踪。否则，返回默认的 [StackTrace.empty]。
