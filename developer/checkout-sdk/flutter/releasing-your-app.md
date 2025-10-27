# Releasing Your App

## [Android](releasing-your-app.md#android)

Follow these steps to prepare and release your Android app properly:

{% stepper %}
{% step %}
**Update the `proguard-rules.pro` File**

When you are ready to release your app, the first thing to do is update your `proguard-rules.pro` file.

*   Navigate to your project directory:

    ```kotlin
    App/android/app/
    ```
* Open the `build.gradle.kts` file.
{% endstep %}

{% step %}
**Verify or Add ProGuard Configuration**

* Scroll to the `buildTypes` section.
* Check whether the ProGuard configuration has been added.
*   If it’s missing, add the following lines:

    ```kotlin
    buildTypes {
        getByName("release") {
           ...
           ...
            // Verify this configuration in your build.gradle file
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
            signingConfig = signingConfigs.getByName("debug")
        }
    }
    ```
* Ensure that the `proguard-rules.pro` file exists in the `App/android/app/` directory.
  * If it doesn’t exist, create it manually.
{% endstep %}

{% step %}
**Build the Release APK**

Run the following command to build your release APK and verify that all required properties are properly configured:

```kotlin
flutter build apk
```
{% endstep %}

{% step %}
**Resolve R8 Compilation Errors (If Any)**

If you encounter an error such as the following:

{% code overflow="wrap" %}
```kotlin
ERROR: Missing classes detected while running R8. Please add the missing classes or apply additional keep rules that are generated in /build/app/outputs/mapping/release/missing_rules.txt.
ERROR: R8: Missing class com.google.devtools.ksp.processing.SymbolProcessorProvider (referenced from: com.squareup.moshi.kotlin.codegen.ksp.JsonClassSymbolProcessorProvider)
FAILURE: Build failed with an exception.
* What went wrong:
Execution failed for task ':app:minifyReleaseWithR8'.
> A failure occurred while executing com.android.build.gradle.internal.tasks.R8Task$R8Runnable
   > Compilation failed to complete
```
{% endcode %}

**Do the following:**

1.  Navigate to the file path below:

    ```
    build/app/outputs/mapping/release/missing_rules.txt
    ```
2. Copy the rules listed in this file.
3. Paste them into your `proguard-rules.pro` file.
{% endstep %}

{% step %}
**Rebuild and Verify**

After updating the ProGuard rules, run the release command again to ensure all issues are resolved:

```gedcom
flutter build apk
```

If the build completes successfully, your app is ready for release.
{% endstep %}
{% endstepper %}
