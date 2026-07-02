# print()

```dart
void print(Object? object)
```

将一个对象输出到控制台。

在 Web 平台上，`object` 会被转换为字符串，并通过 `console.log` 将该字符串输出到 Web 控制台。

在原生（非 Web）平台上，`object` 会被转换为字符串，并在该字符串末尾添加换行符（`'\n'`，U+000A）后写入 `stdout`。在 Windows 上，末尾的换行符以及字符串表示中出现的所有换行符，都会使用 Windows 的行终止符序列（`'\r\n'`，U+000D + U+000A）输出。

对 `print` 的调用可以被 [Zone.print] 拦截。
