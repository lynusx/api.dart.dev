# Point

```dart
class Point<T extends num> {}
```

用于表示二维位置的实用工具类。

示例：

```dart
var leftTop = const Point(0, 0);
var rightBottom = const Point(200, 400);
```

**遗留特性：** 不建议在新代码中使用 [Point]。

- 如果你正在将 `Point` 类与 `dart:html` 一起使用，建议迁移到 `package:web`。要了解如何迁移及原因，请查看[迁移指南](https://dart.dev/go/package-web)。
- 如果你想组合 `x` 和 `y` 坐标，可以考虑使用[记录（record）](https://dart.dev/language/records)。根据具体用法，可以写成 `var point = (x, y)` 或 `var point = (x: x, y: y)`。
- 如果你想执行向量运算，例如向量加法或标量乘法，可以考虑使用专门的向量数学库，例如 [`package:vector_math`](https://pub.dev/packages/vector_math)。
- 如果你正在开发 Flutter 应用或包，可以考虑使用 `dart:ui` 中的 [`Offset`](https://api.flutter.dev/flutter/dart-ui/Offset-class.html) 类型。

## 构造函数

### Point()

```dart
Point(T x, T y)
```

使用提供的 [x] 和 [y] 坐标创建一个点。

**遗留特性：** 不建议在新代码中使用 [Point]。要了解更多信息，请查看 [Point] 类的 API 文档。

## 属性

### magnitude

```dart
double get magnitude
```

获取原点 (0, 0) 与该点之间的直线（欧几里得）距离。

示例：

```dart
var magnitude = const Point(0, 0).magnitude; // 0.0
magnitude = const Point(10, 0).magnitude;  // 10.0
magnitude = const Point(0, -10).magnitude; // 10.0
magnitude = const Point(10, 10).magnitude;  // 14.142135623730951
```

## 方法

### distanceTo()

```dart
double distanceTo(Point<T> other)
```

返回 `this` 与 [other] 之间的距离。

```dart
var distanceTo = const Point(0, 0).distanceTo(const Point(0, 0)); // 0.0
distanceTo = const Point(0, 0).distanceTo(const Point(10, 0)); // 10.0
distanceTo = const Point(0, 0).distanceTo(const Point(0, -10)); // 10.0
distanceTo = const Point(-10, 0).distanceTo(const Point(100, 0)); // 110.0
```

### squaredDistanceTo()

```dart
T squaredDistanceTo(Point<T> other)
```

返回 `this` 与 [other] 之间的平方距离。

当不需要实际值时，平方距离可用于比较。

示例：

```dart
var squaredDistance =
    const Point(0, 0).squaredDistanceTo(const Point(0, 0)); // 0.0
squaredDistance =
    const Point(0, 0).squaredDistanceTo(const Point(10, 0)); // 100
squaredDistance =
    const Point(0, 0).squaredDistanceTo(const Point(0, -10)); // 100
squaredDistance =
    const Point(-10, 0).squaredDistanceTo(const Point(100, 0)); // 12100
```

## 运算符

### operator ==

```dart
bool operator ==(Object other)
```

判断 [other] 是否为与该点坐标相同的点。

如果 [other] 是一个 [Point]，且其 [x] 和 [y] 坐标与该点对应的坐标相等，则返回 `true`；否则返回 `false`。

示例：

```dart
var result = const Point(0, 0) == const Point(0, 0); // true
result = const Point(1.0, 0) == const Point(-1.0, 0); // false
```

### operator +

```dart
Point<T> operator +(Point<T> other)
```

将 [other] 与 `this` 相加，如同两者都是向量一样。

将得到的“向量”结果作为 Point 返回。

示例：

```dart
var point = const Point(10, 100) + const Point(10, 10); // Point(20, 110)
point = const Point(-10, -20) + const Point(10, 100); // Point(0, 80)
```

### operator -

```dart
Point<T> operator -(Point<T> other)
```

从 `this` 中减去 [other]，如同两者都是向量一样。

将得到的“向量”结果作为 Point 返回。

示例：

```dart
var point = const Point(10, 100) - const Point(10, 10); // Point(0, 90)
point = const Point(-10, -20) - const Point(100, 100); // Point(-110, -120)
```

### operator \*

```dart
Point<T> operator *(num factor)
```

将该点按 [factor] 缩放，如同它是一个向量一样。

**重要说明**：该函数接受 `num` 作为参数，仅是为了让你可以用 `int` 类型的因子缩放 `Point<double>` 对象。由于 `*` 运算符始终返回与调用者相同类型的 `Point`，因此在 `Point<int>` 上传入 double 类型的 [factor] 会**导致运行时错误**。

示例：

```dart
// 整数值。
var point = const Point(10, 100) * 10; // Point(100, 1000)
point = const Point(-10, -100) * 5; // Point(-50, -500)
// 浮点数值。
var doublePoint = Point(10.0, 100.0) * 1.5; // Point(15.0, 150.0)
// 由于类型转换无效而引发的运行时错误。
var newPoint = const Point(10, 100) * 1.5; // Throws.
```
