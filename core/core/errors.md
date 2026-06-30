# Error

```dart
class Error {}
```

Error objects thrown in the case of a program failure.

An `Error` object represents a program failure that the programmer should have avoided.

Examples include calling a function with invalid arguments, or even with the wrong number of arguments, or calling it at a time when it is not allowed.

These are not errors that a caller should expect or catch &mdash; if they occur, the program is erroneous, and terminating the program may be the safest response.

When deciding that a function should throw an error, the conditions where it happens should be clearly described, and they should be detectable and predictable, so the programmer using the function can avoid triggering the error.

Such descriptions often uses words like "must" or "must not" to describe the condition, and if you see words like that in a function's documentation, then not satisfying the requirement is very likely to cause an error to be thrown.

Example (from [String.contains]):

```plaintext
`startIndex` must not be negative or greater than `length`.
```

In this case, an error will be thrown if `startIndex` is negative or too large.

If the conditions are not detectable before calling a function, the called function should not throw an `Error`. It may still throw, but the caller will have to catch the thrown value, effectively making it an alternative result rather than an error. If so, we consider the thrown object an _exception_ rather than an error. The thrown object can choose to implement [Exception] to document that it represents an exceptional, but not erroneous, occurrence, but being an [Exception] has no other effect than documentation.

All non-`null` values can be thrown in Dart. Objects _extending_ the `Error` class are handled specially: The first time they are thrown, the stack trace at the throw point is recorded and stored in the error object. It can be retrieved using the [stackTrace] getter. An error object that merely implements `Error`, and doesn't extend it, will not store the stack trace automatically.

Error objects are also used for system wide failures like stack overflow or an out-of-memory situation, which the user is also not expected to catch or handle.

Since errors are not created to be caught, there is no need for subclasses to distinguish the errors. Instead subclasses have been created in order to make groups of related errors easy to create with consistent error messages. For example, the [String.contains] method will use a [RangeError] if its `startIndex` isn't in the range `0..length`, which is easily created by `RangeError.range(startIndex, 0, length)`. Catching specific subclasses of [Error] is not intended, and shouldn't happen outside of testing your own code.

### Error()

```dart
Error()
```

### safeToString()

```dart
String safeToString(Object? object)
```

Safely convert a value to a [String] description.

The conversion is guaranteed to not throw, so it won't use the object's toString method except for specific known and trusted types.

### stackTrace

```dart
StackTrace? get stackTrace
```

The stack trace at the point where this error was first thrown.

Classes which _extend_ `Error` will automatically have a stack trace filled in the first time they are thrown by a `throw` expression.

### throwWithStackTrace()

```dart
Never throwWithStackTrace(Object error, StackTrace stackTrace)
```

Throws [error] with associated stack trace [stackTrace].

Behaves like `throw error` would if the [current stack trace][StackTrace.current] was [stackTrace] at the time of the `throw`.

Like for a `throw`, if [error] extends [Error], and it has not been thrown before, its [Error.stackTrace] property will be set to the [stackTrace].

This function does not guarantee to preserve the identity of [stackTrace]. The [StackTrace] object that is caught by a `try`/`catch` of this error, or which is set as the [Error.stackTrace] of an [error], may not be the same [stackTrace] object provided as argument, but it will have the same contents according to [StackTrace.toString].

# AssertionError

```dart
class AssertionError extends Error {}
```

Error thrown by the runtime system when an assert statement fails.

### message

```dart
Object? message
```

Message describing the assertion error.

### AssertionError()

```dart
AssertionError([Object? message])
```

Creates an assertion error with the provided [message].

### toString()

```dart
String toString()
```

# TypeError

```dart
class TypeError extends Error {}
```

Error thrown by the runtime system when a dynamic type error happens.

# ArgumentError

```dart
class ArgumentError extends Error {}
```

Error thrown when a function is passed an unacceptable argument.

The method should document restrictions on the arguments it accepts, for example if an integer argument must be non-nullable, a string argument must be non-empty, or a `dynamic`-typed argument must actually have one of a few accepted types.

The user should be able to predict which arguments will cause an error to be throw, and avoid calling with those.

It's almost always a good idea to provide the unacceptable value as part of the error, to help the user figure out what went wrong, so the [ArgumentError.value] constructor is the preferred constructor. Use [ArgumentError.new] only when the value cannot be provided for some reason.

### invalidValue

```dart
dynamic invalidValue
```

The invalid value.

### name

```dart
String? name
```

Name of the invalid argument, if available.

### message

```dart
dynamic message
```

Message describing the problem.

### ArgumentError()

```dart
ArgumentError([dynamic message, String? name])
```

Creates an error with [message] describing the problem with an argument.

Existing code may be using `message` to hold the invalid value. If the `message` is not a [String], it is assumed to be a value instead of a message.

