# ZLibOption

```dart
abstract final class ZLibOption {}
```

暴露 ZLib 的输入参数选项。

更多文档参见 http://www.zlib.net/manual.html。

### minWindowBits

```dart
int minWindowBits
```

[ZLibCodec.windowBits]、[ZLibEncoder.windowBits] 和 [ZLibDecoder.windowBits] 的最小值。

### maxWindowBits

```dart
int maxWindowBits
```

[ZLibCodec.windowBits]、[ZLibEncoder.windowBits] 和 [ZLibDecoder.windowBits] 的最大值。

### defaultWindowBits

```dart
int defaultWindowBits
```

[ZLibCodec.windowBits]、[ZLibEncoder.windowBits] 和 [ZLibDecoder.windowBits] 的默认值。

### minLevel

```dart
int minLevel
```

[ZLibCodec.level] 和 [ZLibEncoder.level] 的最小值。

### maxLevel

```dart
int maxLevel
```

[ZLibCodec.level] 和 [ZLibEncoder.level] 的最大值。

### defaultLevel

```dart
int defaultLevel
```

[ZLibCodec.level] 和 [ZLibEncoder.level] 的默认值。

### minMemLevel

```dart
int minMemLevel
```

[ZLibCodec.memLevel] 和 [ZLibEncoder.memLevel] 的最小值。

### maxMemLevel

```dart
int maxMemLevel
```

[ZLibCodec.memLevel] 和 [ZLibEncoder.memLevel] 的最大值。

### defaultMemLevel

```dart
int defaultMemLevel
```

[ZLibCodec.memLevel] 和 [ZLibEncoder.memLevel] 的默认值。

### strategyFiltered

```dart
int strategyFiltered
```

针对经过滤波（或预测）处理的数据推荐使用的策略。

### strategyHuffmanOnly

```dart
int strategyHuffmanOnly
```

使用此策略可强制仅使用哈夫曼编码（不进行字符串匹配）。

### strategyRle

```dart
int strategyRle
```

使用此策略可将匹配距离限制为 1（游程编码）。

### strategyFixed

```dart
int strategyFixed
```

此策略禁止使用动态哈夫曼编码，从而便于实现更简单的解码器。

### strategyDefault

```dart
int strategyDefault
```

针对普通数据推荐使用的策略。

# zlib

```dart
ZLibCodec zlib
```

