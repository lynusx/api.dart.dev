# ZLibOption

```dart
abstract final class ZLibOption {}
```

Exposes ZLib options for input parameters.

See http://www.zlib.net/manual.html for more documentation.

### minWindowBits

```dart
int minWindowBits
```

Minimal value for [ZLibCodec.windowBits], [ZLibEncoder.windowBits] and [ZLibDecoder.windowBits].

### maxWindowBits

```dart
int maxWindowBits
```

Maximal value for [ZLibCodec.windowBits], [ZLibEncoder.windowBits] and [ZLibDecoder.windowBits].

### defaultWindowBits

```dart
int defaultWindowBits
```

Default value for [ZLibCodec.windowBits], [ZLibEncoder.windowBits] and [ZLibDecoder.windowBits].

### minLevel

```dart
int minLevel
```

Minimal value for [ZLibCodec.level] and [ZLibEncoder.level].

### maxLevel

```dart
int maxLevel
```

Maximal value for [ZLibCodec.level] and [ZLibEncoder.level]

### defaultLevel

```dart
int defaultLevel
```

Default value for [ZLibCodec.level] and [ZLibEncoder.level].

### minMemLevel

```dart
int minMemLevel
```

Minimal value for [ZLibCodec.memLevel] and [ZLibEncoder.memLevel].

### maxMemLevel

```dart
int maxMemLevel
```

Maximal value for [ZLibCodec.memLevel] and [ZLibEncoder.memLevel].

### defaultMemLevel

```dart
int defaultMemLevel
```

Default value for [ZLibCodec.memLevel] and [ZLibEncoder.memLevel].

### strategyFiltered

```dart
int strategyFiltered
```

Recommended strategy for data produced by a filter (or predictor)

### strategyHuffmanOnly

```dart
int strategyHuffmanOnly
```

Use this strategy to force Huffman encoding only (no string match)

### strategyRle

```dart
int strategyRle
```

Use this strategy to limit match distances to one (run-length encoding)

### strategyFixed

```dart
int strategyFixed
```

This strategy prevents the use of dynamic Huffman codes, allowing for a simpler decoder

### strategyDefault

```dart
int strategyDefault
```

Recommended strategy for normal data

# zlib

```dart
ZLibCodec zlib
```

An instance of the default implementation of the [ZLibCodec].

# ZLibCodec

```dart
final class ZLibCodec extends Codec<List<int>, List<int>> {}
```

The [ZLibCodec] encodes raw bytes to ZLib compressed bytes and decodes ZLib compressed bytes to raw bytes.

### gzip

```dart
bool gzip
```

When true, `GZip` frames will be added to the compressed data.

### level

```dart
int level
```

The compression-[level] can be set in the range of `-1..9`, with `6` being the default compression level. Levels above `6` will have higher compression rates at the cost of more CPU and memory usage. Levels below `6` will use less CPU and memory at the cost of lower compression rates.

### memLevel

```dart
int memLevel
```

Specifies how much memory should be allocated for the internal compression state. `1` uses minimum memory but is slow and reduces compression ratio; `9` uses maximum memory for optimal speed. The default value is `8`.

The memory requirements for deflate are (in bytes):

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

that is: 128K for windowBits = 15 + 128K for memLevel = 8 (default values)

### strategy

```dart
int strategy
```

Tunes the compression algorithm. Use the value strategyDefault for normal data, strategyFiltered for data produced by a filter (or predictor), strategyHuffmanOnly to force Huffman encoding only (no string match), or strategyRle to limit match distances to one (run-length encoding).

### windowBits

```dart
int windowBits
```

Base two logarithm of the window size (the size of the history buffer). It should be in the range 8..15. Larger values result in better compression at the expense of memory usage. The default value is 15

### raw

```dart
bool raw
```

When true, deflate generates raw data with no zlib header or trailer, and will not compute an adler32 check value

### dictionary

```dart
List<int>? dictionary
```

Initial compression dictionary.

It should consist of strings (byte sequences) that are likely to be encountered later in the data to be compressed, with the most commonly used strings preferably put towards the end of the dictionary. Using a dictionary is most useful when the data to be compressed is short and can be predicted with good accuracy; the data can then be compressed better than with the default empty dictionary.

