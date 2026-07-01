# Duration

```dart
class Duration implements Comparable<Duration> {}
```

一段时间跨度，例如 27 天、4 小时、12 分钟和 3 秒。

`Duration` 表示从一个时间点到另一个时间点的差值。如果差值是从较晚的时间到较早的时间，则该时长可能为"负数"。

时长与上下文无关。例如，2 天的时长始终等于 48 小时，即使将其添加到某个即将发生夏令时切换的 `DateTime` 上也是如此。（参见 [DateTime.add]）。

尽管名称相同，`Duration` 对象并未实现 ISO 8601 规范中所定义的"时长"。具体而言，时长对象不会记录各个单独提供的成员（如"天"或"小时"），而只是利用这些参数来计算对应时间间隔的长度。

要创建新的 `Duration` 对象，请使用该类的单一构造函数并传入相应的参数：

```dart
const fastestMarathon = Duration(hours: 2, minutes: 3, seconds: 2);
```

[Duration] 表示一个单一的微秒数，它是构造函数所有单个参数之和。

属性可以通过不同方式访问这个单一数值。例如 [inMinutes] 给出总时长中的整分钟数，其中包括以"小时"形式提供给构造函数的分钟数，因此该值可以大于 59。

```dart
const fastestMarathon = Duration(hours: 2, minutes: 0, seconds: 35);
print(fastestMarathon.inDays); // 0
print(fastestMarathon.inHours); // 2
print(fastestMarathon.inMinutes); // 120
print(fastestMarathon.inSeconds); // 7235
print(fastestMarathon.inMilliseconds); // 7235000
```

时长可以为负数，此时从该时长派生出的所有属性也均为非正数。

```dart
const overDayAgo = Duration(days: -1, hours: -10);
print(overDayAgo.inDays); // -1
print(overDayAgo.inHours); // -34
print(overDayAgo.inMinutes); // -2040
```

使用诸如 [inDays] 之类的属性之一，可以获取以指定时间单位表示的 `Duration` 整数值。请注意，返回值会向下取整。例如，

```dart
const aLongWeekend = Duration(hours: 88);
print(aLongWeekend.inDays); // 3
```

该类提供了一系列算术和比较运算符，以及一组用于转换时间单位的常量。

```dart
const firstHalf = Duration(minutes: 45); // 00:45:00.000000
const secondHalf = Duration(minutes: 45); // 00:45:00.000000
const overTime = Duration(minutes: 30); // 00:30:00.000000
final maxGameTime = firstHalf + secondHalf + overTime;
print(maxGameTime.inMinutes); // 120

// The duration of the firstHalf and secondHalf is the same, returns 0.
var result = firstHalf.compareTo(secondHalf);
print(result); // 0

// Duration of overTime is shorter than firstHalf, returns < 0.
result = overTime.compareTo(firstHalf);
print(result); // < 0

// Duration of secondHalf is longer than overTime, returns > 0.
result = secondHalf.compareTo(overTime);
print(result); // > 0
```

**另请参阅：**

- [DateTime] 用于表示某个时间点。
- [Stopwatch] 用于测量时间跨度。

## 构造函数

### Duration()

```dart
Duration({int days = 0, int hours = 0, int minutes = 0, int seconds = 0, int milliseconds = 0, int microseconds = 0})
```

创建一个新的 [Duration] 对象，其值为所有单独部分之和。

各个部分的值可以大于该部分在下一个更大单位中所对应的数量。例如，[hours] 可以大于 23。如果出现这种情况，该值会溢出到下一个更大的单位，因此 26 [hours] 等同于 2 [hours] 加上多出的 1 个 [days]。同样，各部分的值也可以为负数，此时会向下一个更大的单位借位相减。

如果微秒总数无法用整数值表示，微秒数可能会溢出并被截断为较少的位数，或者可能会丢失精度。

所有参数默认值均为 0。

```dart
const duration = Duration(days: 1, hours: 8, minutes: 56, seconds: 59,
  milliseconds: 30, microseconds: 10);
print(duration); // 32:56:59.030010
```

## 静态属性

### microsecondsPerMillisecond

```dart
static const int microsecondsPerMillisecond = 1000;
```

每毫秒的微秒数。

### millisecondsPerSecond

```dart
static const int millisecondsPerSecond = 1000;
```

每秒的毫秒数。

### secondsPerMinute

```dart
static const int secondsPerMinute = 60;
```

每分钟的秒数。

请注意，由于闰秒的存在，官方时钟时间中某些分钟的实际长度可能有所不同。[Duration] 和 [DateTime] 类会忽略闰秒，并将所有分钟都视为拥有 60 秒。

