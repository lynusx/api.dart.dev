# Sink

```dart
abstract interface class Sink<T> {}
```

A generic destination for data.

Multiple data values can be put into a sink, and when no more data is available, the sink should be closed.

This is a generic interface that other data receivers can implement.

### add()

```dart
void add(T data)
```

Adds [data] to the sink.

Must not be called after a call to [close].

### close()

```dart
void close()
```

Closes the sink.

The [add] method must not be called after this method.

Calling this method more than once is allowed, but does nothing.
