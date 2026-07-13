# DateTime

```dart
class DateTime implements Comparable<DateTime> {}
```

时间中的一个瞬间，例如 1969 年 7 月 20 日晚上 8:18（GMT）。

`DateTime` 可以表示与纪元（1970-01-01 UTC）相距最多 100,000,000 天的时间值：-271821-04-20 到 275760-09-13。

可以使用其中一个构造函数创建 `DateTime` 对象，也可以通过解析符合 ISO 8601 子集格式的字符串来创建。**注意：** 小时的取值范围为 0 到 23，采用 24 小时制。

例如：

```dart
final now = DateTime.now();
final berlinWallFell = DateTime.utc(1989, 11, 9);
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z'); // 8:18pm
```

`DateTime` 对象要么锚定在 UTC 时区，要么锚定在创建该对象时所在计算机的本地时区。

`DateTime` 对象一旦创建，其值和时区都不可更改。

可以使用属性获取 `DateTime` 对象的各个组成部分。

```
print(berlinWallFell.year); // 1989
print(berlinWallFell.month); // 11
print(berlinWallFell.day); // 9
print(moonLanding.hour); // 20
print(moonLanding.minute); // 18
```

为了方便和提高可读性，`DateTime` 类为每个 `day` 和 `month` 名称都提供了一个常量——例如 [august] 和 [friday]。可以使用这些常量来提高代码可读性：

```dart
final berlinWallFell = DateTime.utc(1989, DateTime.november, 9);
print(DateTime.november); // 11
assert(berlinWallFell.month == DateTime.november);
assert(berlinWallFell.weekday == DateTime.thursday);
```

`Day` 和 `month` 的取值从 1 开始，且一周从 `Monday` 开始计算。也就是说，常量 [january] 和 [monday] 的值均为 1。

## 使用 UTC 和本地时间

除非显式在 UTC 时区中创建，否则 `DateTime` 对象将处于本地时区。使用 [isUtc] 可判断某个 `DateTime` 对象是否基于 UTC。

```dart
final dDay = DateTime.utc(1944, 6, 6);
print(dDay.isUtc); // true

final dDayLocal = DateTime(1944, 6, 6);
print(dDayLocal.isUtc); // false
```

使用 [toLocal] 和 [toUtc] 方法可获取以另一时区表示的等效日期/时间值。

```
final localDay = dDay.toLocal(); // e.g. 1944-06-06 02:00:00.000
print(localDay.isUtc); // false

final utcFromLocal = localDay.toUtc(); // 1944-06-06 00:00:00.000Z
print(utcFromLocal.isUtc); // true
```

使用 [timeZoneName] 可获取 `DateTime` 对象所在时区的缩写名称。

```
print(dDay.timeZoneName); // UTC
print(localDay.timeZoneName); // e.g. EET
```

要查找 UTC 与 `DateTime` 对象所在时区之间的差值，请调用 [timeZoneOffset]。

```
print(dDay.timeZoneOffset); // 0:00:00.000000
print(localDay.timeZoneOffset); // e.g. 2:00:00.000000
```

## 比较 DateTime 对象

`DateTime` 类包含用于按时间顺序比较 `DateTime` 的方法，例如 [isAfter]、[isBefore] 和 [isAtSameMomentAs]。

```
print(berlinWallFell.isAfter(moonLanding)); // true
print(berlinWallFell.isBefore(moonLanding)); // false
print(dDay.isAtSameMomentAs(localDay)); // true
```

## 将 DateTime 与 Duration 结合使用

