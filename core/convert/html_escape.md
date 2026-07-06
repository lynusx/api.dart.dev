# htmlEscape

```dart
HtmlEscape const htmlEscape
```

一个将字符转换为 HTML 实体的 `String` 转换器。

用于在将文本插入 HTML 文档之前对其进行净化处理。在 HTML 中具有特殊含义的字符会被转换为 HTML 实体（例如 `&` 转换为 `&amp;`）。

通用转换器会转义所有在 HTML 属性或普通元素上下文中具有特殊含义的字符。具有特殊内容类型的元素（如 CSS 或 JavaScript）可能需要理解该内容类型语法的更专门的转义方式。

如果更详细地了解文本将被插入的上下文，则可以省略对某些字符的转义（例如，当不在属性值内部时可省略引号的转义）。

转义后的文本只应在带引号的 HTML 属性值内部，或作为普通元素的文本内容使用。在标签内部但不在带引号的属性值中使用转义后的文本仍然是危险的。

---

# HtmlEscapeMode

```dart
final class HtmlEscapeMode {}
```

HTML 转义模式。

允许根据转义结果的使用上下文指定转义模式。相关的上下文包括：

- 作为 HTML 元素的文本内容。
- 作为（单引号或双引号）带引号属性值的值。

所有模式都要求转义 `&`（和号）字符，并可以启用对更多字符的转义。

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

## 构造函数

### HtmlEscapeMode()

```dart
HtmlEscapeMode({
  String name = "custom",
  bool escapeLtGt = false,
  bool escapeQuot = false,
  bool escapeApos = false,
  bool escapeSlash = false,
})
```

创建一个自定义转义模式。

[name] 仅用于 [toString] 的返回结果，如果未提供，默认为 `'custom'`。

所有模式都会转义 `&`。该模式还可以进一步设置为转义 `<` 和 `>`（[escapeLtGt]）、`"`（[escapeQuot]）、`'`（[escapeApos]）和/或 `/`（[escapeSlash]）。

## 静态属性

### unknown

```dart
HtmlEscapeMode unknown
```

默认转义模式，转义所有字符。

这种转义的结果既可用于元素内容，也可用于任何属性值。

该转义仅适用于具有普通 HTML 内容的元素，不适用于例如脚本或样式元素的内容，这些内容需要与其特定内容语法相匹配的转义方式。

### attribute

```dart
HtmlEscapeMode attribute
```

用于进入双引号 HTML 属性值的文本的转义模式。

其结果不应用作不带引号或单引号属性值的内容。

转义双引号（`"`）但不转义单引号（`'`），并转义 `<` 和 `>` 字符，因为它们在严格的 XHTML 属性中是不允许的。

### sqAttribute

```dart
HtmlEscapeMode sqAttribute
```

用于进入单引号 HTML 属性值的文本的转义模式。

其结果不应用作不带引号或双引号属性值的内容。

转义单引号（`'`）但不转义双引号（`"`），并转义 `<` 和 `>` 字符，因为它们在严格的 XHTML 属性中是不允许的。

### element

```dart
HtmlEscapeMode element
```

用于进入 HTML 元素内容的文本的转义模式。

该转义仅适用于具有普通 HTML 内容的元素，不适用于例如脚本或样式元素的内容，这些内容需要与其特定内容语法相匹配的转义方式。

转义 `<` 和 `>` 字符。

## 属性

### escapeLtGt

```dart
bool escapeLtGt
```

是否转义 '<' 和 '>'。

### escapeQuot

```dart
bool escapeQuot
```

是否转义 '"'（引号）。

### escapeApos

```dart
bool escapeApos
```

是否转义 "'"（撇号）。

### escapeSlash

```dart
bool escapeSlash
```

是否转义 "/"（正斜杠）。

建议转义斜杠，以避免[开放式Web应用程序安全项目（OWASP）](<https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet#RULE_.231_-_HTML_Escape_Before_Inserting_Untrusted_Data_into_HTML_Element_Content>)所指出的跨站脚本攻击。

---

# HtmlEscape

```dart
final class HtmlEscape extends Converter<String, String> {}
```

用于转义 HTML 中具有特殊含义的字符的转换器。

该转换器会找出 HTML 源码中具有特殊意义的字符，并将其替换为相应的 HTML 实体。

在 HTML 中需要转义的字符包括：

- `&`（和号）始终需要转义。
- `<`（小于号）和 `>`（大于号）在元素内部时需要转义。
- `"`（引号）在双引号属性值内部时需要转义。
- `'`（撇号）在单引号属性值内部时需要转义。撇号被转义为 `&#39;` 而不是 `&apos;`，因为并非所有浏览器都能识别 `&apos;`。
- `/`（斜杠）建议转义，因为在某些 HTML 方言中它可能被用于终止一个元素。

转义 `>`（大于号）并非必需，但当同时转义了小于号时，转义大于号往往会使结果更易读。

示例：

```dart
const HtmlEscape htmlEscape = HtmlEscape();
String unescaped = 'Text & subject';
String escaped = htmlEscape.convert(unescaped);
print(escaped); // Text &amp; subject

unescaped = '10 > 1 and 1 < 10';
escaped = htmlEscape.convert(unescaped);
print(escaped); // 10 &gt; 1 and 1 &lt; 10

unescaped = "Single-quoted: 'text'";
escaped = htmlEscape.convert(unescaped);
print(escaped); // Single-quoted: &#39;text&#39;

unescaped = 'Double-quoted: "text"';
escaped = htmlEscape.convert(unescaped);
print(escaped); // Double-quoted: &quot;text&quot;

unescaped = 'Path: /system/';
escaped = htmlEscape.convert(unescaped);
print(escaped); // Path: &#47;system&#47;
```

## 构造函数

### HtmlEscape()

```dart
HtmlEscape([HtmlEscapeMode mode = HtmlEscapeMode.unknown])
```

创建一个转义 HTML 字符的转换器。

如果提供的 [mode] 为 [HtmlEscapeMode.attribute] 或 [HtmlEscapeMode.element]，则只转义对应子集的 HTML 字符。默认情况下会转义所有 HTML 字符。

## 属性

### mode

```dart
HtmlEscapeMode mode
```

转换器使用的 [HtmlEscapeMode]。

## 方法

### convert()

```dart
String convert(String text)
```

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<String> sink)
```
