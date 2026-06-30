# Struct

```dart
abstract base class Struct extends _Compound implements SizedNativeType {}
```

The supertype of all FFI struct types.

FFI struct types should extend this class and declare fields corresponding to the underlying native structure.

Field declarations in a [Struct] subclass declaration are automatically given a setter and getter implementation which accesses the native struct's field in memory.

All field declarations in a [Struct] subclass declaration must either have type [int] or [double] and be annotated with a [NativeType] representing the native type, or must be of type [Pointer], [Array] or a subtype of [Struct] or [Union]. For example:

```c
typedef struct {
 int a;
 float b;
 void* c;
} my_struct;
```

```dart
final class MyStruct extends Struct {
  @Int32()
  external int a;

  @Float()
  external double b;

  external Pointer<Void> c;
}
```

The field declarations of a [Struct] subclass _must_ be marked `external`. A struct subclass points directly into a location of native memory ([Pointer]) or Dart memory ([TypedData]), and the external field's getter and setter implementations directly read and write bytes at appropriate offsets from that location. This does not allow for non-native fields to also exist.

An instance of a struct subclass cannot be created with a generative constructor. Instead, an instance can be created by [StructPointer.ref], [Struct.create], FFI call return values, FFI callback arguments, [StructArray], and accessing [Struct] fields. To create an instance backed by native memory, use [StructPointer.ref]. To create an instance backed by Dart memory, use [Struct.create].

### Struct()

```dart
Struct()
```

Construct a reference to the [nullptr].

Use [StructPointer]'s `.ref` to gain references to native memory backed structs.

### create()

```dart
T create<T extends Struct>([TypedData typedData, int offset])
```

Creates a struct view of bytes in [typedData].

The created instance of the struct subclass will then be backed by the bytes at [TypedData.offsetInBytes] plus [offset] times [TypedData.elementSizeInBytes]. That is, the getters and setters of the external instance variables declared by the subclass, will read an write their values from the bytes of the [TypedData.buffer] of [typedData], starting at [TypedData.offsetInBytes] plus [offset] times [TypedData.elementSizeInBytes]. The [TypedData.lengthInBytes] of [typedData] _must_ be sufficient to contain the [sizeOf] of the struct subclass. _It doesn't matter whether the [typedData] is, for example, a [Uint8List], a [Float64List], or any other [TypedData], it's only treated as a view into a [ByteBuffer], through its [TypedData.buffer], [TypedData.offsetInBytes] and [TypedData.lengthInBytes]._

If [typedData] is omitted, a fresh [ByteBuffer], with precisely enough bytes for the [sizeOf] of the created struct, is allocated on the Dart heap, and used as memory to store the struct fields.

If [offset] is provided, the indexing into [typedData] is offset by [offset] times [TypedData.elementSizeInBytes].

Example:

```dart import:typed_data
final class Point extends Struct {
  @Double()
  external double x;

  @Double()
  external double y;

  /// Creates Dart managed memory to hold a `Point` and returns the
  /// `Point` view on it.
  factory Point(double x, double y) {
    return Struct.create()
      ..x = x
      ..y = y;
  }

  /// Creates a [Point] view on [typedData].
  factory Point.fromTypedData(TypedData typedData) {
    return Struct.create(typedData);
  }
}
```

To create a struct object from a [Pointer], use [StructPointer.ref].

# Packed

```dart
final class Packed {}
```

Annotation to specify on `Struct` subtypes to indicate that its members need to be packed.

Valid values for [memberAlignment] are 1, 2, 4, 8, and 16.

### memberAlignment

```dart
int memberAlignment
```

### Packed()

```dart
Packed(int memberAlignment)
```
