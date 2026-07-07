# Capability

```dart
abstract interface class Capability {}
```

一种不可伪造的对象，在通过其他 isolate 传递后返回时，仍与原对象相等。

将 `Capability`（权限凭证）对象发送到另一个 isolate 后再取回，得到的对象会与原始对象相等。除此之外，没有其他方式能创建出与某个 `Capability` 对象相等的对象。

`Capability` 可用作访问保护机制。一个 isolate 可以接受来自其他 isolate 的操作请求，但仅在请求携带正确的 `Capability` 对象时才予以放行。这样便可以向多个客户端暴露同一套接口，同时将部分操作限制为仅对持有相应 `Capability` 的客户端开放。

`Capability` 也可以在单个 isolate 内部使用，但这样做相比直接用 `Object()` 创建一个唯一对象并无优势，对于运行在同一 isolate 中的其他代码而言，也无法提供真正的安全保障。

### Capability()

```dart
Capability()
```

创建一个新的不可伪造的 `Capability` 对象。
