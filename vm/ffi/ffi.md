Foreign Function Interface for interoperability with the C programming language.

For further details, please see: https://dart.dev/server/c-interop.

{@category VM}

# sizeOf()

```dart
int sizeOf<T extends SizedNativeType>()
```

Number of bytes used by native type T.

Includes padding and alignment of structs.

This function must be invoked with a compile-time constant [T].

# nullptr

```dart
Pointer<Never> nullptr
```

Represents a pointer into the native C memory corresponding to 'NULL', e.g. a pointer with address 0.

# Pointer

```dart
final class Pointer<T extends NativeType> implements SizedNativeType {}
```

Represents a pointer into the native C memory. Cannot be extended.

### Pointer.fromAddress()

```dart
Pointer.fromAddress(int ptr)
```

Construction from raw integer.

### fromFunction()

```dart
Pointer<NativeFunction<T>> fromFunction<T extends Function>(Function f, [Object? exceptionalReturn])
```

Convert Dart function to a C function pointer, automatically marshalling the arguments and return value

If an exception is thrown while calling `f()`, the native function will return `exceptionalReturn`, which must be assignable to return type of `f`.

The returned function address can only be invoked on the mutator (main) thread of the current isolate. It will abort the process if invoked on any other thread. Use [NativeCallable.listener] to create callbacks that can be invoked from any thread.

The pointer returned will remain alive for the duration of the current isolate's lifetime. After the isolate it was created in is terminated, invoking it from native code will cause undefined behavior.

[Pointer.fromFunction] only accepts static or top level functions. Use [NativeCallable.isolateLocal] to create callbacks from any Dart function or closure.

### address

```dart
int get address
```

Access to the raw pointer value. On 32-bit systems, the upper 32-bits of the result are 0.

### cast()

```dart
Pointer<U> cast<U extends NativeType>()
```

Reinterprets the address of this pointer as the address of a [U].

This function is completely unchecked, no attempts are made to validate that the bytes at the address are actually a valid native value of type [U].

### operator ==()

```dart
bool operator ==(Object other)
```

Equality for Pointers only depends on their address.

### hashCode

```dart
int get hashCode
```

The hash code for a Pointer only depends on its address.

# Array

```dart
final class Array<T extends NativeType> extends _Compound {}
```

A fixed-sized array of [T]s.

### Array()

```dart
Array(int dimension1, [int dimension2, int dimension3, int dimension4, int dimension5])
```

Annotation to specify [Array] dimensions in [Struct]s.

```dart
final class MyStruct extends Struct {
  @Array(8)
  external Array<Uint8> inlineArray;

  @Array(2, 2, 2)
  external Array<Array<Array<Uint8>>> threeDimensionalInlineArray;
}
```

Do not invoke in normal code.

### Array.multi()

```dart
Array.multi(List<int> dimensions)
```

Annotation to specify [Array] dimensions in [Struct]s.

```dart
final class MyStruct extends Struct {
  @Array.multi([2, 2, 2])
  external Array<Array<Array<Uint8>>> threeDimensionalInlineArray;

  @Array.multi([2, 2, 2, 2, 2, 2, 2, 2])
  external Array<Array<Array<Array<Array<Array<Array<Array<Uint8>>>>>>>> eightDimensionalInlineArray;
}
```

Do not invoke in normal code.

### Array.variable()

```dart
Array.variable([int dimension2, int dimension3, int dimension4, int dimension5])
```

Annotation to specify a variable length [Array] in [Struct]s.

Can only be used on the last field of a struct. The last field of the struct is _not_ taken into account in [sizeOf]. Using an [AllocatorAlloc.call] will _not_ allocate any backing storage for the variable length array. Instead use [Allocator.allocate] and calculate the required number of bytes manually.

```dart
import 'dart:ffi';
import 'package:ffi/ffi.dart';

final class MyStruct extends Struct {
  @Size()
  external int length;

  @Array.variable()
  external Array<Uint8> inlineArray;

  static Pointer<MyStruct> allocate(Allocator allocator, int length) {
    final lengthInBytes = sizeOf<MyStruct>() + sizeOf<Uint8>() * length;
    final result = allocator.allocate<MyStruct>(lengthInBytes);
    result.ref.length = length;
    return result;
  }
}

void main() {
  final myStruct = MyStruct.allocate(calloc, 10);
}
```

The variable length is always the outermost dimension of the array.

```dart
import 'dart:ffi';
import 'package:ffi/ffi.dart';

final class MyStruct extends Struct {
  @Size()
  external int length;

  @Array.variable(10, 10)
  external Array<Array<Array<Uint8>>> inlineArray;

  static Pointer<MyStruct> allocate(Allocator allocator, int length) {
    final lengthInBytes = sizeOf<MyStruct>() + sizeOf<Uint8>() * length * 100;
    final result = allocator.allocate<MyStruct>(lengthInBytes);
    result.ref.length = length;
    return result;
  }
}
```

Accessing variable length inline arrays of structs passed by value in FFI calls and callbacks is undefined behavior. Accessing variable length inline arrays in structs passed by value is undefined behavior in C.

For more information about variable length inline arrays in C, please refer to: https://gcc.gnu.org/onlinedocs/gcc/Zero-Length.html.

Do not invoke in normal code.

### Array.variableWithVariableDimension()

```dart
Array.variableWithVariableDimension([int dimension1, int dimension2, int dimension3, int dimension4, int dimension5])
```

Annotation to specify a variable length [Array] with a configurable variable dimension ([dimension1]) in [Struct]s.

Can only be used on the last field of a struct. When [dimension1] is set to a value greater than zero (`0`), the last field of the struct is taken into account in [sizeOf] and [AllocatorAlloc.call]. This is particularly useful when working with Windows APIs, where most structs with variable length arrays are defined to have an initial dimension of one (`1`).

```dart
import 'dart:ffi';
import 'package:ffi/ffi.dart';

final class MyStruct extends Struct {
  @Size()
  external int length;

  @Array.variableWithVariableDimension(1)
  external Array<Uint8> inlineArray;

  static Pointer<MyStruct> allocate(Allocator allocator, int length) {
    final lengthInBytes = sizeOf<MyStruct>() + sizeOf<Uint8>() * (length - 1);
    final result = allocator.allocate<MyStruct>(lengthInBytes);
    result.ref.length = length;
    return result;
  }
}

void main() {
  final myStruct = MyStruct.allocate(calloc, 10);
}
```

The variable length is always the outermost dimension of the array.

Do not invoke in normal code.

### Array.variableMulti()

```dart
Array.variableMulti(List<int> dimensions, {int variableDimension})
```

Annotation to a variable length [Array] in [Struct]s.

