# Exception

```dart
abstract interface class Exception {}
```

A marker interface implemented by all core library exceptions.

An [Exception] is intended to convey information to the user about a failure, so that the error can be addressed programmatically. It is intended to be caught, and it should contain useful data fields.

Creating instances of [Exception] directly with `Exception("message")` is discouraged in library code since it doesn't give users a precise type they can catch. It may be reasonable to use instances of this class in tests or during development.

For failures that are not intended to be caught, use [Error] and its subclasses.

### Exception()

```dart
Exception([dynamic message])
```

# FormatException

```dart
class FormatException implements Exception {}
```

Exception thrown when a string or some other data does not have an expected format and cannot be parsed or processed.

### message

```dart
String message
```

A message describing the format error.

### source

```dart
dynamic source
```

The actual source input which caused the error.

This is usually a [String], but can be other types too. If it is a string, parts of it may be included in the [toString] message.

The source is `null` if omitted or unknown.

### offset

```dart
int? offset
```

The offset in [source] where the error was detected.

A zero-based offset into the source that marks the format error causing this exception to be created. If `source` is a string, this should be a string index in the range `0 <= offset <= source.length`.

If input is a string, the [toString] method may represent this offset as a line and character position. The offset should be inside the string, or at the end of the string.

May be omitted. If present, [source] should also be present if possible.

### FormatException()

```dart
FormatException([String message = "", dynamic source, int? offset])
```

Creates a new `FormatException` with an optional error [message].

Optionally also supply the actual [source] with the incorrect format, and the [offset] in the format where a problem was detected.

### toString()

```dart
String toString()
```

Returns a description of the format exception.

The description always contains the [message].

If [source] is present and is a string, the description will contain (at least a part of) the source. If [offset] is also provided, the part of the source included will contain that offset, and the offset will be marked.

If the source is a string and it contains a line break before offset, only the line containing offset will be included, and its line number will also be part of the description. Line and character offsets are 1-based.

# IntegerDivisionByZeroException

```dart
class IntegerDivisionByZeroException implements Exception, UnsupportedError {}
```

### message

```dart
String? get message
```

### stackTrace

```dart
StackTrace? get stackTrace
```

### IntegerDivisionByZeroException()

```dart
IntegerDivisionByZeroException()
```

### toString()

```dart
String toString()
```
