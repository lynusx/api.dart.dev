# FutureIterable

```dart
extension FutureIterable<T> on Iterable<Future<T>> {}
```

### wait

```dart
Future<List<T>> get wait
```

并行等待多个 Future。

等待此可迭代对象中的所有 Future。如果所有 Future 都成功完成，则返回一个结果值列表，其顺序与创建这些 Future 的顺序相同。

与 [Future.wait] 类似，但使用 [ParallelWaitError] 报告错误，使调用方可以在必要时处理错误并释放已成功的结果。

当所有 Future 都完成时，返回的 Future 才会完成。如果其中任意一个 Future 未完成，则返回的 Future 也不会完成。

如果任意一个 Future 以错误完成，则返回的 Future 会以 [ParallelWaitError] 完成。[ParallelWaitError.values] 是一个列表，包含成功的 Future 的值，出错的 Future 对应位置为 `null`。[ParallelWaitError.errors] 是一个长度相同的列表，成功的 Future 对应位置为 `null`，出错的 Future 对应位置为包含该错误的 [AsyncError]。

# FutureRecord2

```dart
extension FutureRecord2<T1, T2> on (Future<T1>, Future<T2>) {}
```

对一个 Future 记录（record）执行并行操作。

并行等待多个 Future。

等待此记录中的所有 Future。如果所有 Future 都成功完成，则返回一个值记录。

当所有 Future 都完成时，返回的 Future 才会完成。如果其中任意一个 Future 未完成，则返回的 Future 也不会完成。

如果部分 Future 以错误完成，则返回的 Future 会以 [ParallelWaitError] 完成。[ParallelWaitError.values] 是一个记录，包含成功的 Future 的值，出错的 Future 对应位置为 `null`。[ParallelWaitError.errors] 是一个形状相同的记录，成功的 Future 对应位置为 `null`，出错的 Future 对应位置为包含该错误的 [AsyncError]。

### wait

```dart
Future<(T1, T2)> get wait
```

并行等待多个 Future。

等待此记录中的所有 Future。如果所有 Future 都成功完成，则返回一个值记录。

当所有 Future 都完成时，返回的 Future 才会完成。如果其中任意一个 Future 未完成，则返回的 Future 也不会完成。

如果部分 Future 以错误完成，则返回的 Future 会以 [ParallelWaitError](https://api.dart.dev/dart-async/ParallelWaitError-class.html) 完成。[ParallelWaitError.values](https://api.dart.dev/dart-async/ParallelWaitError/values.html) 是一个记录，包含成功的 Future 的值，出错的 Future 对应位置为 `null`。[ParallelWaitError.errors](https://api.dart.dev/dart-async/ParallelWaitError/errors.html) 是一个形状相同的记录，成功的 Future 对应位置为 `null`，出错的 Future 对应位置为包含该错误的 [AsyncError](https://api.dart.dev/dart-async/AsyncError-class.html)。

# ParallelWaitError

```dart
class ParallelWaitError<V, E> extends Error {}
```

等待多个 Future 时，若部分出错则抛出此错误。

[V] 和 [E] 类型的基本形状与所等待的原始 Future 集合相同。

例如，如果原始等待的 Future 是一个记录 `(Future<T1>, ..., Future<Tn>)`，则类型 `V` 将是 `(T1?, ..., Tn?)`，用于保留以值完成的 Future 的结果；`E` 将是 `(AsyncError?, ..., AsyncError?)`，同样具有 _n_ 个字段，用于保存以错误完成的 Future 的错误信息。

等待一个 Future 列表或可迭代对象时，应提供长度相同的可空值列表和错误列表。

## 构造函数

### ParallelWaitError()

```dart
ParallelWaitError<V, E>(
  V values,
  E errors, {
  @Since.new("3.4") int? errorCount,
  @Since.new("3.4") AsyncError? defaultError,
})
```

使用提供的 [values] 和 [errors] 创建错误。

如果提供了 [defaultError]，则其 [AsyncError.error] 会用于此并行错误的 [toString]，其 [AsyncError.stackTrace] 会作为 [stackTrace] 返回。

如果提供了 [errorCount] 且大于 1，则该数字会在 [toString] 中报告。

## 属性

### values

```dart
V values
```

成功的 Future 的值。

形状与原始 Future 集合相同，每个成功的 Future 对应其值，每个失败的 Future 对应 `null`。

### errors

```dart
E errors
```

失败的 Future 的错误。

形状与原始 Future 集合相同，每个失败的 Future 对应其错误（通常为 [AsyncError]），每个成功的 Future 对应 `null`。
