# Android Build for PixhawkGCS (BDA-Lab Rebrand Layer)

This directory contains an Android Studio (Gradle) wrapper layer for the existing CMake/Qt project, rebranded as "PixhawkGCS" for Android deployment.

## Disclaimer

This Android layer is **purely additive** - no original source files have been modified. The existing CMakeLists.txt, resources, and all project files remain unchanged. Only the Android-facing application label has been rebranded to "PixhawkGCS" while preserving all internal project structure and functionality.

## Project Lineage

This Android wrapper is built on top of the QGroundControl (QGC) architecture present in the BDA-Lab repository, providing ground control station functionality with Android compatibility.

## Requirements

Before building the Android APK, ensure you have the following installed:

- **Qt for Android**: Qt 6.5+ with Android components
  - Qt Creator or Qt installation with Android support
  - Qt Android deployment tools
- **Android SDK**: API Level 34 (compileSdk)
  - Minimum SDK: API Level 24 (Android 7.0+)
  - Target SDK: API Level 34
- **Android NDK**: Version 25.0.8775105 or later
  - Required for CMake native build integration
  - Supports armeabi-v7a, arm64-v8a, and x86_64 architectures
- **CMake**: Version 3.28.0 or later
  - Must be available in Android SDK's CMake installation
- **Java Development Kit (JDK)**: JDK 8 or later

## Build Instructions

### Using Android Studio

1. **Import Project**: Open Android Studio and select "Import Project"
2. **Select Directory**: Choose the root directory of this repository (containing `settings.gradle.kts`)
3. **Gradle Sync**: Android Studio will automatically trigger a Gradle sync
   - Wait for sync to complete (this may take several minutes on first run)
   - Resolve any SDK/NDK path issues in the SDK Manager if prompted
4. **Build**: Once sync is complete:
   - **Development Build**: `Build > Make Project` or `Ctrl+F9`
   - **Release APK**: `Build > Generate Signed Bundle / APK` or use `Build > Build Bundle(s) / APK(s) > Build APK(s)`

### Using Command Line

```bash
# Debug build
./gradlew :app:assembleDebug

# Release build  
./gradlew :app:assembleRelease

# Install debug APK to connected device
./gradlew :app:installDebug
```

## Configuration Notes

- **Application ID**: `com.pixhawk.gcslab`
- **Display Name**: "PixhawkGCS" (user-facing only)
- **CMake Integration**: The `app/build.gradle.kts` references the root `CMakeLists.txt` via `externalNativeBuild`
- **Multi-ABI Support**: Builds for `armeabi-v7a`, `arm64-v8a`, and `x86_64` architectures
- **Qt Activity**: Uses `org.qtproject.qt.android.bindings.QtActivity` as the main Android activity

## Potential Build Issues

If the build fails, consider these common solutions:

1. **Qt Android Environment**: Ensure Qt is properly configured for Android development:
   ```bash
   # You may need to set ANDROID=ON in CMake configuration
   # Ensure Qt Android deployment tool is properly configured
   ```

2. **CMake Path Issues**: Verify CMake version and path in Android SDK Manager

3. **NDK Compatibility**: Ensure NDK version is compatible with your Qt installation

4. **SDK/NDK Paths**: Check that Android Studio has correct paths to SDK and NDK installations

## Output

- **Debug APK**: `app/build/outputs/apk/debug/app-debug.apk`
- **Release APK**: `app/build/outputs/apk/release/app-release.apk`

The generated APK will contain native libraries (if produced by the CMake build) and Qt/QML assets packaged via the Qt resource system.

## Architecture

This Android wrapper maintains the original QGroundControl architecture while providing Android-specific packaging and deployment. The CMake build system handles cross-compilation for Android targets, and Qt manages the Android activity lifecycle and resource deployment.