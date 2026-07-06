# JsonUnsupportedObjectError

```dart
class JsonUnsupportedObjectError extends Error {}
```

当对象无法被序列化时，JSON 序列化抛出的错误。

[unsupportedObject] 字段保存序列化失败的对象。

如果对象不能直接序列化，序列化器会调用该对象的 `toJson` 方法。如果该调用失败，错误将存储在 [cause] 字段中。如果调用返回的对象仍不能直接序列化，则 [cause] 为 null。

## 构造函数

### JsonUnsupportedObjectError()

```dart
JsonUnsupportedObjectError(
  Object? unsupportedObject, {
  Object? cause,
  String? partialResult
})
```

## 属性

### unsupportedObject

```dart
Object? unsupportedObject
```

无法被序列化的对象。

### cause

```dart
Object? cause
```

尝试转换对象时抛出的异常。

### partialResult

```dart
String? partialResult
```

转换过程中，直到发生错误为止的部分结果。

可能为 null。

---

# JsonCyclicError

```dart
class JsonCyclicError extends JsonUnsupportedObjectError {}
```

报告对象由于循环引用而无法被字符串化。

引用自身的对象无法被 [JsonCodec.encode]/[JsonEncoder.convert] 序列化。检测到循环时，将抛出 [JsonCyclicError]。

## 构造函数

### JsonCyclicError()

```dart
JsonCyclicError(Object? object)
```

第一个被检测为循环一部分的对象。

---

# json

```dart
JsonCodec const json
```

[JsonCodec] 默认实现的一个实例。

该实例便于访问最常见的 JSON 使用场景。

示例：

```dart
var encoded = json.encode([1, 2, { "a": null }]);
var decoded = json.decode('["foo", { "bar": 499 }]');
```

如果局部变量遮蔽了 [json] 常量，可以使用顶层函数 [jsonEncode] 和 [jsonDecode] 代替。

---

# jsonEncode()

```dart
String jsonEncode(
  Object? object, {
  Object? Function(Object? nonEncodable)? toEncodable
})
```

将 [object] 转换为 JSON 字符串。

如果值中包含无法直接编码为 JSON 字符串的对象（即不是数字、布尔值、字符串、null、列表，或键为字符串的映射的值），则使用 [toEncodable] 函数将其转换为可直接编码的对象。

如果省略 [toEncodable]，默认使用对不可编码对象调用 `.toJson()` 的结果。

是 `json.encode` 的简写形式。当局部变量遮蔽了全局常量 [json] 时非常有用。

示例：

```dart
const data = {'text': 'foo', 'value': 2, 'status': false, 'extra': null};
final String jsonString = jsonEncode(data);
print(jsonString); // {"text":"foo","value":2,"status":false,"extra":null}
```

将其他不受支持的对象转换为自定义 JSON 格式的示例：

```dart
class CustomClass {
  final String text;
  final int value;
  CustomClass({required this.text, required this.value});
  CustomClass.fromJson(Map<String, dynamic> json)
      : text = json['text'],
        value = json['value'];

  static Map<String, dynamic> toJson(CustomClass value) =>
      {'text': value.text, 'value': value.value};
}

void main() {
  final CustomClass cc = CustomClass(text: 'Dart', value: 123);
  final jsonText = jsonEncode({'cc': cc},
      toEncodable: (Object? value) => value is CustomClass
          ? CustomClass.toJson(value)
          : throw UnsupportedError('Cannot convert to JSON: $value'));
  print(jsonText); // {"cc":{"text":"Dart","value":123}}
}
```

---

# jsonDecode()

```dart
dynamic jsonDecode(
  String source, {
  Object? Function(Object? key, Object? value)? reviver
})
```

解析字符串并返回结果 Json 对象。

可选的 [reviver] 函数会在解码过程中对每个已解析的对象或列表属性调用一次。`key` 参数对于列表属性是整数索引，对于对象属性是字符串键，对于最终结果则为 `null`。

