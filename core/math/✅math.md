数学常量和函数，以及一个随机数生成器。

要在代码中使用该库：

```dart
import 'dart:math';
```

## Random

[Random](https://www.yuque.com/thyname/dart.math/random) 是用于生成 [bool](https://www.yuque.com/thyname/dart.core/bool)、[int](https://www.yuque.com/thyname/dart.core/int) 或 [double](https://www.yuque.com/thyname/dart.core/double) 值的生成器。

```dart
var intValue = Random().nextInt(10); // Value is >= 0 and < 10.
var doubleValue = Random().nextDouble(); // Value is >= 0.0 and < 1.0.
var boolValue = Random().nextBool(); // true or false, with equal chance.
```

## Point

[Point](https://www.yuque.com/thyname/dart.math/point) 是用于表示二维位置的实用工具类。

```dart
var leftTop = const Point(0, 0);
var rightBottom = const Point(200, 400);
```

## Rectangle

[Rectangle](https://www.yuque.com/thyname/dart.math/rectangle) 是用于表示属性不可变的二维轴对齐矩形的类。

创建一个由两点确定的矩形。

```dart
var leftTop = const Point(20, 50);
var rightBottom = const Point(300, 600);
var rectangle = Rectangle.fromPoints(leftTop, rightBottom);
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 300
print(rectangle.bottom); // 600
```

创建一个由 `(left, top)` 和 `(left+width, top+height)` 确定的矩形。

```dart
var rectangle = const Rectangle(20, 50, 300, 600);
print(rectangle.left); // 20
print(rectangle.top); // 50
print(rectangle.right); // 320
print(rectangle.bottom); // 650
```

## MutableRectangle

[MutableRectangle](https://www.yuque.com/thyname/dart.math/mutablerectangle) 是用于表示属性可变的二维轴对齐矩形的类。

创建一个由 `(left, top)` 和 `(left+width, top+height)` 确定的可变矩形。

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