If [name] is provided, it should be the name of the parameter which received an invalid argument.

Prefer using [ArgumentError.value] instead to retain and document the invalid value as well.

### ArgumentError.value()

```dart
ArgumentError.value(dynamic value, [String? name, dynamic message])
```

Creates error containing the invalid [value].

A message is built by suffixing the [message] argument with the [name] argument (if provided) and the value. Example:

```plaintext
Invalid argument (foo): null
```

The `name` should match the argument name of the function, but if the function is a method implementing an interface, and its argument names differ from the interface, it might be more useful to use the interface method's argument name (or just rename arguments to match).

### ArgumentError.notNull()

```dart
ArgumentError.notNull([String? name])
```

Creates an argument error for a `null` argument that must not be `null`.

### checkNotNull()

```dart
T checkNotNull<T>(T? argument, [String? name])
```

Throws if [argument] is `null`.

If [name] is supplied, it is used as the parameter name in the error message.

Returns the [argument] if it is not null.

### toString()

```dart
String toString()
```

# RangeError

```dart
class RangeError extends ArgumentError {}
```

Error thrown due to an argument value being outside an accepted range.

### start

```dart
num? start
```

The minimum value that [value] is allowed to assume.

### end

```dart
num? end
```

The maximum value that [value] is allowed to assume.

### invalidValue

```dart
num? get invalidValue
```

### RangeError()

```dart
RangeError(dynamic message)
```

Create a new [RangeError] with the given [message].

### RangeError.value()

```dart
RangeError.value(num value, [String? name, String? message])
```

Create a new [RangeError] with a message for the given [value].

An optional [name] can specify the argument name that has the invalid value, and the [message] can override the default error description.

### RangeError.range()

```dart
RangeError.range(num invalidValue, int? minValue, int? maxValue, [String? name, String? message])
```

Create a new [RangeError] for a value being outside the valid range.

The allowed range is from [minValue] to [maxValue], inclusive. If `minValue` or `maxValue` are `null`, the range is infinite in that direction.

For a range from 0 to the length of something, end exclusive, use [RangeError.index].

An optional [name] can specify the argument name that has the invalid value, and the [message] can override the default error description.

### RangeError.index()

```dart
RangeError.index(int index, dynamic indexable, [String? name, String? message, int? length])
```

Creates a new [RangeError] stating that [index] is not a valid index into [indexable].

An optional [name] can specify the argument name that has the invalid value, and the [message] can override the default error description.

The [length] is the length of [indexable] at the time of the error. If `length` is omitted, it defaults to `indexable.length`.

### checkValueInInterval()

```dart
int checkValueInInterval(int value, int minValue, int maxValue, [String? name, String? message])
```

Check that an integer [value] lies in a specific interval.

Throws if [value] is not in the interval. The interval is from [minValue] to [maxValue], both inclusive.

If [name] or [message] are provided, they are used as the parameter name and message text of the thrown error.

Returns [value] if it is in the interval.

### checkValidIndex()

```dart
int checkValidIndex(int index, dynamic indexable, [String? name, int? length, String? message])
```

Check that [index] is a valid index into an indexable object.

Throws if [index] is not a valid index into [indexable].

An indexable object is one that has a `length` and an index-operator `[]` that accepts an index if `0 <= index < length`.

If [name] or [message] are provided, they are used as the parameter name and message text of the thrown error. If [name] is omitted, it defaults to `"index"`.

If [length] is provided, it is used as the length of the indexable object, otherwise the length is found as `indexable.length`.

Returns [index] if it is a valid index.

### checkValidRange()

```dart
int checkValidRange(int start, int? end, int length, [String? startName, String? endName, String? message])
```

Check that a range represents a slice of an indexable object.

Throws if the range is not valid for an indexable object with the given [length]. A range is valid for an indexable object with a given [length]

if `0 <= [start] <= [end] <= [length]`. An `end` of `null` is considered equivalent to `length`.

The [startName] and [endName] defaults to `"start"` and `"end"`, respectively.

Returns the actual `end` value, which is `length` if `end` is `null`, and `end` otherwise.

### checkNotNegative()

```dart
int checkNotNegative(int value, [String? name, String? message])
```

Check that an integer value is non-negative.

Throws if the value is negative.

If [name] or [message] are provided, they are used as the parameter name and message text of the thrown error. If [name] is omitted, it defaults to `index`.

Returns [value] if it is not negative.

# IndexError

```dart
class IndexError extends ArgumentError implements RangeError {}
```

A specialized [RangeError] used when an index is not in the range `0..indexable.length-1`.

Also contains the indexable object, its length at the time of the error, and the invalid index itself.

### indexable

```dart
Object? indexable
```

The indexable object that [invalidValue] was not a valid index into.