### ZLibCodec()

```dart
ZLibCodec({int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false, bool gzip = false})
```

### encoder

```dart
ZLibEncoder get encoder
```

Get a [ZLibEncoder] for encoding to `ZLib` compressed data.

### decoder

```dart
ZLibDecoder get decoder
```

Get a [ZLibDecoder] for decoding `ZLib` compressed data.

# gzip

```dart
GZipCodec gzip
```

An instance of the default implementation of the [GZipCodec].

# GZipCodec

```dart
final class GZipCodec extends Codec<List<int>, List<int>> {}
```

The [GZipCodec] encodes raw bytes to GZip compressed bytes and decodes GZip compressed bytes to raw bytes.

### gzip

```dart
bool gzip
```

When true, `GZip` frames will be added to the compressed data.

### level

```dart
int level
```

The compression-[level] can be set in the range of `-1..9`, with `6` being the default compression level. Levels above `6` will have higher compression rates at the cost of more CPU and memory usage. Levels below `6` will use less CPU and memory at the cost of lower compression rates.

### memLevel

```dart
int memLevel
```

Specifies how much memory should be allocated for the internal compression state. `1` uses minimum memory but is slow and reduces compression ratio; `9` uses maximum memory for optimal speed. The default value is `8`.

The memory requirements for deflate are (in bytes):

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

that is: 128K for windowBits = 15 + 128K for memLevel = 8 (default values)

### strategy

```dart
int strategy
```

Tunes the compression algorithm. Use the value [ZLibOption.strategyDefault] for normal data, [ZLibOption.strategyFiltered] for data produced by a filter (or predictor), [ZLibOption.strategyHuffmanOnly] to force Huffman encoding only (no string match), or [ZLibOption.strategyRle] to limit match distances to one (run-length encoding).

### windowBits

```dart
int windowBits
```

Base two logarithm of the window size (the size of the history buffer). It should be in the range `8..15`. Larger values result in better compression at the expense of memory usage. The default value is `15`

### dictionary

```dart
List<int>? dictionary
```

Initial compression dictionary.

It should consist of strings (byte sequences) that are likely to be encountered later in the data to be compressed, with the most commonly used strings preferably put towards the end of the dictionary. Using a dictionary is most useful when the data to be compressed is short and can be predicted with good accuracy; the data can then be compressed better than with the default empty dictionary.

### raw

```dart
bool raw
```

When true, deflate generates raw data with no zlib header or trailer, and will not compute an adler32 check value

### GZipCodec()

```dart
GZipCodec({int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false, bool gzip = true})
```

### encoder

```dart
ZLibEncoder get encoder
```

Get a [ZLibEncoder] for encoding to `GZip` compressed data.

### decoder

```dart
ZLibDecoder get decoder
```

Get a [ZLibDecoder] for decoding `GZip` compressed data.

# ZLibEncoder

```dart
final class ZLibEncoder extends Converter<List<int>, List<int>> {}
```

The [ZLibEncoder] encoder is used by [ZLibCodec] and [GZipCodec] to compress data.

### gzip

```dart
bool gzip
```

When true, `GZip` frames will be added to the compressed data.

### level

```dart
int level
```

The compression-[level] can be set in the range of `-1..9`, with `6` being the default compression level. Levels above `6` will have higher compression rates at the cost of more CPU and memory usage. Levels below `6` will use less CPU and memory at the cost of lower compression rates.

### memLevel

```dart
int memLevel
```

Specifies how much memory should be allocated for the internal compression state. `1` uses minimum memory but is slow and reduces compression ratio; `9` uses maximum memory for optimal speed. The default value is `8`.

The memory requirements for deflate are (in bytes):

```dart
(1 << (windowBits + 2)) +  (1 << (memLevel + 9))
```

that is: 128K for windowBits = 15 + 128K for memLevel = 8 (default values)

### strategy

```dart
int strategy
```

Tunes the compression algorithm. Use the value [ZLibOption.strategyDefault] for normal data, [ZLibOption.strategyFiltered] for data produced by a filter (or predictor), [ZLibOption.strategyHuffmanOnly] to force Huffman encoding only (no string match), or [ZLibOption.strategyRle] to limit match distances to one (run-length encoding).

