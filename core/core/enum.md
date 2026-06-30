# Enum

```dart
abstract interface class Enum {}
```

An enumerated value.

This class is implemented by all types and values introduced using an `enum` declaration. Non-platform classes cannot extend or mix in this class. Concrete classes cannot implement the interface.

The identifier used to name an `enum` value is available as a [String], via the [EnumName.name] extension property on the `enum` value.

### index

```dart
int get index
```

A numeric identifier for the enumerated value.

The values of a single enumeration are numbered consecutively from zero to one less than the number of values. This is also the index of the value in the enumerated type's static `values` list.

### compareByIndex()

```dart
int compareByIndex<T extends Enum>(T value1, T value2)
```

Compares two enum values by their [index].

A generic [Comparator] function for enum types which orders enum values by their [index] value, which corresponds to the source order of the enum element declarations in the `enum` declaration.

Example:

```dart
enum Season { spring, summer, autumn, winter }

void main() {
  var relationByIndex =
      Enum.compareByIndex(Season.spring, Season.summer); // < 0
  relationByIndex =
      Enum.compareByIndex(Season.summer, Season.spring); // > 0
  relationByIndex =
      Enum.compareByIndex(Season.spring, Season.winter); // < 0
  relationByIndex =
      Enum.compareByIndex(Season.winter, Season.spring); // > 0
}
```

### compareByName()

```dart
int compareByName<T extends Enum>(T value1, T value2)
```

Compares enum values by name.

The [EnumName.name] of an enum value is a string representing the source name used to declare that enum value.

This [Comparator] compares two enum values by comparing their names, and can be used to sort enum values by their names. The comparison uses [String.compareTo], and is therefore case sensitive.

Example:

```dart
enum Season { spring, summer, autumn, winter }

void main() {
  var seasons = [...Season.values]..sort(Enum.compareByName);
  print(seasons);
  // [Season.autumn, Season.spring, Season.summer, Season.winter]
}
```

# EnumName

```dart
extension EnumName on Enum {}
```

Access to the name of an enum value.

This method is declared as an extension method instead of an instance method in order to allow enum values to have the name `name`.

### name

```dart
String get name
```

The name of the enum value.

The name is a string containing the source identifier used to declare the enum value.

For example, given a declaration like:

```dart
enum MyEnum {
  value1,
  value2
}
```

the result of `MyEnum.value1.name` is the string `"value1"`.

# EnumByName

```dart
extension EnumByName<T extends Enum> on Iterable<T> {}
```

Access enum values by name.

Extensions on a collection of enum values, intended for use on the `values` list of an enum type, which allows looking up a value by its name.

Since enum classes are expected to be relatively small, lookup of [byName] is performed by linearly iterating through the values and comparing their name to the provided name. If a more efficient lookup is needed, perhaps because the lookup operation happens very often, consider building a map instead using [asNameMap]:

```dart
static myEnumNameMap = MyEnum.values.asNameMap();
```

and then use that for lookups.

### byName()

```dart
T byName(String name)
```

Finds the enum value in this list with name [name].

Goes through this collection looking for an enum with name [name], as reported by [EnumName.name]. Returns the first value with the given name. Such a value must be found.

### asNameMap()

```dart
Map<String, T> asNameMap()
```

Creates a map from the names of enum values to the values.

The collection that this method is called on is expected to have enums with distinct names, like the `values` list of an enum class. Only one value for each name can occur in the created map, so if two or more enum values have the same name (either being the same value, or being values of different enum type), at most one of them will be represented in the returned map.