```dart
final class MyStruct extends Struct {
  @Array.variableMulti([2, 2])
  external Array<Array<Array<Uint8>>> threeDimensionalInlineArray;
}

final class MyStruct2 extends Struct {
  @Array.variableMulti(variableDimension: 1, [2, 2, 2])
  external Array<Array<Array<Array<Uint8>>>> fourDimensionalInlineArray;
}

final class MyStruct3 extends Struct {
  @Array.variableMulti([2, 2, 2, 2, 2, 2, 2])
  external Array<Array<Array<Array<Array<Array<Array<Array<Uint8>>>>>>>> eightDimensionalInlineArray;
}
```

The variable length is always the outermost dimension of the array.

[variableDimension] is the outermost dimension of the variable length array (defaults to zero (`0`)). When [variableDimension] is set to a value greater than zero (`0`), the last field of the struct is taken into account in [sizeOf] and [AllocatorAlloc.call].

Do not invoke in normal code.

# NativeFunctionPointer

```dart
extension NativeFunctionPointer<NF extends Function> on Pointer<NativeFunction<NF>> {}
```

Extension on [Pointer] specialized for the type argument [NativeFunction].

### asFunction()

```dart
DF asFunction<@DartRepresentationOf('NF') DF extends Function>({bool isLeaf = false})
```

Convert to Dart function, automatically marshalling the arguments and return value.

[isLeaf] specifies whether the function is a leaf function. Leaf functions are small, short-running, non-blocking functions which are not allowed to call back into Dart or use any Dart VM APIs. Leaf functions are invoked bypassing some of the heavier parts of the standard Dart-to-Native calling sequence which reduces the invocation overhead, making leaf calls faster than non-leaf calls. However, this implies that a thread executing a leaf function can't cooperate with the Dart runtime. A long running or blocking leaf function will delay any operation which requires synchronization between all threads associated with an isolate group until after the leaf function returns. For example, if one isolate in a group is trying to perform a GC and a second isolate is blocked in a leaf call, then the first isolate will have to pause and wait until this leaf call returns.

# NativeCallable

```dart
abstract final class NativeCallable<T extends Function> {}
```

A native callable which listens for calls to a native function.

Creates a native function linked to a Dart function, so that calling the native function will call the Dart function in some way, with the arguments converted to Dart values.

### NativeCallable.isolateLocal()

```dart
NativeCallable.isolateLocal(Function callback, {Object? exceptionalReturn})
```

Constructs a [NativeCallable] that must be invoked from the same thread that created it.

If an exception is thrown by the [callback], the native function will return the `exceptionalReturn`, which must be assignable to the return type of the [callback].

The returned function address can only be invoked on the mutator (main) thread of the current isolate. It will abort the process if invoked on any other thread. Use [NativeCallable.listener] to create callbacks that can be invoked from any thread.

Unlike [Pointer.fromFunction], [NativeCallable]s can be constructed from any Dart function or closure, not just static or top level functions.

This callback must be [close]d when it is no longer needed. The [Isolate] that created the callback will be kept alive until [close] is called. After [NativeCallable.close] is called, invoking the [nativeFunction] from native code will cause undefined behavior.

### NativeCallable.isolateGroupBound()

```dart
NativeCallable.isolateGroupBound(Function callback, {Object? exceptionalReturn})
```

Constructs a [NativeCallable] that can be invoked from any thread.

When the native code invokes the function [nativeFunction], the [callback] will be executed within the isolate group of the [Isolate] which originally constructed the callable. Specifically, this means that an attempt to access any static or global field which is not shared between isolates in a group will result in a [Error].

If an exception is thrown by the [callback], the native function will return the `exceptionalReturn`, which must be assignable to the return type of the [callback].

[callback] and [exceptionalReturn] must be _trivially shareable_.

This callback must be [close]d when it is no longer needed.

After [NativeCallable.close] is called, invoking the [nativeFunction] from native code will cause undefined behavior.

NOTE: This is an experimental feature and may change in the future.

### NativeCallable.listener()

```dart
NativeCallable.listener(Function callback)
```

Constructs a [NativeCallable] that can be invoked from any thread.

When the native code invokes the function [nativeFunction], the arguments will be sent over a [SendPort] to the [Isolate] that created the [NativeCallable], and the callback will be invoked.

The native code does not wait for a response from the callback, so only functions returning void are supported.