### minutesPerHour

```dart
static const int minutesPerHour = 60;
```

每小时的分钟数。

### hoursPerDay

```dart
static const int hoursPerDay = 24;
```

每天的小时数。

请注意，由于夏令时导致的时区变化，某些天的实际长度可能有所不同。[Duration] 类与时区无关，会将所有天都视为拥有 24 小时。

### microsecondsPerSecond

```dart
static const int microsecondsPerSecond =
  	microsecondsPerMillisecond * millisecondsPerSecond;
```

每秒的微秒数。

### microsecondsPerMinute

```dart
static const int microsecondsPerMinute =
  	microsecondsPerSecond * secondsPerMinute;
```

每分钟的微秒数。

### microsecondsPerHour

```dart
static const int microsecondsPerHour = microsecondsPerMinute * minutesPerHour;
```

每小时的微秒数。

### microsecondsPerDay

```dart
static const int microsecondsPerDay = microsecondsPerHour * hoursPerDay;
```

每天的微秒数。

### millisecondsPerMinute

```dart
static const int millisecondsPerMinute =
  	millisecondsPerSecond * secondsPerMinute;
```

每分钟的毫秒数。

### millisecondsPerHour

```dart
static const int millisecondsPerHour = millisecondsPerMinute * minutesPerHour;
```

每小时的毫秒数。

### millisecondsPerDay

```dart
static const int millisecondsPerDay = millisecondsPerHour * hoursPerDay;
```

每天的毫秒数。

### secondsPerHour

```dart
static const int secondsPerHour = secondsPerMinute * minutesPerHour;
```

每小时的秒数。

### secondsPerDay

```dart
static const int secondsPerDay = secondsPerHour * hoursPerDay;
```

每天的秒数。

### minutesPerDay

```dart
static const int minutesPerDay = minutesPerHour * hoursPerDay;
```

每天的分钟数。

### zero

```dart
static const Duration zero = Duration(seconds: 0);
```

一个空时长，表示零时间。

## 属性

### inDays

```dart
int get inDays
```

此 [Duration] 所跨越的整天数。

例如，4 天 3 小时的时长拥有 4 个整天。

```dart
const duration = Duration(days: 4, hours: 3);
print(duration.inDays); // 4
```

### inHours

```dart
int get inHours
```

此 [Duration] 所跨越的整小时数。

返回值可以大于 23。例如，4 天 3 小时的时长拥有 99 个整小时。

```dart
const duration = Duration(days: 4, hours: 3);
print(duration.inHours); // 99
```

### inMinutes

```dart
int get inMinutes
```

此 [Duration] 所跨越的整分钟数。

返回值可以大于 59。例如，3 小时 12 分钟的时长共有 192 分钟。

```dart
const duration = Duration(hours: 3, minutes: 12);
print(duration.inMinutes); // 192
```

### inSeconds

```dart
int get inSeconds
```

此 [Duration] 所跨越的整秒数。

返回值可以大于 59。例如，3 分钟 12 秒的时长共有 192 秒。

```dart
const duration = Duration(minutes: 3, seconds: 12);
print(duration.inSeconds); // 192
```

### inMilliseconds

```dart
int get inMilliseconds
```

此 [Duration] 所跨越的整毫秒数。

返回值可以大于 999。例如，3 秒 125 毫秒的时长共有 3125 毫秒。

```dart
const duration = Duration(seconds: 3, milliseconds: 125);
print(duration.inMilliseconds); // 3125
```

### inMicroseconds

```dart
int get inMicroseconds
```

此 [Duration] 所跨越的整微秒数。

返回值可以大于 999999。例如，3 秒、125 毫秒和 369 微秒的时长共有 3125369 微秒。

```dart
const duration = Duration(seconds: 3, milliseconds: 125,
    microseconds: 369);
print(duration.inMicroseconds); // 3125369
```

### hashCode

此对象的哈希码。

