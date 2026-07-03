# Function

```dart
abstract final class Function {}
```

一个函数值。

`Function` 类是所有*函数类型*的超类型，其本身不包含任何值。所有实现 `Function` 的对象，其运行时类型都是某个函数类型。

`Function` 类型不携带函数的参数签名或返回类型信息。要表达更精确的函数类型，请使用函数类型语法，即关键字 `Function` 后接参数列表，或类型参数列表加参数列表，并可选地附加返回类型。

函数类型语法与函数定义类似，只是将函数名替换为单词 "Function"。

示例：

```dart
String numberToString(int n) => "$n";
String Function(int n) fun = numberToString; // Type annotation
assert(fun is String Function(int)); // Type check.
List<String Function(int)> functions = [fun]; // Type argument.
```

类型 `String Function(int)` 表示一个接受一个位置参数 `int` 并返回 `String` 的函数类型。

泛型函数类型示例：

```dart
T id<T>(T value) => value;
X Function<X>(X) anotherId = id; // Parameter name may be omitted.
int Function(int) intId = id<int>;
```

函数类型可以用在任何允许使用类型的地方，常用于接受其他函数（即"回调"）作为参数的函数。

```dart
void doSomething(String Function(int) callback) {
  print(callback(1));
}
```

由于函数类型是 [Object] 的子类型，因此函数类型拥有 [Object] 声明的所有成员。

函数类型还具有一个 `call` 方法，其签名与该函数类型本身相同。调用 `call` 方法的行为与直接调用该函数完全相同。这主要用于在可空函数值上进行条件调用。

```dart
String Function(int) fun = (n) => "$n";
String Function(int) fun2 = fun.call; // Valid.
print(fun2.call(1)); // Prints "1".

String Function(int)? maybeFun = Random().nextBool() ? fun : null;
print(maybeFun?.call(1)); // Prints "1" or "null".
```

[Function] 类型具有若干在此 `class` 声明中不可见的特殊特性。

`Function` 类型本身允许将任意函数赋值给它，因为它是任何函数类型的超类型，但它并未说明该函数应如何被调用。

不过，静态类型为 `Function` 的值*仍然*可以像函数一样被调用。

```dart
Function f = (int x) => "$x";
print(f(1)); // Prints "1".

f("not", "one", "int"); // Throws! No static warning.
```

这样的调用是一种*动态*调用，其效果与该函数值被静态标注为 [dynamic] 时完全相同，其不安全程度也与其他任何动态调用相同。运行时会执行检查以确保参数列表与函数参数匹配，如果不匹配，调用将以 [Error] 失败。此类调用没有静态类型检查，任何参数列表都会被接受，并在运行时进行检查。

正如每个函数类型都拥有自己函数类型的 `call` 方法一样，`Function` 类型也拥有一个特殊的 `call` 成员，其行为类似于一个函数类型为 `Function` 的方法（这是一种无法用普通 Dart 代码表达的方法签名）。

```dart
Function fun = (int x) => "$x";

var fun2 = fun.call; // Inferred type of `fun2` is `Function`.

print(fun2.call(1)); // Prints "1";

Function? maybeFun = Random().nextBool() ? fun : null;
print(maybeFun?.call(1)); // Prints "1" or "null".
```

## 静态方法

### apply()

```dart
dynamic apply(
  Function function,
  List? positionalArguments, [
  Map<Symbol, dynamic>? namedArguments
])
```

使用指定的参数动态调用 [function]。

其效果等同于使用与 [positionalArguments] 各元素对应的位置参数，以及与 [namedArguments] 各元素对应的命名参数，动态调用 [function]。

如果 [function] 所期望的参数不同，此调用也会产生相同的错误。

示例：

```dart
void printWineDetails(int vintage, {String? country, String? name}) {
  print('Name: $name, Country: $country, Vintage: $vintage');
}

void main() {
  Function.apply(
      printWineDetails, [2018], {#country: 'USA', #name: 'Dominus Estate'});
}

// Output of the example is:
// Name: Dominus Estate, Country: USA, Vintage: 2018
```

如果 [positionalArguments] 为 null，将被视为空列表。如果省略 [namedArguments] 或其为 null，将被视为空映射。

```dart
void helloWorld() {
  print('Hello world!');
}

void main() {
  Function.apply(helloWorld, null);
}
// Output of the example is:
// Hello world!
```

## 属性

### hashCode

```dart
int get hashCode
```

与 `operator==` 兼容的哈希码值。

## 运算符

### operator ==()

```dart
bool operator ==(Object other)
```

判断另一个对象是否与此函数相等。

函数对象只与其他函数对象（即满足 `object is Function` 的对象）相等，永远不会与非函数对象相等。

某些函数对象会被 `==` 判定为相等，因为它们被识别为表示"同一个函数"：

- 是同一个对象。静态函数和顶层函数在作为值使用时是编译期常量，因此两次引用同一个函数总会得到同一个对象，在声明局部函数的同一作用域内两次引用该局部函数声明时也是如此。

  ```dart
  void main() {
    assert(identical(print, print));
    int add(int x, int y) => x + y;
    assert(identical(add, add));
  }
  ```

- 这些函数是从同一个对象中提取出的同一个成员方法。反复将同一对象的同一实例方法"撕下"（tear off）为函数值，会得到相等但不一定相同（identical）的函数值。

  ```dart
  var o = Object();
  assert(o.toString == o.toString);
  ```

- 使用*相同*类型对相等的泛型函数进行实例化，会得到相等的结果。

  ```dart
  T id<T>(T value) => value;
  assert(id<int> == id<int>);
  ```

（如果该函数是常量，且类型参数在编译期已知，结果也可能是相同的（identical）。）

不同的函数字面量求值不保证也不要求产生相同（identical）或相等的函数对象。例如：

```dart
var functions = <Function>[];
for (var i = 0; i < 2; i++) {
  functions.add((x) => x);
}
print(identical(functions[0], functions[1])); // 'true' or 'false'
print(functions[0] == functions[1]); // 'true' or 'false'
```

如果两个不同的值是相同的（identical），它们必定相等。

如果两个函数值相等，则可以保证它们对所有参数的行为都是一致的。

如果两个函数值的行为不同，它们永远不会相等或相同（identical）。

之所以不要求函数表达式的值具有特定的相等性或同一性，是为了允许编译器进行优化。如果一个函数表达式不依赖于周围的变量，实现可以安全地在多次求值之间共享。例如：

```dart
List<int> ints = [6, 2, 5, 1, 4, 3];
ints.sort((x, y) => x - y);
print(ints);
```

编译器可以将闭包 `(x, y) => x - y` 转换为一个顶层函数。