The callback will be invoked at some time in the future. The native caller cannot assume the callback will be run immediately. Resources passed to the callback (such as pointers to malloc'd memory, or output parameters) must be valid until the call completes.

This callback must be [close]d when it is no longer needed. The [Isolate] that created the callback will be kept alive until [close] is called. After [NativeCallable.close] is called, invoking the [nativeFunction] from native code will cause undefined behavior.

For example:

```dart
import 'dart:async';
import 'dart:ffi';
import 'package:ffi/ffi.dart';

// Processes a simple HTTP GET request using a native HTTP library that
// processes the request on a background thread.
Future<String> httpGet(String uri) async {
  final uriPointer = uri.toNativeUtf8();

  // Create the NativeCallable.listener.
  final completer = Completer<String>();
  late final NativeCallable<NativeHttpCallback> callback;
  void onResponse(Pointer<Utf8> responsePointer) {
    completer.complete(responsePointer.toDartString());
    calloc.free(responsePointer);
    calloc.free(uriPointer);

    // Remember to close the NativeCallable once the native API is
    // finished with it, otherwise this isolate will stay alive
    // indefinitely.
    callback.close();
  }
  callback = NativeCallable<NativeHttpCallback>.listener(onResponse);

  // Invoke the native HTTP API. Our example HTTP library processes our
  // request on a background thread, and calls the callback on that same
  // thread when it receives the response.
  nativeHttpGet(uriPointer, callback.nativeFunction);

  return completer.future;
}

// Load the native functions from a DynamicLibrary.
final DynamicLibrary dylib = DynamicLibrary.process();
typedef NativeHttpCallback = Void Function(Pointer<Utf8>);

typedef HttpGetFunction = void Function(
    Pointer<Utf8>, Pointer<NativeFunction<NativeHttpCallback>>);
typedef HttpGetNativeFunction = Void Function(
    Pointer<Utf8>, Pointer<NativeFunction<NativeHttpCallback>>);
final nativeHttpGet =
    dylib.lookupFunction<HttpGetNativeFunction, HttpGetFunction>(
        'http_get');
```

### nativeFunction

```dart
Pointer<NativeFunction<T>> get nativeFunction
```

The native function pointer which can be used to invoke the `callback` passed to the constructor.

This pointer must not be read after the callable has been [close]d.

### close()

```dart
void close()
```

Closes this callback and releases its resources.

Further calls to existing [nativeFunction]s will result in undefined behavior.

Subsequent calls to [close] will be ignored.

It is safe to call [close] inside the [callback].

### keepIsolateAlive

```dart
bool get keepIsolateAlive
```

Whether this [NativeCallable] keeps its [Isolate] alive.

By default, [NativeCallable]s keep the [Isolate] that created them alive until [close] is called. If [keepIsolateAlive] is set to `false`, the isolate may exit even if the [NativeCallable] isn't closed.

Note: IsolateGroupBound-callbacks are not associated with an [Isolate], so won't keep an isolate alive. Attempts to set [keepIsolateAlive] to true, will throw.

### keepIsolateAlive

```dart
set keepIsolateAlive(bool value)
```

# Int8Pointer

```dart
extension Int8Pointer on Pointer<Int8> {}
```

Extension on [Pointer] specialized for the type argument [Int8].

### value

```dart
int get value
```

The 8-bit two's complement integer at [address].

A Dart integer is truncated to 8 bits (as if by `.toSigned(8)`) before being stored, and the 8-bit value is sign-extended when it is loaded.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 8-bit two's complement integer at `address + sizeOf<Int8>() * index`.

A Dart integer is truncated to 8 bits (as if by `.toSigned(8)`) before being stored, and the 8-bit value is sign-extended when it is loaded.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 8-bit two's complement integer at `address + sizeOf<Int8>() * index`.

A Dart integer is truncated to 8 bits (as if by `.toSigned(8)`) before being stored, and the 8-bit value is sign-extended when it is loaded.

### elementAt()

```dart
Pointer<Int8> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Int8> operator +(int offset)
```

A pointer to the [offset]th [Int8] after this one.

Returns a pointer to the [Int8] whose address is [offset] times the size of `Int8` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Int8>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Int8> operator -(int offset)
```

A pointer to the [offset]th [Int8] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Int8] whose address is [offset] times the size of `Int8` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Int8>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Int8List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Int8>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

# Int16Pointer

```dart
extension Int16Pointer on Pointer<Int16> {}
```

Extension on [Pointer] specialized for the type argument [Int16].

### value

```dart
int get value
```

The 16-bit two's complement integer at [address].

A Dart integer is truncated to 16 bits (as if by `.toSigned(16)`) before being stored, and the 16-bit value is sign-extended when it is loaded.

The [address] must be 2-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 16-bit two's complement integer at `address + sizeOf<Int16>() * index`.

A Dart integer is truncated to 16 bits (as if by `.toSigned(16)`) before being stored, and the 16-bit value is sign-extended when it is loaded.

The [address] must be 2-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 16-bit two's complement integer at `address + sizeOf<Int16>() * index`.

A Dart integer is truncated to 16 bits (as if by `.toSigned(16)`) before being stored, and the 16-bit value is sign-extended when it is loaded.

The [address] must be 2-byte aligned.

### elementAt()

```dart
Pointer<Int16> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Int16> operator +(int offset)
```

A pointer to the [offset]th [Int16] after this one.

Returns a pointer to the [Int16] whose address is [offset] times the size of `Int16` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Int16>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Int16> operator -(int offset)
```

A pointer to the [offset]th [Int16] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Int16] whose address is [offset] times the size of `Int16` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Int16>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Int16List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Int16>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 2-byte aligned.

# Int32Pointer

```dart
extension Int32Pointer on Pointer<Int32> {}
```

Extension on [Pointer] specialized for the type argument [Int32].

### value

```dart
int get value
```

The 32-bit two's complement integer at [address].

A Dart integer is truncated to 32 bits (as if by `.toSigned(32)`) before being stored, and the 32-bit value is sign-extended when it is loaded.

The [address] must be 4-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 32-bit two's complement integer at `address + sizeOf<Int32>() * index`.

A Dart integer is truncated to 32 bits (as if by `.toSigned(32)`) before being stored, and the 32-bit value is sign-extended when it is loaded.

The [address] must be 4-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 32-bit two's complement integer at `address + sizeOf<Int32>() * index`.

A Dart integer is truncated to 32 bits (as if by `.toSigned(32)`) before being stored, and the 32-bit value is sign-extended when it is loaded.

The [address] must be 4-byte aligned.

### elementAt()

```dart
Pointer<Int32> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Int32> operator +(int offset)
```

A pointer to the [offset]th [Int32] after this one.

Returns a pointer to the [Int32] whose address is [offset] times the size of `Int32` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Int32>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Int32> operator -(int offset)
```

A pointer to the [offset]th [Int32] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Int32] whose address is [offset] times the size of `Int32` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Int32>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Int32List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Int32>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 4-byte aligned.

# Int64Pointer

```dart
extension Int64Pointer on Pointer<Int64> {}
```

Extension on [Pointer] specialized for the type argument [Int64].

### value

```dart
int get value
```

The 64-bit two's complement integer at [address].

The [address] must be 8-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 64-bit two's complement integer at `address + sizeOf<Int64>() * index`.

The [address] must be 8-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 64-bit two's complement integer at `address + sizeOf<Int64>() * index`.

The [address] must be 8-byte aligned.

### elementAt()

```dart
Pointer<Int64> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Int64> operator +(int offset)
```

A pointer to the [offset]th [Int64] after this one.

Returns a pointer to the [Int64] whose address is [offset] times the size of `Int64` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Int64>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Int64> operator -(int offset)
```

A pointer to the [offset]th [Int64] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Int64] whose address is [offset] times the size of `Int64` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Int64>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Int64List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Int64>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 8-byte aligned.

# Uint8Pointer

```dart
extension Uint8Pointer on Pointer<Uint8> {}
```

Extension on [Pointer] specialized for the type argument [Uint8].

### value

```dart
int get value
```

The 8-bit unsigned integer at [address].

A Dart integer is truncated to 8 bits (as if by `.toUnsigned(8)`) before being stored, and the 8-bit value is zero-extended when it is loaded.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 8-bit unsigned integer at `address + sizeOf<Uint8>() * index`.

A Dart integer is truncated to 8 bits (as if by `.toUnsigned(8)`) before being stored, and the 8-bit value is zero-extended when it is loaded.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 8-bit unsigned integer at `address + sizeOf<Uint8>() * index`.

A Dart integer is truncated to 8 bits (as if by `.toUnsigned(8)`) before being stored, and the 8-bit value is zero-extended when it is loaded.

### elementAt()

```dart
Pointer<Uint8> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Uint8> operator +(int offset)
```

A pointer to the [offset]th [Uint8] after this one.

