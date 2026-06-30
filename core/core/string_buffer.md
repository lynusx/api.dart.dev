# StringBuffer

```dart
class StringBuffer implements StringSink {}
```

A class for concatenating strings efficiently.

Allows for the incremental building of a string using `write*()` methods. The strings are concatenated to a single string only when [toString] is called.

Example:

```dart
final buffer = StringBuffer('DART');
print(buffer.length); // 4
```

To add the string representation of an object, as returned by [Object.toString], to the buffer, use [write]. Is also used for adding a string directly.

```
buffer.write(' is open source');
print(buffer.length); // 19
print(buffer); // DART is open source

const int dartYear = 2011;
buffer
  ..write(' since ') // Writes a string.
  ..write(dartYear); // Writes an int.
print(buffer); // DART is open source since 2011
print(buffer.length); // 30
```

To add a newline after the object's string representation, use [writeln]. Calling [writeln] with no argument adds a single newline to the buffer.

```
buffer.writeln(); // Contains "DART is open source since 2011\n".
buffer.writeln('-' * (buffer.length - 1)); // 30 '-'s and a newline.
print(buffer.length); // 62
```

To write multiple objects to the buffer, use [writeAll].

```
const separator = '-';
buffer.writeAll(['Dart', 'is', 'fun!'], separator);
print(buffer.length); // 74
print(buffer);
// DART is open source since 2011
// ------------------------------
// Dart-is-fun!
```

To add the string representation of a Unicode code point, `charCode`, to the buffer, use [writeCharCode].

```
buffer.writeCharCode(0x0A); // LF (line feed)
buffer.writeCharCode(0x44); // 'D'
buffer.writeCharCode(0x61); // 'a'
buffer.writeCharCode(0x72); // 'r'
buffer.writeCharCode(0x74); // 't'
print(buffer.length); // 79
```

To convert the content to a single string, use [toString].

```
final text = buffer.toString();
print(text);
// DART is open source since 2011
// ------------------------------
// Dart-is-fun!
// Dart
```

To clear the buffer, so that it can be reused, use [clear].

```
buffer.clear();
print(buffer.isEmpty); // true
print(buffer.length); // 0
```

### StringBuffer()

```dart
StringBuffer([Object content = ""])
```

Creates a string buffer containing the provided [content].

### length

```dart
int get length
```

Returns the length of the content that has been accumulated so far. This is a constant-time operation.

### isEmpty

```dart
bool get isEmpty
```

Returns whether the buffer is empty. This is a constant-time operation.

### isNotEmpty

```dart
bool get isNotEmpty
```

Returns whether the buffer is not empty. This is a constant-time operation.

### write()

```dart
void write(Object? object)
```

### writeCharCode()

```dart
void writeCharCode(int charCode)
```

### writeAll()

```dart
void writeAll(Iterable<dynamic> objects, [String separator = ""])
```

### writeln()

```dart
void writeln([Object? obj = ""])
```

Writes the string representation of [object] followed by a newline.

Equivalent to `buffer.write(object)` followed by `buffer.write("\n")`.

The newline is always represented as `"\n"`, and does not use a platform specific line ending, e.g., `"\r\n"` on Windows.

Notice that calling `buffer.writeln(null)` will write the `"null"` string before the newline. Omitting the argument, or explicitly passing an empty string, is the recommended way to emit just the newline.

### clear()

```dart
void clear()
```

Clears the string buffer.

### toString()

```dart
String toString()
```

Returns the contents of buffer as a single string.
