# RegExp

```dart
abstract interface class RegExp implements Pattern {}
```

正则表达式模式。

正则表达式（regular expression，缩写为 regex 或 regexp）由一系列字符组成，这些字符指定了一种针对文本 _输入_ 的匹配检查算法。将正则表达式应用于输入文本，其结果要么是正则表达式匹配（接受）该文本，要么是该文本被拒绝。当正则表达式匹配文本时，它还会进一步提供一些关于 _如何_ 匹配该文本的信息。

Dart 正则表达式与 JavaScript 正则表达式具有相同的语法和语义。要了解更多关于 JavaScript 正则表达式的信息，请参阅 <https://ecma-international.org/ecma-262/9.0/#sec-regexp-regular-expression-objects>。

Dart 通过 [matchAsPrefix] 提供了基本的正则表达式匹配算法，该方法用于检查正则表达式是否从输入的某个特定位置开始匹配其中的一部分。如果匹配成功，Dart 会以 [RegExpMatch] 的形式返回匹配的详细信息。

你可以基于这个基本匹配检查，构建出 [RegExp] 的所有其他方法。

正则表达式最常见的用途是在输入中 _搜索_ 匹配项。[firstMatch] 方法提供了这一功能：它在字符串中搜索正则表达式首次匹配的位置。同样，如果找到匹配项，Dart 会以 [RegExpMatch] 的形式返回其详细信息。

以下示例在字符串中查找正则表达式的第一个匹配项。

```dart
RegExp exp = RegExp(r'(\w+)');
String str = 'Parse my string';
RegExpMatch? match = exp.firstMatch(str);
print(match![0]); // "Parse"
```

使用 [allMatches] 可以在字符串中查找正则表达式的所有匹配项。

以下示例查找字符串中正则表达式的所有匹配项。

```dart
RegExp exp = RegExp(r'(\w+)');
String str = 'Parse my string';
Iterable<RegExpMatch> matches = exp.allMatches(str);
for (final m in matches) {
  print(m[0]);
}
```

该示例的输出为：

```
Parse
my
string
```