默认的 [reviver]（未提供时）是恒等函数。

是 `json.decode` 的简写形式。当局部变量遮蔽了全局常量 [json] 时非常有用。

示例：

```dart
const jsonString =
    '{"text": "foo", "value": 1, "status": false, "extra": null}';

final data = jsonDecode(jsonString);
print(data['text']); // foo
print(data['value']); // 1
print(data['status']); // false
print(data['extra']); // null

const jsonArray = '''
  [{"text": "foo", "value": 1, "status": true},
   {"text": "bar", "value": 2, "status": false}]
''';

final List<dynamic> dataList = jsonDecode(jsonArray);
print(dataList[0]); // {text: foo, value: 1, status: true}
print(dataList[1]); // {text: bar, value: 2, status: false}

final item = dataList[0];
print(item['text']); // foo
print(item['value']); // 1
print(item['status']); // false
```

---

# JsonCodec

```dart
final class JsonCodec extends Codec<Object?, String> {}
```

[JsonCodec] 将 JSON 对象编码为字符串，并将字符串解码为 JSON 对象。

示例：

```dart
var encoded = json.encode([1, 2, { "a": null }]);
var decoded = json.decode('["foo", { "bar": 499 }]');
```

## 构造函数

### JsonCodec()

```dart
JsonCodec({
  Object? Function(Object?, Object?)? reviver,
  Object? Function(dynamic)? toEncodable
})
```

使用给定的 reviver 和编码函数创建一个 `JsonCodec`。

[reviver] 函数在解码期间被调用。它会对每个已解析的对象或列表属性调用一次。`key` 参数对于列表属性是整数索引，对于对象属性是字符串键，对于最终结果则为 `null`。

如果省略 [reviver]，默认返回 value 参数本身。

[toEncodable] 函数在编码期间使用。它会对无法直接编码为字符串的值（即不是数字、布尔值、字符串、null、列表，或键为字符串的映射的值）调用。该函数必须返回一个可直接编码的对象。返回的列表的元素和映射的值不必是可直接编码的，如果它们不是，`toEncodable` 也会被用于处理它们。请注意，通过反复调用 `toEncodable`，有可能以这种方式无意中创建无限递归的数据结构，从而导致无限递归。

如果省略 [_toEncodable]，默认使用对不可编码对象调用 `.toJson()` 的结果。

### JsonCodec.withReviver()

```dart
JsonCodec.withReviver(dynamic Function(Object? key, Object? value) reviver)
```

使用给定的 reviver 创建一个 `JsonCodec`。

[reviver] 函数会在解码过程中对每个已解析的对象或列表属性调用一次。`key` 参数对于列表属性是整数索引，对于对象属性是字符串键，对于最终结果则为 `null`。

## 属性

### encoder

```dart
JsonEncoder get encoder
```

### decoder

```dart
JsonDecoder get decoder
```

## 方法

### decode()

```dart
dynamic decode(
  String source, {
  Object? Function(Object? key, Object? value)? reviver
})
```

解析字符串并返回结果 Json 对象。

可选的 [reviver] 函数会在解码过程中对每个已解析的对象或列表属性调用一次。`key` 参数对于列表属性是整数索引，对于对象属性是字符串键，对于最终结果则为 `null`。

默认的 [reviver]（未提供时）是恒等函数。

### encode()

```dart
String encode(
  Object? value, {
  Object? Function(dynamic object)? toEncodable
})
```

将 [value] 转换为 JSON 字符串。

如果值中包含无法直接编码为 JSON 字符串的对象（即不是数字、布尔值、字符串、null、列表，或键为字符串的映射的值），则使用 [toEncodable] 函数将其转换为可直接编码的对象。

如果省略 [toEncodable]，默认使用对不可编码对象调用 `.toJson()` 的结果。

---

# JsonEncoder

