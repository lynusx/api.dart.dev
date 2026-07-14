# Uri

```dart
abstract interface class Uri {}
```

已解析的 URI（如 URL）。

如需创建具有特定组件的 URI，请使用 [Uri.new]：

```dart
var httpsUri = Uri(
    scheme: 'https',
    host: 'dart.dev',
    path: 'language/built-in-types',
    fragment: 'numbers');
print(httpsUri); // https://dart.dev/language/built-in-types#numbers

httpsUri = Uri(
    scheme: 'https',
    host: 'example.com',
    path: '/page/',
    queryParameters: {'search': 'blue', 'limit': '10'});
print(httpsUri); // https://example.com/page/?search=blue&limit=10

final mailtoUri = Uri(
    scheme: 'mailto',
    path: 'John.Doe@example.com',
    queryParameters: {'subject': 'Example'});
print(mailtoUri); // mailto:John.Doe@example.com?subject=Example
```

## HTTP 与 HTTPS URI

如需创建 scheme 为 https 的 URI，请使用 [Uri.https] 或 [Uri.http]：

```dart
final httpsUri = Uri.https('example.com', 'api/fetch', {'limit': '10'});
print(httpsUri); // https://example.com/api/fetch?limit=10
```

## File URI

如需从文件路径创建 URI，请使用 [Uri.file]：

```dart
final fileUriUnix =
    Uri.file(r'/home/myself/images/image.png', windows: false);
print(fileUriUnix); // file:///home/myself/images/image.png

final fileUriWindows =
    Uri.file(r'C:\Users\myself\Documents\image.png', windows: true);
print(fileUriWindows); // file:///C:/Users/myself/Documents/image.png
```

如果该 URI 不是 file URI，调用此方法将抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

## Directory URI

与 [Uri.file] 类似，区别在于非空的 URI 路径会以斜杠结尾。

```dart
final fileDirectory =
    Uri.directory('/home/myself/data/image', windows: false);
print(fileDirectory); // file:///home/myself/data/image/

final fileDirectoryWindows = Uri.directory('/data/images', windows: true);
print(fileDirectoryWindows); //  file:///data/images/
```

## 从字符串创建 URI

如需从字符串创建 URI，请使用 [Uri.parse] 或 [Uri.tryParse]：

```dart
final uri = Uri.parse(
    'https://dart.dev/libraries/dart-core#utility-classes');
print(uri); // https://dart.dev
print(uri.isScheme('https')); // true
print(uri.origin); // https://dart.dev
print(uri.host); // dart.dev
print(uri.authority); // dart.dev
print(uri.port); // 443
print(uri.path); // libraries/dart-core
print(uri.pathSegments); // [libraries, dart-core]
print(uri.fragment); // utility-classes
print(uri.hasQuery); // false
print(uri.data); // null
```

**另请参阅：**

