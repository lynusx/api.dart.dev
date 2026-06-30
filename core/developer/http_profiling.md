# addHttpClientProfilingData()

```dart
void addHttpClientProfilingData(Map<String, dynamic> requestProfile)
```

Records the data associated with an HTTP request for profiling purposes.

This function should never be called directly. Instead, use [package:http_profile](https://pub.dev/packages/http_profile).

# getHttpClientProfilingData()

```dart
List<Map<String, dynamic>> getHttpClientProfilingData()
```

Returns the data added through [addHttpClientProfilingData].

This function is only meant for use by networking profilers and the format of the returned data may change over time.
