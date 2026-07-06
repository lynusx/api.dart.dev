# ByteData

```dart
abstract final class ByteData implements TypedData {}
```

一个固定长度、支持随机访问的字节序列，同时提供对这些字节所表示的定宽整数和浮点数的随机及非对齐访问。

`ByteData` 可用于打包和解包来自外部源（如网络或文件系统）的数据，并且比使用普通的 [List] 实现更高效地处理大量数值数据。`ByteData` 可以通过消除对象头的需要来节省空间，并通过消除数据复制的需要来节省时间。

如果数据以字节形式传入，可以通过共享相同的缓冲区将其转换为 `ByteData`。

```dart
Uint8List bytes = ...;
var blob = ByteData.sublistView(bytes);
if (blob.getUint32(0, Endian.little) == 0x04034b50) { // Zip file marker
  ...
}
```

最后，`ByteData` 还可用于有意地将表示一种算术类型的字节重新解释为另一种类型。例如，以下代码片段确定一个 32 位浮点数（以大端序存储）的字节所表示的 32 位有符号整数是什么（同样以大端序存储）：

```dart
var bdata = ByteData(8);
bdata.setFloat32(0, 3.04);
int huh = bdata.getInt32(0); // 0x40428f5c
```

类尝试继承或实现 `ByteData` 是编译时错误。

### ByteData()

```dart
ByteData(int length)
```

创建一个指定长度（以元素为单位）的 [ByteData]，其所有字节初始均为零。

### ByteData.view()

```dart
ByteData.view(ByteBuffer buffer, [int offsetInBytes = 0, int? length])
```

创建 [buffer] 中指定区域的 [ByteData] _视图_。

对 [ByteData] 的更改将在字节缓冲区中可见，反之亦然。如果未指定该区域的 [offsetInBytes] 索引，则默认为零（字节缓冲区中的第一个字节）。如果未提供长度，则视图将延伸到字节缓冲区的末尾。

[offsetInBytes] 和 [length] 必须为非负数，且 [offsetInBytes] + [length] 必须小于或等于 [buffer] 的长度。

请注意，当从 [TypedData] 列表或字节数据创建视图时，该列表或字节数据本身可能是一个更大缓冲区上的视图，其 [TypedData.offsetInBytes] 大于零。仅仅执行 `ByteData.view(other.buffer, 0, count)` 可能不会指向你预期的字节。相反，你可能需要执行：

```dart
ByteData.view(other.buffer, other.offsetInBytes, count)
```

或者，使用 [ByteData.sublistView]，它包含了这个计算过程：

```dart
ByteData.sublistView(other, 0, count);
```

（第三个参数是结束索引而不是长度，因此如果从大于零的位置开始，则不需要相应地减少计数。）

### ByteData.sublistView()

```dart
ByteData.sublistView(TypedData data, [int start = 0, int? end])
```

在 [data] 的元素范围上创建一个 [ByteData] 视图。

在与 [data] 从 [start] 到 [end] 的元素相对应的 `data.buffer` 范围上创建一个视图。如果 [data] 是一个类型化数据列表，如 [Uint16List]，则该视图基于索引从 [start] 到 [end] 的元素所对应的字节。如果 [data] 是一个 [ByteData]，则将其视为字节列表处理。

如果提供了 [start] 和 [end]，它们必须满足

0 &le; `start` &le; `end` &le; _elementCount_

其中 _elementCount_ 是 [data] 中的元素个数，对于类型化数据列表，它与 [List.length] 相同。

如果省略，[start] 默认为零，[end] 默认为 _elementCount_。

### asUnmodifiableView()

```dart
ByteData asUnmodifiableView()
```

此 [ByteData] 的一个只读视图。

### getInt8()

```dart
int getInt8(int byteOffset)
```

此对象中指定 [byteOffset] 处的字节所表示的（可能为负的）整数，采用二进制补码表示。

返回值将介于 -128 到 127 之间（包含两端）。

[byteOffset] 必须为非负数，且小于此对象的长度。

### setInt8()

```dart
void setInt8(int byteOffset, int value)
```

将此对象中指定 [byteOffset] 处的字节设置为指定 [value] 的二进制补码表示，该值必须能放入一个字节中。

换句话说，[value] 必须介于 -128 到 127 之间（包含两端）。

[byteOffset] 必须为非负数，且小于此对象的长度。

### getUint8()

```dart
int getUint8(int byteOffset)
```

此对象中指定 [byteOffset] 处的字节所表示的正整数，采用无符号二进制形式。

返回值将介于 0 到 255 之间（包含两端）。

[byteOffset] 必须为非负数，且小于此对象的长度。

### setUint8()

