# UserTag

```dart
abstract final class UserTag {}
```

A UserTag can be used to group samples in the [DevTools CPU profiler](https://docs.flutter.dev/tools/devtools/cpu-profiler).

### maxUserTags

```dart
dynamic maxUserTags
```

The maximum number of UserTag instances that can be created by a program.

### UserTag()

```dart
UserTag(String label)
```

### label

```dart
String get label
```

Label of this [UserTag].

### makeCurrent()

```dart
UserTag makeCurrent()
```

Make this [UserTag] the current tag for the isolate. Returns the current tag before setting.

### defaultTag

```dart
UserTag get defaultTag
```

The default [UserTag] with label 'Default'.

# getCurrentTag()

```dart
UserTag getCurrentTag()
```

Returns the current [UserTag] for the isolate.
