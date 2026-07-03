# Pattern

```dart
abstract interface class Pattern {}
```

用于在字符串中进行基本搜索的接口。

## 方法

### allMatches()

```dart
Iterable<Match> allMatches(String string, [int start = 0])
```

将此模式与字符串反复匹配。

如果提供了 [start]，匹配将从该索引处开始。

返回的可迭代对象会惰性地查找 [string] 中该模式的非重叠匹配项。如果用户只请求第一个匹配项，此函数不应计算所有可能的匹配项。

匹配的查找方式是：从 [start] 开始，反复查找字符串中该模式的第一个匹配项，然后从上一个匹配项结束处继续查找（但为防止模式匹配空子串，至少要比上一个匹配项的*起始位置*晚一个位置）。

```dart
RegExp exp = RegExp(r'(\w+)');
var str = 'Dash is a bird';
Iterable<Match> matches = exp.allMatches(str, 8);
for (final Match m in matches) {
  String match = m[0]!;
  print(match);
}
```

该示例的输出为：

```
a
bird
```

### matchAsPrefix()

```dart
Match? matchAsPrefix(String string, [int start = 0])
```

将此模式与 `string` 的起始部分进行匹配。

如果模式与 [string] 中从 [start] 开始的子串匹配，则返回一个匹配项；如果模式在该位置不匹配，则返回 `null`。

[start] 必须为非负数，且不得大于 `string.length`。

```dart
final string = 'Dash is a bird';

var regExp = RegExp(r'bird');
var match = regExp.matchAsPrefix(string, 10); // Match found.

regExp = RegExp(r'bird');
match = regExp.matchAsPrefix(string); // null
```

# Match

```dart
abstract interface class Match {}
```

字符串搜索的结果。

`Match` 或 [Match] 对象的 [Iterable] 是从 [Pattern] 的匹配方法（[Pattern.allMatches] 和 [Pattern.matchAsPrefix]）中返回的。

以下示例在一个 [String] 中查找 [RegExp] 的所有匹配项，并遍历返回的 `Match` 对象可迭代集合。

```dart
final regExp = RegExp(r'(\w+)');
const string = 'Parse my string';
final matches = regExp.allMatches(string);
for (final m in matches) {
  String match = m[0]!;
  print(match);
}
```

该示例的输出为：

```
Parse
my
string
```

某些模式（尤其是正则表达式）可能会记录参与匹配的部分子串，这些被称为 `Match` 对象中的*分组*（group）。有些模式可能永远不会有任何分组，其匹配项的 [groupCount] 始终为零。

## 属性

### start

```dart
int get start
```

字符串中匹配开始的索引位置。

### end

```dart
int get end
```

字符串中匹配的最后一个字符之后的索引位置。

### groupCount

```dart
int get groupCount
```

返回匹配中捕获的分组数量。

某些模式可能会捕获用于计算完整匹配的输入部分。这就是捕获分组的数量，也是 [group] 方法所允许的最大参数值。

### input

```dart
String get input
```

计算此匹配所依据的字符串。

### pattern

```dart
Pattern get pattern
```

用于在 [input] 中进行搜索的模式。

## 方法

### group()

```dart
String? group(int group)
```

给定 [group] 所匹配的字符串。

如果 [group] 为 0，则返回该模式的整个匹配项。

如果模式在此次匹配中未为其赋值，结果可能为 `null`。

```dart import:convert

final string = '[00:13.37] This is a chat message.';
final regExp = RegExp(r'^\[\s*(\d+):(\d+)\.(\d+)\]\s*(.*)$');
final match = regExp.firstMatch(string)!;
final message = jsonEncode(match.group(0)!); // '[00:13.37] This is a chat message.'
final hours = jsonEncode(match.group(1)!); // '00'
final minutes = jsonEncode(match.group(2)!); // '13'
final seconds = jsonEncode(match.group(3)!); // '37'
final text = jsonEncode(match.group(4)!); // 'This is a chat message.'
```

### groups()

```dart
List<String?> groups(List<int> groupIndices)
```

给定索引对应的分组列表。

该列表包含针对 [groupIndices] 中每个索引调用 [group] 所返回的字符串。

```dart import:convert
final string = '[00:13.37] This is a chat message.';
final regExp = RegExp(r'^\[\s*(\d+):(\d+)\.(\d+)\]\s*(.*)$');
final match = regExp.firstMatch(string)!;
final message = jsonEncode(match.groups([1, 2, 3, 4]));
// ['00','13','37','This is a chat message.']
```

## 运算符

### operator []

```dart
String? operator [](int group)
```

给定 [group] 所匹配的字符串。

如果 [group] 为 0，返回该模式的匹配项。

是 [Match.group] 的简写别名。