[ZLibCodec](https://www.yuque.com/thyname/dart.io/zlibcodec) 默认实现的一个实例。

# ZLibCodec

```dart
final class ZLibCodec extends Codec<List<int>, List<int>> {}
```

[ZLibCodec](https://www.yuque.com/thyname/dart.io/zlibcodec) 用于将原始字节编码为 ZLib 压缩字节，并将 ZLib 压缩字节解码为原始字节。

### gzip

```dart
bool gzip
```

若为 true，将在压缩数据中添加 `GZip` 帧。

### level

```dart
int level
```

压缩[级别][level]的取值范围为 `-1..9`，默认压缩级别为 `6`。高于 `6` 的级别会以更多的 CPU 与内存开销换取更高的压缩率；低于 `6` 的级别则以较低的压缩率换取更少的 CPU 与内存占用。

### memLevel

```dart
int memLevel
```

指定为内部压缩状态分配多少内存。`1` 使用最少内存但速度较慢且降低压缩比；`9` 使用最大内存以获得最佳速度。默认值为 `8`。

deflate 所需的内存量（以字节为单位）为：

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

即：windowBits = 15（默认值）时为 128K，memLevel = 8（默认值）时为 128K。

### strategy

```dart
int strategy
```

调整压缩算法。对普通数据使用 strategyDefault；对经过滤波（或预测）处理的数据使用 strategyFiltered；若要强制仅使用哈夫曼编码（不进行字符串匹配）则使用 strategyHuffmanOnly；若要将匹配距离限制为 1（游程编码）则使用 strategyRle。

### windowBits

```dart
int windowBits
```

窗口大小（历史缓冲区大小）以 2 为底的对数，取值范围应为 8..15。数值越大压缩效果越好，但内存开销也越大。默认值为 15。

### raw

```dart
bool raw
```

若为 true，deflate 生成的原始数据将不包含 zlib 头部与尾部，也不会计算 adler32 校验值。

### dictionary

```dart
List<int>? dictionary
```

初始压缩字典。

字典应由预期会在待压缩数据中later出现的字符串（字节序列）组成，最常用的字符串应尽量放在字典末尾。当待压缩数据较短且能被较为准确地预测时，使用字典最为有效；此时压缩效果会优于默认的空字典。

### ZLibCodec()

```dart
ZLibCodec({int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false, bool gzip = false})
```

### encoder

```dart
ZLibEncoder get encoder
```

获取用于编码为 `ZLib` 压缩数据的 [ZLibEncoder](https://www.yuque.com/thyname/dart.io/zlibencoder)。

### decoder

```dart
ZLibDecoder get decoder
```

获取用于解码 `ZLib` 压缩数据的 [ZLibDecoder](https://www.yuque.com/thyname/dart.io/zlibdecoder)。

# gzip

```dart
GZipCodec gzip
```

[GZipCodec](https://www.yuque.com/thyname/dart.io/gzipcodec) 默认实现的一个实例。

# GZipCodec

```dart
final class GZipCodec extends Codec<List<int>, List<int>> {}
```

[GZipCodec](https://www.yuque.com/thyname/dart.io/gzipcodec) 用于将原始字节编码为 GZip 压缩字节，并将 GZip 压缩字节解码为原始字节。

### gzip

```dart
bool gzip
```

若为 true，将在压缩数据中添加 `GZip` 帧。

### level

```dart
int level
```

压缩[级别][level]的取值范围为 `-1..9`，默认压缩级别为 `6`。高于 `6` 的级别会以更多的 CPU 与内存开销换取更高的压缩率；低于 `6` 的级别则以较低的压缩率换取更少的 CPU 与内存占用。

### memLevel

```dart
int memLevel
```

指定为内部压缩状态分配多少内存。`1` 使用最少内存但速度较慢且降低压缩比；`9` 使用最大内存以获得最佳速度。默认值为 `8`。

deflate 所需的内存量（以字节为单位）为：

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

即：windowBits = 15（默认值）时为 128K，memLevel = 8（默认值）时为 128K。

### strategy

```dart
int strategy
```

调整压缩算法。对普通数据使用 [ZLibOption.strategyDefault]；对经过滤波（或预测）处理的数据使用 [ZLibOption.strategyFiltered]；若要强制仅使用哈夫曼编码（不进行字符串匹配）则使用 [ZLibOption.strategyHuffmanOnly]；若要将匹配距离限制为 1（游程编码）则使用 [ZLibOption.strategyRle]。

### windowBits

```dart
int windowBits
```

窗口大小（历史缓冲区大小）以 2 为底的对数，取值范围应为 `8..15`。数值越大压缩效果越好，但内存开销也越大。默认值为 `15`。

### dictionary

```dart
List<int>? dictionary
```

初始压缩字典。

字典应由预期会在待压缩数据中later出现的字符串（字节序列）组成，最常用的字符串应尽量放在字典末尾。当待压缩数据较短且能被较为准确地预测时，使用字典最为有效；此时压缩效果会优于默认的空字典。

### raw

```dart
bool raw
```

若为 true，deflate 生成的原始数据将不包含 zlib 头部与尾部，也不会计算 adler32 校验值。

### GZipCodec()

```dart
GZipCodec({int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false, bool gzip = true})
```

### encoder

```dart
ZLibEncoder get encoder
```

获取用于编码为 `GZip` 压缩数据的 [ZLibEncoder](https://www.yuque.com/thyname/dart.io/zlibencoder)。

### decoder

```dart
ZLibDecoder get decoder
```

获取用于解码 `GZip` 压缩数据的 [ZLibDecoder](https://www.yuque.com/thyname/dart.io/zlibdecoder)。

# ZLibEncoder

```dart
final class ZLibEncoder extends Converter<List<int>, List<int>> {}
```

[ZLibEncoder](https://www.yuque.com/thyname/dart.io/zlibencoder) 编码器由 [ZLibCodec](https://www.yuque.com/thyname/dart.io/zlibcodec) 和 [GZipCodec](https://www.yuque.com/thyname/dart.io/gzipcodec) 用于压缩数据。

### gzip

```dart
bool gzip
```

若为 true，将在压缩数据中添加 `GZip` 帧。

### level

```dart
int level
```

压缩[级别][level]的取值范围为 `-1..9`，默认压缩级别为 `6`。高于 `6` 的级别会以更多的 CPU 与内存开销换取更高的压缩率；低于 `6` 的级别则以较低的压缩率换取更少的 CPU 与内存占用。

### memLevel

```dart
int memLevel
```

指定为内部压缩状态分配多少内存。`1` 使用最少内存但速度较慢且降低压缩比；`9` 使用最大内存以获得最佳速度。默认值为 `8`。

deflate 所需的内存量（以字节为单位）为：

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

即：windowBits = 15（默认值）时为 128K，memLevel = 8（默认值）时为 128K。

### strategy

```dart
int strategy
```

调整压缩算法。对普通数据使用 [ZLibOption.strategyDefault]；对经过滤波（或预测）处理的数据使用 [ZLibOption.strategyFiltered]；若要强制仅使用哈夫曼编码（不进行字符串匹配）则使用 [ZLibOption.strategyHuffmanOnly]；若要将匹配距离限制为 1（游程编码）则使用 [ZLibOption.strategyRle]。

### windowBits

```dart
int windowBits
```

窗口大小（历史缓冲区大小）以 2 为底的对数，取值范围应为 `8..15`。数值越大压缩效果越好，但内存开销也越大。默认值为 `15`。

### dictionary

```dart
List<int>? dictionary
```

初始压缩字典。

字典应由预期会在待压缩数据中later出现的字符串（字节序列）组成，最常用的字符串应尽量放在字典末尾。当待压缩数据较短且能被较为准确地预测时，使用字典最为有效；此时压缩效果会优于默认的空字典。

### raw

```dart
bool raw
```

若为 true，deflate 生成的原始数据将不包含 zlib 头部与尾部，也不会计算 adler32 校验值。

### ZLibEncoder()

```dart
ZLibEncoder({bool gzip = false, int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false})
```

### convert()

```dart
List<int> convert(List<int> bytes)
```

使用 ZLibEncoder 构造函数中给定的选项转换字节列表。

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<List<int>> sink)
```

使用 [ZLibEncoder](https://www.yuque.com/thyname/dart.io/zlibencoder) 构造函数中给定的选项开始分块转换。

可接受任意 `Sink<List<int>>`，但优先使用 [ByteConversionSink](https://www.yuque.com/thyname/dart.convert/byteconversionsink)；若传入其他类型的 sink，会先将其转换为 [ByteConversionSink](https://www.yuque.com/thyname/dart.convert/byteconversionsink) 再使用。

# ZLibDecoder

```dart
final class ZLibDecoder extends Converter<List<int>, List<int>> {}
```

[ZLibDecoder](https://www.yuque.com/thyname/dart.io/zlibdecoder) 由 [ZLibCodec](https://www.yuque.com/thyname/dart.io/zlibcodec) 和 [GZipCodec](https://www.yuque.com/thyname/dart.io/gzipcodec) 用于解压数据。

### gzip

```dart
bool gzip
```

若为 true，将对输入中所有拼接在一起的压缩数据集进行解压，并将结果拼接输出。

### windowBits

```dart
int windowBits
```

窗口大小（历史缓冲区大小）以 2 为底的对数，取值范围应为 `8..15`。数值越大压缩效果越好，但内存开销也越大。默认值为 `15`。

### dictionary

```dart
List<int>? dictionary
```

初始压缩字典。

字典应由预期会在待压缩数据中later出现的字符串（字节序列）组成，最常用的字符串应尽量放在字典末尾。当待压缩数据较短且能被较为准确地预测时，使用字典最为有效；此时压缩效果会优于默认的空字典。

### raw

```dart
bool raw
```

若为 true，deflate 生成的原始数据将不包含 zlib 头部与尾部，也不会计算 adler32 校验值。

### ZLibDecoder()

```dart
ZLibDecoder({bool gzip = false, int windowBits = ZLibOption.defaultWindowBits, List<int>? dictionary, bool raw = false})
```

### convert()

```dart
List<int> convert(List<int> bytes)
```

使用 [ZLibDecoder](https://www.yuque.com/thyname/dart.io/zlibdecoder) 构造函数中给定的选项转换字节列表。

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<List<int>> sink)
```

开始分块转换。

可接受任意 `Sink<List<int>>`，但优先使用 [ByteConversionSink](https://www.yuque.com/thyname/dart.convert/byteconversionsink)；若传入其他类型的 sink，会先将其转换为 [ByteConversionSink](https://www.yuque.com/thyname/dart.convert/byteconversionsink) 再使用。

# RawZLibFilter

```dart
abstract interface class RawZLibFilter {}
```

[RawZLibFilter](https://www.yuque.com/thyname/dart.io/rawzlibfilter) 类提供了一个访问 zlib 的底层接口。

### RawZLibFilter.deflateFilter()

```dart
RawZLibFilter.deflateFilter({bool gzip = false, int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false})
```

返回一个 [RawZLibFilter](https://www.yuque.com/thyname/dart.io/rawzlibfilter)，其 [process] 和 [processed] 方法用于压缩数据。

### RawZLibFilter.inflateFilter()

```dart
RawZLibFilter.inflateFilter({bool gzip = false, int windowBits = ZLibOption.defaultWindowBits, List<int>? dictionary, bool raw = false})
```

返回一个 [RawZLibFilter](https://www.yuque.com/thyname/dart.io/rawzlibfilter)，其 [process] 和 [processed] 方法用于解压数据。

### process()

```dart
void process(List<int> data, int start, int end)
```

处理一块数据。

此方法只能在 [processed] 返回 `null` 时调用。

### processed()

```dart
List<int>? processed({bool flush = true, bool end = false})
```

获取一块已处理的数据。

当没有更多数据可用时，[processed] 将返回 `null`。对于非最终调用，可将 [flush] 设为 `false` 以提升某些过滤器的性能。

对 [processed] 的最后一次调用应将 [end] 设为 `true`，这将确保在流上写入一个"结束"数据包。
