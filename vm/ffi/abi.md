# Abi

```dart
class Abi {}
```

An application binary interface (ABI).

An ABI defines the memory layout of data and the function call protocol for native code. It is usually defined by the an operating system for each architecture that operating system runs on.

The Dart VM can run on a variety of operating systems and architectures. Supported ABIs are represented by `Abi` objects. See [values] for all the supported ABIs.

### androidArm

```dart
dynamic androidArm
```

The application binary interface for Android on the Arm architecture.

### androidArm64

```dart
dynamic androidArm64
```

The application binary interface for Android on the Arm64 architecture.

### androidIA32

```dart
dynamic androidIA32
```

The application binary interface for Android on the IA32 architecture.

### androidX64

```dart
dynamic androidX64
```

The application binary interface for Android on the X64 architecture.

### androidRiscv64

```dart
dynamic androidRiscv64
```

The application binary interface for Android on 64-bit RISC-V.

### fuchsiaArm64

```dart
dynamic fuchsiaArm64
```

The application binary interface for Fuchsia on the Arm64 architecture.

### fuchsiaX64

```dart
dynamic fuchsiaX64
```

The application binary interface for Fuchsia on the X64 architecture.

### fuchsiaRiscv64

```dart
dynamic fuchsiaRiscv64
```

The application binary interface for Fuchsia on the Riscv64 architecture.

### iosArm

```dart
dynamic iosArm
```

The application binary interface for iOS on the Arm architecture.

### iosArm64

```dart
dynamic iosArm64
```

The application binary interface for iOS on the Arm64 architecture.

### iosX64

```dart
dynamic iosX64
```

The application binary interface for iOS on the X64 architecture.

### linuxArm

```dart
dynamic linuxArm
```

The application binary interface for Linux on the Arm architecture.

Does not distinguish between hard and soft fp. Currently, no uses of Abi require this distinction.

### linuxArm64

```dart
dynamic linuxArm64
```

The application binary interface for linux on the Arm64 architecture.

### linuxIA32

```dart
dynamic linuxIA32
```

The application binary interface for linux on the IA32 architecture.

### linuxX64

```dart
dynamic linuxX64
```

The application binary interface for linux on the X64 architecture.

### linuxRiscv32

```dart
dynamic linuxRiscv32
```

The application binary interface for linux on 32-bit RISC-V.

### linuxRiscv64

```dart
dynamic linuxRiscv64
```

The application binary interface for linux on 64-bit RISC-V.

### macosArm64

```dart
dynamic macosArm64
```

The application binary interface for MacOS on the Arm64 architecture.

### macosX64

```dart
dynamic macosX64
```

The application binary interface for MacOS on the X64 architecture.

### windowsArm64

```dart
dynamic windowsArm64
```

The application binary interface for Windows on the Arm64 architecture.

### windowsIA32

```dart
dynamic windowsIA32
```

The application binary interface for Windows on the IA32 architecture.

### windowsX64

```dart
dynamic windowsX64
```

The application binary interface for Windows on the X64 architecture.

### values

```dart
dynamic values
```

The ABIs that the DartVM can run on.

Does not contain a `macosIA32`. We have stopped supporting 32-bit MacOS.

### Abi.current()

```dart
Abi.current()
```

The ABI the Dart VM is currently running on.

### toString()

```dart
String toString()
```

A string representation of this ABI.

The string is equal to the 'on' part from `Platform.version` and `dart --version`.
