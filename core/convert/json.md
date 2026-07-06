# JsonUnsupportedObjectError

```dart
class JsonUnsupportedObjectError extends Error {}
```

Error thrown by JSON serialization if an object cannot be serialized.

The [unsupportedObject] field holds that object that failed to be serialized.

If an object isn't directly serializable, the serializer calls the `toJson` method on the object. If that call fails, the error will be stored in the [cause] field. If the call returns an object that isn't directly serializable, the [cause] is null.

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

The object that could not be serialized.

### cause

```dart
Object? cause
```

The exception thrown when trying to convert the object.

### partialResult

```dart
String? partialResult
```

The partial result of the conversion, up until the error happened.

May be null.

---



# JsonCyclicError

```dart
class JsonCyclicError extends JsonUnsupportedObjectError {}
```

Reports that an object could not be stringified due to cyclic references.

An object that references itself cannot be serialized by [JsonCodec.encode]/[JsonEncoder.convert]. When the cycle is detected, a [JsonCyclicError] is thrown.

## 构造函数

### JsonCyclicError()

```dart
JsonCyclicError(Object? object)
```

The first object that was detected as part of a cycle.

---



# json

```dart
JsonCodec const json
```

An instance of the default implementation of the [JsonCodec].

This instance provides a convenient access to the most common JSON use cases.

Examples:

```dart
var encoded = json.encode([1, 2, { "a": null }]);
var decoded = json.decode('["foo", { "bar": 499 }]');
```

The top-level [jsonEncode] and [jsonDecode] functions may be used instead if a local variable shadows the [json] constant.

---



# jsonEncode()

```dart
String jsonEncode(
  Object? object, {
  Object? Function(Object? nonEncodable)? toEncodable
})
```

Converts [object] to a JSON string.

If value contains objects that are not directly encodable to a JSON string (a value that is not a number, boolean, string, null, list or a map with string keys), the [toEncodable] function is used to convert it to an object that must be directly encodable.

If [toEncodable] is omitted, it defaults to a function that returns the result of calling `.toJson()` on the unencodable object.

Shorthand for `json.encode`. Useful if a local variable shadows the global [json] constant.

Example:

```dart
const data = {'text': 'foo', 'value': 2, 'status': false, 'extra': null};
final String jsonString = jsonEncode(data);
print(jsonString); // {"text":"foo","value":2,"status":false,"extra":null}
```

Example of converting an otherwise unsupported object to a custom JSON format:

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

Parses the string and returns the resulting Json object.

The optional [reviver] function is called once for each object or list property that has been parsed during decoding. The `key` argument is either the integer list index for a list property, the string map key for object properties, or `null` for the final result.

The default [reviver] (when not provided) is the identity function.

Shorthand for `json.decode`. Useful if a local variable shadows the global [json] constant.

Example:

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

A [JsonCodec] encodes JSON objects to strings and decodes strings to JSON objects.

Examples:

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

Creates a `JsonCodec` with the given reviver and encoding function.

The [reviver] function is called during decoding. It is invoked once for each object or list property that has been parsed. The `key` argument is either the integer list index for a list property, the string map key for object properties, or `null` for the final result.

If [reviver] is omitted, it defaults to returning the value argument.

The [toEncodable] function is used during encoding. It is invoked for values that are not directly encodable to a string (a value that is not a number, boolean, string, null, list or a map with string keys). The function must return an object that is directly encodable. The elements of a returned list and values of a returned map do not need to be directly encodable, and if they aren't, `toEncodable` will be used on them as well. Please notice that it is possible to cause an infinite recursive regress in this way, by effectively creating an infinite data structure through repeated call to `toEncodable`.

If [_toEncodable] is omitted, it defaults to a function that returns the result of calling `.toJson()` on the unencodable object.

### JsonCodec.withReviver()

```dart
JsonCodec.withReviver(dynamic Function(Object? key, Object? value) reviver)
```

Creates a `JsonCodec` with the given reviver.

The [reviver] function is called once for each object or list property that has been parsed during decoding. The `key` argument is either the integer list index for a list property, the string map key for object properties, or `null` for the final result.

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

Parses the string and returns the resulting Json object.

The optional [reviver] function is called once for each object or list property that has been parsed during decoding. The `key` argument is either the integer list index for a list property, the string map key for object properties, or `null` for the final result.

The default [reviver] (when not provided) is the identity function.

### encode()

```dart
String encode(
  Object? value, {
  Object? Function(dynamic object)? toEncodable
})
```

Converts [value] to a JSON string.

If value contains objects that are not directly encodable to a JSON string (a value that is not a number, boolean, string, null, list or a map with string keys), the [toEncodable] function is used to convert it to an object that must be directly encodable.

If [toEncodable] is omitted, it defaults to a function that returns the result of calling `.toJson()` on the unencodable object.

---



# JsonEncoder

```dart
final class JsonEncoder extends Converter<Object?, String> {}
```

This class converts JSON objects to strings.

Example:

```dart
const JsonEncoder encoder = JsonEncoder();
const data = {'text': 'foo', 'value': '2'};

final String jsonString = encoder.convert(data);
print(jsonString); // {"text":"foo","value":"2"}
```

Example of pretty-printed output:

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

Creates a JSON encoder.

The JSON encoder handles numbers, strings, booleans, null, lists and maps with string keys directly.

Any other object is attempted converted by [toEncodable] to an object that is of one of the convertible types.

If [toEncodable] is omitted, it defaults to calling `.toJson()` on the object.

