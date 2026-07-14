# systemEncoding

```dart
SystemEncoding systemEncoding
```

当前的系统编码。

用于在 stdin、stdout 和 stderr 上进行通信时，实现字节与字符串之间的相互转换。

在 Windows 上，此转换将使用当前激活的代码页。在其他所有系统上，始终使用 UTF-8。

# SystemEncoding

```dart
final class SystemEncoding extends Encoding {}
```

系统编码在 Windows 上是当前的代码页，在 Linux 和 Mac 上是 UTF-8。

### SystemEncoding()

```dart
SystemEncoding()
```

创建一个 const 的 SystemEncoding。

用户应使用顶层常量 [systemEncoding](https://www.yuque.com/thyname/dart.io/systemencoding)。

### name

```dart
String get name
```

### encode()

```dart
List<int> encode(String input)
```

### decode()

```dart
String decode(List<int> encoded)
```

### encoder

```dart
Converter<String, List<int>> get encoder
```

### decoder

```dart
Converter<List<int>, String> get decoder
```
