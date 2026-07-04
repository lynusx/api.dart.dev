# String

```dart
abstract final class String implements Comparable<String>, Pattern {}
```

UTF-16 代码单元的序列。

字符串主要用于表示文本。一个字符可能由多个码点（code point）表示，每个码点由一个或两个代码单元组成。例如，巴布亚新几内亚国旗字符需要四个代码单元来表示两个码点，但应被视为单个字符处理："🇵🇬"。不支持该旗帜字符的平台可能会改为显示字母 "PG"。如果码点顺序互换，则会变成瓜德罗普岛国旗 "🇬🇵"（"GP"）。

字符串可以是单行的，也可以是多行的。单行字符串使用匹配的单引号或双引号书写，多行字符串使用三重引号书写。以下都是合法的 Dart 字符串：

```dart
'Single quotes';
"Double quotes";
'Double quotes in "single" quotes';
"Single quotes in 'double' quotes";

'''A
multiline
string''';

"""
Another
multiline
string""";
```

字符串是不可变的。虽然不能更改字符串本身，但可以对字符串执行操作以创建一个新字符串：

```dart
const string = 'Dart is fun';
print(string.substring(0, 4)); // 'Dart'
```

可以使用加号（`+`）运算符拼接字符串：

```dart
const string = 'Dart ' + 'is ' + 'fun!';
print(string); // 'Dart is fun!'
```

相邻的字符串字面量会自动拼接：

```dart
const string = 'Dart ' 'is ' 'fun!';
print(string); // 'Dart is fun!'
```

可以使用 `${}` 在字符串中插入 Dart 表达式的值。当插值对象是标识符时，可以省略花括号：

```dart
const string = 'dartlang';
print('$string has ${string.length} letters'); // dartlang has 8 letters
```

字符串由一系列 Unicode UTF-16 代码单元表示，可通过 [codeUnitAt] 或 [codeUnits] 成员访问：

```dart
const string = 'Dart';
final firstCodeUnit = string.codeUnitAt(0);
print(firstCodeUnit); // 68, aka U+0044, the code point for 'D'.
final allCodeUnits = string.codeUnits;
print(allCodeUnits); // [68, 97, 114, 116]
```

可以通过索引运算符获取各个代码单元对应的字符串表示：

```dart
const string = 'Dart';
final charAtIndex = string[0];
print(charAtIndex); // 'D'
```

字符串中的字符以 UTF-16 编码。对 UTF-16 进行解码（即合并代理对）会得到 Unicode 码点。与 Go 语言中的术语类似，Dart 使用 "rune"（符文）来表示代表一个 Unicode 码点的整数。使用 [runes] 属性可以获取字符串的 rune 序列：

```dart
const string = 'Dart';
final runes = string.runes.toList();
print(runes); // [68, 97, 114, 116]
```

对于超出基本多语言平面（Basic Multilingual Plane，平面 0）范围、由代理对组成的字符，[runes] 会将该代理对合并并返回一个整数。例如，高音谱号音乐符号（'𝄞'）的 Unicode 字符，其 rune 值为 0x1D11E，由 UTF-16 代理对 `0xD834` 和 `0xDD1E` 组成。使用 [codeUnits] 会返回该代理对，而使用 `runes` 则返回它们合并后的值：

```dart
const clef = '\u{1D11E}';
for (final item in clef.codeUnits) {
  print(item.toRadixString(16));
  // d834
  // dd1e
}
for (final item in clef.runes) {
  print(item.toRadixString(16)); // 1d11e
}
```

`String` 类不能被继承（extend）或实现（implement）。尝试这样做会产生编译时错误。

## 其他资源