```dart
final class JsonEncoder extends Converter<Object?, String> {}
```

此类将 JSON 对象转换为字符串。

示例：

```dart
const JsonEncoder encoder = JsonEncoder();
const data = {'text': 'foo', 'value': '2'};

final String jsonString = encoder.convert(data);
print(jsonString); // {"text":"foo","value":"2"}
```

美化打印输出的示例：

```dart
const JsonEncoder encoder = JsonEncoder.withIndent('  ');

const data = {'text': 'foo', 'value': '2'};
final String jsonString = encoder.convert(data);
print(jsonString);
// {
//   "text": "foo",
//   "value": "2"
// }
```

## 构造函数

### JsonEncoder()

```dart
JsonEncoder([Object? Function(dynamic object)? toEncodable])
```

创建一个 JSON 编码器。

JSON 编码器直接处理数字、字符串、布尔值、null、列表以及键为字符串的映射。

其他任何对象都会尝试通过 [toEncodable] 转换为上述可转换类型之一的对象。

如果省略 [toEncodable]，默认调用对象的 `.toJson()` 方法。

### JsonEncoder.withIndent()

```dart
JsonEncoder.withIndent(
  String? indent, [
  Object? Function(dynamic object)? toEncodable
])
```

创建一个生成多行 JSON 的 JSON 编码器。

列表和映射的元素编码后会被缩进并放在单独的行上。[indent] 字符串会根据缩进级别在这些元素前重复添加。

如果 [indent] 为 `null`，输出将被编码为单行。

JSON 编码器直接处理数字、字符串、布尔值、null、列表以及键为字符串的映射。

其他任何对象都会尝试通过 [toEncodable] 转换为上述可转换类型之一的对象。

如果省略 [toEncodable]，默认调用对象的 `.toJson()` 方法。

## 属性

### indent

```dart
String? indent
```

用于缩进的字符串。

生成多行输出时，该字符串会在每个缩进级别的每个缩进行开头插入一次。

如果为 `null`，输出将被编码为单行。

## 方法

### convert()

```dart
String convert(Object? object)
```

将 [object] 转换为 JSON [String]。

可直接序列化的值为 [num]、[String]、[bool] 和 [Null]，以及某些 [List] 和 [Map] 值。对于 [List]，其所有元素都必须是可序列化的。对于 [Map]，键必须是 [String]，值必须是可序列化的。

如果尝试序列化其他类型的值，将调用构造函数中提供的 `toEncodable` 函数，并以该值作为参数。返回结果必须是可直接序列化的值，随后将替代原始值进行序列化。

如果转换抛出异常，或返回的值不可直接序列化，则抛出 [JsonUnsupportedObjectError] 异常。如果调用抛出异常，该错误会被捕获并存储在 [JsonUnsupportedObjectError] 的 `cause` 字段中。

如果 [List] 或 [Map] 直接或通过其他列表或映射间接引用了自身，则无法被序列化，将抛出 [JsonCyclicError]。

序列化过程中 [object] 不应发生变化。

如果一个对象被序列化多次，[convert] 可能会缓存其文本。换句话说，如果对象的内容在首次序列化后发生变化，结果中可能不会反映新的值。

### startChunkedConversion()

```dart
ChunkedConversionSink<Object?> startChunkedConversion(Sink<String> sink)
```

开始一次分块转换。

如果给定的 [sink] 是 [StringConversionSink]，转换器的工作效率会更高。

返回一个最多接受一个对象的分块转换 sink。对返回的 sink 多次调用 `add` 是错误的。

### bind()

```dart
Stream<String> bind(Stream<Object?> stream)
```

### fuse()

```dart
Converter<Object?, T> fuse<T>(Converter<String, T> other)
```

---

# JsonUtf8Encoder

```dart
final class JsonUtf8Encoder extends Converter<Object?, List<int>> {}
```

将单个对象编码为 UTF-8 编码的 JSON 字符串的编码器。

