# StringSink

```dart
abstract interface class StringSink {}
```

## 方法

### write()

```dart
void write(Object? object)
```

写入 [object] 的字符串表示形式。

使用 `object.toString()` 将 [object] 转换为字符串。

注意，调用 `sink.write(null)` 将写入字符串 `"null"`。

### writeAll()

```dart
void writeAll( Iterable objects, [ String separator = "" ])
```

写入 [objects] 的所有元素，元素之间以 [separator] 分隔。

按迭代顺序写入 [objects] 中每个元素的字符串表示形式，并在任意两个元素之间写入 [separator]。

```dart
sink.writeAll(["Hello", "World"], " Beautiful ");
```

等价于：

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

写入 [object] 的字符串表示形式，随后写入一个换行符。

等价于先调用 `buffer.write(object)`，再调用 `buffer.write("\n")`。

注意，调用 `buffer.writeln(null)` 会在换行符之前写入字符串 `"null"`。若只想输出换行符，推荐的方式是省略参数，或显式传入空字符串。

### writeCharCode()

```dart
void writeCharCode(int charCode)
```

写入一个包含码点为 [charCode] 的字符的字符串。

等价于 `write(String.fromCharCode(charCode))`。
