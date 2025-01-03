# Retrofit/OkHttp: Custom Interceptors, Timeout Management

## Overview of Retrofit and OkHttp

**Retrofit** is a type-safe HTTP client for Android and Java, which simplifies network requests by providing a clean API and handling serialization/deserialization of HTTP responses.

**OkHttp** is a networking library used by Retrofit under the hood. It's a lower-level HTTP client that handles connections, making it faster and more efficient. While Retrofit handles the higher-level tasks like parsing JSON, OkHttp manages the network operations like sending requests and handling responses.

---

## Custom Interceptors

Interceptors in **OkHttp** are used to intercept and modify requests and responses. You can use interceptors in **Retrofit** by configuring them through **OkHttpClient**.

### Types of Interceptors:
- **Application Interceptors**: These interceptors are used for modifying requests or responses globally, such as adding headers for authentication.
- **Network Interceptors**: These interceptors work on the network layer and are useful for things like monitoring request/response time, or handling caching.

### Example: Adding a Custom Header with an Interceptor

```kotlin
// Custom Interceptor to add a header
class CustomInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request().newBuilder()
            .addHeader("Authorization", "Bearer your_token")
            .build()
        return chain.proceed(request)
    }
}

// Setting up OkHttpClient with the custom interceptor
val client = OkHttpClient.Builder()
    .addInterceptor(CustomInterceptor())
    .build()

// Retrofit setup using the OkHttpClient
val retrofit = Retrofit.Builder()
    .baseUrl("https://yourapi.com/")
    .client(client)  // Use OkHttpClient with interceptors
    .addConverterFactory(GsonConverterFactory.create())
    .build()
```

# Timeout Management

Timeouts are crucial to prevent requests from hanging indefinitely. You can set timeouts for connection, read, and write operations in OkHttp.

## Types of Timeouts:
- **Connect Timeout**: The maximum time allowed for establishing a connection with the server.
- **Read Timeout**: The maximum time allowed for reading a response from the server after the connection is established.
- **Write Timeout**: The maximum time allowed for writing a request to the server.

## Example: Setting Timeouts with OkHttp

```kotlin
val client = OkHttpClient.Builder()
    .connectTimeout(10, TimeUnit.SECONDS)  // Set connect timeout
    .readTimeout(30, TimeUnit.SECONDS)     // Set read timeout
    .writeTimeout(15, TimeUnit.SECONDS)    // Set write timeout
    .build()

val retrofit = Retrofit.Builder()
    .baseUrl("https://yourapi.com/")
    .client(client)
    .addConverterFactory(GsonConverterFactory.create())
    .build()
```

# Retrofit vs OkHttp

| Feature                | Retrofit                                          | OkHttp                                       |
|------------------------|--------------------------------------------------|----------------------------------------------|
| **Purpose**            | Type-safe HTTP client for handling API requests  | Low-level HTTP client handling network operations |
| **Level of Abstraction** | Higher-level (simplifies API calls, response parsing) | Lower-level (handles connection management, raw HTTP requests) |
| **Main Use**           | Network requests, API data parsing (JSON/XML)    | Manages HTTP connections, requests, responses |
| **Built-in Features**  | Data serialization/deserialization, converters   | Custom interceptors, connection pooling, caching |
| **Dependency**         | Built on top of OkHttp                           | Used by Retrofit internally for HTTP requests |

# 🌐 Resources

- [Retrofit Documentation](https://square.github.io/retrofit/)
- [OkHttp Documentation](https://square.github.io/okhttp/)
