# StringBuffer

```dart
class StringBuffer implements StringSink {}
```

一个用于高效拼接字符串的类。

允许使用 `write*()` 方法增量构建字符串。只有在调用 [toString] 时，这些字符串才会被拼接成一个单一的字符串。

示例：

```dart
final buffer = StringBuffer('DART');
print(buffer.length); // 4
```

要将对象的字符串表示形式（由 [Object.toString] 返回）添加到缓冲区，请使用 [write]。也可用于直接添加字符串。

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

要在对象的字符串表示形式后添加换行符，请使用 [writeln]。不带参数调用 [writeln] 会向缓冲区添加一个换行符。

```
buffer.writeln(); // Contains "DART is open source since 2011\n".
buffer.writeln('-' * (buffer.length - 1)); // 30 '-'s and a newline.
print(buffer.length); // 62
```

要将多个对象写入缓冲区，请使用 [writeAll]。

```
const separator = '-';
buffer.writeAll(['Dart', 'is', 'fun!'], separator);
print(buffer.length); // 74
print(buffer);
// DART is open source since 2011
// ------------------------------
// Dart-is-fun!
```

要将 Unicode 码位 `charCode` 的字符串表示形式添加到缓冲区，请使用 [writeCharCode]。

```
buffer.writeCharCode(0x0A); // LF (line feed)
buffer.writeCharCode(0x44); // 'D'
buffer.writeCharCode(0x61); // 'a'
buffer.writeCharCode(0x72); // 'r'
buffer.writeCharCode(0x74); // 't'
print(buffer.length); // 79
```

要将内容转换为单个字符串，请使用 [toString]。

```
final text = buffer.toString();
print(text);
// DART is open source since 2011
// ------------------------------
// Dart-is-fun!
// Dart
```

要清空缓冲区以便重复使用，请使用 [clear]。

```
buffer.clear();
print(buffer.isEmpty); // true
print(buffer.length); // 0
```

## 构造函数

### StringBuffer()

```dart
StringBuffer([Object content = ""])
```

创建一个包含所提供的 [content] 的字符串缓冲区。

## 属性

### length

```dart
int get length
```

返回目前已累积内容的长度。这是一个常数时间操作。

### isEmpty

```dart
bool get isEmpty
```

返回缓冲区是否为空。这是一个常数时间操作。

### isNotEmpty

```dart
bool get isNotEmpty
```

返回缓冲区是否不为空。这是一个常数时间操作。

## 方法

### write()

```dart
void write(Object? object)
```

写入 `object` 的字符串表示形式。

使用 `object.toString()` 将 `object` 转换为字符串。

请注意，调用 `sink.write(null)` 将写入字符串 `"null"`。

### writeCharCode()

```dart
void writeCharCode(int charCode)
```

写入一个包含码位为 `charCode` 的字符的字符串。

等效于 `write(String.fromCharCode(charCode))`。

### writeAll()

```dart
void writeAll( Iterable objects, [ String separator = "" ])
```

写入 `objects` 的元素，并以 `separator` 分隔。

按迭代顺序写入 `objects` 中每个元素的字符串表示形式，并在任意两个元素之间写入 `separator`。

```dart
sink.writeAll(["Hello", "World"], " Beautiful ");
```

等效于：

```dart
sink
  ..write("Hello");
  ..write(" Beautiful ");
  ..write("World");
```

### writeln()

```dart
void writeln([Object? obj = ""])
```

写入 [object] 的字符串表示形式，随后写入一个换行符。

等效于先调用 `buffer.write(object)`，再调用 `buffer.write("\n")`。

换行符始终表示为 `"\n"`，不会使用特定平台的行结束符（例如 Windows 上的 `"\r\n"`）。

请注意，调用 `buffer.writeln(null)` 会在换行符之前写入字符串 `"null"`。推荐的做法是省略参数，或显式传入空字符串，以仅输出换行符。

### clear()

```dart
void clear()
```

清空字符串缓冲区。

### toString()

```dart
String toString()
```

将缓冲区的内容作为单个字符串返回。
