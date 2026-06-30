## Welcome!

> dart sdk：Dart 3.12.2

Welcome to the Dart API reference documentation, covering the [Dart core libraries](https://dart.dev/libraries). These include:

- [dart:core](https://api.dart.dev/dart-core/dart-core-library.html): Core functionality such as strings, numbers, collections, errors, dates, and URIs.
- [dart:io](https://api.dart.dev/dart-io/dart-io-library.html): I/O for non-web apps.
- [dart:async](https://api.dart.dev/dart-async/dart-async-library.html): Functionality for asynchronous programming with futures, streams, and zones.

You'll also find reference documentation covering Dart's various platform interoperability options, such as:

- [dart:js_interop](https://api.dart.dev/dart-js_interop/dart-js_interop-library.html): Library including a sound type hierarchy and helper functions for interoperability with JavaScript.
- [package:web](https://pub.dev/documentation/web): DOM manipulation for web apps.
- [dart:ffi](https://api.dart.dev/dart-ffi/dart-ffi-library.html): Foreign function interfaces for interoperability with the C language.

The core libraries - except for `dart:core` - must be imported before they're available for use:

```dart
import 'dart:math';
```

Additionally, you can find Dart packages at [pub.dev](https://pub.dev/).

### Language docs

The main site for learning and using Dart is [dart.dev](https://dart.dev/). Check out these pages:

- [Dart overview](https://dart.dev/overview)
- [Dart language documentation](https://dart.dev/language)
- [Library tour](https://dart.dev/libraries)
- [Tutorials](https://dart.dev/tutorials)

This API reference is generated from the SDK source at [dart-lang/sdk](https://github.com/dart-lang/sdk). If you'd like to give feedback on or edit this documentation, see [Contributing](https://github.com/dart-lang/sdk/blob/main/CONTRIBUTING.md).

---

## Libraries

### Core

- [dart:async](https://api.dart.dev/dart-async/)

  Support for asynchronous programming, with classes such as Future and Stream.

- [dart:collection](https://api.dart.dev/dart-collection/)

  Classes and utilities that supplement the collection support in dart:core.

- [dart:convert](https://api.dart.dev/dart-convert/)

  Encoders and decoders for converting between different data representations, including JSON and UTF-8.

- [dart:core](https://api.dart.dev/dart-core/)

  Built-in types, collections, and other core functionality for every Dart program.

- [dart:developer](https://api.dart.dev/dart-developer/)

  Interact with developer tools such as the debugger and inspector.

- [dart:math](https://api.dart.dev/dart-math/)

  Mathematical constants and functions, plus a random number generator.

- [dart:typed_data](https://api.dart.dev/dart-typed_data/)

  Lists that efficiently handle fixed sized data (for example, unsigned 8 byte integers) and SIMD numeric types.

### VM

- [dart:ffi](https://api.dart.dev/dart-ffi/)

  Foreign Function Interface for interoperability with the C programming language.

- [dart:io](https://api.dart.dev/dart-io/)

  File, socket, HTTP, and other I/O support for non-web applications.

- [dart:isolate](https://api.dart.dev/dart-isolate/)

  Concurrent programming using _isolates_: independent workers that are similar to threads but don't share memory, communicating only via messages.

- [dart:mirrors](https://api.dart.dev/dart-mirrors/)

  Basic reflection in Dart, with support for introspection and dynamic invocation.
