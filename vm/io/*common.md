# IOException

```dart
abstract class IOException implements Exception {}
```

所有 IO 相关异常的基类。

### toString()

```dart
String toString()
```

# OSError

```dart
class OSError implements Exception {}
```

一个 [Exception](https://www.yuque.com/thyname/dart.core/exception)，用于持有来自操作系统的错误信息。

### noErrorCode

```dart
int noErrorCode
```

用于表示没有可用操作系统错误码的常量。

### message

```dart
String message
```

操作系统提供的错误消息。若该错误没有关联的消息，则为空字符串。

### errorCode

```dart
int errorCode
```

操作系统提供的错误码。

若该错误没有关联的错误码，则其值为 [OSError.noErrorCode]。

### OSError()

```dart
OSError([String message = "", int errorCode = noErrorCode])
```

根据消息和错误码创建一个 OSError 对象。

### toString()

```dart
String toString()
```

将 OSError 对象转换为字符串表示形式。
