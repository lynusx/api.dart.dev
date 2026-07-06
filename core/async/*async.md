支持异步编程的库，包含 Future 和 Stream 等类。

[Future] 和 [Stream] 是 Dart 中异步编程的基本构建模块。它们通过 `async` 和 `async*` 函数在语言层面直接得到支持，并通过 `dart:core` 库提供给所有库使用。

本库提供了用于创建、使用和转换 Future 与 Stream 的更多工具，并可直接访问其他异步原语，如[计时器][Timer]、[微任务][scheduleMicrotask]和 [Zone]。

在代码中使用本库：

```dart
import 'dart:async';
```

## Future

Future 对象表示一个其返回值可能尚不可用的计算。当该计算在未来的某个时刻完成时，Future 会返回计算结果。Future 常用于通过不同线程或 isolate 实现的 API（例如 `dart:io` 的异步 I/O 操作或 `dart:html` 的 HTTP 请求）。

Dart 库中的许多方法在执行任务时会返回 `Future`。例如，将 `HttpServer` 绑定到主机和端口时，`bind()` 方法会返回一个 Future。

```dart import:io
 HttpServer.bind('127.0.0.1', 4444)
     .then((server) => print('${server.isBroadcast}'))
     .catchError(print);
```

[Future.then] 用于注册一个回调函数，当 Future 的操作（本例中为 `bind()` 方法）成功完成时运行该回调。操作返回的值会传入回调函数。在此示例中，`bind()` 方法返回 HttpServer 对象，回调函数打印该对象的一个属性。[Future.catchError] 用于注册一个回调函数，当 Future 内部发生错误时运行该回调。

## Stream

Stream 提供了异步的数据序列。数据序列的例子包括鼠标点击等单个事件，或诸如文件内容的多个字节列表这样的大数据的顺序分块，例如鼠标点击，以及从文件中读取的字节列表流。下面的示例打开一个文件进行读取。[Stream.listen] 用于注册回调函数，每当有更多数据可用、发生错误或流结束时运行这些回调。[Stream] 提供了更多功能，可通过调用 [Stream.listen] 来获取实际数据来实现。

```dart import:io import:convert
Stream<List<int>> stream = File('quotes.txt').openRead();
stream.transform(utf8.decoder).forEach(print);
```

该流会发出一系列字节列表。程序随后必须以某种方式处理这些字节列表。此处代码使用 UTF-8 解码器（由 `dart:convert` 库提供）将字节序列转换为 Dart 字符串序列。

流的另一个常见用途是处理 Web 应用中用户产生的事件：以下代码监听按钮上的鼠标点击事件。

```dart import:html
querySelector('#myButton')!.onClick.forEach((_) => print('Click.'));
```

## 其他资源

- [库导览中的 dart:async 部分](https://dart.dev/guides/libraries/library-tour#dartasync---asynchronous-programming)：异步编程简要概述。

- [使用基于 Future 的 API](https://dart.dev/codelabs/async-await)：深入了解 Future 以及如何使用它们编写异步 Dart 代码。

- [Future 与错误处理](https://dart.dev/guides/libraries/futures-error-handling)：关于处理 Future 时的错误和异常，你想知道却又不敢问的一切。

- [事件循环与 Dart](https://dart.dev/articles/event-loop/)：了解 Dart 如何处理事件队列和微任务队列，从而编写更好、更少意外的异步代码。

- [test 包：异步测试](https://pub.dev/packages/test)：如何测试异步代码。
