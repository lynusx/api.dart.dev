# ServiceExtensionResponse

```dart
final class ServiceExtensionResponse {}
```

A response to a service protocol extension RPC.

If the RPC was successful, use [ServiceExtensionResponse.result], otherwise use [ServiceExtensionResponse.error].

### result

```dart
String? result
```

The result of a successful service protocol extension RPC.

### errorCode

```dart
int? errorCode
```

The error code associated with a failed service protocol extension RPC.

### errorDetail

```dart
String? errorDetail
```

The details of a failed service protocol extension RPC.

### ServiceExtensionResponse.result()

```dart
ServiceExtensionResponse.result(String result)
```

Creates a successful response to a service protocol extension RPC.

Requires [result] to be a JSON object encoded as a string. When forming the JSON-RPC message [result] will be inlined directly.

### ServiceExtensionResponse.error()

```dart
ServiceExtensionResponse.error(int errorCode, String errorDetail)
```

Creates an error response to a service protocol extension RPC.

Requires [errorCode] to be [invalidParams] or between [extensionErrorMin] and [extensionErrorMax]. Requires [errorDetail] to be a JSON object encoded as a string. When forming the JSON-RPC message [errorDetail] will be inlined directly.

### invalidParams

```dart
dynamic invalidParams
```

Invalid method parameter(s) error code.

### extensionError

```dart
dynamic extensionError
```

Generic extension error code.

### extensionErrorMax

```dart
dynamic extensionErrorMax
```

Maximum extension provided error code.

### extensionErrorMin

```dart
dynamic extensionErrorMin
```

Minimum extension provided error code.

### isError()

```dart
bool isError()
```

Determines if this response represents an error.

A service protocol extension handler. Registered with [registerExtension].

Must complete to a [ServiceExtensionResponse]. [method] is the method name of the service protocol request, and [parameters] is a map holding the parameters to the service protocol request.

_NOTE_: all parameter names and values are encoded as strings.

# registerExtension()

```dart
void registerExtension(String method, ServiceExtensionHandler handler)
```

Register a [ServiceExtensionHandler] that will be invoked in this isolate for [method]. _NOTE_: Service protocol extensions must be registered in each isolate.

_NOTE_: [method] must begin with 'ext.' and you should use the following structure to avoid conflicts with other packages: 'ext.package.command'. That is, immediately following the 'ext.' prefix, should be the registering package name followed by another period ('.') and then the command name. For example: 'ext.dart.io.getOpenFiles'.

Because service extensions are isolate specific, clients using extensions must always include an 'isolateId' parameter with each RPC.

# extensionStreamHasListener

```dart
bool get extensionStreamHasListener
```

Whether the "Extension" stream currently has at least one listener.

A client of the VM service can register as a listener on the extension stream using `listenStream` method. The extension stream has a listener while at least one such client has registered as a listener, and has not yet disconnected again.

Calling [postEvent] while the stream has listeners will attempt to deliver that event to all current listeners, although a listener can disconnect before the event is delivered. Calling [postEvent] when the stream has no listener means that no-one will receive the event, and the call is effectively a no-op.

# postEvent()

```dart
void postEvent(String eventKind, Map eventData, {String stream = 'Extension'})
```

Post an event of [eventKind] with payload of [eventData] to the "Extension" event stream.

If [extensionStreamHasListener] is false, this method is a no-op. Override [stream] to set the destination stream that the event should be posted to. The [stream] may not start with an underscore or be a core VM Service stream.
