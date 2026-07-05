# ListBase

```dart
abstract mixin class ListBase<E> implements List<E> {}
```

Abstract implementation of a list.

`ListBase` can be used as a base class for implementing the `List` interface.

This class implements all read operations using only the `length` and `operator[]` and members. It implements write operations using those and `add`, `length=` and `operator[]=` Classes using this base class should implement those five operations.

**NOTICE**: For backwards compatibility reasons, there is a default implementation of `add` which only works for lists with a nullable element type. For list with a non-nullable element type, the `add` method must be implemented.

**NOTICE**: Forwarding just the four `length` and `[]` read/write operations to a normal growable [List] (as created by a `[]` literal) will give very bad performance for `add` and `addAll` operations of `ListBase`. These operations are implemented by increasing the length of the list by one for each `add` operation, and repeatedly increasing the length of a growable list is not efficient. To avoid this, override 'add' and 'addAll' to also forward directly to the growable list, or, if possible, use `DelegatingList` from "package:collection/collection.dart" instead of a `ListMixin`.

## 构造函数

### ListBase()

```dart
const ListBase<E>()
```

## 属性

* iterator
* isEmpty
* isNotEmpty
* first
* last
* single

## 方法

* elementAt()
* followedBy()
* forEach()
* contains()
* every()
* any()
* firstWhere()
* lastWhere()
* singleWhere()
* join()
* where()
* whereType()
* map()
* expand()
* reduce()
* fold()
* skip()
* skipWhile()
* take()
* takeWhile()
* toList()
* toSet()
* add()
* addAll()
* remove()
* removeWhere()
* retainWhere()
* clear()
* cast()
* removeLast()
* sort()
* shuffle()
* asMap()
* sublist()
* getRange()
* removeRange()
* fillRange()
* setRange()
* replaceRange()
* indexOf()
* indexWhere()
* lastIndexOf()
* lastIndexWhere()
* insert()
* removeAt()
* insertAll()
* setAll()
* reversed

### listToString()

```dart
String listToString(List<Object?> list)
```

Converts a [List] to a [String].

Converts [list] to a string by converting each element to a string (by calling [Object.toString]), joining them with ", ", and wrapping the result in `[` and `]`.

Handles circular references where converting one of the elements to a string ends up converting [list] to a string again.

## 运算符

* operator +

---



# ListMixin

```dart
typedef ListMixin<E> = ListBase<E>
```

Base mixin implementation of a [List] class.

`ListMixin` can be used as a mixin to make a class implement the `List` interface.

This mixin implements all read operations using only the `length` and `operator[]` and members. It implements write operations using those and `add`, `length=` and `operator[]=`. Classes using this mixin should implement those five operations.

**NOTICE**: For backwards compatibility reasons, there is a default implementation of `add` which only works for lists with a nullable element type. For lists with a non-nullable element type, the `add` method must be implemented.

**NOTICE**: Forwarding just the four `length` and `[]` read/write operations to a normal growable [List] (as created by a `[]` literal) will give very bad performance for `add` and `addAll` operations of `ListMixin`. These operations are implemented by increasing the length of the list by one for each `add` operation, and repeatedly increasing the length of a growable list is not efficient. To avoid this, override 'add' and 'addAll' to also forward directly to the growable list, or, if possible, use `DelegatingList` from "package:collection/collection.dart" instead of a `ListMixin`.
