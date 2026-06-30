# ServiceProtocolInfo

```dart
final class ServiceProtocolInfo {}
```

Service protocol is the protocol that a client like the Observatory could use to access the services provided by the Dart VM for debugging and inspecting Dart programs. This class encapsulates the version number and Uri for accessing this service.

### majorVersion

```dart
int majorVersion
```

The major version of the protocol. If the running Dart environment does not support the service protocol, this is 0.

### minorVersion

```dart
int minorVersion
```

The minor version of the protocol. If the running Dart environment does not support the service protocol, this is 0.

### serverUri

```dart
Uri? serverUri
```

The Uri to connect to the debugger client hosted by the service. If the web server is not running, this will be null.

### serverWebSocketUri

```dart
Uri? get serverWebSocketUri
```

The Uri to connect to the service via web socket. If the web server is not running, this will be null.

### ServiceProtocolInfo()

```dart
ServiceProtocolInfo(Uri? serverUri)
```

### toString()

```dart
String toString()
```

# Service

```dart
final class Service {}
```

Access information about the service protocol and control the web server that provides access to the services provided by the Dart VM for debugging and inspecting Dart programs.

### getInfo()

```dart
Future<ServiceProtocolInfo> getInfo()
```

Get information about the service protocol (version number and Uri to access the service).

### controlWebServer()

```dart
Future<ServiceProtocolInfo> controlWebServer({bool enable = false, bool? silenceOutput})
```

Control the web server that the service protocol is accessed through. [enable] is used as a toggle to enable or disable the web server servicing requests. If [silenceOutput] is provided and is true, the server will not output information to the console.

### getIsolateId()

```dart
String? getIsolateId(Isolate isolate)
```

Returns a [String] token representing the ID of [isolate].

Returns null if the running Dart environment does not support the service protocol.

To get the isolate id of the current isolate, pass [Isolate.current] as the [isolate] parameter.

### getIsolateID()

```dart
String? getIsolateID(Isolate isolate)
```

Returns a [String] token representing the ID of [isolate].

Returns null if the running Dart environment does not support the service protocol.

### getObjectId()

```dart
String? getObjectId(Object object)
```

Returns a [String] token representing the ID of [object].

Returns null if the running Dart environment does not support the service protocol.
