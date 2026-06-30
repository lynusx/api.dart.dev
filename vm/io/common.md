# IOException

```dart
abstract class IOException implements Exception {}
```

Base class for all IO related exceptions.

### toString()

```dart
String toString()
```

# OSError

```dart
class OSError implements Exception {}
```

An [Exception] holding information about an error from the operating system.

### noErrorCode

```dart
int noErrorCode
```

Constant used to indicate that no OS error code is available.

### message

```dart
String message
```

Error message supplied by the operating system. This will be empty if no message is associated with the error.

### errorCode

```dart
int errorCode
```

Error code supplied by the operating system.

Will have the value [OSError.noErrorCode] if there is no error code associated with the error.

### OSError()

```dart
OSError([String message = "", int errorCode = noErrorCode])
```

Creates an OSError object from a message and an errorCode.

### toString()

```dart
String toString()
```

Converts an OSError object to a string representation.
