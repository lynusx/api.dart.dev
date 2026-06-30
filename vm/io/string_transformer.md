# systemEncoding

```dart
SystemEncoding systemEncoding
```

The current system encoding.

This is used for converting from bytes to and from Strings when communicating on stdin, stdout and stderr.

On Windows this will use the currently active code page for the conversion. On all other systems it will always use UTF-8.

# SystemEncoding

```dart
final class SystemEncoding extends Encoding {}
```

The system encoding is the current code page on Windows and UTF-8 on Linux and Mac.

### SystemEncoding()

```dart
SystemEncoding()
```

Creates a const SystemEncoding.

Users should use the top-level constant, [systemEncoding].

### name

```dart
String get name
```

### encode()

```dart
List<int> encode(String input)
```

### decode()

```dart
String decode(List<int> encoded)
```

### encoder

```dart
Converter<String, List<int>> get encoder
```

### decoder

```dart
Converter<List<int>, String> get decoder
```
