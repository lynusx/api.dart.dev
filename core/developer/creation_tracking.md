# CreationLocation

```dart
final class CreationLocation {}
```

Holds the source code location where an object was created, when its class was annotated with `@pragma('track-creation-locations')`.

When a class definition is annotated with `@pragma('track-creation-locations')`, the Dart compiler injects the call-site location into any invocation of that class's or any subclass's constructors and stores it in the created object.

The location of such an object can be read by calling [CreationLocation.of] with the object as the argument.

## Example

```dart
import 'dart:developer';

// Marks this and any subclass to have their constructor call-sites tracked.
@pragma('track-creation-locations')
class TargetClass {
  TargetClass();
}

void main() {
  // The source-code location of this constructor call is injected into the object.
  final instance = TargetClass();

  final location = CreationLocation.of(instance);
  print(location); // Will print the current file path, line 11, column 20
}
```

## Limitations

The compiler transformation relies on injecting a named parameter into the target class's constructors. Since Dart semantics do not permit a function to have both optional positional parameters and named parameters simultaneously, this transformation **will silently skip** any constructor that declares optional positional parameters. Calling [CreationLocation.of] on an object whose constructor was skipped, will return `null`.

### of()

```dart
CreationLocation? of(Object? object)
```

Returns the creation location of [object].

The provided object must be an instance of a class annotated with `@pragma('track-creation-locations')`.

### file

```dart
String file
```

File path of the location.

### line

```dart
int line
```

1-based line number.

### column

```dart
int column
```

1-based column number.

### name

```dart
String? name
```

Optional name of the parameter or function at this location.

### toJsonMap()

```dart
Map<String, Object?> toJsonMap()
```

JSON representation of this location.

### toString()

```dart
String toString()
```
