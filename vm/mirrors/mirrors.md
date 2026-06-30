Basic reflection in Dart, with support for introspection and dynamic invocation.

_Introspection_ is that subset of reflection by which a running program can examine its own structure. For example, a function that prints out the names of all the members of an arbitrary object.

_Dynamic invocation_ refers the ability to evaluate code that has not been literally specified at compile time, such as calling a method whose name is provided as an argument (because it is looked up in a database, or provided interactively by the user).

## How to interpret this library's documentation

As a rule, the names of Dart declarations are represented using instances of class [Symbol]. Whenever the doc speaks of an object _s_ of class [Symbol] denoting a name, it means the string that was used to construct _s_.

The documentation frequently abuses notation with Dart pseudo-code such as [:o.x(a):], where o and a are defined to be objects; what is actually meant in these cases is [:o'.x(a'):] where _o'_ and _a'_ are Dart variables bound to _o_ and _a_ respectively. Furthermore, _o'_ and _a'_ are assumed to be fresh variables (meaning that they are distinct from any other variables in the program).

Sometimes the documentation refers to _serializable_ objects. An object is serializable across isolates if and only if it is an instance of num, bool, String, a list of objects that are serializable across isolates, or a map with keys and values that are all serializable across isolates.

## Status: Unstable

The dart:mirrors library is unstable and its API might change slightly as a result of user feedback. This library is only supported by the Dart VM and only available on some platforms.

{@category VM}

# AbstractClassInstantiationError

```dart
class AbstractClassInstantiationError extends Error {}
```

Error thrown when trying to instantiate an abstract class.

### AbstractClassInstantiationError()

```dart
AbstractClassInstantiationError(String className)
```

### toString()

```dart
String toString()
```

# MirrorSystem

```dart
abstract class MirrorSystem {}
```

### libraries

```dart
Map<Uri, LibraryMirror> get libraries
```

### findLibrary()

```dart
LibraryMirror findLibrary(Symbol libraryName)
```

### isolate

```dart
IsolateMirror get isolate
```

### dynamicType

```dart
TypeMirror get dynamicType
```

### voidType

```dart
TypeMirror get voidType
```

### neverType

```dart
TypeMirror get neverType
```

### getName()

```dart
String getName(Symbol symbol)
```

### getSymbol()

```dart
Symbol getSymbol(String name, [LibraryMirror? library])
```

# currentMirrorSystem()

```dart
MirrorSystem currentMirrorSystem()
```

# reflect()

```dart
InstanceMirror reflect(dynamic reflectee)
```

# reflectClass()

```dart
ClassMirror reflectClass(Type key)
```

# reflectType()

```dart
TypeMirror reflectType(Type key, [List<Type>? typeArguments])
```

# Mirror

```dart
abstract class Mirror {}
```

# IsolateMirror

```dart
abstract class IsolateMirror implements Mirror {}
```

### debugName

```dart
String get debugName
```

### isCurrent

```dart
bool get isCurrent
```

### rootLibrary

```dart
LibraryMirror get rootLibrary
```

### operator ==()

```dart
bool operator ==(Object other)
```

### loadUri()

```dart
Future<LibraryMirror> loadUri(Uri uri)
```

# DeclarationMirror

```dart
abstract class DeclarationMirror implements Mirror {}
```

### simpleName

```dart
Symbol get simpleName
```

### qualifiedName

```dart
Symbol get qualifiedName
```

### owner

```dart
DeclarationMirror? get owner
```

### isPrivate

```dart
bool get isPrivate
```

### isTopLevel

```dart
bool get isTopLevel
```

### location

```dart
SourceLocation? get location
```

### metadata

```dart
List<InstanceMirror> get metadata
```

# ObjectMirror

```dart
abstract class ObjectMirror implements Mirror {}
```

### invoke()

```dart
InstanceMirror invoke(Symbol memberName, List<dynamic> positionalArguments, [Map<Symbol, dynamic> namedArguments = const <Symbol, dynamic>{}])
```

