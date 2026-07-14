# ZoneCallback

```dart
typedef ZoneCallback<R> = R Function()
```

无参数函数，如 `Zone.run` 的参数。

---

# ZoneUnaryCallback

```dart
typedef ZoneUnaryCallback<R, T> = R Function(T)
```

单参数函数，如 `Zone.runUnary` 的参数。

---

# ZoneBinaryCallback

```dart
typedef ZoneBinaryCallback<R, T1, T2> = R Function(T1, T2)
```

双参数函数，如 `Zone.runBinary` 的参数。

---

# runZoned()

```dart
R runZoned<R>(
  R body(), {
  Map<Object?, Object?>? zoneValues,
  ZoneSpecification? zoneSpecification,
  Function? onError
})
```

在其自己的 zone 中运行 [body]。

基于 [zoneSpecification] 和 [zoneValues]，使用 [Zone.fork] 创建一个新的 zone，然后在该 zone 中运行 [body] 并返回其结果。

使用示例：

```dart
var secret = "arglebargle"; // 或使用随机生成的字符串。
var result = runZoned(
    () async {
      await Future.delayed(Duration(seconds: 5), () {
        print("${Zone.current[#_secret]} glop glyf");
      });
    },
    zoneValues: {#_secret: secret},
    zoneSpecification:
        ZoneSpecification(print: (Zone self, parent, zone, String value) {
      if (value.contains(Zone.current[#_secret] as String)) {
        value = "--censored--";
      }
      parent.print(zone, value);
    }));
secret = ""; // 清除痕迹。
await result; // 等待异步计算完成。
```

这个新 zone 拦截了 `print`，并将一个值存储在私有符号 `#_secret` 下。该密钥可从新的 [Zone](https://www.yuque.com/thyname/dart.async/zone) 对象中获取，该对象即为 [body] 的 [Zone.current]，同时也是 `print` 处理函数的第一个参数 `self`。

如果设置了 [ZoneSpecification.handleUncaughtError]，或者传入了已弃用的 [onError] 回调，创建的 zone 将成为一个 _错误 zone（error zone）_。在 [Zone.errorZone] 不同的 zone 之间，Future 中的异步错误永远不会跨越 zone 边界。这种行为可能导致的一个后果是：在创建的 zone 中以错误方式完成的 [Future](https://www.yuque.com/thyname/dart.async/future)，当在属于不同错误 zone 的 zone 中使用时，看起来将永远不会完成。在错误不可访问的 zone 中多次尝试使用该 Future，将导致该错误在其原始错误 zone 中被 _再次_ 报告。

请参阅 [runZonedGuarded](https://www.yuque.com/thyname/dart.async/runzonedguarded)，以替代使用已弃用的 [onError] 参数。如果提供了 [onError]，此函数也会尝试捕获并处理来自 [body] 的同步错误，但如果泛型参数 [R] 不可为空，仍可能抛出错误并返回 `null`。

---

# runZonedGuarded()

```dart
R? runZonedGuarded<R>(
  R body(),
  void onError(Object error, StackTrace stack), {
  Map<Object?, Object?>? zoneValues,
  ZoneSpecification? zoneSpecification
})
```

在其自己的错误 zone 中运行 [body]。

基于 [zoneSpecification] 和 [zoneValues]，使用 [Zone.fork] 创建一个新的 zone，然后在该 zone 中运行 [body] 并返回其结果。

[onError] 函数既用于通过覆盖 [zoneSpecification] 中的 [ZoneSpecification.handleUncaughtError]（如果有的话）来处理异步错误，也用于处理 [body] 同步抛出的错误。

如果 [body] 中同步发生错误，那么在 [onError] 处理函数中抛出错误会使对 `runZonedGuarded` 的调用抛出该错误，否则对 `runZonedGuarded` 的调用将返回 `null`。

创建的 zone 始终是一个 _错误 zone_。在 [Zone.errorZone] 不同的 zone 之间，Future 中的异步错误永远不会跨越 zone 边界。这种行为可能导致的一个后果是：在创建的 zone 中以错误方式完成的 [Future](https://www.yuque.com/thyname/dart.async/future)，当在属于不同错误 zone 的 zone 中使用时，看起来将永远不会完成。在错误不可访问的 zone 中多次尝试使用该 Future，将导致该错误在其原始错误 zone 中被 _再次_ 报告。
