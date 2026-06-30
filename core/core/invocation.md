# Invocation

```dart
abstract class Invocation {}
```

Representation of the invocation of a member on an object.

This is the type of objects passed to [Object.noSuchMethod] when an object doesn't support the member invocation that was attempted on it.

### Invocation()

```dart
Invocation()
```

### Invocation.method()

```dart
Invocation.method(Symbol memberName, Iterable<Object?>? positionalArguments, [Map<Symbol, Object?>? namedArguments])
```

Creates an invocation corresponding to a method invocation.

The method invocation has no type arguments. If the named arguments are omitted, they default to no named arguments.

### Invocation.genericMethod()

```dart
Invocation.genericMethod(Symbol memberName, Iterable<Type>? typeArguments, Iterable<Object?>? positionalArguments, [Map<Symbol, Object?>? namedArguments])
```

Creates an invocation corresponding to a generic method invocation.

If [typeArguments] is `null` or empty, the constructor is equivalent to calling [Invocation.method] with the remaining arguments. All the individual type arguments must be non-null.

If the named arguments are omitted, they default to no named arguments.

### Invocation.getter()

```dart
Invocation.getter(Symbol name)
```

Creates an invocation corresponding to a getter invocation.

### Invocation.setter()

```dart
Invocation.setter(Symbol memberName, Object? argument)
```

Creates an invocation corresponding to a setter invocation.

This constructor accepts any [Symbol] as [memberName], but remember that _actual setter names_ end in `=`, so the invocation corresponding to `object.member = value` is

```dart
Invocation.setter(const Symbol("member="), value)
```

### memberName

```dart
Symbol get memberName
```

The name of the invoked member.

### typeArguments

```dart
List<Type> get typeArguments
```

An unmodifiable view of the type arguments of the call.

If the member is a getter, setter or operator, the type argument list is always empty.

### positionalArguments

```dart
List<dynamic> get positionalArguments
```

An unmodifiable view of the positional arguments of the call.

If the member is a getter, the positional arguments list is always empty.

### namedArguments

```dart
Map<Symbol, dynamic> get namedArguments
```

An unmodifiable view of the named arguments of the call.

If the member is a getter, setter or operator, the named arguments map is always empty.

### isMethod

```dart
bool get isMethod
```

Whether the invocation was a method call.

### isGetter

```dart
bool get isGetter
```

Whether the invocation was a getter call. If so, all three types of arguments lists are empty.

### isSetter

```dart
bool get isSetter
```

Whether the invocation was a setter call.

If so, [positionalArguments] has exactly one positional argument, [namedArguments] is empty, and typeArguments is empty.

### isAccessor

```dart
bool get isAccessor
```

Whether the invocation was a getter or a setter call.
