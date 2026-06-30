# StringSink

```dart
abstract interface class StringSink {}
```

### write()

```dart
void write(Object? object)
```

Writes the string representation of [object].

Converts [object] to a string using `object.toString()`.

Notice that calling `sink.write(null)` will will write the `"null"` string.

### writeAll()

```dart
void writeAll(Iterable<dynamic> objects, [String separator = ""])
```

Writes the elements of [objects] separated by [separator].

Writes the string representation of every element of [objects], in iteration order, and writes [separator] between any two elements.

```dart
sink.writeAll(["Hello", "World"], " Beautiful ");
```

is equivalent to:

```dart
sink
  ..write("Hello");
  ..write(" Beautiful ");
  ..write("World");
```

### writeln()

```dart
void writeln([Object? object = ""])
```

Writes the string representation of [object] followed by a newline.

Equivalent to `buffer.write(object)` followed by `buffer.write("\n")`.

Notice that calling `buffer.writeln(null)` will write the `"null"` string before the newline. Omitting the argument, or explicitly passing an empty string, is the recommended way to emit just the newline.

### writeCharCode()

```dart
void writeCharCode(int charCode)
```

Writes a string containing the character with code point [charCode].

Equivalent to `write(String.fromCharCode(charCode))`.
