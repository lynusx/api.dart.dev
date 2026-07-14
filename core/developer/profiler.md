# UserTag

```dart
abstract final class UserTag {}
```

UserTag 可用于在 [DevTools CPU 性能分析器](https://docs.flutter.dev/tools/devtools/cpu-profiler)中对样本进行分组。

### maxUserTags

```dart
dynamic maxUserTags
```

程序可创建的 UserTag 实例的最大数量。

### UserTag()

```dart
UserTag(String label)
```

### label

```dart
String get label
```

此 [UserTag](https://www.yuque.com/thyname/dart.developer/usertag) 的标签。

### makeCurrent()

```dart
UserTag makeCurrent()
```

将此 [UserTag](https://www.yuque.com/thyname/dart.developer/usertag) 设为该 isolate 的当前标签。返回设置之前的当前标签。

### defaultTag

```dart
UserTag get defaultTag
```

标签为 'Default' 的默认 [UserTag](https://www.yuque.com/thyname/dart.developer/usertag)。

# getCurrentTag()

```dart
UserTag getCurrentTag()
```

返回该 isolate 的当前 [UserTag](https://www.yuque.com/thyname/dart.developer/usertag)。
