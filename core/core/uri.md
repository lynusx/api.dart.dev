# Uri

```dart
abstract interface class Uri {}
```

A parsed URI, such as a URL.

To create a URI with specific components, use [Uri.new]:

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

## HTTP and HTTPS URI

To create a URI with https scheme, use [Uri.https] or [Uri.http]:

```dart
final httpsUri = Uri.https('example.com', 'api/fetch', {'limit': '10'});
print(httpsUri); // https://example.com/api/fetch?limit=10
```

## File URI

To create a URI from file path, use [Uri.file]:

```dart
final fileUriUnix =
    Uri.file(r'/home/myself/images/image.png', windows: false);
print(fileUriUnix); // file:///home/myself/images/image.png

final fileUriWindows =
    Uri.file(r'C:\Users\myself\Documents\image.png', windows: true);
print(fileUriWindows); // file:///C:/Users/myself/Documents/image.png
```

If the URI is not a file URI, calling this throws [UnsupportedError].

## Directory URI

Like [Uri.file] except that a non-empty URI path ends in a slash.

```dart
final fileDirectory =
    Uri.directory('/home/myself/data/image', windows: false);
print(fileDirectory); // file:///home/myself/data/image/

final fileDirectoryWindows = Uri.directory('/data/images', windows: true);
print(fileDirectoryWindows); //  file:///data/images/
```

## URI from string

To create a URI from string, use [Uri.parse] or [Uri.tryParse]:

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

**See also:**