此编码器的工作方式等同于先将对象转换为 JSON 字符串，再进行 UTF-8 编码，但不会创建中间字符串。

## 构造函数

### JsonUtf8Encoder()

```dart
JsonUtf8Encoder([
  String? indent,
  dynamic Function(dynamic object)? toEncodable,
  int? bufferSize
])
```

创建转换器。

如果 [indent] 不为 `null`，转换器会尝试对 JSON 进行"美化打印"，并使用 `indent` 作为缩进。否则，结果中除字符串字面量外不包含任何空白字符。如果 `indent` 包含非有效 JSON 空白字符的字符，则结果将不是有效的 JSON。JSON 空白字符包括空格（U+0020）、制表符（U+0009）、换行符（U+000a）和回车符（U+000d）（[ECMA 404](http://www.ecma-international.org/publications/standards/Ecma-404.htm)）。

[bufferSize] 是用于收集 UTF-8 码元的内部缓冲区大小。如果使用 [startChunkedConversion]，它将是分块的大小。

JSON 编码器直接处理数字、字符串、布尔值、null、列表和映射。

其他任何对象都会尝试通过 [toEncodable] 转换为上述可转换类型之一的对象。

如果省略 [toEncodable]，默认调用对象的 `.toJson()` 方法。

## 方法

### convert()

```dart
List<int> convert(Object? object)
```

将 [object] 转换为 UTF-8 编码的 JSON。

### startChunkedConversion()

```dart
ChunkedConversionSink<Object?> startChunkedConversion(Sink<List<int>> sink)
```

开始一次分块转换。

只能有一个对象被传入返回的 sink。

参数 [sink] 将接收字节列表，其大小取决于创建此编码器时传入构造函数的 `bufferSize`。

### bind()

```dart
Stream<List<int>> bind(Stream<Object?> stream)
```

---

# JsonDecoder

```dart
final class JsonDecoder extends Converter<String, Object?> {}
```

此类解析 JSON 字符串并构建相应的对象。

JSON 输入必须是单个 JSON 值的 JSON 编码，该值可以是包含其他值的列表或映射。

如果输入不是有效的 JSON 文本，则抛出 [FormatException]。

示例：

```dart
const JsonDecoder decoder = JsonDecoder();

const String jsonString = '''
  {
    "data": [{"text": "foo", "value": 1 },
             {"text": "bar", "value": 2 }],
    "text": "Dart"
  }
''';

final Map<String, dynamic> object = decoder.convert(jsonString);

final item = object['data'][0];
print(item['text']); // foo
print(item['value']); // 1

print(object['text']); // Dart
```

当用作 [StreamTransformer] 时，输入流可能会发出多个字符串。所有这些字符串的拼接必须是单个 JSON 值的有效 JSON 编码。

## 构造函数

### JsonDecoder()

```dart
JsonDecoder([Object? Function(Object? key, Object? value)? reviver])
```

构造一个新的 JsonDecoder。

[reviver] 可以为 `null`。

## 方法

### convert()

```dart
dynamic convert(String input)
```

将给定的 JSON 字符串 [input] 转换为其对应的对象。

解析后的 JSON 值的类型为 [num]、[String]、[bool]、[Null]，或由已解析的 JSON 值组成的 [List]，或从 [String] 到已解析 JSON 值的 [Map]。

如果 `this` 在初始化时提供了 reviver，则解析操作会对每个已解析的对象或列表属性调用该 reviver。参数为属性名称（[String]）或列表索引（[int]），以及已解析的值。reviver 的返回值将代替解析值用作该属性的值。

如果输入不是有效的 JSON 文本，则抛出 [FormatException]。

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<Object?> sink)
```

开始从分块 JSON 字符串到其对应对象的转换。

输出 [sink] 通过 `add` 恰好接收一个解码后的元素。

### bind()

```dart
Stream<Object?> bind(Stream<String> stream)
```
