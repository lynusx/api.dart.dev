# AsyncError

```dart
final class AsyncError implements Error {}
```

An error and a stack trace.

Used when an error and stack trace need to be handled as a single value, for example when returned by [Zone.errorCallback].

### error

```dart
Object error
```

### stackTrace

```dart
StackTrace stackTrace
```

### AsyncError()

```dart
AsyncError(Object error, StackTrace? stackTrace)
```

### defaultStackTrace()

```dart
StackTrace defaultStackTrace(Object error)
```

A default stack trace for an error.

If [error] is an [Error] and it has an [Error.stackTrace], that stack trace is returned. If not, the [StackTrace.empty] default stack trace is returned.

### toString()

```dart
String toString()
```