### JsonEncoder.withIndent()

```dart
JsonEncoder.withIndent(
  String? indent, [
  Object? Function(dynamic object)? toEncodable
])
```

Creates a JSON encoder that creates multi-line JSON.

The encoding of elements of lists and maps are indented and put on separate lines. The [indent] string is prepended to these elements, once for each level of indentation.

If [indent] is `null`, the output is encoded as a single line.

The JSON encoder handles numbers, strings, booleans, null, lists and maps with string keys directly.

Any other object is attempted converted by [toEncodable] to an object that is of one of the convertible types.

If [toEncodable] is omitted, it defaults to calling `.toJson()` on the object.

## 属性

### indent

```dart
String? indent
```

The string used for indention.

When generating multi-line output, this string is inserted once at the beginning of each indented line for each level of indentation.

If `null`, the output is encoded as a single line.

## 方法

### convert()

```dart
String convert(Object? object)
```

Converts [object] to a JSON [String].

Directly serializable values are [num], [String], [bool], and [Null], as well as some [List] and [Map] values. For [List], the elements must all be serializable. For [Map], the keys must be [String] and the values must be serializable.

If a value of any other type is attempted to be serialized, the `toEncodable` function provided in the constructor is called with the value as argument. The result, which must be a directly serializable value, is serialized instead of the original value.

If the conversion throws, or returns a value that is not directly serializable, a [JsonUnsupportedObjectError] exception is thrown. If the call throws, the error is caught and stored in the [JsonUnsupportedObjectError]'s `cause` field.

If a [List] or [Map] contains a reference to itself, directly or through other lists or maps, it cannot be serialized and a [JsonCyclicError] is thrown.

[object] should not change during serialization.

If an object is serialized more than once, [convert] may cache the text for it. In other words, if the content of an object changes after it is first serialized, the new values may not be reflected in the result.

### startChunkedConversion()

```dart
ChunkedConversionSink<Object?> startChunkedConversion(Sink<String> sink)
```

Starts a chunked conversion.

The converter works more efficiently if the given [sink] is a [StringConversionSink].

Returns a chunked-conversion sink that accepts at most one object. It is an error to invoke `add` more than once on the returned sink.

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

Encoder that encodes a single object as a UTF-8 encoded JSON string.

This encoder works equivalently to first converting the object to a JSON string, and then UTF-8 encoding the string, but without creating an intermediate string.

## 构造函数

### JsonUtf8Encoder()

```dart
JsonUtf8Encoder([
  String? indent, 
  dynamic Function(dynamic object)? toEncodable, 
  int? bufferSize
])
```

Create converter.

If [indent] is non-`null`, the converter attempts to "pretty-print" the JSON, and uses `indent` as the indentation. Otherwise the result has no whitespace outside of string literals. If `indent` contains characters that are not valid JSON whitespace characters, the result will not be valid JSON. JSON whitespace characters are space (U+0020), tab (U+0009), line feed (U+000a) and carriage return (U+000d) ([ECMA 404](http://www.ecma-international.org/publications/standards/Ecma-404.htm)).

The [bufferSize] is the size of the internal buffers used to collect UTF-8 code units. If using [startChunkedConversion], it will be the size of the chunks.

The JSON encoder handles numbers, strings, booleans, null, lists and maps directly.

Any other object is attempted converted by [toEncodable] to an object that is of one of the convertible types.

If [toEncodable] is omitted, it defaults to calling `.toJson()` on the object.

## 方法

### convert()

```dart
List<int> convert(Object? object)
```

Convert [object] into UTF-8 encoded JSON.

### startChunkedConversion()

```dart
ChunkedConversionSink<Object?> startChunkedConversion(Sink<List<int>> sink)
```

Start a chunked conversion.

Only one object can be passed into the returned sink.

The argument [sink] will receive byte lists in sizes depending on the `bufferSize` passed to the constructor when creating this encoder.

### bind()

```dart
Stream<List<int>> bind(Stream<Object?> stream)
```

---



# JsonDecoder

```dart
final class JsonDecoder extends Converter<String, Object?> {}
```

This class parses JSON strings and builds the corresponding objects.

A JSON input must be the JSON encoding of a single JSON value, which can be a list or map containing other values.

Throws [FormatException] if the input is not valid JSON text.

Example:

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

When used as a [StreamTransformer], the input stream may emit multiple strings. The concatenation of all of these strings must be a valid JSON encoding of a single JSON value.

## 构造函数

### JsonDecoder()

```dart
JsonDecoder([Object? Function(Object? key, Object? value)? reviver])
```

Constructs a new JsonDecoder.

The [reviver] may be `null`.

## 方法

### convert()

```dart
dynamic convert(String input)
```

Converts the given JSON-string [input] to its corresponding object.

Parsed JSON values are of the types [num], [String], [bool], [Null], [List]s of parsed JSON values or [Map]s from [String] to parsed JSON values.

If `this` was initialized with a reviver, then the parsing operation invokes the reviver on every object or list property that has been parsed. The arguments are the property name ([String]) or list index ([int]), and the value is the parsed value. The return value of the reviver is used as the value of that property instead the parsed value.

Throws [FormatException] if the input is not valid JSON text.

### startChunkedConversion()

```dart
StringConversionSink startChunkedConversion(Sink<Object?> sink)
```

Starts a conversion from a chunked JSON string to its corresponding object.

The output [sink] receives exactly one decoded element through `add`.

### bind()

```dart
Stream<Object?> bind(Stream<String> stream)
```
