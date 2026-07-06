# Random

```dart
abstract interface class Random {}
```

用于生成随机 bool、int 或 double 值的生成器。

默认实现提供一串伪随机比特，不适用于加密用途。

如需用于加密用途，请使用 [Random.secure] 构造函数。

如需创建一个在 0（含）到 max（不含）范围内均匀分布的非负随机整数，请使用 [nextInt(int max)]。

```dart
var intValue = Random().nextInt(10); // Value is >= 0 and < 10.
intValue = Random().nextInt(100) + 50; // Value is >= 50 and < 150.
```

如需创建一个在 0.0（含）到 1.0（不含）范围内均匀分布的非负随机浮点值，请使用 [nextDouble]。

```dart
var doubleValue = Random().nextDouble(); // Value is >= 0.0 and < 1.0.
doubleValue = Random().nextDouble() * 256; // Value is >= 0.0 and < 256.0.
```

如需创建一个随机布尔值，请使用 [nextBool]。

```dart
var boolValue = Random().nextBool(); // true or false, with equal chance.
```

## 构造函数

### Random()

```dart
Random([int? seed])
```

创建一个随机数生成器。

可选参数 [seed] 用于初始化生成器的内部状态。随机数流的具体实现可能会在库的不同版本之间发生变化。

### Random.secure()

```dart
Random.secure()
```

创建一个密码学安全的随机数生成器。

如果程序无法提供密码学安全的随机数来源，则会抛出 [UnsupportedError]。

## 方法

### nextInt()

```dart
int nextInt(int max)
```

生成一个在 0（含）到 [max]（不含）范围内均匀分布的非负随机整数。

实现说明：默认实现支持的 [max] 取值范围为 1 到 (1<<32)（含）。

示例：

```dart
var intValue = Random().nextInt(10); // Value is >= 0 and < 10.
intValue = Random().nextInt(100) + 50; // Value is >= 50 and < 150.
```

### nextDouble()

```dart
double nextDouble()
```

生成一个在 0.0（含）到 1.0（不含）范围内均匀分布的非负随机浮点值。

示例：

```dart
var doubleValue = Random().nextDouble(); // Value is >= 0.0 and < 1.0.
doubleValue = Random().nextDouble() * 256; // Value is >= 0.0 and < 256.0.
```

### nextBool()

```dart
bool nextBool()
```

生成一个随机布尔值。

示例：

```dart
var boolValue = Random().nextBool(); // true or false, with equal chance.
```
