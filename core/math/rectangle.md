# Rectangle

```dart
class Rectangle<T extends num> extends _RectangleBase<T> {}
```

A class for representing two-dimensional rectangles whose properties are immutable.

**Legacy:** New usages of [Rectangle] are discouraged.

- If you are using the `Rectangle` class with `dart:html`, we recommend migrating to `package:web`. To learn how and why to migrate, check out the [migration guide](https://dart.dev/go/package-web).
- If you want to store the boundaries of a rectangle in some coordinate system, consider using a [record](https://dart.dev/language/records). Depending on how you will use it, this could look like `var boundaries = (mixX: x1, maxX: x2, minY: y1, maxY: y2)`.
- If you need to perform intersection calculations or containment checks, consider using a dedicated library, such as [`package:vector_math`](https://pub.dev/packages/vector_math).
- If you are developing a Flutter application or package, consider using the [`Rect`](https://api.flutter.dev/flutter/dart-ui/Rect-class.html) type from `dart:ui`.

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

Create a rectangle spanned by `(left, top)` and `(left+width, top+height)`.

The rectangle contains the points with x-coordinate between `left` and `left + width`, and with y-coordinate between `top` and `top + height`, both inclusive.

The `width` and `height` should be non-negative. If `width` or `height` are negative, they are clamped to zero.

If `width` and `height` are zero, the "rectangle" comprises only the single point `(left, top)`.

Example:

```dart
var rectangle = const Rectangle(20, 50, 300, 600);
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 320
print(rectangle.bottom); // 650
```

**Legacy:** New usages of [Rectangle] are discouraged. To learn more, check out the [Rectangle] class API docs.

### Rectangle.fromPoints()

```dart
Rectangle<T extends num>.fromPoints(Point<T> a, Point<T> b)
```

Create a rectangle spanned by the points [a] and [b];

The rectangle contains the points with x-coordinate between `a.x` and `b.x`, and with y-coordinate between `a.y` and `b.y`, both inclusive.

If the distance between `a.x` and `b.x` is not representable (which can happen if one or both is a double), the actual right edge might be slightly off from `max(a.x, b.x)`. Similar for the y-coordinates and the bottom edge.

Example:

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

The x-coordinate of the left edge.

### top

```dart
T top
```

The y-coordinate of the top edge.

### width

```dart
T width
```

The width of the rectangle.

### height

```dart
T height
```

The height of the rectangle.

---

# MutableRectangle

```dart
class MutableRectangle<T extends num> extends _RectangleBase<T> implements Rectangle<T> {}
```

A class for representing two-dimensional axis-aligned rectangles with mutable properties.

**Legacy:** New usages of [MutableRectangle] are discouraged.

- If you are using the `MutableRectangle` class with `dart:html`, we recommend migrating to `package:web`. To learn how and why to migrate, check out the [migration guide](https://dart.dev/go/package-web).
- If you want to store the boundaries of a rectangle in some coordinate system, consider using a [record](https://dart.dev/language/records). Depending on how you will use it, this could look like `var boundaries = (mixX: x1, maxX: x2, minY: y1, maxY: y2)`.
- If you need to perform intersection calculations or containment checks, consider using a dedicated library, such as [`package:vector_math`](https://pub.dev/packages/vector_math).
- If you are developing a Flutter application or package, consider using the [`Rect`](https://api.flutter.dev/flutter/dart-ui/Rect-class.html) type from `dart:ui`.

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

Create a mutable rectangle spanned by `(left, top)` and `(left+width, top+height)`.

The rectangle contains the points with x-coordinate between `left` and `left + width`, and with y-coordinate between `top` and `top + height`, both inclusive.

The `width` and `height` should be non-negative. If `width` or `height` are negative, they are clamped to zero.

If `width` and `height` are zero, the "rectangle" comprises only the single point `(left, top)`.

Example:

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

**Legacy:** New usages of [MutableRectangle] are discouraged. To learn more, check out the [MutableRectangle] class API docs.

### MutableRectangle.fromPoints()

```dart
MutableRectangle<T extends num>.fromPoints(Point<T> a, Point<T> b)
```

Create a mutable rectangle spanned by the points [a] and [b];

The rectangle contains the points with x-coordinate between `a.x` and `b.x`, and with y-coordinate between `a.y` and `b.y`, both inclusive.

If the distance between `a.x` and `b.x` is not representable (which can happen if one or both is a double), the actual right edge might be slightly off from `max(a.x, b.x)`. Similar for the y-coordinates and the bottom edge.

Example:

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

The x-coordinate of the left edge.

Setting the value will move the rectangle without changing its width.

### top

```dart
T top
```

The y-coordinate of the left edge.

Setting the value will move the rectangle without changing its height.

### width

```dart
set width(T width)
```

Sets the width of the rectangle.

The width must be non-negative. If a negative width is supplied, it is clamped to zero.

Setting the value will change the right edge of the rectangle, but will not change [left].

### height

```dart
set height(T height)
```

Sets the height of the rectangle.

The height must be non-negative. If a negative height is supplied, it is clamped to zero.

Setting the value will change the bottom edge of the rectangle, but will not change [top].