### getField()

```dart
InstanceMirror getField(Symbol fieldName)
```

### setField()

```dart
InstanceMirror setField(Symbol fieldName, dynamic value)
```

### delegate()

```dart
delegate(Invocation invocation)
```

# InstanceMirror

```dart
abstract class InstanceMirror implements ObjectMirror {}
```

### type

```dart
ClassMirror get type
```

### hasReflectee

```dart
bool get hasReflectee
```

### reflectee

```dart
dynamic get reflectee
```

### operator ==()

```dart
bool operator ==(Object other)
```

# ClosureMirror

```dart
abstract class ClosureMirror implements InstanceMirror {}
```

### function

```dart
MethodMirror get function
```

### apply()

```dart
InstanceMirror apply(List<dynamic> positionalArguments, [Map<Symbol, dynamic> namedArguments = const <Symbol, dynamic>{}])
```

# LibraryMirror

```dart
abstract class LibraryMirror implements DeclarationMirror, ObjectMirror {}
```

### uri

```dart
Uri get uri
```

### declarations

```dart
Map<Symbol, DeclarationMirror> get declarations
```

### operator ==()

```dart
bool operator ==(Object other)
```

### libraryDependencies

```dart
List<LibraryDependencyMirror> get libraryDependencies
```

# LibraryDependencyMirror

```dart
abstract class LibraryDependencyMirror implements Mirror {}
```

A mirror on an import or export declaration.

### isImport

```dart
bool get isImport
```

Is `true` if this dependency is an import.

### isExport

```dart
bool get isExport
```

Is `true` if this dependency is an export.

### isDeferred

```dart
bool get isDeferred
```

Returns true iff this dependency is a deferred import. Otherwise returns false.

### sourceLibrary

```dart
LibraryMirror get sourceLibrary
```

Returns the library mirror of the library that imports or exports the [targetLibrary].

### targetLibrary

```dart
LibraryMirror? get targetLibrary
```

Returns the library mirror of the library that is imported or exported, or null if the library is not loaded.

### prefix

```dart
Symbol? get prefix
```

Returns the prefix if this is a prefixed import and `null` otherwise.

### combinators

```dart
List<CombinatorMirror> get combinators
```

Returns the list of show/hide combinators on the import/export declaration.

### location

```dart
SourceLocation? get location
```

Returns the source location for this import/export declaration.

### metadata

```dart
List<InstanceMirror> get metadata
```

### loadLibrary()

```dart
Future<LibraryMirror> loadLibrary()
```

Returns a future that completes with a library mirror on the library being imported or exported when it is loaded, and initiates a load of that library if it is not loaded.

# CombinatorMirror

```dart
abstract class CombinatorMirror implements Mirror {}
```

A mirror on a show/hide combinator declared on a library dependency.

### identifiers

```dart
List<Symbol> get identifiers
```

The list of identifiers on the combinator.

### isShow

```dart
bool get isShow
```

Is `true` if this is a 'show' combinator.

### isHide

```dart
bool get isHide
```

Is `true` if this is a 'hide' combinator.

# TypeMirror

```dart
abstract class TypeMirror implements DeclarationMirror {}
```

### hasReflectedType

```dart
bool get hasReflectedType
```

### reflectedType

```dart
Type get reflectedType
```

### typeVariables

```dart
List<TypeVariableMirror> get typeVariables
```

### typeArguments

```dart
List<TypeMirror> get typeArguments
```

### isOriginalDeclaration

```dart
bool get isOriginalDeclaration
```

### originalDeclaration

```dart
TypeMirror get originalDeclaration
```

### isSubtypeOf()

```dart
bool isSubtypeOf(TypeMirror other)
```

### isAssignableTo()

```dart
bool isAssignableTo(TypeMirror other)
```

# ClassMirror

```dart
abstract class ClassMirror implements TypeMirror, ObjectMirror {}
```

### superclass

