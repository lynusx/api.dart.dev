# AbiSpecificInteger

```dart
base class AbiSpecificInteger implements SizedNativeType {}
```

The supertype of all [Abi]-specific integer types.

[Abi]-specific integers should extend this class and annotate it with [AbiSpecificIntegerMapping] to declare the integer size and signedness for [Abi.values].

For example:

```
/// The C `uintptr_t` type.
///
/// The [UintPtr] type is a native type, and should not be constructed in
/// Dart code.
/// It occurs only in native type signatures and as annotation on [Struct]
/// and [Union] fields.
@AbiSpecificIntegerMapping({
  Abi.androidArm: Uint32(),
  Abi.androidArm64: Uint64(),
  Abi.androidIA32: Uint32(),
  Abi.androidX64: Uint64(),
  Abi.androidRiscv64: Uint64(),
  Abi.fuchsiaArm64: Uint64(),
  Abi.fuchsiaX64: Uint64(),
  Abi.fuchsiaRiscv64: Uint64(),
  Abi.iosArm: Uint32(),
  Abi.iosArm64: Uint64(),
  Abi.linuxArm: Uint32(),
  Abi.linuxArm64: Uint64(),
  Abi.linuxIA32: Uint32(),
  Abi.linuxX64: Uint64(),
  Abi.linuxRiscv32: Uint32(),
  Abi.linuxRiscv64: Uint64(),
  Abi.macosArm64: Uint64(),
  Abi.macosX64: Uint64(),
  Abi.windowsIA32: Uint32(),
  Abi.windowsX64: Uint64(),
})
final class UintPtr extends AbiSpecificInteger {
  const UintPtr();
}
```

### AbiSpecificInteger()

```dart
AbiSpecificInteger()
```

# AbiSpecificIntegerMapping

```dart
final class AbiSpecificIntegerMapping {}
```

Mapping for a subtype of [AbiSpecificInteger].

See documentation on [AbiSpecificInteger].

### mapping

```dart
Map<Abi, NativeType> mapping
```

### AbiSpecificIntegerMapping()

```dart
AbiSpecificIntegerMapping(Map<Abi, NativeType> mapping)
```
