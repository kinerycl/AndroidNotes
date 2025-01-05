## Mocking and Stubbing

Mocking and stubbing are techniques used in automated testing to simulate the behavior of complex objects or external dependencies, allowing for isolated unit tests.

### Libraries

- **Mockito**: A popular Java-based library for mocking objects in tests. It provides functionality to create mock objects, define their behavior, and verify interactions.
  - **Pros**: Widely used, well-documented, integrates with JUnit and other testing frameworks.
  - **Cons**: Tests depend on how code is written, meaning changes to the code might break the tests, even if the outcome is correct.

- **MockK**: A Kotlin-first mocking library designed for better support of Kotlin features like coroutines, nullability, and extension functions.
  - **Pros**: Fully compatible with Kotlin, supports coroutines and other Kotlin-specific features, simpler syntax.
  - **Cons**: Smaller community compared to Mockito, may have less support for non-Kotlin libraries.

### Use Cases

- **Mocking**: Simulating external dependencies like APIs, databases, or network calls in unit tests.
- **Stubbing**: Providing predefined responses for methods of mocked objects, controlling the behavior of dependencies to test specific scenarios.

### Example

```kotlin
// Example using Mockito
val mockService = mock(MyService::class.java)
`when`(mockService.fetchData()).thenReturn("Mocked Data")

val result = mockService.fetchData()
assertEquals("Mocked Data", result)
```

### Considerations

- **Test Fragility**: Mocking tests can break if the code changes, even if the functionality does not.
- **Test Maintenance**: Over-mocking can lead to difficult-to-maintain tests. Only mock external dependencies, not internal logic.