- [URIs][uris] in the [introduction to `dart:core`][libtour]
- [RFC-3986](https://tools.ietf.org/html/rfc3986)
- [RFC-2396](https://tools.ietf.org/html/rfc2396)
- [RFC-2045](https://tools.ietf.org/html/rfc2045)

[uris]: https://dart.dev/libraries/dart-core#uris [libtour]: https://dart.dev/libraries/dart-core

### base

```dart
Uri get base
```

The natural base URI for the current platform.

When running in a browser, this is the current URL of the current page (from `window.location.href`).

When not running in a browser, this is the file URI referencing the current working directory.

### Uri()

```dart
Uri({String? scheme, String? userInfo, String? host, int? port, String? path, Iterable<String>? pathSegments, String? query, Map<String, dynamic>? queryParameters, String? fragment})
```

Creates a new URI from its components.

Each component is set through a named argument. Any number of components can be provided. The [path] and [query] components can be set using either of two different named arguments.

The scheme component is set through [scheme]. The scheme is normalized to all lowercase letters. If the scheme is omitted or empty, the URI will not have a scheme part.

The user info part of the authority component is set through [userInfo]. It defaults to the empty string, which will be omitted from the string representation of the URI.

The host part of the authority component is set through [host]. The host can either be a hostname, an IPv4 address or an IPv6 address, contained in `'['` and `']'`. If the host contains a ':' character, the `'['` and `']'` are added if not already provided. The host is normalized to all lowercase letters.

The port part of the authority component is set through [port]. If [port] is omitted or `null`, it implies the default port for the URI's scheme, and is equivalent to passing that port explicitly. The recognized schemes, and their default ports, are "http" (80) and "https" (443). All other schemes are considered as having zero as the default port.

If any of `userInfo`, `host` or `port` are provided, the URI has an authority according to [hasAuthority].

The path component is set through either [path] or [pathSegments]. When [path] is used, it should be a valid URI path, but invalid characters, except the general delimiters ':/@[]?#', will be escaped if necessary. A backslash, `\`, will be converted to a slash `/`. When [pathSegments] is used, each of the provided segments is first percent-encoded and then joined using the forward slash separator.

The percent-encoding of the path segments encodes all characters except for the unreserved characters and the following list of characters: `!$&'()*+,;=:@`. If the other components necessitate an absolute path, a leading slash `/` is prepended if not already there.

The query component is set through either [query] or [queryParameters]. When [query] is used, the provided string should be a valid URI query, but invalid characters, other than general delimiters, will be escaped if necessary. When [queryParameters] is used, the query is built from the provided map. Each key and value in the map is percent-encoded and joined using equal and ampersand characters. A value in the map must be either `null`, a string, or an [Iterable] of strings. An iterable corresponds to multiple values for the same key, and an empty iterable or `null` corresponds to no value for the key.

The percent-encoding of the keys and values encodes all characters except for the unreserved characters, and replaces spaces with `+`. If [query] is the empty string, it is equivalent to omitting it. To have an actual empty query part, use an empty map for [queryParameters].

If both [query] and [queryParameters] are omitted or `null`, the URI has no query part.

The fragment component is set through [fragment]. It should be a valid URI fragment, but invalid characters other than general delimiters are escaped if necessary. If [fragment] is omitted or `null`, the URI has no fragment part.

Example:

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
Uri.http(String authority, [String unencodedPath, Map<String, dynamic>? queryParameters])
```

Creates a new `http` URI from authority, path and query.

Example:

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

The `scheme` is always set to `http`.

The `userInfo`, `host` and `port` components are set from the [authority] argument. If `authority` is `null` or empty, the created `Uri` has no authority, and isn't directly usable as an HTTP URL, which must have a non-empty host.

The `path` component is set from the [unencodedPath] argument. The path passed must not be encoded as this constructor encodes the path. Only `/` is recognized as path separator. If omitted, the path defaults to being empty.

The `query` component is set from the optional [queryParameters] argument.

### Uri.https()

```dart
Uri.https(String authority, [String unencodedPath, Map<String, dynamic>? queryParameters])
```

Creates a new `https` URI from authority, path and query.

This constructor is the same as [Uri.http] except for the scheme which is set to `https`.

Example:

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
Uri.file(String path, {bool? windows})
```

Creates a new file URI from an absolute or relative file path.

The file path is passed in [path].

This path is interpreted using either Windows or non-Windows semantics.

With non-Windows semantics, the slash (`/`) is used to separate path segments in the input [path].

With Windows semantics, backslash (`\`) and forward-slash (`/`) are used to separate path segments in the input [path], except if the path starts with `\\?\` in which case only backslash (`\`) separates path segments in [path].

If the path starts with a path separator, an absolute URI (with the `file` scheme and an empty authority) is created. Otherwise a relative URI reference with no scheme or authority is created. One exception to this rule is that when Windows semantics is used and the path starts with a drive letter followed by a colon (":") and a path separator, then an absolute URI is created.

The default for whether to use Windows or non-Windows semantics is determined from the platform Dart is running on. When running in the standalone VM, this is detected by the VM based on the operating system. When running in a browser, non-Windows semantics is always used.

To override the automatic detection of which semantics to use pass a value for [windows]. Passing `true` will use Windows semantics and passing `false` will use non-Windows semantics.

Examples using non-Windows semantics:

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

Examples using Windows semantics:

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

// This throws an error. A path with a drive letter, but no following
// path, is not allowed.
Uri.file(r'C:', windows: true);

// This throws an error. A path with a drive letter is not absolute.
Uri.file(r'C:xxx\yyy', windows: true);

// file://server/share/file
Uri.file(r'\\server\share\file', windows: true);
```

If the path passed is not a valid file path, an error is thrown.

### Uri.directory()

```dart
Uri.directory(String path, {bool? windows})
```

Like [Uri.file] except that a non-empty URI path ends in a slash.

If [path] is not empty, and it doesn't end in a directory separator, then a slash is added to the returned URI's path. In all other cases, the result is the same as returned by `Uri.file`.

Example:

```dart
final fileDirectory = Uri.directory('data/images', windows: false);
print(fileDirectory); // data/images/

final fileDirectoryWindows =
   Uri.directory(r'C:\data\images', windows: true);
print(fileDirectoryWindows); // file:///C:/data/images/
```

### Uri.dataFromString()

```dart
Uri.dataFromString(String content, {String? mimeType, Encoding? encoding, Map<String, String>? parameters, bool base64 = false})
```

Creates a `data:` URI containing the [content] string.

Converts the content to bytes using [encoding] or the charset specified in [parameters] (defaulting to US-ASCII if not specified or unrecognized), then encodes the bytes into the resulting data URI.

Defaults to encoding using percent-encoding (any non-ASCII or non-URI-valid bytes is replaced by a percent encoding). If [base64] is true, the bytes are instead encoded using [base64].

If [encoding] is not provided and [parameters] has a `charset` entry, that name is looked up using [Encoding.getByName], and if the lookup returns an encoding, that encoding is used to convert [content] to bytes. If providing both an [encoding] and a charset in [parameters], they should agree, otherwise decoding won't be able to use the charset parameter to determine the encoding.

If [mimeType] and/or [parameters] are supplied, they are added to the created URI. If any of these contain characters that are not allowed in the data URI, the character is percent-escaped. If the character is non-ASCII, it is first UTF-8 encoded and then the bytes are percent encoded. An omitted [mimeType] in a data URI means `text/plain`, just as an omitted `charset` parameter defaults to meaning `US-ASCII`.

To read the content back, use [UriData.contentAsString].

Example:

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
Uri.dataFromBytes(List<int> bytes, {String mimeType = "application/octet-stream", Map<String, String>? parameters, bool percentEncoded = false})
```

Creates a `data:` URI containing an encoding of [bytes].

Defaults to Base64 encoding the bytes, but if [percentEncoded] is `true`, the bytes will instead be percent encoded (any non-ASCII or non-valid-ASCII-character byte is replaced by a percent encoding).

To read the bytes back, use [UriData.contentAsBytes].

It defaults to having the mime-type `application/octet-stream`. The [mimeType] and [parameters] are added to the created URI. If any of these contain characters that are not allowed in the data URI, the character is percent-escaped. If the character is non-ASCII, it is first UTF-8 encoded and then the bytes are percent encoded.

Example:

```dart
final uri = Uri.dataFromBytes([68, 97, 114, 116]);
print(uri); // data:application/octet-stream;base64,RGFydA==
```

### scheme

```dart
String get scheme
```

The scheme component of the URI.

The value is the empty string if there is no scheme component.

A URI scheme is case insensitive. The returned scheme is canonicalized to lowercase letters.

### authority

```dart
String get authority
```

The authority component.

The authority is formatted from the [userInfo], [host] and [port] parts.

The value is the empty string if there is no authority component.

### userInfo

```dart
String get userInfo
```

The user info part of the authority component.

The value is the empty string if there is no user info in the authority component.

### host

```dart
String get host
```

The host part of the authority component.

The value is the empty string if there is no authority component and hence no host.

If the host is an IP version 6 address, the surrounding `[` and `]` is removed.

The host string is case-insensitive. The returned host name is canonicalized to lower-case with upper-case percent-escapes.

### port

```dart
int get port
```

The port part of the authority component.

The value is the default port if there is no port number in the authority component. That's 80 for http, 443 for https, and 0 for everything else.

### path

```dart
String get path
```

The path component.

The path is the actual substring of the URI representing the path, and it is encoded where necessary. To get direct access to the decoded path, use [pathSegments].

The path value is the empty string if there is no path component.

### query

```dart
String get query
```

The query component.

The value is the actual substring of the URI representing the query part, and it is encoded where necessary. To get direct access to the decoded query, use [queryParameters].

The value is the empty string if there is no query component.

### fragment

```dart
String get fragment
```

The fragment identifier component.

The value is the empty string if there is no fragment identifier component.

### pathSegments

```dart
List<String> get pathSegments
```

The URI path split into its segments.

Each of the segments in the list has been decoded. If the path is empty, the empty list will be returned. A leading slash `/` does not affect the segments returned.

The list is unmodifiable and will throw [UnsupportedError] on any calls that would mutate it.

### queryParameters

```dart
Map<String, String> get queryParameters
```

The URI query split into a map according to the rules specified for FORM post in the [HTML 4.01 specification section 17.13.4](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4').

Each key and value in the resulting map has been decoded. If there is no query, the empty map is returned.

Keys in the query string that have no value are mapped to the empty string. If a key occurs more than once in the query string, it is mapped to an arbitrary choice of possible value. The [queryParametersAll] getter can provide a map that maps keys to all of their values.

Example:

```dart import:convert
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10,20,30&max=100');
print(jsonEncode(uri.queryParameters));
// {"limit":"10,20,30","max":"100"}
```

The map is unmodifiable.

### queryParametersAll

```dart
Map<String, List<String>> get queryParametersAll
```

Returns the URI query split into a map according to the rules specified for FORM post in the [HTML 4.01 specification section 17.13.4](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4').

Each key and value in the resulting map has been decoded. If there is no query, the map is empty.

Keys are mapped to lists of their values. If a key occurs only once, its value is a singleton list. If a key occurs with no value, the empty string is used as the value for that occurrence.

Example:

```dart import:convert
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10&limit=20&limit=30&max=100');
print(jsonEncode(uri.queryParametersAll)); // {"limit":["10","20","30"],"max":["100"]}
```

The map and the lists it contains are unmodifiable.

### isAbsolute

```dart
bool get isAbsolute
```

Whether the URI is absolute.

A URI is an absolute URI in the sense of RFC 3986 if it has a scheme and no fragment.

### hasScheme

```dart
bool get hasScheme
```

Whether the URI has a [scheme] component.

### hasAuthority

```dart
bool get hasAuthority
```

Whether the URI has an [authority] component.

### hasPort

```dart
bool get hasPort
```

Whether the URI has an explicit port.

If the port number is the default port number (zero for unrecognized schemes, with http (80) and https (443) being recognized), then the port is made implicit and omitted from the URI.

### hasQuery

```dart
bool get hasQuery
```

Whether the URI has a query part.

### hasFragment

```dart
bool get hasFragment
```

Whether the URI has a fragment part.

### hasEmptyPath

```dart
bool get hasEmptyPath
```

Whether the URI has an empty path.

### hasAbsolutePath

```dart
bool get hasAbsolutePath
```

Whether the URI has an absolute path (starting with '/').

### origin

```dart
String get origin
```

Returns the origin of the URI in the form scheme://host:port for the schemes http and https.

It is an error if the scheme is not "http" or "https", or if the host name is missing or empty.

See: https://www.w3.org/TR/2011/WD-html5-20110405/origin-0.html#origin

### isScheme()

```dart
bool isScheme(String scheme)
```

Whether the scheme of this [Uri] is [scheme].

The [scheme] should be the same as the one returned by [Uri.scheme], but doesn't have to be case-normalized to lower-case characters.

Example:

```dart
var uri = Uri.parse('http://example.com');
print(uri.isScheme('HTTP')); // true

final uriNoScheme = Uri(host: 'example.com');
print(uriNoScheme.isScheme('HTTP')); // false
```

An empty [scheme] string matches a URI with no scheme (one where [hasScheme] returns false).

### toFilePath()

```dart
String toFilePath({bool? windows})
```

Creates a file path from a file URI.

The returned path has either Windows or non-Windows semantics.

For non-Windows semantics, the slash ("/") is used to separate path segments.

For Windows semantics, the backslash ("\\") separator is used to separate path segments.

If the URI is absolute, the path starts with a path separator unless Windows semantics is used and the first path segment is a drive letter. When Windows semantics is used, a host component in the uri in interpreted as a file server and a UNC path is returned.

The default for whether to use Windows or non-Windows semantics is determined from the platform Dart is running on. When running in the standalone VM, this is detected by the VM based on the operating system. When running in a browser, non-Windows semantics is always used.

To override the automatic detection of which semantics to use pass a value for [windows]. Passing `true` will use Windows semantics and passing `false` will use non-Windows semantics.

If the URI ends with a slash (i.e. the last path component is empty), the returned file path will also end with a slash.

With Windows semantics, URIs starting with a drive letter cannot be relative to the current drive on the designated drive. That is, for the URI `file:///c:abc` calling `toFilePath` will throw as a path segment cannot contain colon on Windows.

Examples using non-Windows semantics (resulting of calling toFilePath in comment):

```dart
Uri.parse("xxx/yyy");  // xxx/yyy
Uri.parse("xxx/yyy/");  // xxx/yyy/
Uri.parse("file:///xxx/yyy");  // /xxx/yyy
Uri.parse("file:///xxx/yyy/");  // /xxx/yyy/
Uri.parse("file:///C:");  // /C:
Uri.parse("file:///C:a");  // /C:a
```

Examples using Windows semantics (resulting URI in comment):

```dart
Uri.parse("xxx/yyy");  // xxx\yyy
Uri.parse("xxx/yyy/");  // xxx\yyy\
Uri.parse("file:///xxx/yyy");  // \xxx\yyy
Uri.parse("file:///xxx/yyy/");  // \xxx\yyy\
Uri.parse("file:///C:/xxx/yyy");  // C:\xxx\yyy
Uri.parse("file:C:xxx/yyy");  // Throws as a path segment
                              // cannot contain colon on Windows.
Uri.parse("file://server/share/file");  // \\server\share\file
```

If the URI is not a file URI, calling this throws [UnsupportedError].

If the URI cannot be converted to a file path, calling this throws [UnsupportedError].

### data

```dart
UriData? get data
```

Access the structure of a `data:` URI.

Returns a [UriData] object for `data:` URIs and `null` for all other URIs. The [UriData] object can be used to access the media type and data of a `data:` URI.

### hashCode

```dart
int get hashCode
```

Returns a hash code computed as `toString().hashCode`.

This guarantees that URIs with the same normalized string representation have the same hash code.

### operator ==()

```dart
bool operator ==(Object other)
```

A URI is equal to another URI with the same normalized representation.

### toString()

```dart
String toString()
```

The normalized string representation of the URI.

### replace()

```dart
Uri replace({String? scheme, String? userInfo, String? host, int? port, String? path, Iterable<String>? pathSegments, String? query, Map<String, dynamic>? queryParameters, String? fragment})
```

Creates a new `Uri` based on this one, but with some parts replaced.

This method takes the same parameters as the [Uri] constructor, and they have the same meaning.

At most one of [path] and [pathSegments] must be provided. Likewise, at most one of [query] and [queryParameters] must be provided.

Each part that is not provided will default to the corresponding value from this `Uri` instead.

This method is different from [Uri.resolve], which overrides in a hierarchical manner, and can instead replace each part of a `Uri` individually.

Example:

```dart
final uri1 = Uri.parse(
    'https://dart.dev/libraries/dart-core#utility-classes');

final uri2 = uri1.replace(
    scheme: 'https',
    path: 'libraries/dart-core',
    fragment: 'uris');
print(uri2); // https://dart.dev/libraries/dart-core#uris
```

This method acts similarly to using the `Uri` constructor with some of the arguments taken from this `Uri`. Example:

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

Using this method can be seen as shorthand for the `Uri` constructor call above, but may also be slightly faster because the parts taken from this `Uri` need not be checked for validity again.

### removeFragment()

```dart
Uri removeFragment()
```

Creates a `Uri` that differs from this only in not having a fragment.

If this `Uri` does not have a fragment, it is itself returned.

Example:

```dart
final uri =
    Uri.parse('https://example.org:8080/foo/bar#frag').removeFragment();
print(uri); // https://example.org:8080/foo/bar
```

### resolve()

```dart
Uri resolve(String reference)
```

Resolve [reference] as an URI relative to `this`.

First turn [reference] into a URI using [Uri.parse]. Then resolve the resulting URI relative to `this`.

Returns the resolved URI.

See [resolveUri] for details.

### resolveUri()

```dart
Uri resolveUri(Uri reference)
```

Resolve [reference] as a URI relative to `this`.

Returns the resolved URI.

The algorithm "Transform Reference" for resolving a reference is described in [RFC-3986 Section 5](https://tools.ietf.org/html/rfc3986#section-5 'RFC-1123').

Updated to handle the case where the base URI is just a relative path - that is: when it has no scheme and no authority and the path does not start with a slash. In that case, the paths are combined without removing leading "..", and an empty path is not converted to "/".

### normalizePath()

```dart
Uri normalizePath()
```

Returns a URI where the path has been normalized.

A normalized path does not contain `.` segments or non-leading `..` segments. Only a relative path with no scheme or authority may contain leading `..` segments; a path that starts with `/` will also drop any leading `..` segments.

This uses the same normalization strategy as `Uri().resolve(this)`.

Does not change any part of the URI except the path.

The default implementation of `Uri` always normalizes paths, so calling this function has no effect.

### parse()

```dart
Uri parse(String uri, [int start = 0, int? end])
```

Creates a new `Uri` object by parsing a URI string.

If [start] and [end] are provided, they must specify a valid substring of [uri], and only the substring from `start` to `end` is parsed as a URI.

If the [uri] string is not valid as a URI or URI reference, a [FormatException] is thrown.

Example:

```dart
final uri =
    Uri.parse('https://example.com/api/fetch?limit=10,20,30&max=100');
print(uri); // https://example.com/api/fetch?limit=10,20,30&max=100

Uri.parse('::Not valid URI::'); // Throws FormatException.
```

### tryParse()

```dart
Uri? tryParse(String uri, [int start = 0, int? end])
```

Creates a new `Uri` object by parsing a URI string.

If [start] and [end] are provided, they must specify a valid substring of [uri], and only the substring from `start` to `end` is parsed as a URI.

Returns `null` if the [uri] string is not valid as a URI or URI reference.

Example:

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

Encode the string [component] using percent-encoding to make it safe for literal use as a URI component.

All characters except uppercase and lowercase letters, digits and the characters `-_.!~*'()` are percent-encoded. This is the set of characters specified in RFC 2396 and which is specified for the encodeUriComponent in ECMA-262 version 5.1.

When manually encoding path segments or query components, remember to encode each part separately before building the path or query string.

For encoding the query part consider using [encodeQueryComponent].

To avoid the need for explicitly encoding, use the [pathSegments] and [queryParameters] optional named arguments when constructing a [Uri].

Example:

```dart
const request = 'http://example.com/search=Dart';
final encoded = Uri.encodeComponent(request);
print(encoded); // http%3A%2F%2Fexample.com%2Fsearch%3DDart
```

### encodeQueryComponent()

```dart
String encodeQueryComponent(String component, {Encoding encoding = utf8})
```

### decodeComponent()

```dart
String decodeComponent(String encodedComponent)
```

Decodes the percent-encoding in [encodedComponent].

Note that decoding a URI component might change its meaning as some of the decoded characters could be characters which are delimiters for a given URI component type. Always split a URI component using the delimiters for the component before decoding the individual parts.

For handling the [path] and [query] components, consider using [pathSegments] and [queryParameters] to get the separated and decoded component.

Example:

```dart
final decoded =
    Uri.decodeComponent('http%3A%2F%2Fexample.com%2Fsearch%3DDart');
print(decoded); // http://example.com/search=Dart
```

### decodeQueryComponent()

```dart
String decodeQueryComponent(String encodedComponent, {Encoding encoding = utf8})
```

Decodes the percent-encoding in [encodedComponent], converting pluses to spaces.

It will create a byte-list of the decoded characters, and then use [encoding] to decode the byte-list to a String. The default encoding is UTF-8.

### encodeFull()

```dart
String encodeFull(String uri)
```

Encodes the string [uri] using percent-encoding to make it safe for literal use as a full URI.

All characters except uppercase and lowercase letters, digits and the characters `!#$&'()*+,-./:;=?@_~` are percent-encoded. This is the set of characters specified in ECMA-262 version 5.1 for the encodeURI function.

Example:

```dart
final encoded =
    Uri.encodeFull('https://example.com/api/query?search= dart is');
print(encoded); // https://example.com/api/query?search=%20dart%20is
```

### decodeFull()

```dart
String decodeFull(String uri)
```

Decodes the percent-encoding in [uri].

Note that decoding a full URI might change its meaning as some of the decoded characters could be reserved characters. In most cases, an encoded URI should be parsed into components using [Uri.parse] before decoding the separate components.

Example:

```dart
final decoded =
    Uri.decodeFull('https://example.com/api/query?search=%20dart%20is');
print(decoded); // https://example.com/api/query?search= dart is
```

### splitQueryString()

```dart
Map<String, String> splitQueryString(String query, {Encoding encoding = utf8})
```

Splits the [query] into a map according to the rules specified for FORM post in the [HTML 4.01 specification section 17.13.4](https://www.w3.org/TR/REC-html40/interact/forms.html#h-17.13.4 'HTML 4.01 section 17.13.4').

Each key and value in the returned map has been decoded. If the [query] is the empty string, an empty map is returned.

Keys in the query string that have no value are mapped to the empty string.

Each query component will be decoded using [encoding]. The default encoding is UTF-8.

Example:

```dart import:convert
final queryStringMap =
    Uri.splitQueryString('limit=10&max=100&search=Dart%20is%20fun');
print(jsonEncode(queryStringMap));
// {"limit":"10","max":"100","search":"Dart is fun"}

```

### parseIPv4Address()

```dart
List<int> parseIPv4Address(String host, [int start = 0, int? end])
```

Parses the [host] as an IP version 4 (IPv4) address, returning the address as a list of 4 bytes in network byte order (big endian).

If [start] and [end] are provided, only that range, which must be a valid range of [host], is parsed. Defaults to all of [host].

Throws a [FormatException] if (the range of) [host] is not a valid IPv4 address representation, meaning something of the form `<octet>.<octet>.<octet>.<octet>` where each octet is a decimal numeral in the range 0..255 with no leading zeros.

### parseIPv6Address()

```dart
List<int> parseIPv6Address(String host, [int start = 0, int? end])
```

Parses the [host] as an IP version 6 (IPv6) address.

Returns the address as a list of 16 bytes in network byte order (big endian).

Throws a [FormatException] if [host] is not a valid IPv6 address representation.

Acts on the substring from [start] to [end]. If [end] is omitted, it defaults to the end of the string.

Some examples of IPv6 addresses:

- `::1`
- `FEDC:BA98:7654:3210:FEDC:BA98:7654:3210`
- `3ffe:2a00:100:7031::1`
- `::FFFF:129.144.52.38`
- `2010:836B:4179::836B:4179`

The grammar for IPv6 addresses are:

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
                ;; least-significant 32 bits of address
h16         ::= HEXDIG{1,4}
                ;; 16 bits of address represented in hexadecimal
```

That is

- eight 1-to-4-digit hexadecimal numerals separated by `:`, or
- one to seven such `:`-separated numerals, with either one pair is separated by `::`, or a leading or trailing `::`.
- either of the above with a trailing two `:`-separated numerals replaced by an IPv4 address.

An IPv6 address with a zone ID (from RFC 6874) is an IPv6 address followed by `%25` (an escaped `%`) and valid zone characters.

````
IPv6addrz ::= IPv6address "%25" ZoneID
ZoneID    ::= (unreserved | pct-encoded)+
```.

# UriData
```dart
final class UriData {}
````

A way to access the structure of a `data:` URI.

Data URIs are non-hierarchical URIs that can contain any binary data. They are defined by [RFC 2397](https://tools.ietf.org/html/rfc2397).

This class allows parsing the URI text, extracting individual parts of the URI, as well as building the URI text from structured parts.

### UriData.fromString()

```dart
UriData.fromString(String content, {String? mimeType, Encoding? encoding, Map<String, String>? parameters, bool base64 = false})
```

Creates a `data:` URI containing the [content] string.

Equivalent to `Uri.dataFromString(...).data`, but may be more efficient if the [uri] itself isn't used.

### UriData.fromBytes()

```dart
UriData.fromBytes(List<int> bytes, {String mimeType = "application/octet-stream", Map<String, String>? parameters, bool percentEncoded = false})
```

Creates a `data:` URI containing an encoding of [bytes].

Equivalent to `Uri.dataFromBytes(...).data`, but may be more efficient if the [uri] itself isn't used.

### UriData.fromUri()

```dart
UriData.fromUri(Uri uri)
```

Creates a `DataUri` from a [Uri] which must have `data` as [Uri.scheme].

The [uri] must have scheme `data` and no authority or fragment, and the path (concatenated with the query, if there is one) must be valid as data URI content with the same rules as [parse].

### parse()

```dart
UriData parse(String uri)
```

Parses a string as a `data` URI.

The string must have the format:

```plaintext
'data:' (type '/' subtype)? (';' attribute '=' value)* (';base64')? ',' data
```

where `type`, `subtype`, `attribute` and `value` are specified in RFC-2045, and `data` is a sequence of URI-characters (RFC-2396 `uric`).

This means that all the characters must be ASCII, but the URI may contain percent-escapes for non-ASCII byte values that need an interpretation to be converted to the corresponding string.

Parsing checks that Base64 encoded data is valid, and it normalizes it to use the default Base64 alphabet and to use padding. Non-Base64 data is escaped using percent-escapes as necessary to make it valid, and existing escapes are case normalized.

Accessing the individual parts may fail later if they turn out to have content that cannot be decoded successfully as a string, for example if existing percent escapes represent bytes that cannot be decoded by the chosen [Encoding] (see [contentAsString]).

A [FormatException] is thrown if [uri] is not a valid data URI.

### uri

```dart
Uri get uri
```

The [Uri] that this `UriData` is giving access to.

Returns a `Uri` with scheme `data` and the remainder of the data URI as path.

### mimeType

```dart
String get mimeType
```

The MIME type of the data URI.

A data URI consists of a "media type" followed by data. The media type starts with a MIME type and can be followed by extra parameters. If the MIME type representation in the URI text contains URI escapes, they are unescaped in the returned string. If the value contain non-ASCII percent escapes, they are decoded as UTF-8.

Example:

```
data:text/plain;charset=utf-8,Hello%20World!
```

This data URI has the media type `text/plain;charset=utf-8`, which is the MIME type `text/plain` with the parameter `charset` with value `utf-8`. See [RFC 2045](https://tools.ietf.org/html/rfc2045) for more detail.

If the first part of the data URI is empty, it defaults to `text/plain`.

### isMimeType()

```dart
bool isMimeType(String mimeType)
```

Whether the [UriData.mimeType] is equal to [mimeType].

Compares the `data:` URI's MIME type to [mimeType] with a case- insensitive comparison which ignores the case of ASCII letters.

An empty [mimeType] is considered equivalent to `text/plain`, both in the [mimeType] argument and in the `data:` URI itself.

### charset

```dart
String get charset
```

The charset parameter of the media type.

If the parameters of the media type contains a `charset` parameter then this returns its value, otherwise it returns `US-ASCII`, which is the default charset for data URIs. If the values contain non-ASCII percent escapes, they are decoded as UTF-8.

If the MIME type representation in the URI text contains URI escapes, they are unescaped in the returned string.

### isCharset()

```dart
bool isCharset(String charset)
```

Checks whether the charset parameter of the mime type is [charset].

If this URI has no "charset" parameter, it is assumed to have a default of `charset=US-ASCII`. If [charset] is empty, it's treated like `"US-ASCII"`.

Returns true if [charset] and the "charset" parameter value are equal strings, ignoring the case of ASCII letters, or both correspond to the same [Encoding], as given by [Encoding.getByName].

### isEncoding()

```dart
bool isEncoding(Encoding encoding)
```

Whether the charset parameter represents [encoding].

If the "charset" parameter is not present in the URI, it defaults to "US-ASCII", which is the [ascii] encoding. If present, it's converted to an [Encoding] using [Encoding.getByName], and compared to [encoding].

### isBase64

```dart
bool get isBase64
```

Whether the data is Base64 encoded or not.

### contentText

```dart
String get contentText
```

The content part of the data URI, as its actual representation.

This string may contain percent escapes.

### contentAsBytes()

```dart
Uint8List contentAsBytes()
```

The content part of the data URI as bytes.

If the data is Base64 encoded, it will be decoded to bytes.

If the data is not Base64 encoded, it will be decoded by unescaping percent-escaped characters and returning byte values of each unescaped character. The bytes will not be, e.g., UTF-8 decoded.

### contentAsString()

```dart
String contentAsString({Encoding? encoding})
```

Creates a string from the content of the data URI.

If the content is Base64 encoded, it will be decoded to bytes and then decoded to a string using [encoding]. If encoding is omitted, the value of a `charset` parameter is used if it is recognized by [Encoding.getByName]; otherwise it defaults to the [ascii] encoding, which is the default encoding for data URIs that do not specify an encoding.

If the content is not Base64 encoded, it will first have percent-escapes converted to bytes and then the character codes and byte values are decoded using [encoding].

### parameters

```dart
Map<String, String> get parameters
```

A map representing the parameters of the media type.

A data URI may contain parameters between the MIME type and the data. This converts these parameters to a map from parameter name to parameter value. The map only contains parameters that actually occur in the URI. The `charset` parameter has a default value even if it doesn't occur in the URI, which is reflected by the [charset] getter. This means that [charset] may return a value even if `parameters["charset"]` is `null`.

If the values contain non-ASCII values or percent escapes, they are decoded as UTF-8.

### toString()

```dart
String toString()
```
