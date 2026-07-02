# Sink

```dart
abstract interface class Sink<T> {}
```

一个通用的数据目的地。

多个数据值可以被放入 sink 中，当没有更多数据可用时，应该关闭该 sink。

这是一个通用接口，其他数据接收方可以实现它。

## 方法

### add()

```dart
void add(T data)
```

将 [data] 添加到该 sink。

不能在调用 [close] 之后调用此方法。

### close()

```dart
void close()
```

关闭该 sink。

在调用此方法之后，不能再调用 [add] 方法。

多次调用此方法是允许的，但不会产生任何效果。