前面的示例使用了 _原始字符串_（raw string），这是一种在字符串字面量前加上 `r` 前缀的特殊字符串类型。使用原始字符串可以将字符串中的每个字符（包括 `\` 和 `$`）都视为字面字符，然后逐一传递给 [RegExp] 解析器。你应该将原始字符串作为 [RegExp] 构造函数的参数使用。

**性能提示**：正则表达式并不能神奇地解决所有问题。任何人都可能写出这样的正则表达式：应用于某些字符串输入时性能低下。通常，这类正则表达式在处理小型或常见输入时表现尚可，但在处理大型或不常见的输入时会出现病态的性能问题。这种不一致的行为使得性能问题在测试中更难被发现。

正则表达式查找文本的速度不一定比使用 `String` 操作检查字符串更快。正则表达式的优势在于能够用极少的字符指定 _相当_ 复杂的模式。这类正则表达式在大多数常见情况下都能提供合理的效率。但这种简洁性是以可读性为代价的：由于语法复杂，正则表达式无法做到自解释。

Dart 的正则表达式实现了 ECMAScript RegExp 规范。该规范提供了一种通用且广为人知的正则表达式行为。当 Dart 编译为 Web 平台代码时，编译后的代码可以使用浏览器自带的正则表达式实现。

该规范使用 _回溯_（backtracking）定义 ECMAScript 正则表达式的行为。当正则表达式可以在不同的匹配方式之间进行选择时，它会按照模式中给出的顺序依次尝试每一种方式。例如：`RegExp(r"(foo|bar)baz")` 要检查 `foo` 或 `bar`，因此会先检查 `foo`。如果沿着这条路径继续匹配却无法匹配输入，正则表达式实现就会 _回溯_：将状态重置为检查 `foo` 之前的原始状态，忘记之后所做的所有工作，然后尝试下一个选择——在本例中即为 `bar`。

该规范定义了这些选择以及必须尝试它们的顺序。如果一个正则表达式能以多种方式匹配同一输入，那么选择的顺序就决定了正则表达式返回哪一个匹配结果。常用的正则表达式会通过安排匹配选择的顺序来确保得到特定的结果。ECMAScript 正则表达式规范限制了 Dart 实现正则表达式的方式：它必须是一种按特定顺序检查各个选择的回溯式实现。Dart 不能选择其他的正则表达式实现方式，否则正则表达式的匹配行为就会不同。

回溯方法虽然可行，但需要付出代价。对于某些正则表达式和某些输入，找到 _正确_ 的匹配可能需要 _大量_ 的尝试。而要拒绝一个正则表达式 _几乎_ 匹配的输入，则可能需要更多的尝试。

一种著名的危险正则表达式模式来自像 `*` 这样的嵌套量词：

```dart
var re = RegExp(r"^(a*|b)*c");
print(re.hasMatch("aaaaaaaaaaaaaaaaaaaaaaaaaaaaa"));
```

该正则表达式模式无法匹配仅由 `a` 组成的输入字符串，因为输入中缺少所需的 `c`。`(a*|b)*` 可以以 _指数级_ 数量的不同方式匹配所有的 `a`。回溯式正则表达式实现会在断定没有任何一种方式能达成完整匹配之前，尝试 _所有_ 这些方式。输入中每增加一个 `a`，正则表达式返回 `false` 所需的时间就会翻倍。（当回溯具有这种指数级的可能性时，就被称为 [“灾难性回溯”](https://www.google.com/search?q=regexp+catastrophic+backtracking)。）

顺序排列的量词提供了另一种危险模式，不过它们“只”会带来多项式级别的复杂度。

```dart
// Like `\w*-\d`, but check for `b` and `c` in that order.
var re = RegExp(r"^\w*(b)?\w*(c)?\w*-\d");
print(re.hasMatch("a" * 512));
```

同样，输入并不匹配，但 `RegExp` 必须先尝试 _n_<sup>3</sup> 种方式来匹配这 _n_ 个 `a`，然后才能得出这一结论。输入长度翻倍会使返回 `false` 所需的时间增加 _八倍_。这个指数会随着顺序量词数量的增加而增大。

当简化为如此简单的正则表达式时，这两种模式看起来都微不足道。然而，这些“微不足道”的模式常常作为更复杂正则表达式的一部分出现，这会让你更难发现问题所在。

一般来说，如果一个正则表达式具有 _超线性复杂度_ 的可能性，你就可以构造出一个搜索耗时极长的输入。如果将存在漏洞的正则表达式模式应用于用户提供的输入，这些模式就可能被用于[拒绝服务攻击](https://en.wikipedia.org/wiki/ReDoS)。

这个问题目前没有绝对可靠的解决方案。在程序可能将正则表达式应用于不保证匹配成功的输入的场景中，请务必小心，避免使用具有超线性行为的正则表达式。

以下是一些避免正则表达式出现超线性执行时间的经验法则：

- 每当正则表达式存在多个选择时，尽量确保可以仅根据下一个字符（或非常有限的前瞻）来做出选择。这样可以限制在两个分支上都需要进行大量计算的情况。
- 使用量词时，确保同一个字符串不能既匹配量词对应正则表达式的一次迭代，又匹配多次迭代。（对于 `(a*|b)*`，字符串 `"aa"` 既能匹配 `(a*|b){1}`，也能匹配 `(a*|b){2}`。）
- Dart 正则表达式的大多数用途都是 _搜索_ 匹配项，例如使用 [firstMatch]。如果你没有使用 `^` 将模式 _锚定_ 到行首或输入的开头，这种搜索的行为就如同正则表达式以一个隐式的 `[^]*` 开头。此时如果你的实际正则表达式又以 `.*` 开头，就可能导致搜索出现二次方级别的行为。请在适当的地方使用锚点或 [matchAsPrefix]，或者避免以带量词的模式作为正则表达式的开头。
- _仅供专家参考：_ Dart 和 ECMAScript 都没有通用的[“原子分组”](https://github.com/tc39/proposal-regexp-atomic-operators)功能。其他正则表达式方言使用该功能来限制回溯——如果一个原子捕获组成功匹配一次，正则表达式之后就不能再回溯到同一次匹配中。由于环视（lookaround）本身也可以作为原子组使用，因此可以借助 _前瞻_ 实现类似的效果：`var re = RegExp(r"^(?=((a*|b)*))\1d");`。上面这个示例对 `(a*|b)*` 进行的仍然是同样低效的匹配。一旦正则表达式尽可能地完成匹配，它就会完成这个正向前瞻；然后通过反向引用跳过前瞻所匹配的内容。在那之后，它就无法再回溯并尝试其他 `a` 的组合方式了。

尽量减少正则表达式匹配同一字符串的方式数量，这样可以减少在正则表达式找不到匹配时所执行的可能回溯次数。互联网上有不少关于[提升正则表达式性能](https://www.google.com/search?q=performance+of+regular+expressions)的指南，也可以作为参考。

## 构造函数

### RegExp()

```dart
RegExp(
  String source, {
  bool multiLine = false,
  bool caseSensitive = true,
  bool unicode = false,
  bool dotAll = false,
})
```

构造一个正则表达式。

如果 [source] 不符合合法的正则表达式语法，则抛出 [FormatException]。

如果你的代码启用了 `multiLine`，那么 `^` 和 `$` 将分别匹配 _行_ 的开头和结尾，同时也匹配输入的开头和结尾。

如果你的代码禁用了 `caseSensitive`，Dart 在匹配时会忽略字母的大小写。例如，在禁用 `caseSensitive` 的情况下，正则表达式模式 `a` 既能匹配 `a`，也能匹配 `A`。

如果你的代码启用了 `unicode`，Dart 会按照 ECMAScript 标准将该模式视为 Unicode 模式。

如果你的代码启用了 `dotAll`，那么 `.` 模式将匹配 _所有_ 字符，包括行终止符。

示例：

```dart
final wordPattern = RegExp(r'(\w+)');
final digitPattern = RegExp(r'(\d+)');
```

这些示例都使用 _原始字符串_ 作为参数。你应当优先将原始字符串用作 [RegExp] 构造函数的参数，因为这样可以更方便地将 `\` 和 `$` 作为正则表达式保留字符来书写。

如果使用非原始字符串编写，上述示例将会是：

```dart
final wordPattern = RegExp('(\\w+)'); // Should be raw string.
final digitPattern = RegExp('(\\d+)'); // Should be raw string.
```

仅当你需要使用字符串插值时，才应使用非原始字符串。例如：

```dart
Pattern keyValuePattern(String keyIdentifier) =>
    RegExp('$keyIdentifier=(\\w+)');