```dart
void setUint8(int byteOffset, int value)
```

将此对象中指定 [byteOffset] 处的字节设置为指定 [value] 的无符号二进制表示，该值必须能放入一个字节中。

换句话说，[value] 必须介于 0 到 255 之间（包含两端）。

[byteOffset] 必须为非负数，且小于此对象的长度。

### getInt16()

```dart
int getInt16(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的两个字节所表示的（可能为负的）整数，采用二进制补码形式。

返回值将介于 -2<sup>15</sup> 到 2<sup>15</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 2` 必须小于或等于此对象的长度。

### setInt16()

```dart
void setInt16(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的两个字节设置为指定 [value] 的二进制补码表示，该值必须能放入两个字节中。

换句话说，[value] 必须介于 -2<sup>15</sup> 到 2<sup>15</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 2` 必须小于或等于此对象的长度。

### getUint16()

```dart
int getUint16(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的两个字节所表示的正整数，采用无符号二进制形式。

返回值将介于 0 到 2<sup>16</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 2` 必须小于或等于此对象的长度。

### setUint16()

```dart
void setUint16(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的两个字节设置为指定 [value] 的无符号二进制表示。

换句话说，[value] 必须介于 0 到 2<sup>16</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 2` 必须小于或等于此对象的长度。

### getInt32()

```dart
int getInt32(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的四个字节所表示的（可能为负的）整数，采用二进制补码形式。

返回值将介于 -2<sup>31</sup> 到 2<sup>31</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### setInt32()

```dart
void setInt32(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的四个字节设置为指定 [value] 的二进制补码表示，该值必须能放入四个字节中。

换句话说，[value] 必须介于 -2<sup>31</sup> 到 2<sup>31</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### getUint32()

```dart
int getUint32(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的四个字节所表示的正整数，采用无符号二进制形式。

返回值将介于 0 到 2<sup>32</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### setUint32()

```dart
void setUint32(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的四个字节设置为指定 [value] 的无符号二进制表示，该值必须能放入四个字节中。

换句话说，[value] 必须介于 0 到 2<sup>32</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### getInt64()

```dart
int getInt64(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的八个字节所表示的（可能为负的）整数，采用二进制补码形式。

返回值将介于 -2<sup>63</sup> 到 2<sup>63</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。

### setInt64()

```dart
void setInt64(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的八个字节设置为指定 [value] 的二进制补码表示，该值必须能放入八个字节中。

换句话说，[value] 必须介于 -2<sup>63</sup> 到 2<sup>63</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。

### getUint64()

```dart
int getUint64(int byteOffset, [Endian endian = Endian.big])
```

此对象中从指定 [byteOffset] 开始的八个字节所表示的正整数，采用无符号二进制形式。

返回值将介于 0 到 2<sup>64</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。

### setUint64()

```dart
void setUint64(int byteOffset, int value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的八个字节设置为指定 [value] 的无符号二进制表示，该值必须能放入八个字节中。

换句话说，[value] 必须介于 0 到 2<sup>64</sup> - 1 之间（包含两端）。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。

### getFloat32()

```dart
double getFloat32(int byteOffset, [Endian endian = Endian.big])
```

此对象中指定 [byteOffset] 处的四个字节所表示的浮点数，采用 IEEE 754 单精度二进制浮点格式（binary32）。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### setFloat32()

```dart
void setFloat32(int byteOffset, double value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的四个字节设置为指定 [value] 的 IEEE 754 单精度二进制浮点（binary32）表示。

**请注意，此方法可能会丢失精度。** 输入的 [value] 是一个 64 位浮点值，在存储之前将根据 IEEE 754 舍入规则转换为 32 位浮点值。如果 [value] 不能精确地表示为 binary32，它将被转换为最接近的 binary32 值。如果两个 binary32 值同样接近，则使用最低有效位为零的那个。请注意，有限（但很大）的值可能会被转换为无穷大，而较小的非零值可能会被转换为零。

[byteOffset] 必须为非负数，且 `byteOffset + 4` 必须小于或等于此对象的长度。

### getFloat64()

```dart
double getFloat64(int byteOffset, [Endian endian = Endian.big])
```

此对象中指定 [byteOffset] 处的八个字节所表示的浮点数，采用 IEEE 754 双精度二进制浮点格式（binary64）。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。

### setFloat64()

```dart
void setFloat64(int byteOffset, double value, [Endian endian = Endian.big])
```

将此对象中从指定 [byteOffset] 开始的八个字节设置为指定 [value] 的 IEEE 754 双精度二进制浮点（binary64）表示。

[byteOffset] 必须为非负数，且 `byteOffset + 8` 必须小于或等于此对象的长度。
