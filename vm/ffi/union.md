# Union

```dart
abstract base class Union extends _Compound implements SizedNativeType {}
```

The supertype of all FFI union types.

FFI union types should extend this class and declare fields corresponding to the underlying native union.

Field declarations in a [Union] subclass declaration are automatically given a setter and getter implementation which accesses the native union's field in memory.

All field declarations in a [Union] subclass declaration must either have type [int] or [double] and be annotated with a [NativeType] representing the native type, or must be of type [Pointer], [Array] or a subtype of [Struct] or [Union]. For example:

```c
typedef union {
 int a;
 float b;
 void* c;
} my_union;
```

```dart
final class MyUnion extends Union {
  @Int32()
  external int a;

  @Float()
  external double b;

  external Pointer<Void> c;
}
```

The field declarations of a [Union] subclass _must_ be marked `external`. A union subclass points directly into a location of native memory ([Pointer]) or Dart memory ([TypedData]), and the external field's getter and setter implementations directly read and write bytes at appropriate offsets from that location. This does not allow for non-native fields to also exist.

An instance of a union subclass cannot be created with a generative constructor. Instead, an instance can be created by [UnionPointer.ref], [Union.create], FFI call return values, FFI callback arguments, [UnionArray], and accessing [Union] fields. To create an instance backed by native memory, use [UnionPointer.ref]. To create an instance backed by Dart memory, use [Union.create].

### Union()

```dart
Union()
```

Construct a reference to the [nullptr].

Use [UnionPointer]'s `.ref` to gain references to native memory backed unions.

### create()

```dart
T create<T extends Union>([TypedData typedData, int offset])
```

Creates a union view of bytes in [typedData].

The created instance of the union subclass will then be backed by the bytes at [TypedData.offsetInBytes] plus [offset] times [TypedData.elementSizeInBytes]. That is, the getters and setters of the external instance variables declared by the subclass, will read an write their values from the bytes of the [TypedData.buffer] of [typedData], starting at [TypedData.offsetInBytes] plus [offset] times [TypedData.elementSizeInBytes]. The [TypedData.lengthInBytes] of [typedData] _must_ be sufficient to contain the [sizeOf] of the union subclass. _It doesn't matter whether the [typedData] is, for example, a [Uint8List], a [Float64List], or any other [TypedData], it's only treated as a view into a [ByteBuffer], through its [TypedData.buffer], [TypedData.offsetInBytes] and [TypedData.lengthInBytes]._

If [typedData] is omitted, a fresh [ByteBuffer], with precisely enough bytes for the [sizeOf] of the created union, is allocated on the Dart heap, and used as memory to store the union fields.

If [offset] is provided, the indexing into [typedData] is offset by [offset] times [TypedData.elementSizeInBytes].

Example:

```dart import:typed_data
final class MyUnion extends Union {
  @Int32()
  external int a;

  @Float()
  external double b;

  /// Creates Dart managed memory to hold a `MyUnion` and returns the
  /// `MyUnion` view on it.
  factory MyUnion.a(int a) {
    return Union.create()..a = a;
  }

  /// Creates Dart managed memory to hold a `MyUnion` and returns the
  /// `MyUnion` view on it.
  factory MyUnion.b(double b) {
    return Union.create()..b = b;
  }

  /// Creates a [MyUnion] view on [typedData].
  factory MyUnion.fromTypedData(TypedData typedData) {
    return Union.create(typedData);
  }
}
```

To create a union object from a [Pointer], use [UnionPointer.ref].
