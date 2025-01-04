# Solving Android-Specific Issues 🚀

## Memory Leaks 🧠💧
- **Description**: Occurs when objects are no longer needed but cannot be garbage collected due to lingering references.
- **Common Causes**:
  - Activity/Fragment references in long-lived objects (e.g., static variables, singletons).
  - Unreleased resources (e.g., handlers, listeners).
- **Prevention**:
  - Use **WeakReference** for non-critical references.
  - Release resources in lifecycle methods (e.g., `onDestroy()`).
  - Use tools like **LeakCanary** to detect memory leaks.

---

## Application Not Responding (ANR) 🚨
- **Description**: Happens when the main thread is blocked for more than 5 seconds.
- **Common Causes**:
  - Long-running operations on the main thread (e.g., database queries, network calls).
- **Solutions**:
  - Offload heavy tasks to a background thread using **Coroutines**, **WorkManager**, or **AsyncTask** (deprecated).
  - Keep UI thread tasks lightweight.

---

## Configuration Changes 🔄
- **Description**: Issues caused when the app’s configuration changes (e.g., orientation, language).
- **Challenges**:
  - Loss of UI state or resources.
- **Best Practices**:
  - Use **ViewModel** to store UI-related data that survives configuration changes.
  - Handle configuration changes explicitly with `android:configChanges` or override `onConfigurationChanged()`.
  - Save and restore UI state using **onSaveInstanceState()**.

---

## Tools & Resources 🌐
- [LeakCanary](https://square.github.io/leakcanary/) for memory leak detection.
- [Android Developers - ANR Troubleshooting](https://developer.android.com/topic/performance/vitals/anr) for resolving ANRs.
- [ViewModel Documentation](https://developer.android.com/topic/libraries/architecture/viewmodel) for handling configuration changes.