```dart
ClassMirror? get superclass
```

### superinterfaces

```dart
List<ClassMirror> get superinterfaces
```

### isAbstract

```dart
bool get isAbstract
```

### isEnum

```dart
bool get isEnum
```

### declarations

```dart
Map<Symbol, DeclarationMirror> get declarations
```

### instanceMembers

```dart
Map<Symbol, MethodMirror> get instanceMembers
```

### staticMembers

```dart
Map<Symbol, MethodMirror> get staticMembers
```

### mixin

```dart
ClassMirror get mixin
```

### newInstance()

```dart
InstanceMirror newInstance(Symbol constructorName, List<dynamic> positionalArguments, [Map<Symbol, dynamic> namedArguments = const <Symbol, dynamic>{}])
```

### operator ==()

```dart
bool operator ==(Object other)
```

### isSubclassOf()

```dart
bool isSubclassOf(ClassMirror other)
```

# FunctionTypeMirror

```dart
abstract class FunctionTypeMirror implements ClassMirror {}
```

### returnType

```dart
TypeMirror get returnType
```

### parameters

```dart
List<ParameterMirror> get parameters
```

### callMethod

```dart
MethodMirror get callMethod
```

# TypeVariableMirror

```dart
abstract class TypeVariableMirror extends TypeMirror {}
```

### upperBound

```dart
TypeMirror get upperBound
```

### isStatic

```dart
bool get isStatic
```

### operator ==()

```dart
bool operator ==(Object other)
```

# TypedefMirror

```dart
abstract class TypedefMirror implements TypeMirror {}
```

### referent

```dart
FunctionTypeMirror get referent
```

# MethodMirror

```dart
abstract class MethodMirror implements DeclarationMirror {}
```

### returnType

```dart
TypeMirror get returnType
```

### source

```dart
String? get source
```

### parameters

```dart
List<ParameterMirror> get parameters
```

### isStatic

```dart
bool get isStatic
```

### isAbstract

```dart
bool get isAbstract
```

### isSynthetic

```dart
bool get isSynthetic
```

### isRegularMethod

```dart
bool get isRegularMethod
```

### isOperator

```dart
bool get isOperator
```

### isGetter

```dart
bool get isGetter
```

### isSetter

```dart
bool get isSetter
```

### isConstructor

```dart
bool get isConstructor
```

### constructorName

```dart
Symbol get constructorName
```

### isConstConstructor

```dart
bool get isConstConstructor
```

### isGenerativeConstructor

```dart
bool get isGenerativeConstructor
```

### isRedirectingConstructor

```dart
bool get isRedirectingConstructor
```

### isFactoryConstructor

```dart
bool get isFactoryConstructor
```

### isExtensionMember

```dart
bool get isExtensionMember
```

### isExtensionTypeMember

```dart
bool get isExtensionTypeMember
```

### operator ==()

```dart
bool operator ==(Object other)
```

# VariableMirror

```dart
abstract class VariableMirror implements DeclarationMirror {}
```

### type

```dart
TypeMirror get type
```

### isStatic

```dart
bool get isStatic
```

### isFinal

```dart
bool get isFinal
```

### isConst

```dart
bool get isConst
```

### isExtensionMember

```dart
bool get isExtensionMember
```

### isExtensionTypeMember

```dart
bool get isExtensionTypeMember
```

### operator ==()

```dart
bool operator ==(Object other)
```

# ParameterMirror

```dart
abstract class ParameterMirror implements VariableMirror {}
```

### type

```dart
TypeMirror get type
```

### isOptional

```dart
bool get isOptional
```

### isNamed

```dart
bool get isNamed
```

### hasDefaultValue

```dart
bool get hasDefaultValue
```

### defaultValue

```dart
InstanceMirror? get defaultValue
```

# SourceLocation

```dart
abstract class SourceLocation {}
```

### line

```dart
int get line
```

### column

```dart
int get column
```

### sourceUri

```dart
Uri get sourceUri
```
