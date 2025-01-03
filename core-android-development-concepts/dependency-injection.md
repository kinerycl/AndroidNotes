# 🔗 Dependency Injection with Dagger Hilt  

**Dependency Injection (DI)** is a design pattern that provides dependencies to objects rather than having them create the dependencies themselves. **Dagger Hilt** is Google's recommended DI library for Android, built on top of **Dagger 2**, offering a simpler API for DI in Android apps.  

---

## 📌 **What is Dependency Injection?**  
- **Definition**: A design pattern where objects (dependencies) are provided to a class rather than being instantiated within it.  
- **Purpose**: Improves code modularity, reusability, and testability.  

---

## 🛠️ **Dagger Hilt Overview**  
Hilt simplifies the integration of DI into Android apps by:  
- Providing predefined components for Android classes (e.g., Activities, Fragments, ViewModels).  
- Automatically managing the lifecycle of dependencies.  

### **Key Features**  
- **Predefined Scopes**: Manage dependencies based on their lifecycle.  
- **ViewModel Integration**: Works seamlessly with Jetpack ViewModel.  
- **Test-Friendly**: Provides support for dependency injection in tests.  

---

## 🔧 **Core Components**  

### **1. Modules (@Module)**  
Defines how dependencies are created and provided.  

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    @Provides
    fun provideExampleDependency(): ExampleDependency {
        return ExampleDependency()
    }
}
```

## 2. Components  

Built-in Hilt components represent different Android lifecycle scopes:  
- **SingletonComponent**: Application-wide scope.  
- **ActivityComponent**: Tied to the lifecycle of an `Activity`.  
- **FragmentComponent**: Tied to the lifecycle of a `Fragment`.  
- **ViewModelComponent**: Tied to the lifecycle of a `ViewModel`.  

---

## 3. Annotations  

- **`@HiltAndroidApp`**: Used on the `Application` class to set up Hilt.  
- **`@Inject`**: Used to request dependencies or annotate constructors.  
- **`@Provides`**: Used in modules to define how to provide a dependency.  
- **`@Singleton`**: Defines a dependency with a single instance across the app.  

# 🌀 How Hilt Works

1. **Enable Hilt**  
   Annotate your `Application` class with `@HiltAndroidApp`. This sets up Hilt for dependency injection across your app and generates a base class that manages the application-level dependencies.  

   ```kotlin
   @HiltAndroidApp
   class MyApplication : Application() {
       // Application-level configuration
   }
   ```
2. **Annotate Activities/Fragments**  
Use `@AndroidEntryPoint` on activities, fragments, and other Android classes where you want Hilt to perform dependency injection. This allows these components to receive their required dependencies automatically.

- **Example with Activity**:
    ```kotlin
    @AndroidEntryPoint
    class MainActivity : AppCompatActivity() {
        @Inject lateinit var myDependency: MyDependency

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            // Hilt will automatically inject `myDependency` here
        }
    }
    ```

 - **Example with Fragment**:
    ```kotlin
    @AndroidEntryPoint
    class MyFragment : Fragment(R.layout.fragment_my) {
        @Inject lateinit var myService: MyService

        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            // `myService` is now injected by Hilt
        }
    }
    ```

3. **Provide Dependencies**  
   Create a `@Module` annotated class with methods that define how to create and provide dependencies. Use `@Provides` for non-interface dependencies and `@Binds` for interface bindings.

   - **Example of a Module using `@Provides`**:
   ```kotlin
   @Module
   @InstallIn(SingletonComponent::class)
   object AppModule {
       @Provides
       fun provideMyDependency(): MyDependency {
           return MyDependencyImpl()
       }
   }
   ```

- **Example of a Module using `@Binds` (for interfaces):**
    ```kotlin
    @Module
    @InstallIn(SingletonComponent::class)
    abstract class AppModule {
        @Binds
        abstract fun bindMyService(myServiceImpl: MyServiceImpl): MyService
    }
    ```
- **Explanation:**

    - `@Binds` is used here to bind an interface (`MyService`) to its concrete implementation (`MyServiceImpl`).
    - `@InstallIn(SingletonComponent::class)` ensures that the binding has a singleton scope, meaning only one instance of `MyServiceImpl` will be provided throughout the app.
    - This method eliminates the need for writing the full implementation of `@Provides` for interfaces, making the code more concise and easier to manage.

4. **Inject Dependencies**  
   Use `@Inject` to request dependencies where needed. Hilt resolves these dependencies automatically, based on the bindings defined in modules.

   - **Example**:
   ```kotlin
   class MyViewModel @Inject constructor(
       private val repository: MyRepository
   ) : ViewModel() {
       // Hilt will automatically inject the `repository` dependency here
   }
   ```

- **Explanation:**

- The `@Inject` annotation is used to request a dependency in a class constructor, property, or field.
- Hilt automatically resolves the dependencies based on the bindings you defined in your modules and injects them where they are needed.
- In this example, `MyViewModel` receives the `MyRepository` dependency through constructor injection.

5. **Scopes**  
   Use scopes like `@Singleton`, `@ActivityRetainedScoped`, or `@ViewScoped` to manage the lifecycle of your dependencies. These scopes help you control whether a dependency is shared or unique to a component.

   - **Example with `@Singleton`**:
   ```kotlin
   @Singleton
   class MyRepository @Inject constructor() {
       // Singleton-scoped dependency
   }
   ```

- **Explanation:**

    - `@Singleton` ensures that only one instance of the `MyRepository` class is created and shared throughout the entire app. This instance will live as long as the app is running.
    - `@ActivityRetainedScoped` and `@ViewScoped` can be used for managing lifecycles that are tied to an activity or a view respectively, allowing more granular control over the scope and longevity of dependencies.

6. **Testing Support**  
   Hilt provides test-specific components to inject mock dependencies for testing.

   - **Example**:
   ```kotlin
   @HiltAndroidTest
   class MyTest {
       @Inject lateinit var myService: MyService

       @Before
       fun setup() {
           hiltRule.inject()
       }

       @Test
       fun testMyService() {
           // Use `myService` which has been injected by Hilt for testing
       }
   }
   ```

- **Explanation:**

    - `@HiltAndroidTest` is used to annotate test classes that require dependency injection.
    - `hiltRule.inject()` is used to inject the necessary dependencies before running the test.
    - This allows you to easily inject mock or real dependencies in tests, making it easier to write unit tests for components that depend on Hilt-managed instances.

## ✅ Benefits of Dagger Hilt  

- **Reduces Boilerplate**: Simplifies dependency injection setup.  
- **Scoped Dependencies**: Automatically manages lifecycle-aware dependencies.  
- **Integration**: Works seamlessly with Jetpack libraries (e.g., ViewModel).  
- **Testing Support**: Makes mocking dependencies easier.  

---

## ❌ Potential Drawbacks  

- **Learning Curve**: Understanding DI and Hilt can be challenging for beginners.  
- **Overhead for Small Apps**: May feel excessive for simple projects with few dependencies.  
- **Compile-Time Processing**: Dagger uses annotation processing, which can slightly increase build times.  

---

## 🎯 Common Use Cases  

- Apps requiring shared resources (e.g., API clients, repositories).  
- Modular and testable apps.  
- Managing complex dependency graphs in medium-to-large codebases.  

---

## 🌐 Resources  

- [Hilt Documentation](https://developer.android.com/training/dependency-injection/hilt)  
- [Dagger Hilt CodeLab](https://developer.android.com/codelabs/android-hilt#0)  