# ByteBuffer

```dart
abstract final class ByteBuffer {}
```

一个底层字节序列，是类型化数据对象的基础。

用于通过类型化视图更高效地处理大量二进制或数值数据。

类尝试继承或实现 `ByteBuffer` 是编译时错误。

### lengthInBytes

```dart
int get lengthInBytes
```

此字节缓冲区的长度（以字节为单位）。

### asUint8List()

```dart
Uint8List asUint8List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Uint8List](https://www.yuque.com/thyname/dart.typed_data/uint8list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Uint8List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes] 开始，包含 [length] 个字节。如果省略 [length]，范围将延伸到缓冲区末尾。

起始索引和长度必须描述缓冲区的一个有效范围：

- `offsetInBytes` 不能为负数，
- `length` 不能为负数，且
- `offsetInBytes + length` 不能大于 [lengthInBytes]。

### asInt8List()

```dart
Int8List asInt8List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Int8List](https://www.yuque.com/thyname/dart.typed_data/int8list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Int8List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes] 开始，包含 [length] 个字节。如果省略 [length]，范围将延伸到缓冲区末尾。

起始索引和长度必须描述缓冲区的一个有效范围：

- `offsetInBytes` 不能为负数，
- `length` 不能为负数，且
- `offsetInBytes + length` 不能大于 [lengthInBytes]。

### asUint8ClampedList()

```dart
Uint8ClampedList asUint8ClampedList([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Uint8ClampedList](https://www.yuque.com/thyname/dart.typed_data/uint8clampedlist) _视图_。

该视图由此字节缓冲区的字节支持。对 `Uint8ClampedList` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes] 开始，包含 [length] 个字节。如果省略 [length]，范围将延伸到缓冲区末尾。

起始索引和长度必须描述缓冲区的一个有效范围：

- `offsetInBytes` 不能为负数，
- `length` 不能为负数，且
- `offsetInBytes + length` 不能大于 [lengthInBytes]。

### asUint16List()

```dart
Uint16List asUint16List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Uint16List](https://www.yuque.com/thyname/dart.typed_data/uint16list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Uint16List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 16 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 16 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不是偶数，则最后一个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 16 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 2 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 2` 不能大于 [lengthInBytes]。

### asInt16List()

```dart
Int16List asInt16List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Int16List](https://www.yuque.com/thyname/dart.typed_data/int16list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Int16List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 16 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 16 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不是偶数，则最后一个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 16 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 2 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 2` 不能大于 [lengthInBytes]。

### asUint32List()

```dart
Uint32List asUint32List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Uint32List](https://www.yuque.com/thyname/dart.typed_data/uint32list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Uint32List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 32 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 32 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 4 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 32 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 4 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 4` 不能大于 [lengthInBytes]。

### asInt32List()

```dart
Int32List asInt32List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Int32List](https://www.yuque.com/thyname/dart.typed_data/int32list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Int32List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 32 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 32 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 4 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 32 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 4 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 4` 不能大于 [lengthInBytes]。

### asUint64List()

```dart
Uint64List asUint64List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Uint64List](https://www.yuque.com/thyname/dart.typed_data/uint64list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Uint64List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 64 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 64 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 8 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 64 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 8 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 8` 不能大于 [lengthInBytes]。

### asInt64List()

```dart
Int64List asInt64List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Int64List](https://www.yuque.com/thyname/dart.typed_data/int64list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Int64List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 64 位对齐）开始，包含 [length] 个与主机字节序（[Endian.host]）相同的 64 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 8 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 64 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 8 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 8` 不能大于 [lengthInBytes]。

### asInt32x4List()

```dart
Int32x4List asInt32x4List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Int32x4List](https://www.yuque.com/thyname/dart.typed_data/int32x4list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Int32x4List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 128 位对齐）开始，包含 [length] 个 128 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 16 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 128 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 16 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 16` 不能大于 [lengthInBytes]。

### asFloat32List()

```dart
Float32List asFloat32List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Float32List](https://www.yuque.com/thyname/dart.typed_data/float32list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Float32List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 32 位对齐）开始，包含 [length] 个 32 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 4 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 32 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 4 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 4` 不能大于 [lengthInBytes]。

### asFloat64List()

```dart
Float64List asFloat64List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Float64List](https://www.yuque.com/thyname/dart.typed_data/float64list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Float64List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 64 位对齐）开始，包含 [length] 个 64 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 8 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 64 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 8 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 8` 不能大于 [lengthInBytes]。

### asFloat32x4List()

```dart
Float32x4List asFloat32x4List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Float32x4List](https://www.yuque.com/thyname/dart.typed_data/float32x4list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Float32x4List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 128 位对齐）开始，包含 [length] 个 128 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 16 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 128 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 16 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 16` 不能大于 [lengthInBytes]。

### asFloat64x2List()

```dart
Float64x2List asFloat64x2List([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [Float64x2List](https://www.yuque.com/thyname/dart.typed_data/float64x2list) _视图_。

该视图由此字节缓冲区的字节支持。对 `Float64x2List` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes]（必须为 128 位对齐）开始，包含 [length] 个 128 位整数。如果省略 [length]，范围将尽可能向缓冲区末尾延伸——如果 [lengthInBytes] 不能被 16 整除，则最后几个字节不能作为视图的一部分。

起始索引和长度必须描述缓冲区的一个有效的 128 位对齐范围：

- `offsetInBytes` 不能为负数，
- `offsetInBytes` 必须能被 16 整除，
- `length` 不能为负数，且
- `offsetInBytes + length * 16` 不能大于 [lengthInBytes]。

### asByteData()

```dart
ByteData asByteData([int offsetInBytes = 0, int? length])
```

创建此字节缓冲区某个区域的 [ByteData](https://www.yuque.com/thyname/dart.typed_data/bytedata) _视图_。

该视图由此字节缓冲区的字节支持。对 `ByteData` 所做的任何更改也会更改缓冲区，反之亦然。

被查看的区域从 [offsetInBytes] 开始，包含 [length] 个字节。如果省略 [length]，范围将延伸到缓冲区末尾。

起始索引和长度必须描述缓冲区的一个有效范围：

- `offsetInBytes` 不能为负数，
- `length` 不能为负数，且
- `offsetInBytes + length` 不能大于 [lengthInBytes]。
