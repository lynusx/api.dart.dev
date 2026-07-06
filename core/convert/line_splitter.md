# LineSplitter

```dart
final class LineSplitter extends StreamTransformerBase<String, String> {}
```

一个将 [String] 拆分为独立行的 [StreamTransformer]。

一行以以下之一结束：

- CR，回车符：U+000D（'\r'）
- LF，换行符（Unix 换行）：U+000A（'\n'）或
- CR+LF 序列（DOS/Windows 换行），以及
- 最后一个非空行可以由输入的结尾结束。

结果行中不包含行终止符。

示例：

```dart
const splitter = LineSplitter();
const sampleText =
    'Dart is: \r an object-oriented \n class-based \n garbage-collected '
    '\r\n language with C-style syntax \r\n';

final sampleTextLines = splitter.convert(sampleText);
for (var i = 0; i < sampleTextLines.length; i++) {
  print('$i: ${sampleTextLines[i]}');
}
// 0: Dart is:
// 1:  an object-oriented
// 2:  class-based
// 3:  garbage-collected
// 4:  language with C-style syntax
```

## 构造函数

### LineSplitter()

```dart
const LineSplitter()
```

## 静态方法

### split()

```dart
Iterable<String> split(String lines, [int start = 0, int? end])
```

将 [lines] 拆分为独立的行。

如果提供了 [start] 和 [end]，则仅拆分 `lines.substring(start, end)` 的内容。[start] 和 [end] 值必须指定 [lines] 的有效子范围（`0 <= start <= end <= lines.length`）。

### convert()

```dart
List<String> convert(String data)
```

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<String> sink)
```

### bind()

```dart
Stream<String> bind(Stream<String> stream)
```
