使用 _isolate_ 进行并发编程：isolate 是独立的工作单元，类似于线程，但不共享内存，仅通过消息进行通信。

_注意_：`dart:isolate` 库目前仅受 [Dart Native](https://dart.dev/overview#platform) 平台支持。

在代码中使用该库：

```dart
import 'dart:isolate';
```
