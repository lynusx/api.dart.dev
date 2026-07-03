# Expando

```dart
final class Expando<T extends Object> {}
```

[Expando] 允许为对象添加新的属性。

不适用于数字、字符串、布尔值、记录（record）、`null`、`dart:ffi` 指针、`dart:ffi` 结构体或 `dart:ffi` 联合体。

当对象变得不可访问后，`Expando` 不会继续持有已添加的属性值。

由于你总是可以创建一个与现有数字相同的新数字，这意味着数字上的 expando 属性永远无法被释放。为了避免这种情况，expando 属性不能添加到数字上。同样的论证也适用于字符串、布尔值和 `null`，它们的字面量在多次出现时也会得到相同的值。此外，expando 属性也不能添加到记录（record）上，因为记录没有明确定义的持久性标识。

对于其他类没有这样的限制，即使是编译时常量对象也不例外。如果要为编译时常量添加 expando 属性，请务必小心，因为它们将永远存活。

## 构造函数

### Expando()

```dart
Expando<T extends Object>([ String? name ])
```

创建一个新的 [Expando]。可选的 name 参数仅用于调试目的，使用相同名称创建两个不同的 [Expando] 会得到两个作用于不同属性的 [Expando]（即它们所作用的对象属性彼此独立）。

## 属性

### name

```dart
String? name
```

传递给构造函数的该 [Expando] 的名称。

如果构造函数中未传入名称，则该值为 `null`。

## 方法

### toString()

```dart
String toString()
```

Expando 的 toString 方法重写。

## 运算符

### operator []

```dart
T? operator [](Object object)
```

获取该 [Expando] 在给定对象上的属性值。

如果该对象尚未被扩展，结果为 `null`。

该对象不能是数字、字符串、布尔值、记录（record）、`null`、`dart:ffi` 指针、`dart:ffi` 结构体或 `dart:ffi` 联合体。

### operator []=

```dart
void operator []=(Object object, T? value)
```

将该 [Expando] 在给定对象上的属性值设置为 [value]。

可以通过将值设置为 `null` 来有效地移除属性。

该对象不能是数字、字符串、布尔值、记录（record）、`null`、`dart:ffi` 指针、`dart:ffi` 结构体或 `dart:ffi` 联合体。

# WeakReference

```dart
abstract final class WeakReference<T extends Object> {}
```

指向 Dart 对象的弱引用。

对 [target] 对象的**弱**引用，当程序没有其他方式访问目标对象时，该引用可能在任何时候被清除（改为引用 `null`）。

**作为弱引用的目标对象，并不能阻止该对象被垃圾回收。**

即使指向目标对象的所有引用都是弱引用，也不能保证该弱引用一定会被清除。

并非所有对象都支持作为弱引用的目标。[WeakReference] 构造函数会拒绝任何不支持作为 [Expando] 键的对象。

诸如缓存之类的使用场景可以从弱引用中受益。示例：

````dart
/// [CachedComputation] caches the computation result, weakly holding
/// on to the cache.
///
/// If nothing else in the program is holding on the result, and the
/// garbage collector runs, the cache is purged, freeing the memory.
///
/// Until the cache is purged, the computation will not run again on
/// a subsequent request.
///
/// Example use:
/// ```
/// final cached = CachedComputation(
///     () => jsonDecode(someJsonSource) as Object);
/// print(cached.result); // Executes computation.
/// print(cached.result); // Most likely uses cache.
/// ```
class CachedComputation<R extends Object> {
  final R Function() computation;

  WeakReference<R>? _cache;

  CachedComputation(this.computation);

  R get result {
    final cachedResult = _cache?.target;
    if (cachedResult != null) {
      return cachedResult;
    }

    final result = computation();

    // WeakReferences do not support nulls, bools, numbers, and strings.
    if (result is! bool && result is! num && result is! String) {
      _cache = WeakReference(result);
    }

    return result;
  }
}
````

## 构造函数

### WeakReference()

```dart
WeakReference<T extends Object>( T target )
```

创建一个指向给定 [target] 的 [WeakReference]。

### target

```dart
T? get target
```

该 [WeakReference] 当前弱引用的对象（如果存在）。

该值要么是构造函数中提供的对象，要么在弱引用已被清除时为 `null`。

# Finalizer

```dart
abstract final class Finalizer<T> {}
```

可附加到 Dart 对象上的终结器（finalizer）。

通过调用 [attach] 并传入值、**终结令牌（finalization token）**以及可选的**附加键（attach key）**，终结器可以在自身与任意数量的 Dart 值之间创建附件（attachment），这些参数都是该附件的一部分。

当程序无法再访问某个 Dart 值时，当前附加到该值的终结器**可能**会以该附件的终结令牌为参数调用其回调函数。

示例：

