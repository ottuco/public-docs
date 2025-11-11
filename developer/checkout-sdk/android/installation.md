# Installation

## [Minimum Requirements](installation.md#minimum-requirements) <a href="#minimum-requirements" id="minimum-requirements"></a>

The SDK is compatible with devices running Android 8 or higher (API version 26 or later).

## [Installation with dependency](installation.md#installation-with-dependency) <a href="#installation-with-dependency" id="installation-with-dependency"></a>

```groovy
allprojects {
    repositories {
        // Other repositories...
        maven { url "https://jitpack.io" }
    }
}

dependencies {
    implementation 'com.github.ottuco:ottu-android-checkout:2.1.5'
}
```
