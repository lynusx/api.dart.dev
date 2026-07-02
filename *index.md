## 欢迎！

> dart sdk：Dart 3.12.2

欢迎查阅 Dart API 参考文档，本文档涵盖 [Dart 核心库](https://dart.dev/libraries)，包括：

- [dart:core](https://api.dart.dev/dart-core/dart-core-library.html)：核心功能，例如字符串、数字、集合、错误、日期和 URI。
- [dart:io](https://api.dart.dev/dart-io/dart-io-library.html)：非 Web 应用的 I/O 支持。
- [dart:async](https://api.dart.dev/dart-async/dart-async-library.html)：使用 Future、Stream 和 Zone 进行异步编程的相关功能。

此外，你还可以找到涵盖 Dart 各种平台互操作选项的参考文档，例如：

- [dart:js_interop](https://api.dart.dev/dart-js_interop/dart-js_interop-library.html)：包含健全类型层级结构以及用于与 JavaScript 互操作的辅助函数的库。
- [package:web](https://pub.dev/documentation/web)：用于 Web 应用的 DOM 操作。
- [dart:ffi](https://api.dart.dev/dart-ffi/dart-ffi-library.html)：用于与 C 语言互操作的外部函数接口。

除 `dart:core` 外，其他核心库必须先导入才能使用：

```dart
import 'dart:math';
```

此外，你可以在 [pub.dev](https://pub.dev/) 上找到 Dart 包。

### 语言文档

学习和使用 Dart 的主站点是 [dart.dev](https://dart.dev/)。请查阅以下页面：

- [Dart 概述](https://dart.dev/overview)
- [Dart 语言文档](https://dart.dev/language)
- [库导览](https://dart.dev/libraries)
- [教程](https://dart.dev/tutorials)

本 API 参考文档由 [dart-lang/sdk](https://github.com/dart-lang/sdk) 中的 SDK 源代码生成。如果你想对本文档提供反馈或进行编辑，请参阅[贡献指南](https://github.com/dart-lang/sdk/blob/main/CONTRIBUTING.md)。

---

## 库

### 核心

- [dart:async](https://api.dart.dev/dart-async/)

  支持异步编程，提供 Future 和 Stream 等类。

- [dart:collection](https://api.dart.dev/dart-collection/)

  补充 dart:core 中集合支持的类和工具。

- [dart:convert](https://api.dart.dev/dart-convert/)

  用于在不同数据表示形式（包括 JSON 和 UTF-8）之间进行转换的编码器和解码器。

- [dart:core](https://api.dart.dev/dart-core/)

  每个 Dart 程序都会用到的内置类型、集合及其他核心功能。

- [dart:developer](https://api.dart.dev/dart-developer/)

  与调试器、检查器等开发者工具进行交互。

- [dart:math](https://api.dart.dev/dart-math/)

  数学常量和函数，以及一个随机数生成器。

- [dart:typed_data](https://api.dart.dev/dart-typed_data/)

  能够高效处理固定大小数据（例如无符号 8 字节整数）的列表，以及 SIMD 数值类型。

### VM

- [dart:ffi](https://api.dart.dev/dart-ffi/)

  用于与 C 编程语言互操作的外部函数接口。

- [dart:io](https://api.dart.dev/dart-io/)

  为非 Web 应用提供文件、套接字、HTTP 及其他 I/O 支持。

- [dart:isolate](https://api.dart.dev/dart-isolate/)

  使用 _isolate_（隔离区）进行并发编程：isolate 是类似线程的独立工作单元，但彼此不共享内存，只能通过消息进行通信。

- [dart:mirrors](https://api.dart.dev/dart-mirrors/)

  Dart 中的基础反射机制，支持内省和动态调用。
