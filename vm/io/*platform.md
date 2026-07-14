# Platform

```dart
abstract final class Platform {}
```

有关当前程序运行环境的信息。

Platform 提供诸如操作系统、计算机主机名、环境变量的值、正在运行的程序的路径以及程序运行时其他全局属性等信息。

## 获取当前 Dart 脚本的 URI

使用 [script] 获取器可获取当前正在运行的 Dart 脚本的 URI。

```dart
import 'dart:io' show Platform;

void main() {
  // 获取正在运行脚本的 URI。
  var uri = Platform.script;
  // 将该 URI 转换为路径。
  var path = uri.toFilePath();
}
```

## 获取环境变量的值

[environment] 获取器返回一个 [Map](https://www.yuque.com/thyname/dart.core/map)，其中包含环境变量名称和值的键值对字符串。该 Map 是不可修改的。以下示例展示了如何获取 `PATH` 环境变量的值。

```dart
import 'dart:io' show Platform;

void main() {
  Map<String, String> envVars = Platform.environment;
  print(envVars['PATH']);
}
```

## 确定操作系统

你可以使用 [operatingSystem] 获取器获取字符串形式的操作系统名称。你也可以使用其中一个静态布尔获取器，例如 [isMacOS]、[isLinux]、[isWindows] 等。

```dart
import 'dart:io' show Platform;

void main() {
  // 以字符串形式获取操作系统。
  String os = Platform.operatingSystem;
  // 或者，使用判断式获取器。
  if (Platform.isMacOS) {
    print('is a Mac');
  } else {
    print('is not a Mac');
  }
}
```

### numberOfProcessors

```dart
dynamic numberOfProcessors
```

机器中各独立执行单元的数量。

### pathSeparator

```dart
dynamic pathSeparator
```

操作系统用于分隔文件路径各组成部分的路径分隔符。

### operatingSystem

```dart
dynamic operatingSystem
```

表示操作系统或平台的字符串。

可能的取值包括：

- "android"
- "fuchsia"
- "ios"
- "linux"
- "macos"
- "windows"

请注意，此列表可能会随时间变化，因此与平台相关的逻辑应通过相应的布尔获取器（例如 [isMacOS]）进行保护。

### operatingSystemVersion

```dart
dynamic operatingSystemVersion
```

表示操作系统或平台版本的字符串。

此字符串的格式会因操作系统、平台和版本而异，不适合用于解析。例如："Linux 5.11.0-1018-gcp #20~20.04.2-Ubuntu SMP Fri Sep 3 01:01:37 UTC 2021"、"Version 14.5 (Build 18E182)"、'"Windows 10 Pro" 10.0 (Build 19043)'

### localHostname

```dart
dynamic localHostname
```

系统的本地主机名。

例如："mycomputer.corp.example.com"、"mycomputer"

使用平台的 [`gethostname`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gethostname.html) 实现。

### version

```dart
dynamic version
```

当前 Dart 运行时的版本。

该值是一个 [语义化版本](http://semver.org) 字符串，表示当前 Dart 运行时的版本，其后可能跟有空白字符和其他版本、构建详情。

### localeName

```dart
String get localeName
```

获取当前区域设置（locale）的名称。

结果通常由以下部分组成：

- 语言（例如 "en"），或
- 语言和国家代码（例如 "en_US"、"de_AT"），或
- 语言、国家代码和字符集（例如 "en_US.UTF-8"）。

在 macOS 和 iOS 上，区域设置取自 CFLocaleGetIdentifier。

在 Linux 和 Fuchsia 上，区域设置取自 "LANG" 环境变量，该变量可以设置为任意值。例如：

```shell
LANG=kitten dart myfile.dart  # localeName 为 "kitten"
```

在 Android 上，应用程序运行期间，即使用户调整了语言设置，该值也不会改变。

参见 https://en.wikipedia.org/wiki/Locale_(computer_software)

### isLinux

```dart
bool isLinux
```

操作系统是否为某个版本的 [Linux](https://en.wikipedia.org/wiki/Linux)。

如果操作系统是以不同名称标识自身的特殊版本 Linux（例如 Android，参见 [isAndroid]），则此值为 `false`。

### isMacOS

```dart
bool isMacOS
```

操作系统是否为某个版本的 [macOS](https://en.wikipedia.org/wiki/MacOS)。

### isWindows

```dart
bool isWindows
```

操作系统是否为某个版本的 [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows)。

### isAndroid

```dart
bool isAndroid
```

操作系统是否为某个版本的 [Android](https://en.wikipedia.org/wiki/Android_%28operating_system%29)。

### isIOS

```dart
bool isIOS
```

操作系统是否为某个版本的 [iOS](https://en.wikipedia.org/wiki/IOS)。

### isFuchsia

```dart
bool isFuchsia
```

操作系统是否为某个版本的 [Fuchsia](https://en.wikipedia.org/wiki/Google_Fuchsia)。

### environment

```dart
Map<String, String> get environment
```

此进程的环境变量，以从字符串键到字符串值的 Map 形式表示。

该 Map 是不可修改的，其内容在首次使用时从操作系统获取。

Windows 上的环境变量不区分大小写，因此在 Windows 上该 Map 也不区分大小写，并会将所有键转换为大写。在其他平台上，键可以通过大小写加以区分。

### executable

```dart
String get executable
```

用于在此隔离区（isolate）中运行脚本的可执行文件的路径。在 Dart 虚拟机上运行时通常为 `dart`，或是编译后的脚本名称（`script_name.exe`）。

用于标识可执行文件的字面路径。此路径可能是相对路径，也可能只是通过系统路径搜索找到该可执行文件所依据的名称。

使用 [resolvedExecutable] 可获取该可执行文件的绝对路径。

### resolvedExecutable

```dart
String get resolvedExecutable
```

用于在此隔离区中运行脚本的可执行文件的路径，该路径已由操作系统解析。

这是用于运行脚本的可执行文件的绝对路径，其中所有符号链接均已解析。

未解析的版本参见 [executable]。

### script

```dart
Uri get script
```

在此隔离区中运行的脚本的绝对 URI。

如果命令行上的脚本参数是相对路径，会在获取脚本之前将其解析为绝对 URI，并返回该绝对 URI。

URI 解析仅对脚本路径进行字符串操作，这可能与文件系统的路径解析行为不同。例如，紧跟 '..' 的符号链接不会被查找。

如果正在执行已编译的 Dart 脚本，则返回指向已编译脚本的 URI，例如 `file:///full/path/to/script_name.exe`。

如果在 Dart 虚拟机上运行，则返回指向正在运行的 Dart 脚本的 URI，例如 `file:///full/path/to/script_name.dart`。

如果执行环境不支持 [script]，则该 URI 为空。

### executableArguments

```dart
List<String> get executableArguments
```

传递给用于在此隔离区中运行脚本的可执行文件的标志。

这些是可执行文件在脚本名称之前的命令行标志。每次读取该值时都会提供一个新的列表。

### packageConfig

```dart
String? get packageConfig
```

传递给用于在此隔离区中运行脚本的可执行文件的 `--packages` 标志。

如果存在，它指定了一个描述如何查找 Dart 包的文件。

如果没有 `--packages` 标志，则为 `null`。

### lineTerminator

```dart
String get lineTerminator
```

当前操作系统的默认行终止符。

操作系统用于分隔或终止文本行的默认字符序列。目前，除 Windows 外，所有受支持的操作系统上的行终止符都是单个换行符，U+000A 或 `"\n"`；Windows 使用回车符加换行符的组合，U+000D U+000A 或 `"\r\n"`。