- [`dart:core` 简介][libtour] 中的 [URI][uris]
- [RFC-3986](https://tools.ietf.org/html/rfc3986)
- [RFC-2396](https://tools.ietf.org/html/rfc2396)
- [RFC-2045](https://tools.ietf.org/html/rfc2045)

[uris]: https://dart.dev/libraries/dart-core#uris [libtour]: https://dart.dev/libraries/dart-core

## 构造函数

### Uri()

```dart
Uri({
  String? scheme,
  String? userInfo,
  String? host,
  int? port,
  String? path,
  Iterable<String>? pathSegments,
  String? query,
  Map<String, dynamic>? queryParameters,
  String? fragment,
})
```

根据各组件创建一个新的 URI。

每个组件都通过一个命名参数来设置。可以提供任意数量的组件。[path] 和 [query] 组件都可以通过两种不同的命名参数之一来设置。

scheme 组件通过 [scheme] 设置。scheme 会被规范化为全小写字母。如果省略 scheme 或其为空，则该 URI 将不包含 scheme 部分。

authority 组件中的 user info 部分通过 [userInfo] 设置。它默认为空字符串，此时字符串表示形式中会省略该部分。

authority 组件中的 host 部分通过 [host] 设置。host 可以是主机名、IPv4 地址，或包含在 `'['` 和 `']'` 之间的 IPv6 地址。如果 host 中包含 ':' 字符，且尚未提供 `'['` 和 `']'`，则会自动添加。host 会被规范化为全小写字母。

authority 组件中的 port 部分通过 [port] 设置。如果省略 [port] 或其为 `null`，则表示使用该 URI scheme 对应的默认端口，效果等同于显式传入该端口。可识别的 scheme 及其默认端口为 "http"（80）和 "https"（443）。其他所有 scheme 的默认端口均视为 0。

如果提供了 `userInfo`、`host` 或 `port` 中的任意一个，则根据 [hasAuthority]，该 URI 将拥有 authority。

path 组件通过 [path] 或 [pathSegments] 设置。使用 [path] 时，它应当是一个合法的 URI 路径，但除通用分隔符 ':/@[]?#' 之外的非法字符会在必要时被转义。反斜杠 `\` 会被转换为斜杠 `/`。使用 [pathSegments] 时，会先对每个给定的片段进行百分号编码，然后使用正斜杠分隔符将它们连接起来。

path 片段的百分号编码会对除非保留字符以及以下字符列表之外的所有字符进行编码：`!$&'()*+,;=:@`。如果其他组件要求路径必须为绝对路径，则会在路径前添加前导斜杠 `/`（如果尚不存在）。

query 组件通过 [query] 或 [queryParameters] 设置。使用 [query] 时，所提供的字符串应当是合法的 URI 查询字符串，但除通用分隔符之外的非法字符会在必要时被转义。使用 [queryParameters] 时，会根据所提供的 map 构建 query。map 中的每个键和值都会被百分号编码，并使用等号和 & 符号连接。map 中的值必须为 `null`、字符串，或字符串的 [Iterable](https://www.yuque.com/thyname/dart.core/iterable)。Iterable 对应同一个键的多个值，空 Iterable 或 `null` 则表示该键没有值。

键和值的百分号编码会对除非保留字符之外的所有字符进行编码，并将空格替换为 `+`。如果 [query] 为空字符串，则等同于省略它。若要得到实际为空的 query 部分，请为 [queryParameters] 传入一个空 map。

如果 [query] 和 [queryParameters] 均被省略或为 `null`，则该 URI 不含 query 部分。

fragment 组件通过 [fragment] 设置。它应当是合法的 URI fragment，但除通用分隔符之外的非法字符会在必要时被转义。如果省略 [fragment] 或其为 `null`，则该 URI 不含 fragment 部分。

示例：

```dart
final httpsUri = Uri(
    scheme: 'https',
    host: 'dart.dev',
    path: 'language/built-in-types',
    fragment: 'numbers');
print(httpsUri); // https://dart.dev/language/built-in-types#numbers

final mailtoUri = Uri(
    scheme: 'mailto',
    path: 'John.Doe@example.com',
    queryParameters: {'subject': 'Example'});
print(mailtoUri); // mailto:John.Doe@example.com?subject=Example
```

### Uri.http()

```dart
Uri.http(
  String authority, [
  String unencodedPath,
  Map<String, dynamic>? queryParameters
])
```

根据 authority、path 和 query 创建一个新的 `http` URI。

示例：

```dart
var uri = Uri.http('example.org', '/path', { 'q' : 'dart' });
print(uri); // http://example.org/path?q=dart

uri = Uri.http('user:password@localhost:8080', '');
print(uri); // http://user:password@localhost:8080

uri = Uri.http('example.org', 'a b');
print(uri); // http://example.org/a%20b

uri = Uri.http('example.org', '/a%2F');
print(uri); // http://example.org/a%252F
```

`scheme` 始终被设置为 `http`。

`userInfo`、`host` 和 `port` 组件根据 [authority] 参数设置。如果 `authority` 为 `null` 或空，则创建的 `Uri` 不含 authority，因此无法直接作为 HTTP URL 使用，因为 HTTP URL 必须有一个非空的 host。

`path` 组件根据 [unencodedPath] 参数设置。传入的路径不得进行编码，因为该构造函数会自行对路径进行编码。仅 `/` 会被识别为路径分隔符。如果省略，path 默认为空。

`query` 组件根据可选参数 [queryParameters] 设置。

### Uri.https()

```dart
Uri.https(
  String authority, [
  String unencodedPath,
  Map<String, dynamic>? queryParameters
])
```

根据 authority、path 和 query 创建一个新的 `https` URI。

除 scheme 被设置为 `https` 之外，该构造函数与 [Uri.http] 相同。

示例：

```dart
var uri = Uri.https('example.org', '/path', {'q': 'dart'});
print(uri); // https://example.org/path?q=dart

uri = Uri.https('user:password@localhost:8080', '');
print(uri); // https://user:password@localhost:8080

uri = Uri.https('example.org', 'a b');
print(uri); // https://example.org/a%20b

uri = Uri.https('example.org', '/a%2F');
print(uri); // https://example.org/a%252F
```

### Uri.file()

```dart
Uri.file(
  String path, {
  bool? windows,
})
```

根据绝对或相对文件路径创建一个新的 file URI。

文件路径通过 [path] 传入。

该路径会依据 Windows 或非 Windows 语义进行解析。

在非 Windows 语义下，斜杠（`/`）用作输入 [path] 中路径片段的分隔符。

在 Windows 语义下，反斜杠（`\`）和正斜杠（`/`）都用作输入 [path] 中路径片段的分隔符，但如果路径以 `\\?\` 开头，则只有反斜杠（`\`）会作为 [path] 中的路径分隔符。

如果路径以路径分隔符开头，则会创建一个绝对 URI（带有 `file` scheme 和空 authority）。否则将创建一个不带 scheme 和 authority 的相对 URI 引用。这一规则有一个例外：当使用 Windows 语义，且路径以盘符加冒号（":"）再加路径分隔符开头时，会创建一个绝对 URI。

是否使用 Windows 或非 Windows 语义的默认值取决于运行 Dart 的平台。在 standalone VM 中运行时，该值由 VM 根据操作系统检测得出。在浏览器中运行时，则始终使用非 Windows 语义。

要覆盖这种自动检测的语义选择，可为 [windows] 传入一个值。传入 `true` 将使用 Windows 语义，传入 `false` 将使用非 Windows 语义。

使用非 Windows 语义的示例：

```dart
// xxx/yyy
Uri.file('xxx/yyy', windows: false);

// xxx/yyy/
Uri.file('xxx/yyy/', windows: false);

// file:///xxx/yyy
Uri.file('/xxx/yyy', windows: false);

// file:///xxx/yyy/
Uri.file('/xxx/yyy/', windows: false);

// C%3A
Uri.file('C:', windows: false);
```

使用 Windows 语义的示例：

```dart
// xxx/yyy
Uri.file(r'xxx\yyy', windows: true);

// xxx/yyy/
Uri.file(r'xxx\yyy\', windows: true);

file:///xxx/yyy
Uri.file(r'\xxx\yyy', windows: true);

file:///xxx/yyy/
Uri.file(r'\xxx\yyy/', windows: true);

// file:///C:/xxx/yyy
Uri.file(r'C:\xxx\yyy', windows: true);

// 这会抛出错误。带有盘符但后面没有路径的路径是不允许的。
Uri.file(r'C:', windows: true);

// 这会抛出错误。带有盘符的路径不是绝对路径。
Uri.file(r'C:xxx\yyy', windows: true);

// file://server/share/file
Uri.file(r'\\server\share\file', windows: true);
```

如果传入的路径不是合法的文件路径，则会抛出错误。

### Uri.directory()

```dart
Uri.directory(
  String path, {
  bool? windows,
})
```

与 [Uri.file] 类似，区别在于非空的 URI 路径会以斜杠结尾。

如果 [path] 非空，且不以目录分隔符结尾，则会在返回的 URI 路径末尾添加斜杠。其他所有情况下，结果与 `Uri.file` 返回的结果相同。

示例：

```dart
final fileDirectory = Uri.directory('data/images', windows: false);
print(fileDirectory); // data/images/

final fileDirectoryWindows =
   Uri.directory(r'C:\data\images', windows: true);
print(fileDirectoryWindows); // file:///C:/data/images/
```

### Uri.dataFromString()

```dart
Uri.dataFromString(
  String content, {
  String? mimeType,
  Encoding? encoding,
  Map<String, String>? parameters,
  bool base64 = false,
})
```

创建一个包含 [content] 字符串的 `data:` URI。

使用 [encoding] 或 [parameters] 中指定的字符集（如果二者均未指定或无法识别，则默认为 US-ASCII）将 content 转换为字节，然后将这些字节编码进生成的 data URI 中。

默认使用百分号编码（任何非 ASCII 或 URI 中非法的字节都会被替换为百分号编码）。如果 [base64](https://www.yuque.com/thyname/dart.convert/base64) 为 true，则改为使用 [base64](https://www.yuque.com/thyname/dart.convert/base64) 对字节进行编码。

如果未提供 [encoding]，而 [parameters] 中包含 `charset` 项，则会使用 [Encoding.getByName] 查找该名称，如果查找到相应编码，则使用该编码将 [content] 转换为字节。如果同时提供了 [encoding] 和 [parameters] 中的字符集，二者应当一致，否则解码时将无法使用 charset 参数来确定编码方式。

如果提供了 [mimeType] 和/或 [parameters]，它们会被添加到创建的 URI 中。如果其中包含 data URI 中不允许的字符，该字符会被百分号转义。如果该字符是非 ASCII 字符，会先进行 UTF-8 编码，然后再对字节进行百分号编码。data URI 中省略 [mimeType] 表示 `text/plain`，正如省略 `charset` 参数默认表示 `US-ASCII` 一样。

要将内容读回，请使用 [UriData.contentAsString]。

示例：

```dart
final uri = Uri.dataFromString(
  'example content',
  mimeType: 'text/plain',
  parameters: <String, String>{'search': 'file', 'max': '10'},
);
print(uri); // data:;search=name;max=10,example%20content
```

### Uri.dataFromBytes()

```dart
Uri.dataFromBytes(
  List<int> bytes, {
  String mimeType = "application/octet-stream",
  Map<String, String>? parameters,
  bool percentEncoded = false,
})
```

创建一个包含 [bytes] 编码结果的 `data:` URI。

默认使用 Base64 对字节进行编码，但如果 [percentEncoded] 为 `true`，则改为对字节进行百分号编码（任何非 ASCII 或非法 ASCII 字符的字节都会被替换为百分号编码）。

要将字节读回，请使用 [UriData.contentAsBytes]。

其默认 mime 类型为 `application/octet-stream`。[mimeType] 和 [parameters] 会被添加到创建的 URI 中。如果其中包含 data URI 中不允许的字符，该字符会被百分号转义。如果该字符是非 ASCII 字符，会先进行 UTF-8 编码，然后再对字节进行百分号编码。

示例：

```dart
final uri = Uri.dataFromBytes([68, 97, 114, 116]);
print(uri); // data:application/octet-stream;base64,RGFydA==
```

## 静态属性

### base

```dart
Uri get base
```

当前平台的自然 base URI。

在浏览器中运行时，这是当前页面的当前 URL（来自 `window.location.href`）。

在非浏览器环境中运行时，这是引用当前工作目录的 file URI。

## 静态方法

### parse()

```dart
Uri parse(String uri, [int start = 0, int? end])
```

通过解析 URI 字符串创建一个新的 `Uri` 对象。

如果提供了 [start] 和 [end]，二者必须指定 [uri] 的一个合法子串，此时只有从 `start` 到 `end` 的子串会被解析为 URI。

如果 [uri] 字符串作为 URI 或 URI 引用不合法，则会抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

示例：

```dart
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10,20,30&max=100');
print(uri); // https://example.com/api/fetch?limit=10,20,30&max=100

Uri.parse('::Not valid URI::'); // 抛出 FormatException。
```

### tryParse()

```dart
Uri? tryParse(String uri, [int start = 0, int? end])
```

通过解析 URI 字符串创建一个新的 `Uri` 对象。

如果提供了 [start] 和 [end]，二者必须指定 [uri] 的一个合法子串，此时只有从 `start` 到 `end` 的子串会被解析为 URI。

如果 [uri] 字符串作为 URI 或 URI 引用不合法，则返回 `null`。

示例：

```dart
final uri = Uri.tryParse(
    'https://dart.dev/libraries/dart-core#utility-classes', 0, 16);
print(uri); // https://dart.dev

var notUri = Uri.tryParse('::Not valid URI::');
print(notUri); // null
```

### encodeComponent()

```dart
String encodeComponent(String component)
```

使用百分号编码对字符串 [component] 进行编码，以便可以安全地将其作为字面量用作 URI 组件。

除大小写字母、数字以及字符 `-_.!~*'()` 之外的所有字符都会被百分号编码。这是 RFC 2396 中指定的字符集，也是 ECMA-262 5.1 版中 encodeUriComponent 所指定的字符集。

在手动编码路径片段或 query 组件时，请记住在构建路径或 query 字符串之前分别对每个部分进行编码。

如需对 query 部分进行编码，请考虑使用 [encodeQueryComponent]。

若要避免手动编码，请在构造 [Uri](https://www.yuque.com/thyname/dart.core/uri) 时使用可选命名参数 [pathSegments] 和 [queryParameters]。

示例：

```dart
const request = 'http://example.com/search=Dart';
final encoded = Uri.encodeComponent(request);
print(encoded); // http%3A%2F%2Fexample.com%2Fsearch%3DDart
```

### encodeQueryComponent()

```dart
String decodeQueryComponent(
  String encodedComponent, {
  Encoding encoding = utf8,
})
```

对 `encodedComponent` 中的百分号编码进行解码，并将加号转换为空格。

它会创建一个由解码后字符组成的字节列表，然后使用 `encoding` 将该字节列表解码为字符串。默认编码为 UTF-8。

### decodeComponent()

```dart
String decodeComponent(String encodedComponent)
```

对 [encodedComponent] 中的百分号编码进行解码。

请注意，解码 URI 组件可能会改变其含义，因为部分解码后的字符可能是特定 URI 组件类型的分隔符。在对各部分进行解码之前，请始终先使用该组件类型对应的分隔符对 URI 组件进行拆分。

如需处理 [path] 和 [query] 组件，请考虑使用 [pathSegments] 和 [queryParameters] 来获取已拆分并解码的组件。

示例：

```dart
final decoded =
    Uri.decodeComponent('http%3A%2F%2Fexample.com%2Fsearch%3DDart');
print(decoded); // http://example.com/search=Dart
```

### decodeQueryComponent()

```dart
String decodeQueryComponent(
  String encodedComponent, {
  Encoding encoding = utf8,
})
```

对 [encodedComponent] 中的百分号编码进行解码，并将加号转换为空格。

它会创建一个由解码后字符组成的字节列表，然后使用 [encoding] 将该字节列表解码为字符串。默认编码为 UTF-8。

### encodeFull()

```dart
String encodeFull(String uri)
```

使用百分号编码对字符串 [uri] 进行编码，以便可以安全地将其作为字面量用作完整 URI。

除大小写字母、数字以及字符 `!#$&'()*+,-./:;=?@_~` 之外的所有字符都会被百分号编码。这是 ECMA-262 5.1 版中为 encodeURI 函数指定的字符集。

示例：

```dart
final encoded =
    Uri.encodeFull('https://example.com/api/query?search= dart is');
print(encoded); // https://example.com/api/query?search=%20dart%20is
```

### decodeFull()

```dart
String decodeFull(String uri)
```

对 [uri] 中的百分号编码进行解码。

请注意，解码完整 URI 可能会改变其含义，因为部分解码后的字符可能是保留字符。在大多数情况下，应先使用 [Uri.parse] 将编码后的 URI 解析为各组件，然后再对各个组件分别进行解码。

示例：

```dart
final decoded =
    Uri.decodeFull('https://example.com/api/query?search=%20dart%20is');
print(decoded); // https://example.com/api/query?search= dart is
```

### splitQueryString()

```dart
Map<String, String> splitQueryString(
  String query, {
  Encoding encoding = utf8,
})
```

按照 [HTML 4.01 规范 17.13.4 节](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4') 中为 FORM post 指定的规则，将 [query] 拆分为一个 map。

返回的 map 中的每个键和值都已被解码。如果 [query] 为空字符串，则返回一个空 map。

query 字符串中没有值的键会被映射为空字符串。

每个 query 组件都会使用 [encoding] 进行解码。默认编码为 UTF-8。

示例：

```dart import:convert
final queryStringMap =
    Uri.splitQueryString('limit=10&max=100&search=Dart%20is%20fun');
print(jsonEncode(queryStringMap));
// {"limit":"10","max":"100","search":"Dart is fun"}

```

### parseIPv4Address()

```dart
List<int> parseIPv4Address(
  String host, [
  @Since.new('3.10') int start = 0,
  @Since.new('3.10') int? end
])
```

将 [host] 解析为 IP 版本 4（IPv4）地址，返回以网络字节序（大端序）表示的 4 字节地址列表。

如果提供了 [start] 和 [end]，则只解析该范围（必须是 [host] 的合法范围）。默认解析 [host] 的全部内容。

如果（该范围内的）[host] 不是合法的 IPv4 地址表示形式（即形如 `<octet>.<octet>.<octet>.<octet>` 的形式，其中每个 octet 都是 0 到 255 之间且无前导零的十进制数），则抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

### parseIPv6Address()

```dart
List<int> parseIPv6Address(String host, [int start = 0, int? end])
```

将 [host] 解析为 IP 版本 6（IPv6）地址。

以网络字节序（大端序）表示的 16 字节地址列表形式返回该地址。

如果 [host] 不是合法的 IPv6 地址表示形式，则抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

作用于从 [start] 到 [end] 的子串。如果省略 [end]，则默认为字符串末尾。

以下是一些 IPv6 地址的示例：

- `::1`
- `FEDC:BA98:7654:3210:FEDC:BA98:7654:3210`
- `3ffe:2a00:100:7031::1`
- `::FFFF:129.144.52.38`
- `2010:836B:4179::836B:4179`

IPv6 地址的语法如下：

```
IPv6address ::=                           (h16 ":"){6} ls32
              |                      "::" (h16 ":"){5} ls32
              | (               h16) "::" (h16 ":"){4} ls32
              | ((h16 ":"){0,1} h16) "::" (h16 ":"){3} ls32
              | ((h16 ":"){0,2} h16) "::" (h16 ":"){2} ls32
              | ((h16 ":"){0,3} h16) "::"  h16 ":"     ls32
              | ((h16 ":"){0,4} h16) "::"              ls32
              | ((h16 ":"){0,5} h16) "::"              h16
              | ((h16 ":"){0,6} h16) "::"
ls32        ::= (h16 ":" h16) | IPv4address
                ;; 地址中最低有效的 32 位
h16         ::= HEXDIG{1,4}
                ;; 以十六进制表示的地址中的 16 位
```

也就是说：

- 由 `:` 分隔的八组 1 到 4 位十六进制数字，或者
- 一到七组由 `:` 分隔的这类数字，且其中一对被 `::` 分隔，或存在前导或尾随的 `::`。
- 以上任意一种形式，其中末尾由 `:` 分隔的两组数字可替换为一个 IPv4 地址。

带有区域 ID（来自 RFC 6874）的 IPv6 地址是在 IPv6 地址后跟 `%25`（转义后的 `%`）和合法的区域字符。

```
IPv6addrz ::= IPv6address "%25" ZoneID
ZoneID    ::= (unreserved | pct-encoded)+
```

## 属性

### scheme

```dart
String get scheme
```

URI 的 scheme 组件。

如果不存在 scheme 组件，该值为空字符串。

URI scheme 大小写不敏感。返回的 scheme 会被规范化为小写字母。

### authority

```dart
String get authority
```

authority 组件。

authority 由 [userInfo]、[host] 和 [port] 部分格式化而成。

如果不存在 authority 组件，该值为空字符串。

### userInfo

```dart
String get userInfo
```

authority 组件中的 user info 部分。

如果 authority 组件中不含 user info，该值为空字符串。

### host

```dart
String get host
```

authority 组件中的 host 部分。

如果不存在 authority 组件（因而也就没有 host），该值为空字符串。

如果 host 是 IP 版本 6 地址，则会去除周围的 `[` 和 `]`。

host 字符串大小写不敏感。返回的主机名会被规范化为小写字母，其中的百分号转义则规范化为大写。

### port

```dart
int get port
```

authority 组件中的 port 部分。

如果 authority 组件中没有端口号，该值为默认端口。对于 http 为 80，https 为 443，其他情况为 0。

### path

```dart
String get path
```

path 组件。

path 是表示该 URI 路径部分的实际子串，并在必要时进行了编码。要直接访问解码后的 path，请使用 [pathSegments]。

如果不存在 path 组件，path 的值为空字符串。

### query

```dart
String get query
```

query 组件。

该值是表示该 URI query 部分的实际子串，并在必要时进行了编码。要直接访问解码后的 query，请使用 [queryParameters]。

如果不存在 query 组件，该值为空字符串。

### fragment

```dart
String get fragment
```

fragment 标识符组件。

如果不存在 fragment 标识符组件，该值为空字符串。

### pathSegments

```dart
List<String> get pathSegments
```

按片段拆分后的 URI path。

列表中的每个片段都已被解码。如果 path 为空，将返回空列表。前导斜杠 `/` 不会影响返回的片段。

该列表不可修改，对其进行任何会导致修改的调用都会抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

### queryParameters

```dart
Map<String, String> get queryParameters
```

按照 [HTML 4.01 规范 17.13.4 节](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4') 中为 FORM post 指定的规则拆分后的 URI query。

结果 map 中的每个键和值都已被解码。如果不存在 query，则返回空 map。

query 字符串中没有值的键会被映射为空字符串。如果某个键在 query 字符串中出现多次，则会映射到其中任意一个可能的值。[queryParametersAll] 获取器可以提供一个将键映射到其所有值的 map。

示例：

```dart import:convert
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10,20,30&max=100');
print(jsonEncode(uri.queryParameters));
// {"limit":"10,20,30","max":"100"}
```

该 map 不可修改。

### queryParametersAll

```dart
Map<String, List<String>> get queryParametersAll
```

按照 [HTML 4.01 规范 17.13.4 节](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4') 中为 FORM post 指定的规则，返回拆分后的 URI query。

结果 map 中的每个键和值都已被解码。如果不存在 query，则该 map 为空。

键会被映射为其值组成的列表。如果某个键只出现一次，其值为单元素列表。如果某个键出现但没有值，则使用空字符串作为该次出现的值。

示例：

```dart import:convert
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10&limit=20&limit=30&max=100');
print(jsonEncode(uri.queryParametersAll)); // {"limit":["10","20","30"],"max":["100"]}
```

该 map 及其中包含的列表均不可修改。

### isAbsolute

```dart
bool get isAbsolute
```

该 URI 是否为绝对 URI。

按照 RFC 3986 的定义，如果一个 URI 含有 scheme 且不含 fragment，则它是绝对 URI。

### hasScheme

```dart
bool get hasScheme
```

该 URI 是否含有 [scheme] 组件。

### hasAuthority

```dart
bool get hasAuthority
```

该 URI 是否含有 [authority] 组件。

### hasPort

```dart
bool get hasPort
```

该 URI 是否含有显式指定的端口。

如果端口号为默认端口号（对于未识别的 scheme 为 0，已识别的 http 为 80、https 为 443），则该端口会被视为隐式端口，并从 URI 中省略。

### hasQuery

```dart
bool get hasQuery
```

该 URI 是否含有 query 部分。

### hasFragment

```dart
bool get hasFragment
```

该 URI 是否含有 fragment 部分。

### hasEmptyPath

```dart
bool get hasEmptyPath
```

该 URI 的 path 是否为空。

### hasAbsolutePath

```dart
bool get hasAbsolutePath
```

该 URI 是否含有绝对路径（以 '/' 开头）。

### origin

```dart
String get origin
```

以 scheme://host:port 的形式返回该 URI 的 origin，仅适用于 http 和 https scheme。

如果 scheme 不是 "http" 或 "https"，或 host 名缺失或为空，则为错误。

参见：https://www.w3.org/TR/2011/WD-html5-20110405/origin-0.html#origin

### data

```dart
UriData? get data
```

访问 `data:` URI 的结构。

对于 `data:` URI 返回一个 [UriData](https://www.yuque.com/thyname/dart.core/uridata) 对象，对于其他所有 URI 返回 `null`。[UriData](https://www.yuque.com/thyname/dart.core/uridata) 对象可用于访问 `data:` URI 的媒体类型和数据。

### hashCode

```dart
int get hashCode
```

返回根据 `toString().hashCode` 计算得到的哈希码。

这保证了具有相同规范化字符串表示形式的 URI 拥有相同的哈希码。

## 方法

### isScheme()

```dart
bool isScheme(String scheme)
```

该 [Uri](https://www.yuque.com/thyname/dart.core/uri) 的 scheme 是否为 [scheme]。

[scheme] 应当与 [Uri.scheme] 返回的值相同，但不必规范化为小写字符。

示例：

```dart
var uri = Uri.parse('http://example.com');
print(uri.isScheme('HTTP')); // true

final uriNoScheme = Uri(host: 'example.com');
print(uriNoScheme.isScheme('HTTP')); // false
```

空的 [scheme] 字符串匹配不含 scheme 的 URI（即 [hasScheme] 返回 false 的 URI）。

### toFilePath()

```dart
String toFilePath({
	bool? windows,
})
```

根据 file URI 创建一个文件路径。

返回的路径可以使用 Windows 或非 Windows 语义。

对于非 Windows 语义，使用斜杠（"/"）分隔路径片段。

对于 Windows 语义，使用反斜杠（"\\"）分隔符分隔路径片段。

如果该 URI 是绝对 URI，则路径以路径分隔符开头，除非使用 Windows 语义且第一个路径片段是盘符。使用 Windows 语义时，URI 中的 host 组件会被解释为文件服务器，并返回 UNC 路径。

是否使用 Windows 或非 Windows 语义的默认值取决于运行 Dart 的平台。在 standalone VM 中运行时，该值由 VM 根据操作系统检测得出。在浏览器中运行时，则始终使用非 Windows 语义。

要覆盖这种自动检测的语义选择，可为 [windows] 传入一个值。传入 `true` 将使用 Windows 语义，传入 `false` 将使用非 Windows 语义。

如果该 URI 以斜杠结尾（即最后一个路径组件为空），返回的文件路径也将以斜杠结尾。

在 Windows 语义下，以盘符开头的 URI 无法相对于指定盘符上的当前盘符。也就是说，对于 URI `file:///c:abc`，调用 `toFilePath` 会抛出异常，因为在 Windows 上路径片段不能包含冒号。

使用非 Windows 语义的示例（注释中为调用 toFilePath 的结果）：

```dart
Uri.parse("xxx/yyy");  // xxx/yyy
Uri.parse("xxx/yyy/");  // xxx/yyy/
Uri.parse("file:///xxx/yyy");  // /xxx/yyy
Uri.parse("file:///xxx/yyy/");  // /xxx/yyy/
Uri.parse("file:///C:");  // /C:
Uri.parse("file:///C:a");  // /C:a
```

使用 Windows 语义的示例（注释中为对应 URI）：

```dart
Uri.parse("xxx/yyy");  // xxx\yyy
Uri.parse("xxx/yyy/");  // xxx\yyy\
Uri.parse("file:///xxx/yyy");  // \xxx\yyy
Uri.parse("file:///xxx/yyy/");  // \xxx\yyy\
Uri.parse("file:///C:/xxx/yyy");  // C:\xxx\yyy
Uri.parse("file:C:xxx/yyy");  // 抛出异常，因为在 Windows 上
                              // 路径片段不能包含冒号。
Uri.parse("file://server/share/file");  // \\server\share\file
```

如果该 URI 不是 file URI，调用此方法会抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

如果该 URI 无法转换为文件路径，调用此方法会抛出 [UnsupportedError](https://www.yuque.com/thyname/dart.core/unsupportederror)。

### toString()

```dart
String toString()
```

该 URI 规范化后的字符串表示形式。

### replace()

```dart
Uri replace({
  String? scheme,
  String? userInfo,
  String? host,
  int? port,
  String? path,
  Iterable<String>? pathSegments,
  String? query,
  Map<String, dynamic>? queryParameters,
  String? fragment,
})
```

基于当前 `Uri` 创建一个新的 `Uri`，但替换其中的部分内容。

此方法接受与 [Uri](https://www.yuque.com/thyname/dart.core/uri) 构造函数相同的参数，且含义相同。

[path] 和 [pathSegments] 中至多只能提供一个。同样，[query] 和 [queryParameters] 中至多只能提供一个。

未提供的每个部分都将默认使用当前 `Uri` 实例中对应的值。

此方法与 [Uri.resolve] 不同，后者以分层方式进行覆盖，而此方法可以分别替换 `Uri` 的每个部分。

示例：

```dart
final uri1 = Uri.parse(
    'https://dart.dev/libraries/dart-core#utility-classes');

final uri2 = uri1.replace(
    scheme: 'https',
    path: 'libraries/dart-core',
    fragment: 'uris');
print(uri2); // https://dart.dev/libraries/dart-core#uris
```

此方法的效果类似于使用 `Uri` 构造函数，并从当前 `Uri` 中取出部分参数。示例：

```dart continued
final Uri uri3 = Uri(
    scheme: 'https',
    userInfo: uri1.userInfo,
    host: uri1.host,
    port: uri2.port,
    path: 'language/built-in-types',
    query: uri1.query,
    fragment: null);
print(uri3); // https://dart.dev/language/built-in-types
```

使用此方法可以看作是对上述 `Uri` 构造函数调用的简写，但可能会稍快一些，因为从当前 `Uri` 取用的部分无需再次进行合法性检查。

### removeFragment()

```dart
Uri removeFragment()
```

创建一个与当前 `Uri` 相比仅不含 fragment 的 `Uri`。

如果当前 `Uri` 本身不含 fragment，则返回其自身。

示例：

```dart
final uri =
    Uri.parse('https://example.org:8080/foo/bar#frag').removeFragment();
print(uri); // https://example.org:8080/foo/bar
```

### resolve()

```dart
Uri resolve(String reference)
```

将 [reference] 解析为相对于 `this` 的 URI。

首先使用 [Uri.parse] 将 [reference] 转换为一个 URI，然后将得到的 URI 相对于 `this` 进行解析。

返回解析得到的 URI。

详见 [resolveUri]。

### resolveUri()

```dart
Uri resolveUri(Uri reference)
```

将 [reference] 解析为相对于 `this` 的 URI。

返回解析得到的 URI。

用于解析引用的“转换引用”算法描述于 [RFC-3986 第 5 节](https://tools.ietf.org/html/rfc3986#section-5 'RFC-1123')。

已进行更新，以处理 base URI 仅为相对路径的情况——即当它不含 scheme 和 authority，且路径不以斜杠开头时。在这种情况下，路径会被直接拼接而不移除前导的 ".."，且空路径不会被转换为 "/"。

### normalizePath()

```dart
Uri normalizePath()
```

返回一个路径已被规范化的 URI。

规范化后的路径不含 `.` 片段，也不含非前导的 `..` 片段。只有不含 scheme 或 authority 的相对路径才可以含有前导 `..` 片段；以 `/` 开头的路径也会去除任何前导的 `..` 片段。

该方法使用与 `Uri().resolve(this)` 相同的规范化策略。

除 path 之外，不会更改该 URI 的任何其他部分。

`Uri` 的默认实现总是会对路径进行规范化，因此调用此函数不会产生任何效果。

## 运算符

### operator ==()

```dart
bool operator ==(Object other)
```

如果两个 URI 具有相同的规范化表示形式，则它们相等。

---

# UriData

```dart
final class UriData {}
```

用于访问 `data:` URI 结构的类型。

Data URI 是可以包含任意二进制数据的非分层 URI。它们由 [RFC 2397](https://tools.ietf.org/html/rfc2397) 定义。

该类支持解析 URI 文本、提取 URI 的各个部分，以及根据结构化的各部分构建 URI 文本。

## 构造函数

### UriData.fromString()

```dart
UriData.fromString(
  String content, {
  String? mimeType,
  Encoding? encoding,
  Map<String, String>? parameters,
  bool base64 = false,
})
```

创建一个包含 [content] 字符串的 `data:` URI。

等价于 `Uri.dataFromString(...).data`，但如果不需要使用 [uri] 本身，效率可能更高。

### UriData.fromBytes()

```dart
UriData.fromBytes(
  List<int> bytes, {
  String mimeType = "application/octet-stream",
  Map<String, String>? parameters,
  bool percentEncoded = false,
})
```

创建一个包含 [bytes] 编码结果的 `data:` URI。

等价于 `Uri.dataFromBytes(...).data`，但如果不需要使用 [uri] 本身，效率可能更高。

### UriData.fromUri()

```dart
UriData.fromUri(Uri uri)
```

根据一个 [Uri](https://www.yuque.com/thyname/dart.core/uri) 创建 `DataUri`，该 [Uri](https://www.yuque.com/thyname/dart.core/uri) 的 [Uri.scheme] 必须为 `data`。

[uri] 的 scheme 必须为 `data`，且不含 authority 或 fragment，其 path（如果存在 query，则与 query 拼接后）必须是符合 [parse] 中相同规则的合法 data URI 内容。

## 静态方法

### parse()

```dart
UriData parse(String uri)
```

将字符串解析为 `data` URI。

该字符串必须符合以下格式：

```plaintext
'data:' (type '/' subtype)? (';' attribute '=' value)* (';base64')? ',' data
```

其中 `type`、`subtype`、`attribute` 和 `value` 均按 RFC-2045 中的规定指定，`data` 是一串 URI 字符（RFC-2396 中的 `uric`）。

这意味着所有字符都必须是 ASCII 字符，但该 URI 可以为需要转换为对应字符串的非 ASCII 字节值包含百分号转义。

解析过程会检查 Base64 编码的数据是否合法，并将其规范化为使用默认的 Base64 字母表并带有填充。非 Base64 数据会在必要时使用百分号转义进行转义，已有的转义也会被规范化大小写。

访问各个部分时，如果内容最终无法成功解码为字符串，可能会失败，例如已有的百分号转义所表示的字节无法被所选 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding) 解码（参见 [contentAsString]）。

如果 [uri] 不是合法的 data URI，则抛出 [FormatException](https://www.yuque.com/thyname/dart.core/formatexception)。

## 属性

### uri

```dart
Uri get uri
```

该 `UriData` 用于访问的 [Uri](https://www.yuque.com/thyname/dart.core/uri)。

返回一个 scheme 为 `data`、path 为该 data URI 剩余部分的 `Uri`。

### mimeType

```dart
String get mimeType
```

该 data URI 的 MIME 类型。

data URI 由“媒体类型”和其后的数据组成。媒体类型以 MIME 类型开头，之后可以跟有额外的参数。如果 URI 文本中的媒体类型表示包含 URI 转义，返回的字符串中会将其解除转义。如果值包含非 ASCII 的百分号转义，会将其解码为 UTF-8。

示例：

```
data:text/plain;charset=utf-8,Hello%20World!
```

该 data URI 的媒体类型为 `text/plain;charset=utf-8`，即 MIME 类型 `text/plain` 加上值为 `utf-8` 的 `charset` 参数。详见 [RFC 2045](https://tools.ietf.org/html/rfc2045)。

如果 data URI 的第一部分为空，则默认为 `text/plain`。

### charset

```dart
String get charset
```

媒体类型的 charset 参数。

如果媒体类型的参数中包含 `charset` 参数，则返回其值，否则返回 `US-ASCII`，这是 data URI 的默认字符集。如果值包含非 ASCII 的百分号转义，会将其解码为 UTF-8。

如果 URI 文本中的媒体类型表示包含 URI 转义，返回的字符串中会将其解除转义。

### isBase64

```dart
bool get isBase64
```

数据是否经过 Base64 编码。

### contentText

```dart
String get contentText
```

data URI 中数据部分的实际表示形式。

该字符串可能包含百分号转义。

### parameters

```dart
Map<String, String> get parameters
```

表示媒体类型参数的 map。

data URI 可能在 MIME 类型和数据之间包含参数。此方法将这些参数转换为从参数名到参数值的 map。该 map 只包含 URI 中实际出现的参数。即使 `charset` 参数未在 URI 中出现，它也有一个默认值，这反映在 [charset] 获取器中。这意味着即使 `parameters["charset"]` 为 `null`，[charset] 也可能返回一个值。

如果值包含非 ASCII 值或百分号转义，会将其解码为 UTF-8。

## 方法

### isMimeType()

```dart
@Since.new("2.17")
bool isMimeType( String mimeType )
```

[UriData.mimeType] 是否等于 [mimeType]。

使用忽略 ASCII 字母大小写的比较方式，将该 `data:` URI 的 MIME 类型与 [mimeType] 进行比较。

空的 [mimeType] 在 [mimeType] 参数和 `data:` URI 本身中都被视为等价于 `text/plain`。

### isCharset()

```dart
@Since.new("2.17")
bool isCharset( String charset )
```

检查该 mime 类型的 charset 参数是否为 [charset]。

如果该 URI 没有 "charset" 参数，则假定其默认值为 `charset=US-ASCII`。如果 [charset] 为空，则将其视为 `"US-ASCII"`。

如果 [charset] 与 "charset" 参数值在忽略 ASCII 字母大小写的情况下是相同的字符串，或者二者根据 [Encoding.getByName] 对应于同一个 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding)，则返回 true。

### isEncoding()

```dart
bool isEncoding(Encoding encoding)
```

charset 参数是否表示 [encoding]。

如果 URI 中不存在 "charset" 参数，则默认为 "US-ASCII"，即 [ascii](https://www.yuque.com/thyname/dart.convert/ascii) 编码。如果存在，则使用 [Encoding.getByName] 将其转换为 [Encoding](https://www.yuque.com/thyname/dart.convert/encoding)，并与 [encoding] 进行比较。

### contentAsBytes()

```dart
Uint8List contentAsBytes()
```

以字节形式表示的 data URI 数据部分。

如果数据经过 Base64 编码，将被解码为字节。

如果数据未经过 Base64 编码，将通过解除百分号转义字符并返回每个未转义字符的字节值来进行解码。这些字节不会再经过例如 UTF-8 解码等处理。

### contentAsString()

```dart
String contentAsString({
	Encoding? encoding,
})
```

根据 data URI 的内容创建一个字符串。

如果内容经过 Base64 编码，将被解码为字节，然后使用 [encoding] 解码为字符串。如果省略 encoding，若 `charset` 参数被 [Encoding.getByName] 识别，则使用该参数对应的值；否则默认为 [ascii](https://www.yuque.com/thyname/dart.convert/ascii) 编码，这是未指定编码的 data URI 的默认编码。

如果内容未经过 Base64 编码，将首先把百分号转义转换为字节，然后使用 [encoding] 对字符编码和字节值进行解码。