Returns a pointer to the [Uint8] whose address is [offset] times the size of `Uint8` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Uint8>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Uint8> operator -(int offset)
```

A pointer to the [offset]th [Uint8] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Uint8] whose address is [offset] times the size of `Uint8` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Uint8>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Uint8List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Uint8>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

# Uint16Pointer

```dart
extension Uint16Pointer on Pointer<Uint16> {}
```

Extension on [Pointer] specialized for the type argument [Uint16].

### value

```dart
int get value
```

The 16-bit unsigned integer at [address].

A Dart integer is truncated to 16 bits (as if by `.toUnsigned(16)`) before being stored, and the 16-bit value is zero-extended when it is loaded.

The [address] must be 2-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 16-bit unsigned integer at `address + sizeOf<Uint16>() * index`.

A Dart integer is truncated to 16 bits (as if by `.toUnsigned(16)`) before being stored, and the 16-bit value is zero-extended when it is loaded.

The [address] must be 2-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 16-bit unsigned integer at `address + sizeOf<Uint16>() * index`.

A Dart integer is truncated to 16 bits (as if by `.toUnsigned(16)`) before being stored, and the 16-bit value is zero-extended when it is loaded.

The [address] must be 2-byte aligned.

### elementAt()

```dart
Pointer<Uint16> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Uint16> operator +(int offset)
```

A pointer to the [offset]th [Uint16] after this one.

Returns a pointer to the [Uint16] whose address is [offset] times the size of `Uint16` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Uint16>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Uint16> operator -(int offset)
```

A pointer to the [offset]th [Uint16] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Uint16] whose address is [offset] times the size of `Uint16` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Uint16>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Uint16List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Uint16>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 2-byte aligned.

# Uint32Pointer

```dart
extension Uint32Pointer on Pointer<Uint32> {}
```

Extension on [Pointer] specialized for the type argument [Uint32].

### value

```dart
int get value
```

The 32-bit unsigned integer at [address].

A Dart integer is truncated to 32 bits (as if by `.toUnsigned(32)`) before being stored, and the 32-bit value is zero-extended when it is loaded.

The [address] must be 4-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 32-bit unsigned integer at `address + sizeOf<Uint32>() * index`.

A Dart integer is truncated to 32 bits (as if by `.toUnsigned(32)`) before being stored, and the 32-bit value is zero-extended when it is loaded.

The [address] must be 4-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 32-bit unsigned integer at `address + sizeOf<Uint32>() * index`.

A Dart integer is truncated to 32 bits (as if by `.toUnsigned(32)`) before being stored, and the 32-bit value is zero-extended when it is loaded.

The [address] must be 4-byte aligned.

### elementAt()

```dart
Pointer<Uint32> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Uint32> operator +(int offset)
```

A pointer to the [offset]th [Uint32] after this one.

Returns a pointer to the [Uint32] whose address is [offset] times the size of `Uint32` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Uint32>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Uint32> operator -(int offset)
```

A pointer to the [offset]th [Uint32] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Uint32] whose address is [offset] times the size of `Uint32` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Uint32>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Uint32List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Uint32>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 4-byte aligned.

# Uint64Pointer

```dart
extension Uint64Pointer on Pointer<Uint64> {}
```

Extension on [Pointer] specialized for the type argument [Uint64].

### value

```dart
int get value
```

The 64-bit unsigned integer at [address].

The [address] must be 8-byte aligned.

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The 64-bit unsigned integer at `address + sizeOf<Uint64>() * index`.

The [address] must be 8-byte aligned.

### operator []=()

```dart
void operator []=(int index, int value)
```

The 64-bit unsigned integer at `address + sizeOf<Uint64>() * index`.

The [address] must be 8-byte aligned.

### elementAt()

```dart
Pointer<Uint64> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Uint64> operator +(int offset)
```

A pointer to the [offset]th [Uint64] after this one.

Returns a pointer to the [Uint64] whose address is [offset] times the size of `Uint64` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Uint64>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Uint64> operator -(int offset)
```

A pointer to the [offset]th [Uint64] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Uint64] whose address is [offset] times the size of `Uint64` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Uint64>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Uint64List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Uint64>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 8-byte aligned.

# FloatPointer

```dart
extension FloatPointer on Pointer<Float> {}
```

Extension on [Pointer] specialized for the type argument [Float].

### value

```dart
double get value
```

The float at [address].

A Dart double loses precision before being stored, and the float value is converted to a double when it is loaded.

The [address] must be 4-byte aligned.

### value

```dart
void set value(double value)
```

### operator []()

```dart
double operator [](int index)
```

The float at `address + sizeOf<Float>() * index`.

A Dart double loses precision before being stored, and the float value is converted to a double when it is loaded.

The [address] must be 4-byte aligned.

### operator []=()

```dart
void operator []=(int index, double value)
```

The float at `address + sizeOf<Float>() * index`.

A Dart double loses precision before being stored, and the float value is converted to a double when it is loaded.

The [address] must be 4-byte aligned.

### elementAt()

```dart
Pointer<Float> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Float> operator +(int offset)
```

A pointer to the [offset]th [Float] after this one.

Returns a pointer to the [Float] whose address is [offset] times the size of `Float` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Float>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Float> operator -(int offset)
```

A pointer to the [offset]th [Float] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Float] whose address is [offset] times the size of `Float` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Float>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Float32List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Float>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 4-byte aligned.

# DoublePointer

```dart
extension DoublePointer on Pointer<Double> {}
```

Extension on [Pointer] specialized for the type argument [Double].

### value

```dart
double get value
```

The double at [address].

The [address] must be 8-byte aligned.

### value

```dart
void set value(double value)
```

### operator []()

```dart
double operator [](int index)
```

The double at `address + sizeOf<Double>() * index`.

The [address] must be 8-byte aligned.

### operator []=()

```dart
void operator []=(int index, double value)
```

The double at `address + sizeOf<Double>() * index`.

The [address] must be 8-byte aligned.

### elementAt()

```dart
Pointer<Double> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Double> operator +(int offset)
```

A pointer to the [offset]th [Double] after this one.

Returns a pointer to the [Double] whose address is [offset] times the size of `Double` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Double>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Double> operator -(int offset)
```

A pointer to the [offset]th [Double] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Double] whose address is [offset] times the size of `Double` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Double>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

### asTypedList()

```dart
Float64List asTypedList(int length, {Pointer<NativeFinalizerFunction>? finalizer, Pointer<Void>? token})
```

Creates a typed list view backed by memory in the address space.

The returned view will allow access to the memory range from [address] to `address + sizeOf<Double>() * length`.

The user has to ensure the memory range is accessible while using the returned list.

If provided, [finalizer] will be run on the pointer once the typed list is GCed. If provided, [token] will be passed to [finalizer], otherwise the this pointer itself will be passed.

The [address] must be 8-byte aligned.

# BoolPointer

```dart
extension BoolPointer on Pointer<Bool> {}
```

Extension on [Pointer] specialized for the type argument [Bool].

### value

```dart
bool get value
```

The bool at [address].

### value

```dart
void set value(bool value)
```

### operator []()

```dart
bool operator [](int index)
```

The bool at `address + sizeOf<Bool>() * index`.

### operator []=()

```dart
void operator []=(int index, bool value)
```

The bool at `address + sizeOf<Bool>() * index`.

### elementAt()

```dart
Pointer<Bool> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Bool> operator +(int offset)
```