使用 [add] 和 [subtract] 方法配合 [Duration](https://www.yuque.com/thyname/dart.core/duration) 对象，可以基于另一个 `DateTime` 创建新的 `DateTime` 对象。例如，要找到从现在起 36 小时后的时间点，可以这样写：

```dart
final now = DateTime.now();
final later = now.add(const Duration(hours: 36));
```

要计算两个 `DateTime` 对象之间相隔的时间，请使用 [difference]，它会返回一个 [Duration](https://www.yuque.com/thyname/dart.core/duration) 对象：

```
final difference = berlinWallFell.difference(moonLanding);
print(difference.inDays); // 7416
```

处于不同时区的两个日期之间的差值，仅仅是两个时间点之间相隔的纳秒数。它不考虑日历天数。这意味着，如果两个日期之间存在夏令时变更，那么以本地时间表示的两个午夜时刻之间的差值，可能小于二者相隔天数乘以 24 小时的结果。如果使用澳大利亚本地时间计算上述差值，结果为 7415 天 23 小时，而 `inDays` 报告的整天数仅为 7415 天。

## 其他资源

- 请参阅 [Duration](https://www.yuque.com/thyname/dart.core/duration) 以表示一段时间跨度。
- 请参阅 [Stopwatch](https://www.yuque.com/thyname/dart.core/stopwatch) 以测量时间跨度。
- `DateTime` 类不提供国际化支持。如需为代码添加国际化支持，请使用 [intl](https://pub.dev/packages/intl) 包。

## 构造函数

### DateTime()

```dart
DateTime(
  int year, [
  int month = 1,
  int day = 1,
  int hour = 0,
  int minute = 0,
  int second = 0,
  int millisecond = 0,
  int microsecond = 0,
])
```

构造一个以本地时区表示的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

例如，创建一个表示 2017 年 9 月 7 日下午 5:30 的 `DateTime` 对象：

```dart
final dentistAppointment = DateTime(2017, 9, 7, 17, 30);
```

### DateTime.utc()

```dart
DateTime.utc(
  int year, [
  int month = 1,
  int day = 1,
  int hour = 0,
  int minute = 0,
  int second = 0,
  int millisecond = 0,
  int microsecond = 0,
])
```

构造一个以 UTC 时区表示的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

```dart
final moonLanding = DateTime.utc(1969, 7, 20, 20, 18, 04);
```

在处理日期或历史事件时，建议优先使用 UTC 的 `DateTime`，因为它们不受夏令时变更的影响，也不受本地时区的影响。

### DateTime.now()

```dart
DateTime.now()
```

构造一个表示本地时区当前日期和时间的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

```dart
final now = DateTime.now();
```

### DateTime.timestamp()

```dart
@Since.new("3.0")
DateTime.timestamp()
```

构造一个表示当前 UTC 日期和时间的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime)。

```dart
final mark = DateTime.timestamp();
```

### DateTime.fromMillisecondsSinceEpoch()

```dart
DateTime.fromMillisecondsSinceEpoch(
  int millisecondsSinceEpoch, {
  bool isUtc = false,
})
```

根据给定的 [millisecondsSinceEpoch] 构造一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

如果 [isUtc] 为 false，则该日期处于本地时区。

构造出的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 表示在给定时区（本地或 UTC）中，1970-01-01T00:00:00Z + [millisecondsSinceEpoch] 毫秒所对应的时间。

```dart
final newYearsDay =
    DateTime.fromMillisecondsSinceEpoch(1641031200000, isUtc:true);
print(newYearsDay); // 2022-01-01 10:00:00.000Z
```

### DateTime.fromMicrosecondsSinceEpoch()

```dart
DateTime.fromMicrosecondsSinceEpoch(
  int microsecondsSinceEpoch, {
  bool isUtc = false,
})
```

根据给定的 [microsecondsSinceEpoch] 构造一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

如果 [isUtc] 为 false，则该日期处于本地时区。

构造出的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 表示在给定时区（本地或 UTC）中，1970-01-01T00:00:00Z + [microsecondsSinceEpoch] 微秒所对应的时间。

```dart
final newYearsEve =
    DateTime.fromMicrosecondsSinceEpoch(1640979000000000, isUtc:true);
print(newYearsEve); // 2021-12-31 19:30:00.000Z
```

## 静态属性

### 星期常量

```dart
static const int monday = 1;
static const int tuesday = 2;
static const int wednesday = 3;
static const int thursday = 4;
static const int friday = 5;
static const int saturday = 6;
static const int sunday = 7;
static const int daysPerWeek = 7;
```

### 月份常量

```dart
static const int january = 1;
static const int february = 2;
static const int march = 3;
static const int april = 4;
static const int may = 5;
static const int june = 6;
static const int july = 7;
static const int august = 8;
static const int september = 9;
static const int october = 10;
static const int november = 11;
static const int december = 12;
static const int monthsPerYear = 12;
```

## 静态方法

### parse()

```dart
DateTime parse(String formattedString)
```

根据 [formattedString] 构造一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

如果输入字符串无法解析，将抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

该函数解析 ISO 8601 的一个子集，其中包括 RFC 3339 所接受的子集。

目前可接受的输入包括：

- 日期：一个带符号的四到六位年份、两位月份和两位日期，可选地由 `-` 字符分隔。示例："19700101"、"-0004-12-24"、"81030-04-01"。
- 一个可选的时间部分，与日期部分之间以 `T` 或空格分隔。时间部分为两位数的小时，随后可选地跟两位数的分钟值，再可选地跟两位数的秒值，之后可选地跟一个 `.` 或 `,`，后接至少一位数的秒的小数部分。分钟和秒可以用 `:` 与前面的部分分隔。示例："12"、"12:30:24.124"、"12:30:24,124"、"123010.50"。
- 一个可选的时区偏移部分，可能与前面部分之间以空格分隔。时区可以是 'z' 或 'Z'，也可以是带符号的两位数小时部分和可选的两位数分钟部分。符号必须为 "+" 或 "-"，且不能省略。分钟部分可以用 `:` 与小时部分分隔。示例："Z"、"-10"、"+01:30"、"+1130"。

这包括 [toString] 和 [toIso8601String] 的输出，它们都可以被解析回一个与原始时间相同的 `DateTime` 对象。

结果始终为本地时间或 UTC 时间。如果指定了非 UTC 的时区偏移，则该时间会被转换为等效的 UTC 时间。

可接受字符串示例：

- `"2012-02-27"`
- `"2012-02-27 13:27:00"`
- `"2012-02-27 13:27:00.123456789z"`
- `"2012-02-27 13:27:00,123456789z"`
- `"20120227 13:27:00"`
- `"20120227T132700"`
- `"20120227"`
- `"+20120227"`
- `"2012-02-27T14Z"`
- `"2012-02-27T14+00:00"`
- `"-123450101 00:00:00 Z"`：表示公元前 12345 年。
- `"2002-02-27T14:00:00-0500"`：与 `"2002-02-27T19:00:00Z"` 相同。

该方法接受超出范围的组件值，并将其解释为向下一个更大组件的溢出。例如，"2020-01-42" 将被解析为 2020-02-11，因为该月最后一个有效日期是 2020-01-31，所以 42 天被解释为该月的 31 天加上下个月的 11 天。

如需检测并拒绝无效的组件值，请使用 [intl](https://pub.dev/packages/intl) 包中的 [DateFormat.parseStrict](https://pub.dev/documentation/intl/latest/intl/DateFormat/parseStrict.html)。

### tryParse()

```dart
DateTime? tryParse(String formattedString)
```

根据 [formattedString] 构造一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例。

其工作方式与 [parse] 类似，不同之处在于，当 [parse] 会抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception) 时，该函数会返回 `null`。

## 属性

### isUtc

```dart
bool isUtc
```

如果该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 设置为 UTC 时间，则为 true。

```dart
final dDay = DateTime.utc(1944, 6, 6);
print(dDay.isUtc); // true

final local = DateTime(1944, 6, 6);
print(local.isUtc); // false
```

### millisecondsSinceEpoch

```dart
int get millisecondsSinceEpoch
```

自 "Unix 纪元" 1970-01-01T00:00:00Z（UTC）以来经过的毫秒数。

该值与时区无关。

该值与 Unix 纪元最多相距 8,640,000,000,000,000 毫秒（100,000,000 天）。换句话说：`millisecondsSinceEpoch.abs() <= 8640000000000000`。

### microsecondsSinceEpoch

```dart
int get microsecondsSinceEpoch
```

自 "Unix 纪元" 1970-01-01T00:00:00Z（UTC）以来经过的微秒数。

该值与时区无关。

该值与 Unix 纪元最多相距 8,640,000,000,000,000,000 微秒（100,000,000 天）。换句话说：`microsecondsSinceEpoch.abs() <= 8640000000000000000`。

请注意，该值并不总能容纳于 53 位（IEEE double 的位数）之内。在 Web JavaScript 平台上，对于距纪元足够远的 DateTime 值，可能存在舍入误差。接近纪元且不会出现舍入误差的年份范围大约为 1685 到 2254 年。

### timeZoneName

```dart
String get timeZoneName
```

时区名称。

该值由操作系统提供，可能是缩写或全称。

在浏览器或类 Unix 系统上，通常返回缩写形式，例如 "CET" 或 "CEST"。在 Windows 上则返回全称，例如 "Pacific Standard Time"。

### timeZoneOffset

```dart
Duration get timeZoneOffset
```

时区偏移量，即本地时间与 UTC 之间的差值。

对于 UTC 以东的时区，该偏移量为正值。

请注意，JavaScript、Python 和 C 返回的是 UTC 与本地时间之间的差值。而 Java、C# 和 Ruby 返回的是本地时间与 UTC 之间的差值。

例如，使用美国旧金山的本地时间：

```dart
final dateUS = DateTime.parse('2021-11-01 20:18:04Z').toLocal();
print(dateUS); // 2021-11-01 13:18:04.000
print(dateUS.timeZoneName); // PDT ( Pacific Daylight Time )
print(dateUS.timeZoneOffset.inHours); // -7
print(dateUS.timeZoneOffset.inMinutes); // -420
```

例如，使用澳大利亚堪培拉的本地时间：

```dart
final dateAus = DateTime.parse('2021-11-01 20:18:04Z').toLocal();
print(dateAus); // 2021-11-02 07:18:04.000
print(dateAus.timeZoneName); // AEDT ( Australian Eastern Daylight Time )
print(dateAus.timeZoneOffset.inHours); // 11
print(dateAus.timeZoneOffset.inMinutes); // 660
```

### year

```dart
int get year
```

年份。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.year); // 1969
```

### month

```dart
int get month
```

月份，取值范围 `[1..12]`。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.month); // 7
assert(moonLanding.month == DateTime.july);
```

### day

```dart
int get day
```

月份中的日期，取值范围 `[1..31]`。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.day); // 20
```

### hour

```dart
int get hour
```

一天中的小时数，以 24 小时制表示，取值范围 `[0..23]`。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.hour); // 20
```

### minute

```dart
int get minute
```

分钟，取值范围 `[0...59]`。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.minute); // 18
```

### second

```dart
int get second
```

秒，取值范围 `[0...59]`。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.second); // 4
```

### millisecond

```dart
int get millisecond
```

毫秒，取值范围 `[0...999]`。

```dart
final date = DateTime.parse('1970-01-01 05:01:01.234567Z');
print(date.millisecond); // 234
```

### microsecond

```dart
int get microsecond
```

微秒，取值范围 `[0...999]`。

```dart
final date = DateTime.parse('1970-01-01 05:01:01.234567Z');
print(date.microsecond); // 567
```

### weekday

```dart
int get weekday
```

星期几，取值范围为 [monday] 到 [sunday]。

按照 ISO 8601 的规定，一周从星期一开始，其值为 1。

```dart
final moonLanding = DateTime.parse('1969-07-20 20:18:04Z');
print(moonLanding.weekday); // 7
assert(moonLanding.weekday == DateTime.sunday);
```

### hashCode

```dart
int get hashCode
```

## 方法

### isBefore()

```dart
bool isBefore(DateTime other)
```

判断该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 是否发生在 `other` 之前。

该比较与时间是 UTC 还是本地时区无关。

```dart
final now = DateTime.now();
final earlier = now.subtract(const Duration(seconds: 5));
print(earlier.isBefore(now)); // true
print(!now.isBefore(now)); // true

// This relation stays the same, even when changing timezones.
print(earlier.isBefore(now.toUtc())); // true
print(earlier.toUtc().isBefore(now)); // true

print(!now.toUtc().isBefore(now)); // true
print(!now.isBefore(now.toUtc())); // true
```

### isAfter()

```dart
bool isAfter(DateTime other)
```

判断该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 是否发生在 `other` 之后。

该比较与时间是 UTC 还是本地时区无关。

```dart
final now = DateTime.now();
final later = now.add(const Duration(seconds: 5));
print(later.isAfter(now)); // true
print(!now.isBefore(now)); // true

// This relation stays the same, even when changing timezones.
print(later.isAfter(now.toUtc())); // true
print(later.toUtc().isAfter(now)); // true

print(!now.toUtc().isAfter(now)); // true
print(!now.isAfter(now.toUtc())); // true
```

### isAtSameMomentAs()

```dart
bool isAtSameMomentAs(DateTime other)
```

判断该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 是否与 `other` 处于同一时刻。

该比较与时间是 UTC 还是本地时区无关。

```dart
final now = DateTime.now();
final later = now.add(const Duration(seconds: 5));
print(!later.isAtSameMomentAs(now)); // true
print(now.isAtSameMomentAs(now)); // true

// This relation stays the same, even when changing timezones.
print(!later.isAtSameMomentAs(now.toUtc())); // true
print(!later.toUtc().isAtSameMomentAs(now)); // true

print(now.toUtc().isAtSameMomentAs(now)); // true
print(now.isAtSameMomentAs(now.toUtc())); // true
```

### compareTo()

```dart
int compareTo(DateTime other)
```

将该 DateTime 对象与 `other` 进行比较，若两者值相等，返回零。

[compareTo] 函数返回：

- 若该 DateTime [isBefore] `other`，返回负值。
- 若该 DateTime [isAtSameMomentAs] `other`，返回 `0`；
- 否则（即该 DateTime [isAfter] `other` 时）返回正值。

```dart
final now = DateTime.now();
final future = now.add(const Duration(days: 2));
final past = now.subtract(const Duration(days: 2));
final newDate = now.toUtc();

print(now.compareTo(future)); // -1
print(now.compareTo(past)); // 1
print(now.compareTo(newDate)); // 0
```

### toLocal()

```dart
DateTime toLocal()
```

以本地时区返回该 DateTime 值。

如果该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 已经处于本地时区，则直接返回该对象本身。否则，该方法等价于：

```dart template:expression
DateTime.fromMicrosecondsSinceEpoch(microsecondsSinceEpoch,
                                    isUtc: false)
```

### toUtc()

```dart
DateTime toUtc()
```

以 UTC 时区返回该 DateTime 值。

如果该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 已经处于 UTC，则直接返回该对象本身。否则，该方法等价于：

```dart template:expression
DateTime.fromMicrosecondsSinceEpoch(microsecondsSinceEpoch,
                                    isUtc: true)
```

### toString()

```dart
String toString()
```

返回该实例的人类可读字符串表示。

返回的字符串是针对该实例所在的时区构造的。`toString()` 方法提供一种简单的格式化字符串，不支持国际化字符串。请使用 pub 共享包仓库中的 [intl](https://pub.dev/packages/intl) 包。

生成的字符串可以使用 [parse] 重新解析。

### toIso8601String()

```dart
String toIso8601String()
```

返回 ISO-8601 全精度扩展格式表示。

对于 UTC 时间，格式为 `yyyy-MM-ddTHH:mm:ss.mmmuuuZ`；对于本地/非 UTC 时间，格式为 `yyyy-MM-ddTHH:mm:ss.mmmuuu`（不带末尾的 "Z"），其中：

- `yyyy` 是年份的四位数表示（可能为负数），如果年份在 -9999 到 9999 范围内；否则为带符号的六位数表示。
- `MM` 是月份，取值范围为 01 到 12，
- `dd` 是月份中的日期，取值范围为 01 到 31，
- `HH` 是小时，取值范围为 00 到 23，
- `mm` 是分钟，取值范围为 00 到 59，
- `ss` 是秒，取值范围为 00 到 59（不含闰秒），
- `mmm` 是毫秒，取值范围为 000 到 999，
- `uuu` 是微秒，取值范围为 001 到 999。如果 [microsecond] 等于 0，则省略该部分。

生成的字符串可以使用 [parse] 重新解析。

```dart
final moonLanding = DateTime.utc(1969, 7, 20, 20, 18, 04);
final isoDate = moonLanding.toIso8601String();
print(isoDate); // 1969-07-20T20:18:04.000Z
```

### add()

```dart
DateTime add(Duration duration)
```

返回一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例，该实例是在该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 的基础上加上 [duration] 得到的。

```dart
final today = DateTime.now();
final fiftyDaysFromNow = today.add(const Duration(days: 50));
```

请注意，所加上的时间跨度实际上是 50 _ 24 _ 60 \* 60 秒。如果得到的 `DateTime` 与 `this` 的夏令时偏移不同，那么结果将不会与 `this` 具有相同的时刻，甚至可能不会正好落在 50 天后的那个日历日期上。

在处理本地时间的日期时请务必谨慎。

### subtract()

```dart
DateTime subtract(Duration duration)
```

返回一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 实例，该实例是在该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 的基础上减去 [duration] 得到的。

```dart
final today = DateTime.now();
final fiftyDaysAgo = today.subtract(const Duration(days: 50));
```

请注意，所减去的时间跨度实际上是 50 _ 24 _ 60 \* 60 秒。如果得到的 `DateTime` 与 `this` 的夏令时偏移不同，那么结果将不会与 `this` 具有相同的时刻，甚至可能不会正好落在 50 天前的那个日历日期上。

在处理本地时间的日期时请务必谨慎。

### difference()

```dart
Duration difference(DateTime other)
```

返回一个 [Duration](https://www.yuque.com/thyname/dart.core/duration)，表示用该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 减去 `other` 所得到的差值。

如果 `other` 发生在该 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 之后，则返回的 [Duration](https://www.yuque.com/thyname/dart.core/duration) 为负值。

```dart
final berlinWallFell = DateTime.utc(1989, DateTime.november, 9);
final dDay = DateTime.utc(1944, DateTime.june, 6);

final difference = berlinWallFell.difference(dDay);
print(difference.inDays); // 16592
```

该差值以秒及秒的小数部分来度量。上述差值计算的是这些日期开始时（即午夜）之间相隔的小数秒数。如果上述日期采用的是本地时间而非 UTC，那么由于夏令时的差异，两个午夜之间的差值可能不是 24 小时的整数倍。

例如，在澳大利亚，使用本地时间而非 UTC 的类似代码：

```dart
final berlinWallFell = DateTime(1989, DateTime.november, 9);
final dDay = DateTime(1944, DateTime.june, 6);
final difference = berlinWallFell.difference(dDay);
print(difference.inDays); // 16591
assert(difference.inDays == 16592);
```

这段代码会失败，因为实际差值为 16591 天 23 小时，而 [Duration.inDays] 只返回整天数。

## 运算符

### operator ==()

```dart
bool operator ==(Object other)
```

判断 `other` 是否是与该对象处于同一时刻且位于同一时区（UTC 或本地）的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime)。

```dart
final dDayUtc = DateTime.utc(1944, 6, 6);
final dDayLocal = dDayUtc.toLocal();

// These two dates are at the same moment, but are in different zones.
assert(dDayUtc != dDayLocal);
print(dDayUtc != dDayLocal); // true
```

如需比较两个时刻本身而不考虑其所在时区，请参阅 [isAtSameMomentAs]。

# DateTimeCopyWith

```dart
extension DateTimeCopyWith on DateTime {}
```

为 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 对象添加 [copyWith] 方法。

### copyWith()

```dart
DateTime copyWith({
  int? year,
  int? month,
  int? day,
  int? hour,
  int? minute,
  int? second,
  int? millisecond,
  int? microsecond,
  bool? isUtc,
})
```

通过更新各个属性，基于当前 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 创建一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime)。

[copyWith] 方法会创建一个新的 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 对象，其 [DateTime.year]、[DateTime.hour] 等属性的值，由对应名称的参数提供；如果未提供参数或参数为 `null`，则使用该属性的现有值。

示例：

```dart
final now = DateTime.now();
final sameTimeOnMoonLandingDay =
    now.copyWith(year: 1969, month: 07, day: 20);
```

与 [DateTime](https://www.yuque.com/thyname/dart.core/datetime) 和 [DateTime.utc] 构造函数（该操作正是使用它们来创建新值）一样，允许属性值超出其范围（例如 [month] 超出 1 到 12 的范围），这可能会影响更高位的属性（例如，月份为 13 会导致结果为次年的 1 月）。

还需注意，如果结果是本地时间的 DateTime，季节性时区调整（夏令时）可能导致某些日期、小时和分钟的组合不存在，或存在多次。前一种情况下，会使用两个相邻时区中对应的时间代替；后一种情况下，则会从两个选项中选择其一。