哈希码是表示对象状态的单个整数，该状态会影响 [operator ==](https://api.flutter.dev/flutter/dart-core/Duration/operator_equals.html) 比较结果。

所有对象都拥有哈希码。[Object](https://api.flutter.dev/flutter/dart-core/Object-class.html) 所实现的默认哈希码仅表示对象的标识，这与默认 [operator ==](https://api.flutter.dev/flutter/dart-core/Duration/operator_equals.html) 实现仅在对象完全相同时才判定其相等的方式相同（参见 [identityHashCode](https://api.flutter.dev/flutter/dart-core/identityHashCode.html)）。

如果重写 [operator ==](https://api.flutter.dev/flutter/dart-core/Duration/operator_equals.html) 以改用对象状态进行比较，则哈希码也必须相应更改以表示该状态，否则该对象将无法用于基于哈希的数据结构，例如默认的 [Set](https://api.flutter.dev/flutter/dart-core/Set-class.html) 和 [Map](https://api.flutter.dev/flutter/dart-core/Map-class.html) 实现。

根据 [operator ==](https://api.flutter.dev/flutter/dart-core/Duration/operator_equals.html) 判定为相等的对象，其哈希码必须相同。对象的哈希码只应在该对象发生影响相等性的变化时才改变。除此之外，对哈希码没有其他要求：它们无需在同一程序的多次运行之间保持一致，也不保证其分布特性。

不相等的对象允许拥有相同的哈希码。从技术上讲，甚至允许所有实例都具有相同的哈希码，但如果哈希冲突过于频繁，可能会降低诸如 [HashSet](https://api.flutter.dev/flutter/dart-collection/HashSet-class.html) 或 [HashMap](https://api.flutter.dev/flutter/dart-collection/HashMap-class.html) 等基于哈希的数据结构的效率。

如果子类重写了 [hashCode](https://api.flutter.dev/flutter/dart-core/Duration/hashCode.html)，也应同时重写 [operator ==](https://api.flutter.dev/flutter/dart-core/Duration/operator_equals.html) 运算符，以保持一致性。

```dart
int get hashCode
```

### isNegative

```dart
bool get isNegative
```

此 [Duration] 是否为负数。

负数的 [Duration] 表示从较晚时间到较早时间的差值。

## 方法

### compareTo()

```dart
int compareTo(Duration other)
```

将此 [Duration] 与 [other] 进行比较，如果值相等则返回零。

如果此 [Duration] 短于 [other]，则返回负整数；如果长于 [other]，则返回正整数。

负数的 [Duration] 始终被视为比正数的 [Duration] 短。

始终满足：当且仅当 `(someDate + duration1).compareTo(someDate + duration2) < 0` 时，`duration1.compareTo(duration2) < 0` 成立。

### toString()

```dart
String toString()
```

返回此 [Duration] 的字符串表示形式。

返回一个包含小时、分钟、秒和微秒的字符串，格式如下：`H:MM:SS.mmmmmm`。例如，

```dart
var d = const Duration(days: 1, hours: 1, minutes: 33, microseconds: 500);
print(d.toString()); // 25:33:00.000500

d = const Duration(hours: 1, minutes: 10, microseconds: 500);
print(d.toString()); // 1:10:00.000500
```

### abs()

```dart
Duration abs()
```

创建一个新的 [Duration]，表示此 [Duration] 的绝对长度。

返回的 [Duration] 与此时长长度相同，但在可能的情况下始终为正数。

## 运算符

### operator +

```dart
Duration operator +(Duration other)
```

将此 Duration 与 [other] 相加，并将结果作为新的 Duration 对象返回。

### operator -

```dart
Duration operator -(Duration other)
```

从此 Duration 中减去 [other]，并将差值作为新的 Duration 对象返回。

### operator \*

```dart
Duration operator *(num factor)
```

将此 Duration 乘以给定的 [factor]，并将结果作为新的 Duration 对象返回。

请注意，当 [factor] 为 double 类型且时长超过 53 位时，由于双精度浮点运算的限制会导致精度丢失。

### operator ~/

```dart
Duration operator ~/(int quotient)
```

将此 Duration 除以给定的 [quotient]，并将截断后的结果作为新的 Duration 对象返回。

[quotient] 不得为 `0`。

### operator <

```dart
bool operator <(Duration other)
```

此 [Duration] 是否短于 [other]。

### operator >

```dart
bool operator >(Duration other)
```

此 [Duration] 是否长于 [other]。

### operator <=

```dart
bool operator <=(Duration other)
```

此 [Duration] 是否短于或等于 [other]。

### operator >=

```dart
bool operator >=(Duration other)
```

此 [Duration] 是否长于或等于 [other]。

### operator ==

```dart
bool operator ==(Object other)
```

此 [Duration] 的长度是否与 [other] 相同。

如果两个时长根据 [inMicroseconds] 所报告的微秒数相同，则视为长度相同。

### operator unary-

```dart
Duration operator -()
```

创建一个新的 [Duration]，其方向与此 [Duration] 相反。

返回的 [Duration] 与此时长长度相同，但在可能的情况下其符号（由 [isNegative] 所报告）将与此时长相反。