### windowBits

```dart
int windowBits
```

Base two logarithm of the window size (the size of the history buffer). It should be in the range `8..15`. Larger values result in better compression at the expense of memory usage. The default value is `15`

### dictionary

```dart
List<int>? dictionary
```

Initial compression dictionary.

It should consist of strings (byte sequences) that are likely to be encountered later in the data to be compressed, with the most commonly used strings preferably put towards the end of the dictionary. Using a dictionary is most useful when the data to be compressed is short and can be predicted with good accuracy; the data can then be compressed better than with the default empty dictionary.

### raw

```dart
bool raw
```

When true, deflate generates raw data with no zlib header or trailer, and will not compute an adler32 check value

### ZLibEncoder()

```dart
ZLibEncoder({bool gzip = false, int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false})
```

### convert()

```dart
List<int> convert(List<int> bytes)
```

Convert a list of bytes using the options given to the ZLibEncoder constructor.

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<List<int>> sink)
```

Start a chunked conversion using the options given to the [ZLibEncoder] constructor.

Accepts any `Sink<List<int>>`, but prefers a [ByteConversionSink], and converts any other sink to a [ByteConversionSink] before using it.

# ZLibDecoder

```dart
final class ZLibDecoder extends Converter<List<int>, List<int>> {}
```

The [ZLibDecoder] is used by [ZLibCodec] and [GZipCodec] to decompress data.

### gzip

```dart
bool gzip
```

When true, all concatenated compressed data sets in the input are decompressed and concatenated in the output.

### windowBits

```dart
int windowBits
```

Base two logarithm of the window size (the size of the history buffer). It should be in the range `8..15`. Larger values result in better compression at the expense of memory usage. The default value is `15`.

### dictionary

```dart
List<int>? dictionary
```

Initial compression dictionary.

It should consist of strings (byte sequences) that are likely to be encountered later in the data to be compressed, with the most commonly used strings preferably put towards the end of the dictionary. Using a dictionary is most useful when the data to be compressed is short and can be predicted with good accuracy; the data can then be compressed better than with the default empty dictionary.

### raw

```dart
bool raw
```

When true, deflate generates raw data with no zlib header or trailer, and will not compute an adler32 check value

### ZLibDecoder()

```dart
ZLibDecoder({bool gzip = false, int windowBits = ZLibOption.defaultWindowBits, List<int>? dictionary, bool raw = false})
```

### convert()

```dart
List<int> convert(List<int> bytes)
```

Convert a list of bytes using the options given to the [ZLibDecoder] constructor.

### startChunkedConversion()

```dart
ByteConversionSink startChunkedConversion(Sink<List<int>> sink)
```

Start a chunked conversion.

Accepts any `Sink<List<int>>`, but prefers a [ByteConversionSink], and converts any other sink to a [ByteConversionSink] before using it.

# RawZLibFilter

```dart
abstract interface class RawZLibFilter {}
```

The [RawZLibFilter] class provides a low-level interface to zlib.

### RawZLibFilter.deflateFilter()

```dart
RawZLibFilter.deflateFilter({bool gzip = false, int level = ZLibOption.defaultLevel, int windowBits = ZLibOption.defaultWindowBits, int memLevel = ZLibOption.defaultMemLevel, int strategy = ZLibOption.strategyDefault, List<int>? dictionary, bool raw = false})
```

Returns a [RawZLibFilter] whose [process] and [processed] methods compress data.

### RawZLibFilter.inflateFilter()

```dart
RawZLibFilter.inflateFilter({bool gzip = false, int windowBits = ZLibOption.defaultWindowBits, List<int>? dictionary, bool raw = false})
```

Returns a [RawZLibFilter] whose [process] and [processed] methods decompress data.

### process()

```dart
void process(List<int> data, int start, int end)
```

Process a chunk of data.

This method must only be called when [processed] returns `null`.

### processed()

```dart
List<int>? processed({bool flush = true, bool end = false})
```

Get a chunk of processed data.

When there are no more data available, [processed] will return `null`. Set [flush] to `false` for non-final calls to improve performance of some filters.

The last call to [processed] should have [end] set to `true`. This will make sure an 'end' packet is written on the stream.
