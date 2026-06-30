# Object

```dart
class Object {}
```

The base class for all Dart objects except `null`.

Because `Object` is a root of the non-nullable Dart class hierarchy, every other non-`Null` Dart class is a subclass of `Object`.

When you define a class, you should consider overriding [toString] to return a string describing an instance of that class. You might also need to define [hashCode] and [operator ==], as described in the [Implementing map keys](https://dart.dev/libraries/dart-core#implementing-map-keys) section of the [introduction to `dart:core`](https://dart.dev/libraries/dart-core).

### Object()

```dart
Object()
```

Creates a new [Object] instance.

[Object] instances have no meaningful state, and are only useful through their identity. An [Object] instance is equal to itself only.

### operator ==()

```dart
bool operator ==(Object other)
```

The equality operator.

The default behavior for all [Object]s is to return true if and only if this object and [other] are the same object.

Override this method to specify a different equality relation on a class. The overriding method must still be an equivalence relation. That is, it must be:

- Total: It must return a boolean for all arguments. It should never throw.

- Reflexive: For all objects `o`, `o == o` must be true.

- Symmetric: For all objects `o1` and `o2`, `o1 == o2` and `o2 == o1` must either both be true, or both be false.

- Transitive: For all objects `o1`, `o2`, and `o3`, if `o1 == o2` and `o2 == o3` are true, then `o1 == o3` must be true.

The method should also be consistent over time, so whether two objects are equal should only change if at least one of the objects was modified.

If a subclass overrides the equality operator, it should override the [hashCode] method as well to maintain consistency.

### hashCode

```dart
int get hashCode
```

The hash code for this object.

A hash code is a single integer which represents the state of the object that affects [operator ==] comparisons.

All objects have hash codes. The default hash code implemented by [Object] represents only the identity of the object, the same way as the default [operator ==] implementation only considers objects equal if they are identical (see [identityHashCode]).

If [operator ==] is overridden to use the object state instead, the hash code must also be changed to represent that state, otherwise the object cannot be used in hash based data structures like the default [Set] and [Map] implementations.

Hash codes must be the same for objects that are equal to each other according to [operator ==]. The hash code of an object should only change if the object changes in a way that affects equality. There are no further requirements for the hash codes. They need not be consistent between executions of the same program and there are no distribution guarantees.

Objects that are not equal are allowed to have the same hash code. It is even technically allowed that all instances have the same hash code, but if clashes happen too often, it may reduce the efficiency of hash-based data structures like [HashSet] or [HashMap].

If a subclass overrides [hashCode], it should override the [operator ==] operator as well to maintain consistency.

### toString()

```dart
String toString()
```

A string representation of this object.

Some classes have a default textual representation, often paired with a static `parse` function (like [int.parse]). These classes will provide the textual representation as their string representation.

Other classes have no meaningful textual representation that a program will care about. Such classes will typically override `toString` to provide useful information when inspecting the object, mainly for debugging or logging.

### noSuchMethod()

```dart
dynamic noSuchMethod(Invocation invocation)
```

Invoked when a nonexistent method or property is accessed.

A dynamic member invocation can attempt to call a member which doesn't exist on the receiving object. Example:

```dart
dynamic object = 1;
object.add(42); // Statically allowed, run-time error
```

This invalid code will invoke the `noSuchMethod` method of the integer `1` with an [Invocation] representing the `.add(42)` call and arguments (which then throws).

Classes can override [noSuchMethod] to provide custom behavior for such invalid dynamic invocations.

A class with a non-default [noSuchMethod] invocation can also omit implementations for members of its interface. Example:

```dart
class MockList<T> implements List<T> {
  noSuchMethod(Invocation invocation) {
    log(invocation);
    super.noSuchMethod(invocation); // Will throw.
  }
}
void main() {
  MockList().add(42);
}
```

This code has no compile-time warnings or errors even though the `MockList` class has no concrete implementation of any of the `List` interface methods. Calls to `List` methods are forwarded to `noSuchMethod`, so this code will `log` an invocation similar to `Invocation.method(#add, [42])` and then throw.

If a value is returned from `noSuchMethod`, it becomes the result of the original invocation. If the value is not of a type that can be returned by the original invocation, a type error occurs at the invocation.

The default behavior is to throw a [NoSuchMethodError].

### runtimeType

```dart
Type get runtimeType
```

A representation of the runtime type of the object.

### hash()

```dart
int hash(Object? object1, Object? object2, [Object? object3 = sentinelValue, Object? object4 = sentinelValue, Object? object5 = sentinelValue, Object? object6 = sentinelValue, Object? object7 = sentinelValue, Object? object8 = sentinelValue, Object? object9 = sentinelValue, Object? object10 = sentinelValue, Object? object11 = sentinelValue, Object? object12 = sentinelValue, Object? object13 = sentinelValue, Object? object14 = sentinelValue, Object? object15 = sentinelValue, Object? object16 = sentinelValue, Object? object17 = sentinelValue, Object? object18 = sentinelValue, Object? object19 = sentinelValue, Object? object20 = sentinelValue])
```

Creates a combined hash code for a number of objects.

The hash code is computed for all arguments that are actually supplied, even if they are `null`, by numerically combining the [Object.hashCode] of each argument.

Example:

```dart
class SomeObject {
  final Object a, b, c;
  SomeObject(this.a, this.b, this.c);
  bool operator ==(Object other) =>
      other is SomeObject && a == other.a && b == other.b && c == other.c;
  int get hashCode => Object.hash(a, b, c);
}
```

The computed value will be consistent when the function is called with the same arguments multiple times during the execution of a single program.

The hash value generated by this function is _not_ guaranteed to be stable over different runs of the same program, or between code run in different isolates of the same program. The exact algorithm used may differ between different platforms, or between different versions of the platform libraries, and it may depend on values that change on each program execution.

The [hashAll] function gives the same result as this function when called with a collection containing the actual arguments to this function in the same order.

### hashAll()

```dart
int hashAll(Iterable<Object?> objects)
```

Creates a combined hash code for a sequence of objects.

The hash code is computed for elements in [objects], even if they are `null`, by numerically combining the [Object.hashCode] of each element in iteration order.

The result of `hashAll([o])` is not `o.hashCode`.

Example:

```dart
class SomeObject {
  final List<String> path;
  SomeObject(this.path);
  bool operator ==(Object other) {
    if (other is SomeObject) {
      if (path.length != other.path.length) return false;
      for (int i = 0; i < path.length; i++) {
        if (path[i] != other.path[i]) return false;
      }
      return true;
    }
    return false;
  }

  int get hashCode => Object.hashAll(path);
}
```

The computed value will be consistent when the function is called again with objects that have the same hash codes in the same order during an execution of a single program.

The hash value generated by this function is _not_ guaranteed to be stable over different runs of the same program, or between code run in different isolates of the same program. The exact algorithm used may differ between different platforms, or between different versions of the platform libraries, and it may depend on values that change on each program execution.

### hashAllUnordered()

```dart
int hashAllUnordered(Iterable<Object?> objects)
```

Creates a combined hash code for a collection of objects.

The hash code is computed for elements in [objects], even if they are `null`, by numerically combining the [Object.hashCode] of each element in an order independent way.

The result of `hashAllUnordered({o})` is not `o.hashCode`.

Example:

```dart
bool setEquals<T>(Set<T> set1, Set<T> set2) {
  var hashCode1 = Object.hashAllUnordered(set1);
  var hashCode2 = Object.hashAllUnordered(set2);
  if (hashCode1 != hashCode2) return false;
  // Compare elements ...
}
```

The computed value will be consistent when the function is called again with objects that have the same hash codes during an execution of a single program, even if the objects are not necessarily in the same order,

The hash value generated by this function is _not_ guaranteed to be stable over different runs of the same program. The exact algorithm used may differ between different platforms, or between different versions of the platform libraries, and it may depend on values that change on each program execution.
