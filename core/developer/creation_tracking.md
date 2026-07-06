# CreationLocation

```dart
final class CreationLocation {}
```

保存对象创建时的源代码位置，前提是其类已使用 `@pragma('track-creation-locations')` 注解标记。

当一个类定义被标注为 `@pragma('track-creation-locations')` 时，Dart 编译器会将调用点的位置信息注入到该类或其任何子类的构造函数调用中，并将其存储在创建的对象内。

可以通过将该对象作为参数调用 [CreationLocation.of] 来读取该对象的创建位置。

## 示例

```dart
import 'dart:developer';

// 标记此类及其任何子类的构造函数调用点将被跟踪。
@pragma('track-creation-locations')
class TargetClass {
  TargetClass();
}

void main() {
  // 此构造函数调用的源代码位置将被注入到该对象中。
  final instance = TargetClass();

  final location = CreationLocation.of(instance);
  print(location); // 将打印当前文件路径、第 11 行、第 20 列
}
```

## 限制

该编译器转换依赖于向目标类的构造函数注入一个命名参数。由于 Dart 语义不允许函数同时拥有可选位置参数和命名参数，因此该转换**将静默跳过**任何声明了可选位置参数的构造函数。对构造函数被跳过的对象调用 [CreationLocation.of] 将返回 `null`。

### of()

```dart
CreationLocation? of(Object? object)
```

返回 [object] 的创建位置。

提供的对象必须是已使用 `@pragma('track-creation-locations')` 注解标记的类的实例。

### file

```dart
String file
```

位置的文件路径。

### line

```dart
int line
```

行号（从 1 开始）。

### column

```dart
int column
```

列号（从 1 开始）。

### name

```dart
String? name
```

此位置处的参数名或函数名（可选）。

### toJsonMap()

```dart
Map<String, Object?> toJsonMap()
```

此位置的 JSON 表示形式。

### toString()

```dart
String toString()
```