```

像这样将字符串原样嵌入正则表达式模式时，需要注意该字符串不能包含正则表达式保留字符。如果存在这种风险，可以使用 [escape] 函数将这些字符转换为保留字符的安全版本，从而只匹配字符串本身：

```dart
Pattern keyValuePattern(String anyStringKey) =>
    RegExp('${RegExp.escape(anyStringKey)}=(\\w+)');
```

## 静态方法

### escape()

```dart
String escape(String text)
```

创建能够匹配输入 [text] 的正则表达式语法。

如果 [text] 包含正则表达式保留字符，生成的正则表达式会按字面意义匹配这些字符。如果 [text] 不包含任何正则表达式保留字符，Dart 会原样返回该表达式。

正则表达式中的保留字符包括：`(`、`)`、`[`、`]`、`{`、`}`、`*`、`+`、`?`、`.`、`^`、`$`、`|` 和 `\`。

使用此方法可以创建一个模式，用于嵌入到更大的正则表达式中。由于 [String] 本身就是一个能匹配自身的 [Pattern]，因此如果只是要搜索该确切字符串，并不需要将其转换为正则表达式。

```dart
print(RegExp.escape('dash@example.com')); // dash@example\.com
print(RegExp.escape('a+b')); // a\+b
print(RegExp.escape('a*b')); // a\*b
print(RegExp.escape('{a-b}')); // \{a-b\}
print(RegExp.escape('a?')); // a\?
```

## 属性

### pattern

```dart
String get pattern
```

此 `RegExp` 的正则表达式模式源字符串。

```dart
final regExp = RegExp(r'\p{L}');
print(regExp.pattern); // \p{L}
```

### isMultiLine

```dart
bool get isMultiLine
```

此正则表达式是否匹配多行。

如果该正则表达式确实匹配多行，则 "^" 和 "$" 字符会匹配各行的开头和结尾；否则，这两个字符匹配的是整个输入的开头和结尾。

### isCaseSensitive

```dart
bool get isCaseSensitive
```

此正则表达式是否区分大小写。

如果该正则表达式不区分大小写，即使输入字母与模式字母是同一字母的不同大小写形式，它也会将两者视为匹配。

```dart
final text = 'Parse my string';
var regExp = RegExp(r'STRING', caseSensitive: false);
print(regExp.isCaseSensitive); // false
print(regExp.hasMatch(text)); // true, matches.