- [StringBuffer]：用于高效地增量构建字符串。
- [RegExp]：用于处理正则表达式。
- [字符串与正则表达式](https://dart.dev/libraries/dart-core#strings-and-regular-expressions)

## 构造函数

### String.fromCharCodes()

```dart
String.fromCharCodes(Iterable<int> charCodes, [int start = 0, int? end])
```

分配一个包含指定 [charCodes] 的新字符串。

[charCodes] 可以是 UTF-16 代码单元，也可以是 rune。如果某个字符代码值是 16 位的，则将其用作代码单元：

```dart
final string = String.fromCharCodes([68]);
print(string); // D
```

如果字符代码值大于 16 位，则将其拆分为一个代理对：

```dart
final clef = String.fromCharCodes([0x1D11E]);
clef.codeUnitAt(0); // 0xD834
clef.codeUnitAt(1); // 0xDD1E
```

如果提供了 [start] 和 [end]，则只使用 [charCodes] 中从 `start` 到（不包含）`end` 位置的值。`start` 和 `end` 的取值必须满足 `0 <= start <= end`。如果省略 [start]，则默认为零，即 [charCodes] 的起始位置；如果省略 [end]，则包含 [start] 之后的所有字符代码。如果 [charCodes] 中没有 [end]（甚至没有 [start]）对应的元素，则实际使用的字符代码可能比 `end - start` 短，甚至为空。

### String.fromCharCode()

```dart
String.fromCharCode(int charCode)
```

分配一个包含指定 [charCode] 的新字符串。

如果 [charCode] 可以用单个 UTF-16 代码单元表示，则新字符串只包含一个代码单元。否则，[length] 为 2，且这两个代码单元构成一个代理对。详见 [fromCharCodes] 的文档。

允许使用代理对的一半来创建 [String]。

### String.fromEnvironment()

```dart
const String.fromEnvironment(
  String name, {
  String defaultValue = "",
})
```

编译配置环境声明中 [name] 对应的值。

编译配置环境由编译或运行 Dart 程序的外部工具提供。该环境是一组字符串键与其关联字符串值之间的映射。在同一个程序中，对 `String.fromEnvironment`、[int.fromEnvironment]、[bool.fromEnvironment] 和 [bool.hasEnvironment] 的所有调用，与某个 [name] 关联的字符串值（或值缺失的情况）必须保持一致。

调用该构造函数的结果是与键 [name] 关联的字符串。如果没有与 [name] 关联的值，则结果为 [defaultValue] 字符串，其默认值为空字符串。

查找值的示例：

```dart
const String.fromEnvironment("defaultFloo", defaultValue: "no floo")
```

要检查某个值是否存在，可使用 [bool.hasEnvironment]。示例：

```dart
const maybeDeclared = bool.hasEnvironment("maybeDeclared")
    ? String.fromEnvironment("maybeDeclared")
    : null;
```

在同一个程序中，对 `String.fromEnvironment`、[int.fromEnvironment]、[bool.fromEnvironment] 和 [bool.hasEnvironment] 的所有调用，与某个 [name] 关联的字符串值（或值缺失的情况）必须保持一致。

该构造函数只保证在以 `const` 方式调用时可以正常工作。在某些能够在运行时访问编译器选项的平台上，它也可能以非常量方式正常工作，但大多数预编译（AOT）平台不具备这些信息。

编译配置环境与 POSIX 进程的环境变量并不是一回事。在原生平台上，可以使用 `dart:io` 库中的 `Platform.environment` 访问后者。

## 属性

### length

```dart
int get length
```

字符串的长度。

返回该字符串中 UTF-16 代码单元的数量。如果字符串中包含基本多语言平面（平面 0）之外的字符，则 [runes] 的数量可能会更少：

```dart
'Dart'.length;          // 4
'Dart'.runes.length;    // 4

var clef = '\u{1D11E}';
clef.length;            // 2
clef.runes.length;      // 1
```

### hashCode

```dart
int get hashCode
```

根据字符串的代码单元派生出的哈希码。

该哈希码与 [operator ==] 兼容。具有相同代码单元序列的字符串拥有相同的哈希码。

### isEmpty

```dart
bool get isEmpty
```

该字符串是否为空。

### isNotEmpty

```dart
bool get isNotEmpty
```

该字符串是否非空。

### codeUnits

```dart
List<int> get codeUnits
```

该字符串的 UTF-16 代码单元组成的不可修改列表。

### runes

```dart
Runes get runes
```

该字符串的 Unicode 码点组成的 [Iterable]。

如果字符串中包含代理对，该迭代器会将其合并并作为一个整数返回。未配对的代理单元的一半会被当作有效的 16 位代码单元处理。

## 方法

### codeUnitAt()

```dart
int codeUnitAt(int index)
```

返回给定 [index] 处的 16 位 UTF-16 代码单元。

### compareTo()

```dart
int compareTo(String other)
```

将该字符串与 [other] 进行比较。

如果 `this` 排序在 `other` 之前，则返回负值；如果 `this` 排序在 `other` 之后，则返回正值；如果 `this` 与 `other` 相等，则返回零。

排序结果与两个字符串首个不同位置处代码单元的顺序一致。如果一个字符串是另一个字符串的前缀，则较短的字符串排在较长的字符串之前。如果两个字符串内容完全相同，则在排序上它们是相等的。排序不检查 Unicode 等价性，比较是区分大小写的。

```dart
var relation = 'Dart'.compareTo('Go');
print(relation); // < 0
relation = 'Go'.compareTo('Forward');
print(relation); // > 0
relation = 'Forward'.compareTo('Forward');
print(relation); // 0
```

### endsWith()

```dart
bool endsWith(String other)
```

该字符串是否以 [other] 结尾。

例如：

```dart
const string = 'Dart is open source';
print(string.endsWith('urce')); // true
```

### startsWith()

```dart
bool startsWith(Pattern pattern, [int index = 0])
```

该字符串是否以匹配 [pattern] 的内容开头。

```dart
const string = 'Dart is open source';
print(string.startsWith('Dar')); // true
print(string.startsWith(RegExp(r'[A-Z][a-z]'))); // true
```

如果提供了 [index]，该方法会检查从该索引开始的子字符串是否以匹配 [pattern] 的内容开头：

```dart
const string = 'Dart';
print(string.startsWith('art', 0)); // false
print(string.startsWith('art', 1)); // true
print(string.startsWith(RegExp(r'\w{3}'), 2)); // false
```

[index] 不能为负数，也不能大于 [length]。

如果 [index] 大于零且正则表达式不是多行模式，则包含 '^' 的 [RegExp] 不会匹配。该模式作用于整个字符串，并不会先提取出从 [index] 开始的子字符串：

```dart
const string = 'Dart';
print(string.startsWith(RegExp(r'^art'), 1)); // false
print(string.startsWith(RegExp(r'art'), 1)); // true
```

### indexOf()

```dart
int indexOf(Pattern pattern, [int start = 0])
```

返回 [pattern] 在该字符串中从 [start]（含）位置开始的第一个匹配的位置：

```dart
const string = 'Dartisans';
print(string.indexOf('art')); // 1
print(string.indexOf(RegExp(r'[A-Z][a-z]'))); // 0
```

如果没有找到匹配项，则返回 -1：

```dart
const string = 'Dartisans';
string.indexOf(RegExp(r'dart')); // -1
```

[start] 必须为非负数，且不能大于 [length]。

### lastIndexOf()

```dart
int lastIndexOf(Pattern pattern, [int? start])
```

[pattern] 在该字符串中最后一次匹配的起始位置。

从 [start] 位置开始向前查找 pattern 的匹配项：

```dart
const string = 'Dartisans';
print(string.lastIndexOf('a')); // 6
print(string.lastIndexOf(RegExp(r'a(r|n)'))); // 6
```

如果在该字符串中找不到 [pattern]，则返回 -1。

```dart
const string = 'Dartisans';
print(string.lastIndexOf(RegExp(r'DART'))); // -1
```

如果省略 [start]，则从字符串末尾开始查找。如果提供了 [start]，则它必须为非负数，且不能大于 [length]。

### substring()

```dart
String substring(int start, [int? end])
```

该字符串从 [start]（含）到 [end]（不含）之间的子字符串。

示例：

```dart
const string = 'dartlang';
var result = string.substring(1); // 'artlang'
result = string.substring(1, 4); // 'art'
```

[start] 和 [end] 都必须为非负数，且不能大于 [length]；如果提供了 [end]，其值必须大于或等于 [start]。

### trim()

```dart
String trim()
```

去除首尾空白字符后的字符串。

如果字符串包含首部或尾部空白字符，则返回一个不含首尾空白字符的新字符串：

```dart
final trimmed = '\tDart is fun\n'.trim();
print(trimmed); // 'Dart is fun'
```

否则，返回原字符串本身：

```dart
const string1 = 'Dart';
final string2 = string1.trim(); // 'Dart'
print(identical(string1, string2)); // true
```

空白字符由 Unicode 的 White_Space 属性（6.2 或更高版本中定义）以及 BOM 字符 0xFEFF 定义。

以下是根据 Unicode 6.3 版本被去除的字符列表：

```plaintext
    0009..000D    ; White_Space # Cc   <control-0009>..<control-000D>
    0020          ; White_Space # Zs   SPACE
    0085          ; White_Space # Cc   <control-0085>
    00A0          ; White_Space # Zs   NO-BREAK SPACE
    1680          ; White_Space # Zs   OGHAM SPACE MARK
    2000..200A    ; White_Space # Zs   EN QUAD..HAIR SPACE
    2028          ; White_Space # Zl   LINE SEPARATOR
    2029          ; White_Space # Zp   PARAGRAPH SEPARATOR
    202F          ; White_Space # Zs   NARROW NO-BREAK SPACE
    205F          ; White_Space # Zs   MEDIUM MATHEMATICAL SPACE
    3000          ; White_Space # Zs   IDEOGRAPHIC SPACE

    FEFF          ; BOM                ZERO WIDTH NO_BREAK SPACE
```

部分后续版本的 Unicode 不再将 U+0085 视为空白字符。它是否会被去除，取决于系统所使用的 Unicode 版本。

### trimLeft()

```dart
String trimLeft()
```

去除首部空白字符后的字符串。

与 [trim] 类似，但只去除首部空白字符。

```dart
final string = ' Dart '.trimLeft();
print(string); // 'Dart '
```

### trimRight()

```dart
String trimRight()
```

去除尾部空白字符后的字符串。

与 [trim] 类似，但只去除尾部空白字符。

```dart
final string = ' Dart '.trimRight();
print(string); // ' Dart'
```

### padLeft()

```dart
String padLeft(int width, [String padding = ' '])
```

如果该字符串的长度小于 [width]，则在其左侧填充。

返回一个新字符串，该字符串会在原字符串前面重复添加 [padding]，直到长度达到 [width] 为止（每次添加一个位置）。

```dart
const string = 'D';
print(string.padLeft(4)); // '   D'
print(string.padLeft(2, 'x')); // 'xD'
print(string.padLeft(4, 'y')); // 'yyyD'
print(string.padLeft(4, '>>')); // '>>>>>>D'
```

如果 [width] 已经小于或等于 `this.length`，则不会添加任何填充内容。负数的 `width` 会被视为零。

如果 [padding] 的长度不为 1，则结果字符串的长度将不等于 `width`。当填充内容是表示单个字符的较长字符串（例如 `"&nbsp;"` 或 `"\u{10002}"`）时，这种情况可能会很有用。此时，使用者应确保 `this.length` 是该字符串长度的正确度量。

### padRight()

```dart
String padRight(int width, [String padding = ' '])
```

如果该字符串的长度小于 [width]，则在其右侧填充。

返回一个新字符串，该字符串会在原字符串后面重复添加 [padding]，直到长度达到 [width] 为止（每次添加一个位置）。

```dart
const string = 'D';
print(string.padRight(4)); // 'D    '
print(string.padRight(2, 'x')); // 'Dx'
print(string.padRight(4, 'y')); // 'Dyyy'
print(string.padRight(4, '>>')); // 'D>>>>>>'
```

如果 [width] 已经小于或等于 `this.length`，则不会添加任何填充内容。负数的 `width` 会被视为零。

如果 [padding] 的长度不为 1，则结果字符串的长度将不等于 `width`。当填充内容是表示单个字符的较长字符串（例如 `"&nbsp;"` 或 `"\u{10002}"`）时，这种情况可能会很有用。此时，使用者应确保 `this.length` 是该字符串长度的正确度量。

### contains()

```dart
bool contains(Pattern other, [int startIndex = 0])
```

该字符串是否包含与 [other] 匹配的内容。

示例：

```dart
const string = 'Dart strings';
final containsD = string.contains('D'); // true
final containsUpperCase = string.contains(RegExp(r'[A-Z]')); // true
```

如果提供了 [startIndex]，该方法只会匹配该索引及之后的内容：

```dart
const string = 'Dart strings';
final containsD = string.contains(RegExp('D'), 0); // true
final caseSensitive = string.contains(RegExp(r'[A-Z]'), 1); // false
```

[startIndex] 不能为负数，也不能大于 [length]。

### replaceFirst()

```dart
String replaceFirst(Pattern from, String to, [int startIndex = 0])
```

创建一个新字符串，将 [from] 的第一次出现替换为 [to]。

从 [startIndex] 开始，在该字符串中查找 [from] 的第一个匹配项，并创建一个新字符串，将该匹配项替换为字符串 [to]。

示例：

```dart
'0.0001'.replaceFirst(RegExp(r'0'), ''); // '.0001'
'0.0001'.replaceFirst(RegExp(r'0'), '7', 1); // '0.7001'
```

### replaceFirstMapped()

```dart
String replaceFirstMapped(Pattern from, String replace(Match match), [int startIndex = 0])
```

替换该字符串中 [from] 的第一次出现。

```dart
const string = 'Dart is fun';
print(string.replaceFirstMapped(
    'fun', (m) => 'open source')); // Dart is open source

print(string.replaceFirstMapped(
    RegExp(r'\w(\w*)'), (m) => '<${m[0]}-${m[1]}>')); // <Dart-art> is fun
```

返回一个新字符串，该字符串与原字符串相同，只是从 [startIndex] 开始的 [from] 第一个匹配项被替换为调用 [replace] 并传入匹配对象后得到的结果。

[startIndex] 必须为非负数，且不能大于 [length]。

### replaceAll()

```dart
String replaceAll(Pattern from, String replace)
```

将所有匹配 [from] 的子字符串替换为 [replace]。

创建一个新字符串，其中所有与 [from] 匹配的非重叠子字符串（即通过 `from.allMatches(thisString)` 迭代得到的那些）都被替换为字面字符串 [replace]。

```dart
'resume'.replaceAll(RegExp(r'e'), 'é'); // 'résumé'
```

请注意，[replace] 字符串不会被解释。如果替换内容依赖于匹配结果（例如依赖于 [RegExp] 的捕获组），请改用 [replaceAllMapped] 方法。

### replaceAllMapped()

```dart
String replaceAllMapped(Pattern from, String Function(Match match) replace)
```

将所有匹配 [from] 的子字符串替换为计算得出的字符串。

创建一个新字符串，其中所有与 [from] 匹配的非重叠子字符串（即通过 `from.allMatches(thisString)` 迭代得到的那些）都被替换为对相应的 [Match] 对象调用 [replace] 后得到的结果。

这可用于将匹配项替换为依赖于匹配内容的新内容，这与 [replaceAll] 中替换字符串始终固定不同。

[replace] 函数会以该模式生成的 [Match] 作为参数被调用，其返回结果将被用作替换内容。

下面定义的函数使用 [replaceAllMapped] 将字符串中的每个单词转换为简化版的 "pig latin"（说反话）：

```dart
String pigLatin(String words) => words.replaceAllMapped(
    RegExp(r'\b(\w*?)([aeiou]\w*)', caseSensitive: false),
    (Match m) => "${m[2]}${m[1]}${m[1]!.isEmpty ? 'way' : 'ay'}");

final result = pigLatin('I have a secret now!');
print(result); // 'Iway avehay away ecretsay ownay!'
```

### replaceRange()

```dart
String replaceRange(int start, int? end, String replacement)
```

将从 [start] 到 [end] 的子字符串替换为 [replacement]。

创建一个等价于以下内容的新字符串：

```dart
this.substring(0, start) + replacement + this.substring(end)
```

示例：

```dart
const string = 'Dart is fun';
final result = string.replaceRange(8, null, 'open source');
print(result); // Dart is open source
```

[start] 和 [end] 索引必须指定该字符串的一个有效范围，即 `0 <= start <= end <= this.length`。如果 [end] 为 `null`，则默认为 [length]。

### split()

```dart
List<String> split(Pattern pattern)
```

在与 [pattern] 匹配的位置拆分字符串，并返回子字符串组成的列表。

使用 [Pattern.allMatches] 查找该字符串中 `pattern` 的所有匹配项，并返回位于各匹配项之间、第一个匹配项之前以及最后一个匹配项之后的子字符串组成的列表。

```dart
const string = 'Hello world!';
final splitted = string.split(' ');
print(splitted); // [Hello, world!];
```

如果该模式与字符串完全不匹配，则结果始终是一个只包含原字符串的列表。

如果 [pattern] 是一个 [String]，则以下等式始终成立：

```dart
string.split(pattern).join(pattern) == string
```

如果第一个匹配项是位于字符串开头的空匹配，则其之前的空子字符串不会包含在结果中。如果最后一个匹配项是位于字符串末尾的空匹配，则其之后的空子字符串不会包含在结果中。如果某个匹配项为空，并且紧接在前一个匹配项之后（即从前一个匹配项结束的位置开始），则这两个匹配项之间的空子字符串不会包含在结果中。

```dart
const string = 'abba';
final re = RegExp(r'b*');
// re.allMatches(string) will find four matches:
// * empty match before first "a".
// * match of "bb"
// * empty match after "bb", before second "a"
// * empty match after second "a".
print(string.split(re)); // [a, a]
```

位于字符串开头、结尾，或紧跟在另一个匹配项之后的非空匹配不会被特殊处理，会在结果中引入空子字符串：

```dart
const string = 'abbaa';
final splitted = string.split('a'); // ['', 'bb', '', '']
```

如果该字符串本身为空字符串，那么当 `pattern` 能匹配空字符串时，结果将是一个空列表，因为首尾空匹配前后的空字符串都不会被包含在内。（如果该模式不匹配，则结果仍是包含原始空字符串的列表 `[""]`。）

```dart
const string = '';
print(string.split('')); // []
print(string.split('a')); // []
```

使用空模式进行拆分，会将字符串拆分为单个代码单元组成的字符串。

```dart
const string = 'Pub';
print(string.split('')); // [P, u, b]

// Same as:
var codeUnitStrings = [
  for (final unit in string.codeUnits) String.fromCharCode(unit)
];
print(codeUnitStrings); // [P, u, b]
```

拆分是在 UTF-16 代码单元边界上进行的，而不是在 rune（Unicode 码点）边界上进行的：

```dart
// String made up of two code units, but one rune.
const string = '\u{1D11E}';
final splitted = string.split('');
print(splitted); // ['\ud834', '\udd1e'] - 2 unpaired surrogate values
```

要获取包含字符串中各个 rune 的字符串列表，不应使用 split。可以改用以下方式获取每个 rune 对应的字符串：

```dart
const string = '\u{1F642}';
for (final rune in string.runes) {
  print(String.fromCharCode(rune));
}
```

### splitMapJoin()

```dart
String splitMapJoin(
  Pattern pattern, {
  String onMatch( Match )?,
  String onNonMatch( String )?,
})
```

拆分字符串，转换各个部分，并将其组合为一个新字符串。

[pattern] 用于将字符串拆分为若干部分，并分隔出匹配项。该字符串上 [pattern] 的 [Pattern.allMatches] 得到的每个匹配项都会被当作一个匹配，而位于一个匹配项结束处（或字符串起始处）与下一个匹配项开始处（或字符串结束处）之间的子字符串则被视为非匹配部分。（与 [split] 不同，这里不会省略首尾的空匹配，所有匹配项及其间的部分都会被包含在内。）

每个匹配项都会通过调用 [onMatch] 转换为字符串。如果省略 [onMatch]，则使用匹配到的子字符串本身。

每个非匹配部分都会通过调用 [onNonMatch] 转换为字符串。如果省略 [onNonMatch]，则使用未匹配的子字符串本身。

随后，所有转换后的部分会被拼接成最终的结果字符串。

```dart
final result = 'Eats shoots leaves'.splitMapJoin(RegExp(r'shoots'),
    onMatch: (m) => '${m[0]}', // (or no onMatch at all)
    onNonMatch: (n) => '*');
print(result); // *shoots*
```

### toLowerCase()

```dart
String toLowerCase()
```

将该字符串中的所有字符转换为小写。

如果字符串已经全部为小写，则该方法返回 `this`。

```dart
'ALPHABET'.toLowerCase(); // 'alphabet'
'abc'.toLowerCase(); // 'abc'
```

该函数使用与语言无关的 Unicode 映射，因此仅在部分语言中有效。

### toUpperCase()

```dart
String toUpperCase()
```

将该字符串中的所有字符转换为大写。

如果字符串已经全部为大写，则该方法返回 `this`。

```dart
'alphabet'.toUpperCase(); // 'ALPHABET'
'ABC'.toUpperCase(); // 'ABC'
```

该函数使用与语言无关的 Unicode 映射，因此仅在部分语言中有效。

## 运算符

### operator []

```dart
String operator [](int index)
```

给定 [index] 处的字符（以单代码单元 [String] 形式表示）。

返回的字符串恰好表示一个 UTF-16 代码单元，该单元可能是代理对的一半。代理对中的单个成员是无效的 UTF-16 字符串：

```dart
var clef = '\u{1D11E}';
// These represent invalid UTF-16 strings.
clef[0].codeUnits;      // [0xD834]
clef[1].codeUnits;      // [0xDD1E]
```

该方法等价于 `String.fromCharCode(this.codeUnitAt(index))`。

### operator ==

```dart
bool operator ==(Object other)
```

[other] 是否是一个具有相同代码单元序列的 `String`。

该方法会逐一比较两个字符串的各个代码单元，不检查 Unicode 等价性。例如，以下两个字符串都表示 "Amélie"，但由于编码方式不同，它们并不相等：

```dart
'Am\xe9lie' == 'Ame\u{301}lie'; // false
```

第一个字符串将 'é' 编码为单个 Unicode 码点（同时也是单个 rune），而第二个字符串则将其编码为 'e' 加上组合重音符号 '◌́'。

### operator +

```dart
String operator +(String other)
```

通过将该字符串与 [other] 拼接来创建一个新字符串。

示例：

```dart
const string = 'dart' + 'lang'; // 'dartlang'
```

### operator \*

```dart
String operator *(int times)
```

通过将该字符串与自身重复拼接多次来创建一个新字符串。

`str * n` 的结果等价于 `str + str + ...`（重复 n 次）`... + str`。

```dart
const string = 'Dart';
final multiplied = string * 3;
print(multiplied); // 'DartDartDart'
```

如果 [times] 为零或负数，则返回空字符串。

---

# Runes

```dart
final class Runes extends Iterable<int> {}
```

[String] 的 runes（整数形式的 Unicode 码点）。

字符串中的字符以 UTF-16 编码。对 UTF-16 进行解码（即合并代理对）会得到 Unicode 码点。与 Go 语言中的术语类似，Dart 使用 "rune" 来表示代表一个 Unicode 码点的整数。使用 `runes` 属性可以获取字符串的 rune 序列。

示例：

```dart
const string = 'Dart';
final runes = string.runes.toList();
print(runes); // [68, 97, 114, 116]
```

对于超出基本多语言平面（平面 0）范围、由代理对组成的字符，runes 会将该代理对合并并返回一个整数。

例如，"男人"表情符号（'👨'，`U+1F468`）的 Unicode 字符由代理项 `U+d83d` 和 `U+dc68` 组合而成。

示例：

```dart
const emojiMan = '👨';
print(emojiMan.runes); // (128104)

// Surrogate pairs:
for (final item in emojiMan.codeUnits) {
  print(item.toRadixString(16));
  // d83d
  // dc68
}
```

**另请参阅：**

- [Runes 与字形簇（grapheme clusters）](https://dart.dev/language/built-in-types#runes-and-grapheme-clusters)

## 构造函数

### Runes()

```dart
Runes(String string)
```

为 [string] 创建一个 [Runes] 迭代器。

## 属性

### string

```dart
String string
```

该 runes 所属的字符串。

### iterator

```dart
RuneIterator get iterator
```

一个新的 `Iterator`，用于迭代该 `Iterable` 的元素。

Iterable 类可以指定其元素的迭代顺序（例如 [List](https://api.flutter.dev/flutter/dart-core/List-class.html) 总是按索引顺序迭代），也可以不做规定（例如基于哈希的 [Set](https://api.flutter.dev/flutter/dart-core/Set-class.html) 可能以任意顺序迭代）。

每次读取 `iterator` 时，都会返回一个新的迭代器，可用于再次遍历所有元素。同一个 iterable 的多个迭代器可以相互独立地进行遍历，但只要底层集合未发生变化，它们应以相同的顺序返回相同的元素。

修改集合可能导致新创建的迭代器产生不同的元素，也可能改变现有元素的顺序。[List](https://api.flutter.dev/flutter/dart-core/List-class.html) 精确规定了其迭代顺序，因此修改列表会以可预测的方式改变迭代顺序。基于哈希的 [Set](https://api.flutter.dev/flutter/dart-core/Set-class.html) 在添加新元素时，其迭代顺序可能会发生完全改变。

在创建新迭代器之后修改底层集合，可能会导致下一次对该迭代器调用 [Iterator.moveNext](https://api.flutter.dev/flutter/dart-core/Iterator/moveNext.html) 时出错。任何 _可修改的_ iterable 类都应说明哪些操作会中断迭代。

### last

```dart
int get last
```

最后一个元素。

如果 `this` 为空，则抛出 [StateError](https://api.flutter.dev/flutter/dart-core/StateError-class.html)。否则可能会遍历所有元素，并返回最后遇到的那个。某些 iterable 可能有更高效的方式来查找最后一个元素（例如列表可以直接访问最后一个元素，而无需遍历之前的元素）。

---

# RuneIterator

```dart
final class RuneIterator implements Iterator<int> {}
```

用于读取 Dart 字符串的 runes（整数形式的 Unicode 码点）的 [Iterator]。

## 构造函数

### RuneIterator()

```dart
RuneIterator(String string)
```

创建一个位于字符串起始位置的迭代器。

### RuneIterator.at()

```dart
RuneIterator.at(String string, int index)
```

创建一个位于字符串第 [index] 个代码单元之前的迭代器。

创建时没有 [current] 值。调用 [moveNext] 会将从 [index] 处开始的 rune 设为当前值，调用 [movePrevious] 则会将恰好在 [index] 之前结束的 rune 设为当前值。

[index] 所在位置不能处于代理对的中间。

## 属性

### string

```dart
String string
```

正在被迭代的字符串。

### rawIndex

```dart
int get rawIndex
```

当前 rune 在字符串中的起始位置。

如果没有当前 rune（即 [current] 为 -1），则返回 -1。

### rawIndex

```dart
void set rawIndex(int rawIndex)
```

将迭代器重置到字符串中指定索引处的 rune。

将 [rawIndex] 设置为负数，或设置为大于等于 `string.length` 的值，都是错误的。将其设置在代理对中间同样是错误的。

将位置设置到字符串末尾，意味着不存在当前 rune。

### current

```dart
int get current
```

从字符串当前位置开始的 rune（整数形式的 Unicode 码点）。

如果不存在当前码点，则该值为 -1。

### currentSize

```dart
int get currentSize
```

构成当前 rune 的代码单元数量。

如果没有当前 rune（即 [current] 为 -1），则返回零。

### currentAsString

```dart
String get currentAsString
```

包含当前 rune 的字符串。

对于基本多语言平面之外的 rune，该字符串的长度为 2，包含两个代码单元。

如果不存在 [current] 值，则返回空字符串。

## 方法

### reset()

```dart
void reset([int rawIndex = 0])
```

将迭代器重置到字符串中给定的索引处。

重置之后，[current] 值将处于未设置状态。必须调用 [moveNext] 才能使该位置处的 rune 成为当前值，或调用 [movePrevious] 使该位置之前的最后一个 rune 成为当前值。

[rawIndex] 必须为非负数，且不能大于 `string.length`。它也不能是某个代理对中后一个代理项（trailing surrogate）的索引。

### moveNext()

```dart
bool moveNext()
```

移动到下一个码点。

如果存在下一个码点，则返回 `true` 并更新 [current]。否则返回 `false`，此时不存在当前码点。

### movePrevious()

```dart
bool movePrevious()
```

移动回上一个码点。

如果存在上一个码点，则返回 `true` 并更新 [current]。否则返回 `false`，此时不存在当前码点。
