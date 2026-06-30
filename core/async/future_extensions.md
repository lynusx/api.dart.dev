# FutureIterable

```dart
extension FutureIterable<T> on Iterable<Future<T>> {}
```

### wait

```dart
Future<List<T>> get wait
```

Waits for futures in parallel.

Waits for all the futures in this iterable. Returns a list of the resulting values, in the same order as the futures which created them, if all futures are successful.

Similar to [Future.wait], but reports errors using a [ParallelWaitError], which allows the caller to handle errors and dispose successful results if necessary.

The returned future is completed when all the futures have completed. If any of the futures do not complete, nor does the returned future.

If any future completes with an error, the returned future completes with a [ParallelWaitError]. The [ParallelWaitError.values] is a list of the values for successful futures and `null` for futures with errors. The [ParallelWaitError.errors] is a list of the same length, with `null` values for the successful futures and an [AsyncError] with the error for futures which completed with an error.

# FutureRecord2

```dart
extension FutureRecord2<T1, T2> on (Future<T1>, Future<T2>) {}
```

Parallel operations on a record of futures.

{@template record-parallel-wait} Waits for futures in parallel.

Waits for all the futures in this record. Returns a record of the values, if all futures are successful.

The returned future is completed when all the futures have completed. If any of the futures do not complete, nor does the returned future.

If some futures complete with an error, the returned future completes with a [ParallelWaitError]. The [ParallelWaitError.values] is a record of the values of successful futures, and `null` for futures with errors. The [ParallelWaitError.errors] is a record of the same shape, with `null` values for the successful futures and an [AsyncError] with the error of futures which completed with an error. {@endtemplate}

### wait

```dart
Future<(T1, T2)> get wait
```

{@macro record-parallel-wait}

# FutureRecord3

```dart
extension FutureRecord3<T1, T2, T3> on (Future<T1>, Future<T2>, Future<T3>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3)> get wait
```

{@macro record-parallel-wait}

# FutureRecord4

```dart
extension FutureRecord4<T1, T2, T3, T4> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4)> get wait
```

{@macro record-parallel-wait}

# FutureRecord5

```dart
extension FutureRecord5<T1, T2, T3, T4, T5> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>, Future<T5>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4, T5)> get wait
```

{@macro record-parallel-wait}

# FutureRecord6

```dart
extension FutureRecord6<T1, T2, T3, T4, T5, T6> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>, Future<T5>, Future<T6>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4, T5, T6)> get wait
```

{@macro record-parallel-wait}

# FutureRecord7

```dart
extension FutureRecord7<T1, T2, T3, T4, T5, T6, T7> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>, Future<T5>, Future<T6>, Future<T7>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4, T5, T6, T7)> get wait
```

{@macro record-parallel-wait}

# FutureRecord8

```dart
extension FutureRecord8<T1, T2, T3, T4, T5, T6, T7, T8> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>, Future<T5>, Future<T6>, Future<T7>, Future<T8>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4, T5, T6, T7, T8)> get wait
```

{@macro record-parallel-wait}

# FutureRecord9

```dart
extension FutureRecord9<T1, T2, T3, T4, T5, T6, T7, T8, T9> on (Future<T1>, Future<T2>, Future<T3>, Future<T4>, Future<T5>, Future<T6>, Future<T7>, Future<T8>, Future<T9>) {}
```

Parallel operations on a record of futures.

### wait

```dart
Future<(T1, T2, T3, T4, T5, T6, T7, T8, T9)> get wait
```

{@macro record-parallel-wait}

# ParallelWaitError

```dart
class ParallelWaitError<V, E> extends Error {}
```

Error thrown when waiting for multiple futures, when some have errors.

The [V] and [E] types will have the same basic shape as the original collection of futures that was waited on.

For example, if the original awaited futures were a record `(Future<T1>, ..., Future<Tn>)`, the type `V` will be `(T1?, ..., Tn?)` which allows keeping the values of futures that completed with a value, and `E` will be `(AsyncError?, ..., AsyncError?)`, also with _n_ fields, which can contain the errors for the futures which completed with an error.

Waiting for a list or iterable of futures should provide a list of nullable values and errors of the same length.

### values

```dart
V values
```

Values of successful futures.

Has the same shape as the original collection of futures, with values for each successful future and `null` values for each failing future.

### errors

```dart
E errors
```

Errors of failing futures.

Has the same shape as the original collection of futures, with errors, typically [AsyncError], for each failing future and `null` values for each successful future.

### ParallelWaitError()

```dart
ParallelWaitError(V values, E errors, {int? _errorCount, AsyncError? _defaultError})
```

Creates error with the provided [values] and [errors].

If [_defaultError] is provided, its [AsyncError.error] is used in the [toString] of this parallel error, and its [AsyncError.stackTrace] is returned by [stackTrace].

If [_errorCount] is provided, and it's greater than one, the number is reported in the [toString].

### toString()

```dart
String toString()
```

### stackTrace

```dart
StackTrace? get stackTrace
```