```dart
class Database {
  // Keeps the finalizer itself reachable, otherwise it might be disposed
  // before the finalizer callback gets a chance to run.
  static final Finalizer<DBConnection> _finalizer =
      Finalizer((connection) => connection.close());

  final DBConnection _connection;

  Database._fromConnection(this._connection);

  factory Database.connect() {
    // Wraps the connection in a nice user API,
    // *and* closes connection if the user forgets to.
    final connection = DBConnection.connect();
    final wrapper = Database._fromConnection(connection);
    // Calls finalizer callback when `wrapper` is no longer reachable.
    _finalizer.attach(wrapper, connection, detach: wrapper);
    return wrapper;
  }

  void close() {
    // User requested close.
    _connection.close();
    // Detach from finalizer, no longer needed.
    _finalizer.detach(this);
  }

  // Some useful methods.
}
```

本例中包含一个需要清理的外部资源示例。当 API 的使用者不再持有该连接的访问权限时，终结器用于清理外部连接。如果使用者忘记关闭连接，该示例会使用同一个对象同时作为附加对象和分离键，这在每个附加对象都可以单独分离的场景中是一种实用的方法。作为分离键并不会使对象保持存活。

不保证回调一定会被调用。唯一可以保证的是：如果终结器的回调被调用，并传入了某个特定的终结令牌作为参数，那么至少有一个与该终结器存在附加关系、且使用该终结令牌的值，已经无法被程序访问。

如果终结器**本身**变得不可访问，它可以被垃圾回收，此后将不再触发任何回调。请务必确保在终结器仍需使用期间，保持其自身可被访问。

如果多个终结器附加到同一个对象上，或同一个终结器被多次附加到同一个对象上，且该对象变得不可被程序访问，那么这些附件中的任意数量（包括零个）都可能触发其关联终结器的回调。它们不会保证全部触发或全部不触发。

终结回调将作为**事件**发生。它们不会在其他代码执行期间发生，也不会作为微任务（microtask）执行，而是作为类似于定时器事件的高级事件发生。

终结回调不得抛出异常。

在 Dart 原生运行时上运行时，如果回调是原生函数而非 Dart 函数，请改用 `dart:ffi` 的 [NativeFinalizer]。

## 构造函数

### Finalizer()

```dart
Finalizer(void Function(T) callback)
```

使用给定的终结回调函数创建一个终结器。

[callback] 会在 [Finalizer] 创建时绑定到当前 zone，并在被调用时在该 zone 中运行。

## 方法

### attach()

```dart
void attach(
  Object value,
  T finalizationToken, {
  Object? detach,
})
```

将该终结器附加到 [value] 上。

当 [value] 不再能被程序访问，且仍与该终结器保持附加关系时，该终结器的回调**可能**会以 [finalizationToken] 作为参数被调用。对于每个未通过调用 [Finalizer.detach] 分离的有效附件，其回调最多只会被调用一次。

[detach] 值仅由终结器用于标识当前附件。如果提供了非 `null` 的 [detach] 值，则可以将相同（同一）的值传递给 [Finalizer.detach] 来移除该附件。如果未提供非 `null` 的值，则该附件无法再被移除。

[value] 和 [detach] 参数不会使这些对象被视为可被程序访问。两者都必须是支持作为 [Expando] 键的对象，且可以是**同一个**对象。

示例：

```dart
class Database {
  // Keeps the finalizer itself reachable, otherwise it might be disposed
  // before the finalizer callback gets a chance to run.
  static final Finalizer<DBConnection> _finalizer =
      Finalizer((connection) => connection.close());

  factory Database.connect() {
    // Wraps the connection in a nice user API,
    // *and* closes connection if the user forgets to.
    final connection = DBConnection.connect();
    final wrapper = Database._fromConnection();
    // Calls finalizer callback when `wrapper` is no longer reachable.
    _finalizer.attach(wrapper, connection, detach: wrapper);
    return wrapper;
  }

  Database._fromConnection();

  // Some useful methods.
}
```

多个对象可以使用相同的终结令牌进行附加，同一个终结器也可以使用不同或相同的终结令牌多次附加到同一个对象上。

### detach()

```dart
void detach(Object detach)
```

将该终结器从使用 [detach] 附加的值上分离。

移除该终结器与某个值之间的每一个附件，这些附件是通过调用 [attach] 并将 [detach] 对象作为 `detach` 参数所创建的。

如果该终结器使用不同的分离键（detachment key）多次附加到同一个值上，则只会移除使用了 [detach] 的那些附件。

分离之后，即使对象变得不可访问，该附件也不会再触发任何回调。

示例：

```dart
class Database {
  // Keeps the finalizer itself reachable, otherwise it might be disposed
  // before the finalizer callback gets a chance to run.
  static final Finalizer<DBConnection> _finalizer =
      Finalizer((connection) => connection.close());

  final DBConnection _connection;

  Database._fromConnection(this._connection);

  void close() {
    // User requested close.
    _connection.close();
    // Detach from finalizer, no longer needed.
    // Was attached using this object as `detach` token.
    _finalizer.detach(this);
  }

  // Some useful methods.
}
```
