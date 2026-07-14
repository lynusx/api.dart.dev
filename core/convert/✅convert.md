用于在不同数据表示形式之间进行转换的编码器和解码器，包括 JSON 和 UTF-8。

除了用于常见数据表示形式的转换器外，该库还支持以易于链式调用、并可与流配合使用的方式实现转换器。

在代码中使用该库：

```dart
import 'dart:convert';
```

两个常用的转换器是 [JsonCodec](https://www.yuque.com/thyname/dart.convert/jsoncodec) 和 [Utf8Codec](https://www.yuque.com/thyname/dart.convert/utf8codec) 的顶层实例，分别名为 [json](https://www.yuque.com/thyname/dart.convert/json) 和 [utf8](https://www.yuque.com/thyname/dart.convert/utf8)。

### JSON

JSON 是一种用于表示结构化对象和集合的简单文本格式。

[JsonCodec](https://www.yuque.com/thyname/dart.convert/jsoncodec) 将 JSON 对象编码为字符串，并将字符串解码为 JSON 对象。[json](https://www.yuque.com/thyname/dart.convert/json) 编码器/解码器使用 JSON 格式在字符串与对象结构（例如列表和映射）之间进行转换。

[json](https://www.yuque.com/thyname/dart.convert/json) 是 [JsonCodec](https://www.yuque.com/thyname/dart.convert/jsoncodec) 的默认实现。

示例

```dart
var encoded = json.encode([1, 2, { "a": null }]);
var decoded = json.decode('["foo", { "bar": 499 }]');
```

更多信息请参见 [JsonEncoder](https://www.yuque.com/thyname/dart.convert/jsonencoder) 和 [JsonDecoder](https://www.yuque.com/thyname/dart.convert/jsondecoder)。

### UTF-8

[Utf8Codec](https://www.yuque.com/thyname/dart.convert/utf8codec) 将字符串编码为 UTF-8 编码单元（字节），并将 UTF-8 编码单元解码为字符串。

[utf8](https://www.yuque.com/thyname/dart.convert/utf8) 是 [Utf8Codec](https://www.yuque.com/thyname/dart.convert/utf8codec) 的默认实现。

示例：

```dart
var encoded = utf8.encode('Îñţérñåţîöñåļîžåţîờñ');
var decoded = utf8.decode([
  195, 142, 195, 177, 197, 163, 195, 169, 114, 195, 177, 195, 165, 197,
  163, 195, 174, 195, 182, 195, 177, 195, 165, 196, 188, 195, 174, 197,
  190, 195, 165, 197, 163, 195, 174, 225, 187, 157, 195, 177]);
```

更多信息请参见 [Utf8Encoder](https://www.yuque.com/thyname/dart.convert/utf8encoder) 和 [Utf8Decoder](https://www.yuque.com/thyname/dart.convert/utf8decoder)。

### ASCII

[AsciiCodec](https://www.yuque.com/thyname/dart.convert/asciicodec) 将字符串编码为以字节形式存储的 ASCII 编码，并将 ASCII 字节解码为字符串。并非所有字符都可以用 ASCII 表示，因此并非所有字符串都能成功转换。

[ascii](https://www.yuque.com/thyname/dart.convert/ascii) 是 [AsciiCodec](https://www.yuque.com/thyname/dart.convert/asciicodec) 的默认实现。

示例：

```dart
var encoded = ascii.encode('This is ASCII!');
var decoded = ascii.decode([0x54, 0x68, 0x69, 0x73, 0x20, 0x69, 0x73,
                            0x20, 0x41, 0x53, 0x43, 0x49, 0x49, 0x21]);
```

更多信息请参见 [AsciiEncoder](https://www.yuque.com/thyname/dart.convert/asciiencoder) 和 [AsciiDecoder](https://www.yuque.com/thyname/dart.convert/asciidecoder)。

### Latin-1

[Latin1Codec](https://www.yuque.com/thyname/dart.convert/latin1codec) 将字符串编码为 ISO Latin-1（即 ISO-8859-1）字节，并将 Latin-1 字节解码为字符串。并非所有字符都可以用 Latin-1 表示，因此并非所有字符串都能成功转换。

[latin1](https://www.yuque.com/thyname/dart.convert/latin1) 是 [Latin1Codec](https://www.yuque.com/thyname/dart.convert/latin1codec) 的默认实现。

示例：

```dart
var encoded = latin1.encode('blåbærgrød');
var decoded = latin1.decode([0x62, 0x6c, 0xe5, 0x62, 0xe6,
                             0x72, 0x67, 0x72, 0xf8, 0x64]);
```

更多信息请参见 [Latin1Encoder](https://www.yuque.com/thyname/dart.convert/latin1encoder) 和 [Latin1Decoder](https://www.yuque.com/thyname/dart.convert/latin1decoder)。

### Base64

[Base64Codec](https://www.yuque.com/thyname/dart.convert/base64codec) 使用默认的 base64 字母表进行编码，使用 base64 和 base64url 两种字母表进行解码，不允许无效字符，并且要求填充。

[base64](https://www.yuque.com/thyname/dart.convert/base64) 是 [Base64Codec](https://www.yuque.com/thyname/dart.convert/base64codec) 的默认实现。

示例：

```dart
var encoded = base64.encode([0x62, 0x6c, 0xc3, 0xa5, 0x62, 0xc3, 0xa6,
                             0x72, 0x67, 0x72, 0xc3, 0xb8, 0x64]);
var decoded = base64.decode('YmzDpWLDpnJncsO4ZAo=');
```

更多信息请参见 [Base64Encoder](https://www.yuque.com/thyname/dart.convert/base64encoder) 和 [Base64Decoder](https://www.yuque.com/thyname/dart.convert/base64decoder)。

### 转换器（Converters）

转换器常与流一起使用，以便在数据可用时对流中经过的数据进行转换。以下代码使用了两个转换器。第一个是 UTF-8 解码器，用于在从文件读取数据时将数据从字节转换为 UTF-8；第二个是 [LineSplitter](https://www.yuque.com/thyname/dart.convert/linesplitter) 的实例，用于按换行符对数据进行分割。

```dart import:io
const showLineNumbers = true;
var lineNumber = 1;
var stream = File('quotes.txt').openRead();

stream.transform(utf8.decoder)
      .transform(const LineSplitter())
      .forEach((line) {
        if (showLineNumbers) {
          stdout.write('${lineNumber++} ');
        }
        stdout.writeln(line);
      });
```

有关创建自定义转换器的信息，请参见 [Codec](https://www.yuque.com/thyname/dart.ui/codec) 和 [Converter](https://www.yuque.com/thyname/dart.convert/converter) 类的文档。

### HTML 转义

[HtmlEscape] 转换器用于转义在 HTML 中具有特殊含义的字符。该转换器会找出在 HTML 源码中具有特殊意义的字符，并将其替换为相应的 HTML 实体。

可以使用 [HtmlEscapeMode.new] 构造函数创建自定义转义模式。

示例：

```dart
const htmlEscapeMode = HtmlEscapeMode(
  name: 'custom',
  escapeLtGt: true,
  escapeQuot: false,
  escapeApos: false,
  escapeSlash: false,
 );

const HtmlEscape htmlEscape = HtmlEscape(htmlEscapeMode);
String unescaped = 'Text & subject';
String escaped = htmlEscape.convert(unescaped);
print(escaped); // Text &amp; subject

unescaped = '10 > 1 and 1 < 10';
escaped = htmlEscape.convert(unescaped);
print(escaped); // 10 &gt; 1 and 1 &lt; 10

unescaped = "Single-quoted: 'text'";
escaped = htmlEscape.convert(unescaped);
print(escaped); // Single-quoted: 'text'

unescaped = 'Double-quoted: "text"';
escaped = htmlEscape.convert(unescaped);
print(escaped); // Double-quoted: "text"

unescaped = 'Path: /system/';
escaped = htmlEscape.convert(unescaped);
print(escaped); // Path: /system/
```