A pointer to the [offset]th [Bool] after this one.

Returns a pointer to the [Bool] whose address is [offset] times the size of `Bool` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Bool>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Bool> operator -(int offset)
```

A pointer to the [offset]th [Bool] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Bool] whose address is [offset] times the size of `Bool` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Bool>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

# Int8Array

```dart
extension Int8Array on Array<Int8> {}
```

Bounds checking indexing methods on [Array]s of [Int8].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Int8List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Int16Array

```dart
extension Int16Array on Array<Int16> {}
```

Bounds checking indexing methods on [Array]s of [Int16].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Int16List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Int32Array

```dart
extension Int32Array on Array<Int32> {}
```

Bounds checking indexing methods on [Array]s of [Int32].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Int32List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Int64Array

```dart
extension Int64Array on Array<Int64> {}
```

Bounds checking indexing methods on [Array]s of [Int64].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Int64List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Uint8Array

```dart
extension Uint8Array on Array<Uint8> {}
```

Bounds checking indexing methods on [Array]s of [Uint8].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Uint8List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Uint16Array

```dart
extension Uint16Array on Array<Uint16> {}
```

Bounds checking indexing methods on [Array]s of [Uint16].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Uint16List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Uint32Array

```dart
extension Uint32Array on Array<Uint32> {}
```

Bounds checking indexing methods on [Array]s of [Uint32].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Uint32List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Uint64Array

```dart
extension Uint64Array on Array<Uint64> {}
```

Bounds checking indexing methods on [Array]s of [Uint64].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Uint64List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# FloatArray

```dart
extension FloatArray on Array<Float> {}
```

Bounds checking indexing methods on [Array]s of [Float].

### operator []()

```dart
double operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, double value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Float32List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# DoubleArray

```dart
extension DoubleArray on Array<Double> {}
```

Bounds checking indexing methods on [Array]s of [Double].

### operator []()

```dart
double operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, double value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
Float64List get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# BoolArray

```dart
extension BoolArray on Array<Bool> {}
```

Bounds checking indexing methods on [Array]s of [Bool].

### operator []()

```dart
bool operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, bool value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
List<bool> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# Int8ListAddress

```dart
extension Int8ListAddress on Int8List {}
```

### address

```dart
Pointer<Int8> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Int8>)>(isLeaf: true)
external void myFunction(Pointer<Int8> pointer);

void main() {
  final list = Int8List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Int16ListAddress

```dart
extension Int16ListAddress on Int16List {}
```

### address

```dart
Pointer<Int16> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Int16>)>(isLeaf: true)
external void myFunction(Pointer<Int16> pointer);

void main() {
  final list = Int16List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Int32ListAddress

```dart
extension Int32ListAddress on Int32List {}
```

### address

```dart
Pointer<Int32> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Int32>)>(isLeaf: true)
external void myFunction(Pointer<Int32> pointer);

void main() {
  final list = Int32List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Int64ListAddress

```dart
extension Int64ListAddress on Int64List {}
```

### address

```dart
Pointer<Int64> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Int64>)>(isLeaf: true)
external void myFunction(Pointer<Int64> pointer);

void main() {
  final list = Int64List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Uint8ListAddress

```dart
extension Uint8ListAddress on Uint8List {}
```

### address

```dart
Pointer<Uint8> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Uint8>)>(isLeaf: true)
external void myFunction(Pointer<Uint8> pointer);

void main() {
  final list = Uint8List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Uint16ListAddress

```dart
extension Uint16ListAddress on Uint16List {}
```

### address

```dart
Pointer<Uint16> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Uint16>)>(isLeaf: true)
external void myFunction(Pointer<Uint16> pointer);

void main() {
  final list = Uint16List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Uint32ListAddress

```dart
extension Uint32ListAddress on Uint32List {}
```

### address

```dart
Pointer<Uint32> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Uint32>)>(isLeaf: true)
external void myFunction(Pointer<Uint32> pointer);

void main() {
  final list = Uint32List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Uint64ListAddress

```dart
extension Uint64ListAddress on Uint64List {}
```

### address

```dart
Pointer<Uint64> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Uint64>)>(isLeaf: true)
external void myFunction(Pointer<Uint64> pointer);

void main() {
  final list = Uint64List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Float32ListAddress

```dart
extension Float32ListAddress on Float32List {}
```

### address

```dart
Pointer<Float> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Float>)>(isLeaf: true)
external void myFunction(Pointer<Float> pointer);

void main() {
  final list = Float32List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# Float64ListAddress

```dart
extension Float64ListAddress on Float64List {}
```

### address

```dart
Pointer<Double> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Double>)>(isLeaf: true)
external void myFunction(Pointer<Double> pointer);

