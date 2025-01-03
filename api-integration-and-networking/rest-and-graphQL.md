# 🌐 **REST and GraphQL APIs: Data Parsing, Caching, and Error Handling**

APIs are central to modern mobile app development, and both REST and GraphQL are commonly used to fetch data from remote servers. Understanding how to effectively work with these APIs is crucial for building efficient and reliable Android apps.

## 🔌 **REST APIs**

REST (Representational State Transfer) is an architectural style for designing networked applications. It is based on stateless communication, using standard HTTP methods (GET, POST, PUT, DELETE) to interact with resources.

### 1. **Data Parsing in REST APIs**
Data fetched from a REST API is typically in JSON format, which needs to be parsed into usable Kotlin objects.

- **Libraries**: Commonly used libraries for parsing JSON data include **Gson** and **Moshi**.
- **Example**: 
```kotlin
data class User(val id: Int, val name: String)

val jsonResponse = "{ 'id': 1, 'name': 'John Doe' }"
val user = Gson().fromJson(jsonResponse, User::class.java)
```
- **Coroutines**: You can use suspend functions in conjunction with Retrofit to fetch data asynchronously and handle it without blocking the UI thread.

### 2. **Caching in REST APIs**
Caching can reduce the number of network requests, improving performance and user experience.
- Network Caching: Retrofit supports caching using OkHttp’s built-in cache mechanism.
- Room Database: You can cache data locally using Room to persist it between app sessions, improving performance when users are offline.
- Example: Using Retrofit with OkHttp caching.
    ```kotlin
    val cacheSize = 10 * 1024 * 1024L // 10 MB
    val cache = Cache(context.cacheDir, cacheSize)
    val okHttpClient = OkHttpClient.Builder().cache(cache).build()
    ```

### 3. **Error Handling in REST APIs**
Error handling ensures that the app can gracefully handle issues like network failures, server errors, and invalid responses.
- Response Codes: Always check the HTTP response status code (e.g., 200 for success, 404 for not found).
- Handling Exceptions: Use try-catch blocks to handle exceptions like IOException or HttpException.
- Example:
    ```kotlin
    try {
        val response = apiService.getUserData()
        if (response.isSuccessful) {
            // Handle successful response
        } else {
            // Handle error response
        }
    } catch (e: Exception) {
        // Handle network errors
    }
    ```

## ⚡ **GraphQL APIs**

GraphQL is a query language for APIs that allows clients to request exactly the data they need. It’s a more flexible alternative to REST, as it allows for complex queries and reduces the number of requests.

### 1. **Data Parsing in GraphQL APIs**
GraphQL responses typically return data in a structured format that can be parsed similarly to REST, but you query specific fields rather than predefined endpoints.
- Libraries: Apollo Client is a common library used to interact with GraphQL APIs in Android.
- Example:
    ```kotlin
    data class User(val id: Int, val name: String)

    val query = gql("""
        query {
            user(id: 1) {
                id
                name
            }
        }
    """)

    val response = apolloClient.query(query).execute()
    ```

### 2. Caching in GraphQL APIs
Caching in GraphQL can be a bit more complex due to the nature of queries, but Apollo Client provides built-in support for caching.
- Apollo Cache: Apollo Client caches the results of GraphQL queries automatically and uses that data when possible.
- Custom Caching: You can also configure custom cache policies to manage how data is cached and fetched.

### 3. Error Handling in GraphQL APIs

GraphQL allows both query errors and network errors to be handled separately.
- GraphQL Errors: These are errors returned by the GraphQL server when the query cannot be resolved (e.g., invalid fields, syntax errors).
- Network Errors: Like in REST, network-related errors need to be handled using try-catch.
- Example:
    ```kotlin
    apolloClient.query(query)
        .enqueue(object : ApolloCall.Callback<QueryData>() {
            override fun onResponse(response: Response<QueryData>) {
                if (response.hasErrors()) {
                    // Handle GraphQL errors
                }
            }

            override fun onFailure(e: ApolloException) {
                // Handle network or other errors
            }
        })
    ```

## 📈 Comparison: REST vs GraphQL
# REST vs GraphQL

| Feature            | REST                                    | GraphQL                               |
|--------------------|-----------------------------------------|---------------------------------------|
| **Request Structure** | Fixed endpoints and resources           | Flexible queries, precise data fetch  |
| **Data Fetching**    | Multiple requests for related data      | Single query for related data         |
| **Caching**          | Simple, HTTP cache headers              | More complex, built-in caching support |
| **Error Handling**   | Based on HTTP status codes              | Based on query or network errors      |
| **Performance**      | Can be inefficient for large payloads   | Efficient, fetch only needed data     |


# 🌐 Resources

- [GraphQL Docs](https://graphql.org/)
- [Apollo Client](https://www.apollographql.com/docs/)
- [Retrofit Documentation](https://square.github.io/retrofit/)
- [Moshi Documentation](https://square.github.io/moshi/)
- [Gson Documentation](https://github.com/google/gson)