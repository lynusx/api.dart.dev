# Rectangle

```dart
class Rectangle<T extends num> extends _RectangleBase<T> {}
```

用于表示属性不可变的二维矩形的类。

**遗留特性：** 不建议在新代码中使用 [Rectangle](https://www.yuque.com/thyname/dart.math/rectangle)。

- 如果你正在将 `Rectangle` 类与 `dart:html` 一起使用，建议迁移到 `package:web`。要了解如何迁移及原因，请查看[迁移指南](https://dart.dev/go/package-web)。
- 如果你想在某个坐标系中存储矩形的边界，可以考虑使用[记录（record）](https://dart.dev/language/records)。根据具体用法，可以写成 `var boundaries = (mixX: x1, maxX: x2, minY: y1, maxY: y2)`。
- 如果你需要执行相交计算或包含检查，可以考虑使用专门的库，例如 [`package:vector_math`](https://pub.dev/packages/vector_math)。
- 如果你正在开发 Flutter 应用或包，可以考虑使用 `dart:ui` 中的 [`Rect`](https://api.flutter.dev/flutter/dart-ui/Rect-class.html) 类型。

## 构造函数

### Rectangle()

```dart
Rectangle<T extends num>(
  T left,
  T top,
  T width,
  T height,
)
```

创建一个由 `(left, top)` 和 `(left+width, top+height)` 确定的矩形。

该矩形包含 x 坐标介于 `left` 与 `left + width` 之间、y 坐标介于 `top` 与 `top + height` 之间（均含边界）的所有点。

`width` 和 `height` 应为非负数。如果 `width` 或 `height` 为负数，则会被限制为零。

如果 `width` 和 `height` 均为零，则该“矩形”仅包含单个点 `(left, top)`。

示例：

```dart
var rectangle = const Rectangle(20, 50, 300, 600);
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 320
print(rectangle.bottom); // 650
```

**遗留特性：** 不建议在新代码中使用 [Rectangle](https://www.yuque.com/thyname/dart.math/rectangle)。要了解更多信息，请查看 [Rectangle](https://www.yuque.com/thyname/dart.math/rectangle) 类的 API 文档。

### Rectangle.fromPoints()

```dart
Rectangle<T extends num>.fromPoints(Point<T> a, Point<T> b)
```

创建一个由点 `a` 和 `b` 确定的矩形。

该矩形包含 x 坐标介于 `a.x` 与 `b.x` 之间、y 坐标介于 `a.y` 与 `b.y` 之间（均含边界）的所有点。

如果 `a.x` 与 `b.x` 之间的距离无法精确表示（当其中一个或两个都是 double 时可能发生这种情况），则实际的右边界可能与 `max(a.x, b.x)` 略有偏差。y 坐标及下边界的情况与此类似。

示例：

```dart
var leftTop = const Point(20, 50);
var rightBottom = const Point(300, 600);

var rectangle = Rectangle.fromPoints(leftTop, rightBottom);
print(rectangle); // Rectangle (20, 50) 280 x 550
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 300
print(rectangle.bottom); // 600
```

## 属性

### left

```dart
T left
```

左边界的 x 坐标。

### top

```dart
T top
```

上边界的 y 坐标。

### width

```dart
T width
```

矩形的宽度。

### height

```dart
T height
```

矩形的高度。

---

# MutableRectangle

```dart
class MutableRectangle<T extends num> extends _RectangleBase<T> implements Rectangle<T> {}
```

用于表示属性可变的二维轴对齐矩形的类。

**遗留特性：** 不建议在新代码中使用 [MutableRectangle](https://www.yuque.com/thyname/dart.math/mutablerectangle)。

- 如果你正在将 `MutableRectangle` 类与 `dart:html` 一起使用，建议迁移到 `package:web`。要了解如何迁移及原因，请查看[迁移指南](https://dart.dev/go/package-web)。
- 如果你想在某个坐标系中存储矩形的边界，可以考虑使用[记录（record）](https://dart.dev/language/records)。根据具体用法，可以写成 `var boundaries = (mixX: x1, maxX: x2, minY: y1, maxY: y2)`。
- 如果你需要执行相交计算或包含检查，可以考虑使用专门的库，例如 [`package:vector_math`](https://pub.dev/packages/vector_math)。
- 如果你正在开发 Flutter 应用或包，可以考虑使用 `dart:ui` 中的 [`Rect`](https://api.flutter.dev/flutter/dart-ui/Rect-class.html) 类型。

## 构造函数

### MutableRectangle()

```dart
MutableRectangle<T extends num>(
  T left,
  T top,
  T width,
  T height,
)
```

创建一个由 `(left, top)` 和 `(left+width, top+height)` 确定的可变矩形。

该矩形包含 x 坐标介于 `left` 与 `left + width` 之间、y 坐标介于 `top` 与 `top + height` 之间（均含边界）的所有点。

`width` 和 `height` 应为非负数。如果 `width` 或 `height` 为负数，则会被限制为零。

如果 `width` 和 `height` 均为零，则该“矩形”仅包含单个点 `(left, top)`。

示例：

```dart
var rectangle = MutableRectangle(20, 50, 300, 600);
print(rectangle); // Rectangle (20, 50) 300 x 600
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 320
print(rectangle.bottom); // 650

// Change rectangle width and height.
rectangle.width = 200;
rectangle.height = 100;

print(rectangle); // Rectangle (20, 50) 200 x 100
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 220
print(rectangle.bottom); // 150
```

**遗留特性：** 不建议在新代码中使用 [MutableRectangle](https://www.yuque.com/thyname/dart.math/mutablerectangle)。要了解更多信息，请查看 [MutableRectangle](https://www.yuque.com/thyname/dart.math/mutablerectangle) 类的 API 文档。

### MutableRectangle.fromPoints()

```dart
MutableRectangle<T extends num>.fromPoints(Point<T> a, Point<T> b)
```

创建一个由点 `a` 和 `b` 确定的可变矩形。

该矩形包含 x 坐标介于 `a.x` 与 `b.x` 之间、y 坐标介于 `a.y` 与 `b.y` 之间（均含边界）的所有点。

如果 `a.x` 与 `b.x` 之间的距离无法精确表示（当其中一个或两个都是 double 时可能发生这种情况），则实际的右边界可能与 `max(a.x, b.x)` 略有偏差。y 坐标及下边界的情况与此类似。

示例：

```dart
var leftTop = const Point(20, 50);
var rightBottom = const Point(300, 600);
var rectangle = MutableRectangle.fromPoints(leftTop, rightBottom);
print(rectangle); // Rectangle (20, 50) 280 x 550
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 300
print(rectangle.bottom); // 600
```

## 属性

### left

```dart
T left
```

左边界的 x 坐标。

设置该值会移动矩形，但不会改变其宽度。

### top

```dart
T top
```

左边界的 y 坐标。

设置该值会移动矩形，但不会改变其高度。

### width

```dart
set width(T width)
```

设置矩形的宽度。

宽度必须为非负数。如果传入负数宽度，则会被限制为零。

设置该值会改变矩形的右边界，但不会改变 `left`。

### height

```dart
set height(T height)
```

设置矩形的高度。

高度必须为非负数。如果传入负数高度，则会被限制为零。

设置该值会改变矩形的下边界，但不会改变 `top`。