void main() {
  final list = Float64List(10);
  myFunction(list.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# PointerPointer

```dart
extension PointerPointer<T extends NativeType> on Pointer<Pointer<T>> {}
```

Extension on [Pointer] specialized for the type argument [Pointer].

### value

```dart
Pointer<T> get value
```

The pointer at [address].

A [Pointer] is unboxed before being stored (as if by `.address`), and the pointer is boxed (as if by `Pointer.fromAddress`) when loaded.

On 32-bit platforms the [address] must be 4-byte aligned, and on 64-bit platforms the [address] must be 8-byte aligned.

### value

```dart
void set value(Pointer<T> value)
```

### operator []()

```dart
Pointer<T> operator [](int index)
```

Load a Dart value from this location offset by [index].

A [Pointer] is unboxed before being stored (as if by `.address`), and the pointer is boxed (as if by `Pointer.fromAddress`) when loaded.

On 32-bit platforms the [address] must be 4-byte aligned, and on 64-bit platforms the [address] must be 8-byte aligned.

### operator []=()

```dart
void operator []=(int index, Pointer<T> value)
```

Store a Dart value into this location offset by [index].

A [Pointer] is unboxed before being stored (as if by `.address`), and the pointer is boxed (as if by [Pointer.fromAddress]) when loaded.

On 32-bit platforms the [address] must be 4-byte aligned, and on 64-bit platforms the [address] must be 8-byte aligned.

### elementAt()

```dart
Pointer<Pointer<T>> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<Pointer<T>> operator +(int offset)
```

A pointer to the [offset]th [Pointer<T>] after this one.

Returns a pointer to the [Pointer<T>] whose address is [offset] times the size of `Pointer<T>` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<Pointer<T>>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<Pointer<T>> operator -(int offset)
```

A pointer to the [offset]th [Pointer<T>] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [Pointer<T>] whose address is [offset] times the size of `Pointer<T>` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<Pointer<T>>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

# StructPointer

```dart
extension StructPointer<T extends Struct> on Pointer<T> {}
```

Extension on [Pointer] specialized for the type argument [Struct].

### ref

```dart
T get ref
```

A Dart view of the struct referenced by this pointer.

Reading [ref] creates a reference accessing the fields of this struct backed by native memory at [address]. The [address] must be aligned according to the struct alignment rules of the platform.

Assigning to [ref] copies contents of the struct into the native memory starting at [address].

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### ref

```dart
set ref(T value)
```

### refWithFinalizer()

```dart
T refWithFinalizer(Pointer<NativeFinalizerFunction> finalizer, {Pointer<Void>? token})
```

A Dart view of the struct referenced by this pointer.

Creates a reference accessing the fields of this struct backed by native memory at [address]. The [address] must be aligned according to the struct alignment rules of the platform.

Attaches [finalizer] to the backing store of the struct. If provided, [token] will be passed to [finalizer], otherwise the pointer (`this`) itself will be passed. The pointer (`this`) must _not_ be used anymore if the struct is _not_ guaranteed to be kept alive. Prefer doing any native calls with the pointer _before_ calling [refWithFinalizer].

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### operator []()

```dart
T operator [](int index)
```

Creates a reference to access the fields of this struct backed by native memory at `address + sizeOf<T>() * index`.

The [address] must be aligned according to the struct alignment rules of the platform.

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, T value)
```

Copies the [value] struct into native memory, starting at `address * sizeOf<T>() * index`.

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### elementAt()

```dart
Pointer<T> elementAt(int index)
```

Pointer arithmetic (takes element size into account)

### operator +()

```dart
Pointer<T> operator +(int offset)
```

A pointer to the [offset]th [T] after this one.

Returns a pointer to the [T] whose address is [offset] times the size of `T` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<T>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<T> operator -(int offset)
```

A pointer to the [offset]th [T] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [T] whose address is [offset] times the size of `T` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<T>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

# UnionPointer

```dart
extension UnionPointer<T extends Union> on Pointer<T> {}
```

Extension on [Pointer] specialized for the type argument [Union].

### ref

```dart
T get ref
```

A Dart view of the union referenced by this pointer.

Reading [ref] creates a reference accessing the fields of this union backed by native memory at [address]. The [address] must be aligned according to the union alignment rules of the platform.

Assigning to [ref] copies contents of the union into the native memory starting at [address].

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### ref

```dart
set ref(T value)
```

### refWithFinalizer()

```dart
T refWithFinalizer(Pointer<NativeFinalizerFunction> finalizer, {Pointer<Void>? token})
```

A Dart view of the union referenced by this pointer.

Creates a reference accessing the fields of this union backed by native memory at [address]. The [address] must be aligned according to the union alignment rules of the platform.

Attaches [finalizer] to the backing store of the union. If provided, [token] will be passed to [finalizer], otherwise the pointer (`this`) itself will be passed. The pointer (`this`) must _not_ be used anymore if the union is _not_ guaranteed to be kept alive. Prefer doing any native calls with the pointer _before_ calling [refWithFinalizer].

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### operator []()

```dart
T operator [](int index)
```

Creates a reference to access the fields of this union backed by native memory at `address + sizeOf<T>() * index`.

The [address] must be aligned according to the union alignment rules of the platform.

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, T value)
```

Copies the [value] union into native memory, starting at `address * sizeOf<T>() * index`.

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### elementAt()

```dart
Pointer<T> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<T> operator +(int offset)
```

A pointer to the [offset]th [T] after this one.

Returns a pointer to the [T] whose address is [offset] times the size of `T` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<T>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<T> operator -(int offset)
```

A pointer to the [offset]th [T] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [T] whose address is [offset] times the size of `T` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<T>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

# AbiSpecificIntegerPointer

```dart
extension AbiSpecificIntegerPointer<T extends AbiSpecificInteger> on Pointer<T> {}
```

Extension on [Pointer] specialized for the type argument [AbiSpecificInteger].

### value

```dart
int get value
```

The integer at [address].

### value

```dart
void set value(int value)
```

### operator []()

```dart
int operator [](int index)
```

The integer at `address + sizeOf<T>() * index`.

### operator []=()

```dart
void operator []=(int index, int value)
```

The integer at `address + sizeOf<T>() * index`.

### elementAt()

```dart
Pointer<T> elementAt(int index)
```

Pointer arithmetic (takes element size into account).

### operator +()

```dart
Pointer<T> operator +(int offset)
```

A pointer to the [offset]th [T] after this one.

Returns a pointer to the [T] whose address is [offset] times the size of `T` after the address of this pointer. That is `(this + offset).address == this.address + offset * sizeOf<T>()`.

Also `(this + offset).value` is equivalent to `this[offset]`, and similarly for setting.

### operator -()

```dart
Pointer<T> operator -(int offset)
```

A pointer to the [offset]th [T] before this one.

Equivalent to `this + (-offset)`.

Returns a pointer to the [T] whose address is [offset] times the size of `T` before the address of this pointer. That is, `(this - offset).address == this.address - offset * sizeOf<T>()`.

Also, `(this - offset).value` is equivalent to `this[-offset]`, and similarly for setting,

# PointerArray

```dart
extension PointerArray<T extends NativeType> on Array<Pointer<T>> {}
```

Bounds checking indexing methods on [Array]s of [Pointer].

### operator []()

```dart
Pointer<T> operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, Pointer<T> value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
List<Pointer<T>> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# StructArray

```dart
extension StructArray<T extends Struct> on Array<T> {}
```

Bounds checking indexing methods on [Array]s of [Struct].

### operator []()

```dart
T operator [](int index)
```

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### elements

```dart
List<T> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array.

# UnionArray

```dart
extension UnionArray<T extends Union> on Array<T> {}
```

Bounds checking indexing methods on [Array]s of [Union].

### operator []()

```dart
T operator [](int index)
```

This extension method must be invoked on a receiver of type `Pointer<T>` where `T` is a compile-time constant type.

### elements

```dart
List<T> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array.

# ArrayArray

```dart
extension ArrayArray<T extends NativeType> on Array<Array<T>> {}
```

Bounds checking indexing methods on [Array]s of [Array].

### operator []()

```dart
Array<T> operator [](int index)
```

### operator []=()

```dart
void operator []=(int index, Array<T> value)
```

### elements

```dart
List<Array<T>> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# AbiSpecificIntegerArray

```dart
extension AbiSpecificIntegerArray<T extends AbiSpecificInteger> on Array<T> {}
```

Bounds checking indexing methods on [Array]s of [AbiSpecificInteger].

### operator []()

```dart
int operator [](int index)
```

Loads a Dart value from this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### operator []=()

```dart
void operator []=(int index, int value)
```

Stores a Dart value in this array at [index].

This extension method must be invoked on a receiver of type `Array<T>` where `T` is a compile-time constant type.

### elements

```dart
List<int> get elements
```

A list view of the bytes of this array.

Has the same length and elements (as accessed using the index operator) as this array, and writes to either the list or this arrary are visible in both.

# ArrayAddress

```dart
extension ArrayAddress<T extends NativeType> on Array<T> {}
```

### address

```dart
Pointer<T> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart
@Native<Void Function(Pointer<Int8>)>(isLeaf: true)
external void myFunction(Pointer<Int8> pointer);

final class MyStruct extends Struct {
  @Array(10)
  external Array<Int8> array;
}

void main() {
  final array = Struct.create<MyStruct>().array;
  myFunction(array.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# StructAddress

```dart
extension StructAddress<T extends Struct> on T {}
```

### address

```dart
Pointer<T> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart
@Native<Void Function(Pointer<MyStruct>)>(isLeaf: true)
external void myFunction(Pointer<MyStruct> pointer);

final class MyStruct extends Struct {
  @Int8()
  external int x;
}

void main() {
  final myStruct = Struct.create<MyStruct>();
  myFunction(myStruct.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# UnionAddress

```dart
extension UnionAddress<T extends Union> on T {}
```

### address

```dart
Pointer<T> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Example:

```dart
@Native<Void Function(Pointer<MyUnion>)>(isLeaf: true)
external void myFunction(Pointer<MyUnion> pointer);

final class MyUnion extends Union {
  @Int8()
  external int x;
}

void main() {
  final myUnion = Union.create<MyUnion>();
  myFunction(myUnion.address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# IntAddress

```dart
extension IntAddress on int {}
```

### address

```dart
Pointer<Never> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Can only be used on fields of [Struct] subtypes, fields of [Union] subtypes, [Array] elements, or [TypedData] elements. In other words, the number whose address is being accessed must itself be acccessed through a [Struct], [Union], [Array], or [TypedData].

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Int8>)>(isLeaf: true)
external void myFunction(Pointer<Int8> pointer);

final class MyStruct extends Struct {
  @Int8()
  external int x;

  @Int8()
  external int y;

  @Array(10)
  external Array<Int8> array;
}

void main() {
  final myStruct = Struct.create<MyStruct>();
  myFunction(myStruct.y.address);
  myFunction(myStruct.array[5].address);

  final list = Int8List(10);
  myFunction(list[5].address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# DoubleAddress

```dart
extension DoubleAddress on double {}
```

### address

```dart
Pointer<Never> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Can only be used on fields of [Struct] subtypes, fields of [Union] subtypes, [Array] elements, or [TypedData] elements. In other words, the number whose address is being accessed must itself be acccessed through a [Struct], [Union], [Array], or [TypedData].

Example:

```dart import:typed_data
@Native<Void Function(Pointer<Float>)>(isLeaf: true)
external void myFunction(Pointer<Float> pointer);

final class MyStruct extends Struct {
  @Float()
  external double x;

  @Float()
  external double y;

  @Array(10)
  external Array<Float> array;
}

void main() {
  final myStruct = Struct.create<MyStruct>();
  myFunction(myStruct.y.address);
  myFunction(myStruct.array[5].address);

  final list = Float32List(10);
  myFunction(list[5].address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# BoolAddress

```dart
extension BoolAddress on bool {}
```

### address

```dart
Pointer<Never> get address
```

The memory address of the underlying data.

An expression of the form `expression.address` denoting this `address` can only occurr as an entire argument expression in the invocation of a leaf [Native] external function.

Can only be used on fields of [Struct] subtypes, fields of [Union] subtypes, or [Array] elements. In other words, the boolean whose address is being accessed must itself be acccessed through a [Struct], [Union] or [Array].

Example:

```dart
@Native<Void Function(Pointer<Int8>)>(isLeaf: true)
external void myFunction(Pointer<Int8> pointer);

final class MyStruct extends Struct {
  @Bool()
  external bool x;

  @Bool()
  external bool y;

  @Array(10)
  external Array<Bool> array;
}

void main() {
  final myStruct = Struct.create<MyStruct>();
  myFunction(myStruct.y.address);
  myFunction(myStruct.array[5].address);
}
```

The expression before `.address` is evaluated like the left-hand-side of an assignment, to something that gives access to the storage behind the expression, which can be used both for reading and writing. The `.address` then gives a native pointer to that storage.

The `.address` is evaluated just before calling into native code when invoking a leaf [Native] external function. This ensures the Dart garbage collector will not move the object that the address points in to.

# NativePort

```dart
extension NativePort on SendPort {}
```

Extension to retrieve the native `Dart_Port` from a [SendPort].

### nativePort

```dart
int get nativePort
```

The native port of this [SendPort].

The returned native port can for example be used by C code to post messages to the connected [ReceivePort] via `Dart_PostCObject()` - see `dart_native_api.h`.

Only the send ports from the platform classes [ReceivePort] and [RawReceivePort] are supported. User-defined implementations of [SendPort] are not supported.

# Dart_CObject

```dart
final class Dart_CObject extends Opaque {}
```

Opaque, not exposing it's members.

# Dart_NativeMessageHandler

```dart
typedef Dart_NativeMessageHandler = Void Function(Int64, Pointer<Dart_CObject>)
```

# NativeApi

```dart
abstract final class NativeApi {}
```

Utilities for accessing the Dart VM API from Dart code or from C code via `dart_api_dl.h`.

### majorVersion

```dart
int get majorVersion
```

On breaking changes the major version is increased.

The versioning covers the API surface in `dart_api_dl.h`.

### minorVersion

```dart
int get minorVersion
```

On backwards compatible changes the minor version is increased.

The versioning covers the API surface in `dart_api_dl.h`.

### postCObject

```dart
Pointer<NativeFunction<Int8 Function(Int64, Pointer<Dart_CObject>)>> get postCObject
```

A function pointer to `bool Dart_PostCObject(Dart_Port port_id, Dart_CObject* message)` in `dart_native_api.h`.

### newNativePort

```dart
Pointer<NativeFunction<Int64 Function(Pointer<Uint8>, Pointer<NativeFunction<Dart_NativeMessageHandler>>, Int8)>> get newNativePort
```

A function pointer to

```c
Dart_Port Dart_NewNativePort(const char* name,
                             Dart_NativeMessageHandler handler,
                             bool handle_concurrently)
```

in `dart_native_api.h`.

### closeNativePort

```dart
Pointer<NativeFunction<Int8 Function(Int64)>> get closeNativePort
```

A function pointer to `bool Dart_CloseNativePort(Dart_Port native_port_id)` in `dart_native_api.h`.

### initializeApiDLData

```dart
Pointer<Void> get initializeApiDLData
```

Pass this to `Dart_InitializeApiDL` in your native code to enable using the symbols in `dart_api_dl.h`.

# Native

```dart
final class Native<T> {}
```

Annotation binding an external declaration to its native implementation.

Can only be applied to `external` declarations of static and top-level functions and variables.

A [Native]-annotated `external` function is implemented by native code. The implementation is found in the native library denoted by [assetId]. Similarly, a [Native]-annotated `external` variable is implemented by reading from or writing to native memory.

The compiler and/or runtime provides a binding from [assetId] to native library, which depends on the target platform. The compiler/runtime can then resolve/lookup symbols (identifiers) against the native library, to find a native function or a native global variable, and bind an `external` Dart function or variable declaration to that native declaration. By default, the runtime expects a native symbol with the same name as the annotated function or variable in Dart. This can be overridden with the [symbol] parameter on the annotation.

When used on a function, [T] must be a function type that represents the native function's parameter and return types. The parameter and return types must be subtypes of [NativeType].

When used on a variable, [T] must be a compatible native type. For example, an [int] field can be annotated with [Int32].

If the type argument [T] is omitted in the `@Native` annotation, it is inferred from the static type of the declaration, which must meet the following constraints:

For function or method declarations:

- The return type must be one of the following:
- [Pointer]
- `void`
- Subtype of compound types, such as [Struct] or [Union]
- The parameter types must be subtypes of compound types or [Pointer]

For variable declarations, the type can be any of the following:

- [Pointer]
- Subtype of compound types, such as [Struct] or [Union]

For native global variables that cannot be reassigned, a `final` variable in Dart or a getter can be used to prevent modifications to the native field.

Example:

```dart template:top
@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);

@Native()
external void free(Pointer p);

@Native<Int64>()
external int aGlobalInt;

@Native()
external final Pointer<Char> aGlobalString;
```

Calling a `@Native` function, as well as reading or writing to a `@Native` variable, will try to resolve the [symbol] in (in the order):

1.  the provided or default [assetId],
2.  the native resolver set with `Dart_SetFfiNativeResolver` in `dart_api.h`, and
3.  the current process.

At least one of those three _must_ provide a binding for the symbol, otherwise the method call or the variable access fails.

NOTE: This is an experimental feature and may change in the future.

### symbol

```dart
String? symbol
```

The native symbol to be resolved, if not using the default.

If not specified, the default symbol used for native function lookup is the annotated function's name.

Example:

```dart template:top
@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);
```

Example 2:

```dart template:top
@Native<Int64 Function(Int64, Int64)>(symbol: 'sum')
external int sum(int a, int b);
```

The above two examples are equivalent.

Prefer omitting the [symbol] when possible.

### assetId

```dart
String? assetId
```

The ID of the asset in which [symbol] is resolved, if not using the default.

If no asset name is specified, the default is to use an asset ID specified using an [DefaultAsset] annotation on the current library's `library` declaration, and if there is no [DefaultAsset] annotation on the current library, the library's URI (as a string) is used instead.

Example (file `package:a/a.dart`):

```dart template:top
@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);
```

Example 2 (file `package:a/a.dart`):

```dart template:none
@DefaultAsset('package:a/a.dart')
library a;

import 'dart:ffi';

@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);
```

Example 3 (file `package:a/a.dart`):

```dart template:top
@Native<Int64 Function(Int64, Int64)>(assetId: 'package:a/a.dart')
external int sum(int a, int b);
```

The above three examples are all equivalent.

Prefer using the library URI as an asset name over specifying it. Prefer using an [DefaultAsset] on the `library` declaration over specifying the asset name in a [Native] annotation.

### isLeaf

```dart
bool isLeaf
```

Whether the function is a leaf function.

Leaf functions are small, short-running, non-blocking functions which are not allowed to call back into Dart or use any Dart VM APIs. Leaf functions are invoked bypassing some of the heavier parts of the standard Dart-to-Native calling sequence which reduces the invocation overhead, making leaf calls faster than non-leaf calls. However, this implies that a thread executing a leaf function can't cooperate with the Dart runtime. A long running or blocking leaf function will delay any operation which requires synchronization between all threads associated with an isolate group until after the leaf function returns. For example, if one isolate in a group is trying to perform a GC and a second isolate is blocked in a leaf call, then the first isolate will have to pause and wait until this leaf call returns.

This value has no meaning for native fields.

### Native()

```dart
Native({String? assetId, bool isLeaf = false, String? symbol})
```

### addressOf()

```dart
Pointer<T> addressOf<T extends NativeType>(Object native)
```

The native address of the implementation of [native].

When calling this function, the argument for [native] must be an expression denoting a variable or function declaration which is annotated with [Native]. For a variable declaration, the type [T] must be the same native type as the type argument to that `@Native` annotation. For a function declaration, the type [T] must be `NativeFunction<F>` where `F` was the type argument to that `@Native` annotation.

For example, for a native C library exposing a function:

```C
#include <stdint.h>
int64_t sum(int64_t a, int64_t b) { return a + b; }
```

The following code binds `sum` to a Dart function declaration, and extracts the address of the native `sum` implementation:

```dart
import 'dart:ffi';

typedef NativeAdd = Int64 Function(Int64, Int64);

@Native<NativeAdd>()
external int sum(int a, int b);

void main() {
  Pointer<NativeFunction<NativeAdd>> addressSum = Native.addressOf(sum);
}
```

Similarly, for a native C library exposing a global variable:

```C
const char* myString;
```

The following code binds `myString` to a top-level variable in Dart, and extracts the address of the underlying native field:

```dart
import 'dart:ffi';

@Native()
external Pointer<Char> myString;

void main() {
  // This pointer points to the memory location where the loader has
  // placed the `myString` global itself. To get the string value, read
  // the myString field directly.
  Pointer<Pointer<Char>> addressMyString = Native.addressOf(myString);
}
```

# DefaultAsset

```dart
final class DefaultAsset {}
```

Annotation specifying the default asset ID for the current library.

The annotation applies only to `library` declarations.

The compiler and/or runtime provides a binding from _asset ID_ to native library, which depends on the target platform and architecture. The compiler/runtime can resolve identifiers (symbols) against the native library, looking up native function implementations which are then used as the implementation of `external` Dart function declarations.

If used as annotation on a `library` declaration, all [Native]-annotated external functions in this library will use the specified asset [id] for native function resolution (unless overridden by [Native.assetId]).

If no [DefaultAsset] annotation is provided, the current library's URI is the default asset ID for [Native]-annotated external functions.

Example (file `package:a/a.dart`):

```dart template:top
@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);
```

Example 2 (file `package:a/a.dart`):

```dart template:none
@DefaultAsset('package:a/a.dart')
library a;

import 'dart:ffi';

@Native<Int64 Function(Int64, Int64)>()
external int sum(int a, int b);
```

The above two examples are equivalent.

Prefer using the library URI as asset name when possible.

NOTE: This is an experimental feature and may change in the future.

### id

```dart
String id
```

The default asset name for [Native] external functions in this library.

### DefaultAsset()

```dart
DefaultAsset(String id)
```
