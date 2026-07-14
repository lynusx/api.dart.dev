# Symbol

```dart
abstract class Symbol {}
```

由 mirror（反射）、调用（invocation）以及 [Function.apply] 使用的不透明名称。

## 构造函数

### Symbol()

```dart
const Symbol( String name )
```

构造一个表示所提供名称的新 [Symbol](https://www.yuque.com/thyname/dart.core/symbol)。

由相等的 [name] 字符串创建的符号本身也是相等的。如果这些符号是使用 `const` 创建的，那么具有相同 [name] 字符串的符号会被规范化并且是同一个（identical）。

某些 [name] 字符串所创建的符号，也可以通过符号字面量（symbol literal）创建，或者在运行 Dart 程序期间隐式创建，例如通过 [Object.noSuchMethod]。

如果 [name] 是一个不以下划线开头的单个 Dart 标识符，或者是一个限定标识符（多个用 `.` 分隔的标识符），或者是一个不同于 `unary-` 的用户可定义运算符的名称（即以下之一："`+`"、"`-`"、"`*`"、"`/`"、"`%`"、"`~/`"、"`&`"、"`|`"、"`^`"、"`~`"、"`<<`"、"`>>`"、"`>>>`"、"`<`"、"`<=`"、"`>`"、"`>=`"、"`==`"、"`[]`" 或 "`[]=`"），那么 `Symbol(name)` 的结果等于通过在 [name] 内容前加上 `#` 所创建的符号字面量，并且 `const Symbol(name)` 与该符号字面量是同一个（identical）。也就是说 `#foo == Symbol("foo")` 且 `identical(#foo, const Symbol("foo"))`。

如果 [name] 是一个不以下划线开头、后跟 `=` 的单个标识符，那么该符号是一个 setter 名称，并且可以与 [Object.noSuchMethod] 调用中的 [Invocation.memberName] 相等。

私有符号字面量，例如 `#_foo`，无法通过 Symbol 构造函数创建。像 `const Symbol("_foo")` 这样的符号并不等于任何符号字面量，也不等于任何由 `noSuchMethod` 引入的源名称符号。

```dart
assert(Symbol("foo") == Symbol("foo"));
assert(Symbol("foo") == #foo);
assert(identical(const Symbol("foo"), const Symbol("foo")));
assert(identical(const Symbol("foo"), #foo));
assert(Symbol("[]=") == #[]=);
assert(identical(const Symbol("[]="), #[]=));
assert(Symbol("foo.bar") == #foo.bar);
assert(identical(const Symbol("foo.bar"), #foo.bar));
```

该实例重写了 [Object.==]。

## 静态属性

### unaryMinus

```dart
Symbol const unaryMinus
```

对应于一元负号运算符名称的符号。

### empty

```dart
Symbol const empty
```

空符号。

空符号是没有库声明的库的名称，也是无名构造函数的基础名称（base-name）。

## 属性

### hashCode

```dart
int get hashCode
```

返回与 [operator==] 兼容的哈希码。

相等的符号具有相同的哈希码。

## 运算符

### operator ==()

```dart
bool operator ==(Object other)
```

符号与其他名称字符串相等（`==`）的符号相等。

表示库私有名称的符号还需要表示来自同一个库的名称。