regExp = RegExp(r'STRING', caseSensitive: true);
print(regExp.isCaseSensitive); // true
print(regExp.hasMatch(text)); // false, no match.
```

### isUnicode

```dart
bool get isUnicode
```

此正则表达式是否使用 Unicode 模式。

在 Unicode 模式下，Dart 会将原始字符串中的 UTF-16 代理对（surrogate pair）视为单个码点，而不会分别匹配代理对中的每个代码单元。否则，Dart 会将目标字符串视为一系列独立代码单元的序列，不对代理对做特殊处理。

在 Unicode 模式下，Dart 会对 RegExp 模式的语法进行限制，例如禁止某些受限正则表达式字符的非转义用法，以及禁止不必要的 `\` 转义（即“恒等转义”）——这两者在非 Unicode 模式下历来都是允许的。此外，Dart 还有一些模式特性（如 Unicode 属性转义）仅在此模式下才被允许使用。

```dart
var regExp = RegExp(r'^\p{L}$', unicode: true);
print(regExp.hasMatch('a')); // true
print(regExp.hasMatch('b')); // true
print(regExp.hasMatch('?')); // false
print(regExp.hasMatch(r'p{L}')); // false

// U+1F600 (😀), one code point, two code units.
var smiley = '\ud83d\ude00';

regExp = RegExp(r'^.$', unicode: true); // Matches one code point.
print(regExp.hasMatch(smiley)); // true
regExp = RegExp(r'^..$', unicode: true); // Matches two code points.
print(regExp.hasMatch(smiley)); // false

regExp = RegExp(r'^\p{L}$', unicode: false);
print(regExp.hasMatch('a')); // false
print(regExp.hasMatch('b')); // false
print(regExp.hasMatch('?')); // false
print(regExp.hasMatch(r'p{L}')); // true