Can be, for example, a [List] or [String], which both have index based operations.

### length

```dart
int length
```

The length of [indexable] at the time of the error.

### invalidValue

```dart
int get invalidValue
```

### IndexError()

```dart
IndexError(int invalidValue, dynamic indexable, [String? name, String? message, int? length])
```

Creates a new [IndexError] stating that [invalidValue] is not a valid index into [indexable].

The [length] is the length of [indexable] at the time of the error. If `length` is omitted, it defaults to `indexable.length`.

The message is used as part of the string representation of the error.

### IndexError.withLength()

```dart
IndexError.withLength(int invalidValue, int length, {Object? indexable, String? name, String? message})
```

Creates a new [IndexError] stating that [invalidValue] is not a valid index into [indexable].

The [length] is the length of [indexable] at the time of the error.

The message is used as part of the string representation of the error.

### check()

```dart
int check(int index, int length, {Object? indexable, String? name, String? message})
```

Check that [index] is a valid index into an indexable object.

Throws if [index] is not a valid index.

An indexable object is one that has a `length` and an index-operator `[]` that accepts an index if `0 <= index < length`.

The [length] is the length of the indexable object.

The [indexable], if provided, is the indexable object.

The [name] is the parameter name of the index value. Defaults to "index", and can be set to null to omit a name from the error string, if the invalid index was not a parameter.

The [message], if provided, is included in the error string.

Returns [index] if it is a valid index.

### start

```dart
int get start
```

### end

```dart
int get end
```

# NoSuchMethodError

```dart
class NoSuchMethodError extends Error {}
```

Error thrown on an invalid function or method invocation.

Thrown when a dynamic function or method call provides an invalid type argument or argument list to the function being called. For non-dynamic invocations, static type checking prevents such invalid arguments.

Also thrown by the default implementation of [Object.noSuchMethod].

### NoSuchMethodError.withInvocation()

```dart
NoSuchMethodError.withInvocation(Object? receiver, Invocation invocation)
```

Creates a [NoSuchMethodError] corresponding to a failed method call.

The [receiver] is the receiver of the method call. That is, the object on which the method was attempted called.

The [invocation] represents the method call that failed. It should not be `null`.

### toString()

```dart
String toString()
```

# UnsupportedError

```dart
class UnsupportedError extends Error {}
```

The operation was not allowed by the object.

This [Error] is thrown when an instance cannot implement one of the methods in its signature. For example, it's used by unmodifiable versions of collections, when someone calls a modifying method.

### message

```dart
String? message
```

### UnsupportedError()

```dart
UnsupportedError(String? message)
```

### toString()

```dart
String toString()
```

# UnimplementedError

```dart
class UnimplementedError extends Error implements UnsupportedError {}
```

Thrown by operations that have not been implemented yet.

This [Error] is thrown by unfinished code that hasn't yet implemented all the features it needs.

If the class does not intend to implement the feature, it should throw an [UnsupportedError] instead. This error is only intended for use during development.

### message

```dart
String? message
```

### UnimplementedError()

```dart
UnimplementedError([String? message])
```

### toString()

```dart
String toString()
```

# StateError

```dart
class StateError extends Error {}
```

The operation was not allowed by the current state of the object.

Should be used when this particular object is currently in a state which doesn't support the requested operation, but other similar objects might, or the object itself can later change its state to one which supports the operation.

Example: Asking for `list.first` on a currently empty list. If the operation is never supported by this object or class, consider using [UnsupportedError] instead.

This is a generic error used for a variety of different erroneous actions. The message should be descriptive.

### message

```dart
String message
```

### StateError()

```dart
StateError(String message)
```

### toString()

```dart
String toString()
```

# ConcurrentModificationError

```dart
class ConcurrentModificationError extends Error {}
```

Error occurring when a collection is modified during iteration.

Some modifications may be allowed for some collections, so each collection ([Iterable] or similar collection of values) should declare which operations are allowed during an iteration.

### modifiedObject

```dart
Object? modifiedObject
```

The object that was modified in an incompatible way.

### ConcurrentModificationError()

```dart
ConcurrentModificationError([Object? modifiedObject])
```

### toString()

```dart
String toString()
```

# OutOfMemoryError

```dart
final class OutOfMemoryError implements Error {}
```

Error that the platform can use in case of memory shortage.

### OutOfMemoryError()

```dart
OutOfMemoryError()
```

### toString()

```dart
String toString()
```

### stackTrace

```dart
StackTrace? get stackTrace
```

# StackOverflowError

```dart
final class StackOverflowError implements Error {}
```

Error that the platform can use in case of stack overflow.

### StackOverflowError()

```dart
StackOverflowError()
```

### toString()

```dart
String toString()
```

### stackTrace

```dart
StackTrace? get stackTrace
```