regExp = RegExp(r'^.$', unicode: false);  // Matches one code unit.
print(regExp.hasMatch(smiley)); // false
regExp = RegExp(r'^..$', unicode: false);  // Matches two code units.
print(regExp.hasMatch(smiley)); // true
```

### isDotAll

```dart
bool get isDotAll
```

此正则表达式中的 "." 是否匹配行终止符。

当该属性为 false 时，"." 字符匹配单个字符，但不匹配行终止符；当该属性为 true 时，"." 字符将匹配包括行终止符在内的任意单个字符。

此特性与 [isMultiLine] 不同，二者影响的是不同模式字符的行为，因此既可以搭配使用，也可以单独使用。

## 方法

### firstMatch()

```dart
RegExpMatch? firstMatch(String input)
```

在字符串 [input] 中查找该正则表达式的第一个匹配项。

如果没有匹配项，则返回 `null`。

```dart
final string = '[00:13.37] This is a chat message.';
final regExp = RegExp(r'c\w*');
final match = regExp.firstMatch(string)!;
print(match[0]); // chat
```

### allMatches()

```dart
Iterable<RegExpMatch> allMatches(String input, [int start = 0])
```

### hasMatch()

```dart
bool hasMatch(String input)
```

检查此正则表达式在 [input] 中是否存在匹配项。

```dart
var string = 'Dash is a bird';
var regExp = RegExp(r'(humming)?bird');
var match = regExp.hasMatch(string); // true

regExp = RegExp(r'dog');
match = regExp.hasMatch(string); // false
```

### stringMatch()

```dart
String? stringMatch(String input)
```

查找此正则表达式在 [input] 中第一个匹配项对应的字符串。

与 [firstMatch] 一样，在 [input] 中搜索此正则表达式的匹配项，但如果找到匹配项，只返回匹配到的子字符串，而不是 [RegExpMatch]。

```dart
var string = 'Dash is a bird';
var regExp = RegExp(r'(humming)?bird');
var match = regExp.stringMatch(string); // Match

regExp = RegExp(r'dog');
match = regExp.stringMatch(string); // No match
```

# RegExpMatch

```dart
abstract interface class RegExpMatch implements Match {}
```

正则表达式匹配结果。

正则表达式匹配结果本身就是一个 [Match]。此外，它还具备获取任意命名捕获组名称的能力，并支持通过名称（而非索引）来获取命名捕获组的匹配结果。

示例：

```dart
const pattern =
    r'^\[(?<Time>\s*((?<hour>\d+)):((?<minute>\d+))\.((?<second>\d+)))\]'
    r'\s(?<Message>\s*(.*)$)';

final regExp = RegExp(
  pattern,
  multiLine: true,
);

const multilineText = '[00:13.37] This is a first message.\n'
    '[01:15.57] This is a second message.\n';

RegExpMatch regExpMatch = regExp.firstMatch(multilineText)!;
print(regExpMatch.groupNames.join('-')); // hour-minute-second-Time-Message.
final time = regExpMatch.namedGroup('Time'); // 00:13.37
final hour = regExpMatch.namedGroup('hour'); // 00
final minute = regExpMatch.namedGroup('minute'); // 13
final second = regExpMatch.namedGroup('second'); // 37
final message =
    regExpMatch.namedGroup('Message'); // This is the first message.
final date = regExpMatch.namedGroup('Date'); // Undefined `Date`, throws.

Iterable<RegExpMatch> matches = regExp.allMatches(multilineText);
for (final m in matches) {
  print(m.namedGroup('Time'));
  print(m.namedGroup('Message'));
  // 00:13.37
  // This is the first message.
  // 01:15.57
  // This is the second message.
}
```

## 属性

### groupNames

```dart
Iterable<String> get groupNames
```

[pattern] 中命名捕获组的名称集合。

### pattern

```dart
RegExp get pattern
```

用于在 [input](https://api.flutter.dev/flutter/dart-core/Match/input.html) 中进行搜索的模式。

## 方法

### namedGroup()

```dart
String? namedGroup(String name)
```

由命名捕获组 [name] 捕获到的字符串。

返回输入中被标记为 [name] 的捕获组所匹配到的子字符串；如果该捕获组不属于本次匹配的一部分，则返回 `null`。

[name] 必须是创建此匹配结果的正则表达式 [pattern] 中某个命名捕获组的名称，也就是说，该名称必须存在于 [groupNames] 中。
