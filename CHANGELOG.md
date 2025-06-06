# MMKV Change Log
## v2.2.2 / 2025-05-08
This is a hot fix version mainly **for Android/Linux platforms**. It’s highly recommended for v2.2.0~v2.2.1 users.
### Changes for All platforms
* Improve file lock consistency for Mayfly FD MMKV instances.

### Android
* Fix a potential ANR on multi-process mode MMKV init/creation.

### POSIX
* Fix a potential ANR on multi-process mode MMKV init/creation on Linux.

### iOS/macOS
* Retain the callback handler to prevent a potential short-lived callback handler from crashing.

### Win32
* Improve efficiency on MMKV instance init/creation.

## v2.2.1 / 2025-04-25
### Changes for All platforms
* Add `importFrom()`.

### Android
* Fix a bug on initialization.

## v2.2.0 / 2025-04-24
We introduce the **Mayfly FD** (short-lived file descriptor) enhancement, reducing MMKV's fd footprint by half and more. For a single-process mode MMKV instance, the fd footprint is reduced to zero (except Android/OHOS, details below). For a multi-process mode MMKV instance, the fd footprint is reduced by half, for we still need a long-lived fd to inter-process lock the shared memory.
### Changes for All platforms
* Add **Mayfly FD** (short-lived file descriptor) enhancement.
* Improve multi-process access efficiency by about 20%.
* Add `checkExist()` to check if a MMKV instance exists on disk.
* Drop deprecated armv7 AES hardware acceleration.

### Android
* Reduce the fd footprint by half. For a single-process mode MMKV instance, we still need a long-lived fd to support the **process-mode checker** and **legacy name upgrading**. In the far future, when all legacy names have been upgraded, we might drop the long-lived fd like other platforms.
* Upgrade Android compileSdk/targetSdk to 35, NDK to 28.1, Java to 11, Gradle to 8.11.1, and AGP to 8.9.2.

### HarmonyOS NEXT
* Reduce the fd footprint by half. For a single-process mode MMKV instance, we still need a long-lived fd to support the **legacy name upgrading**. In the far future, when all legacy names have been upgraded, we might drop the long-lived fd like other platforms.
* Drop `checkProcessMode()`, it’s never been used.
* Improve obfuscation configuration with relative path.

### iOS
* Add `+mmkvGroupPath` to get the group folder for MMKV multi-process storage.

### Flutter
* Add `isFileValid()`.
* Add `groupPath()` on iOS.
* Fix `checkContentChangedByOuterProcess()` not working bug.

## v2.1.0 / 2025-02-18
Happy Chinese New Year!  
This is a **breaking change version for the Android/OHOS** platform. Read the change log bellow and upgrade carefully.
### Changes for All platforms
* Add the `NameSpace` feature that easily supports customizing a root directory.
* Add protection from bad disk records of MMKV files.
* Fix FileLock not being unlocked on destruction.
* Improve directory creation on `ReadOnly` mode.

### Android
* **Breaking change**: Migrate legacy MMKV in a custom directory to normal MMKV. Historically Android/OHOS mistakenly use mmapKey as mmapID, which will be problematic with the `NameSpace` feature. Starting from v2.1.0, MMKV will try to migrate them back to normal when possible.  
It's highly recommended that you **upgrade to v2.0.2/v1.3.11 first** with **forward support** of normal MMKV in a custom directory.
* Supports using MMKV directly in C++ code.
* Improve inter-process locking by using `F_OFD_SETLK` instead of `F_SETLK`.
* Add *experimental* protection from bad disk records of MMKV files.

### HarmonyOS NEXT
* **Breaking change**: Migrate legacy MMKV in a custom directory to normal MMKV. Historically Android/OHOS mistakenly use mmapKey as mmapID, which will be problematic with the `NameSpace` feature. Starting from v2.1.0, MMKV will try to migrate them back to normal when possible.  
It's highly recommended that you **upgrade to v2.0.2/v1.3.11 first** with **forward support** of normal MMKV in a custom directory.
* Supports using MMKV directly in C++ code.
* Improve inter-process locking by using `F_OFD_SETLK` instead of `F_SETLK`.
* Add *experimental* protection from bad disk records of MMKV files.

### iOS/macOS
* Upgrade to iOS 13, macOS 10.15, and watchOS 6 to support the `NameSpace` functionality.
* Supports using MMKV directly in C++ code.
* Drop the background `mlock()` protection given that we are iOS 13+.
* Add *tested* protection from bad disk records of MMKV files.
* Fix a package error when using MMKV by submodule.

### Flutter
* Remove unused imports and fix deprecated implementations. 
* Support flutter 3.29.

### Win32
* Add *tested* protection from bad disk records of MMKV files.

### POSIX
* Improve inter-process locking by using `F_OFD_SETLK` instead of `F_SETLK`.
* Add *experimental* protection from bad disk records of MMKV files.
* Fix a compile error on the GNU toolchain.

### Golang
* Fix a link error in the armv8 arch.
* Improve multi-platform building.

## v2.0.2 / 2024-12-27
Mary holiday and a happy new year!
### Changes for All platforms
* Fix a bug that MMKV might fail to backup/restore across different filesystems.
* Add protection from invalid value size of auto-key-expire mmkv.

### Android
* If the running App is 32-bit only, warn about it (by throwing `UnsupportedArchitectureException`) before trying to load native lib.
* Add forward support for the correct filename with a custom root path.

### HarmonyOS NEXT
* Obfuscation fully supported.
* Use atomic file rename on OHOS.
* Add forward support for the correct filename with a custom root path.

### Win32
* Only `mmap()` on `ftruncate()`/`zeroFillFile()` failure iff we have a valid file mapping before.

## v2.0.1 / 2024-11-12
**This is a hotfix release.**
### Changes for All platforms
* Fix a bug that might cause MMKV to become dead-locked for other threads after decoding container-type values. The affected platforms and value types are listed below. So don't be surprised if you find no update on the unaffected platforms.

### HarmonyOS NEXT
* Fix a bug that MMKV might become dead-locked for other threads after `decodeStringSet()` / `decodeNumberSet` / `decodeBoolSet` or decoding `TypedArray`.

### Flutter
* Fix the bug on HarmonyOS NEXT listed above. A version named v2.0.1 was added to fix the Android version conflict between the LTS series & v2.0. Thanks to the federated plugins framework, only the underlying `mmkv_ohos` plugin is upgraded, the `mmkv` plugin stays the same.

### POSIX
* Fix a bug that MMKV might become dead-locked for other threads after decoding `std::vector<T>` or `std::span<T>` values.

## v2.0.0 / 2024-10-21
**This release is a breaking change release, especially for Android.**
### Changes for All platforms
* Add **readonly mode** support.
* Fix a compile error when `MMKV_DISABLE_CRYPT` is defined and MMKV is built by source in DEBUG.

### Android
* Support 16K page size for Android 15.
* Drop the support of **32-bit ABI**.
* Bump the mini **SDK level to API 23**.

### iOS / macOS
* Add Mac Catalyst support

### Flutter
* Add add log/error/content-change callback.

### HarmonyOS NEXT
* Add add log/error/content-change callback.
* Support obfuscation. For the time being, you will have to manually copy the content of MMKV's [consumer-rules.txt](https://github.com/Tencent/MMKV/blob/master/OpenHarmony/MMKV/consumer-rules.txt) into your App's obfuscation-rules.txt.

### Win32
* Replace `random()` with `rand()` to fix a compile error.

## v1.3.9 / 2024-07-26
**This is a Long Term Support (LTS) release.**
### Changes for All platforms
* Fix a data corruption bug on an encrypted MMKV with only one key value stored.
* Make encryption more resilient from brute force cracking.
* Fix a bug that `pthread_mutex` is not being destroyed correctly.

### Android
* Use an alternative way to get the process name to avoid potential App review issues.
* Upgrade to NDK 26.3.11579264.

### Flutter
* Add support for HarmonyOS NEXT. In fact, a temp version named v1.3.8 adds this support with the native lib of v1.3.7. To avoid potential confusion, bump both versions to v1.3.9.

## v1.3.7 / 2024-07-08
### Android & Flutter
**This Long Term Support (LTS) release** primarily reintroduces support for the ARMv7 architecture and lowers the minimum SDK version requirement to 21. Please note that only critical bug fixes will be applied to the 1.3.x series. New features will be introduced in version 2.0 and later, which will discontinue support for 32-bit architectures and raise the minimum SDK version requirement to 23.

### For other platforms
This is exactly the same as v1.3.6, so don't be surprised if you don't see this version in CocoaPods or OHPM.

## v1.3.6 / 2024-07-05
### Changes for All platforms
* The Core library now upgrades the C++ standard from C++17 to C++20 to make the most of morden C++ feature such as `Concept` & unordered containers with `std::string_view` as key. From now on, if you build MMKV by source (iOS, Windows, POSIX, etc), you will need a C++ compiler that supports the C++20 standard. If you only use MMKV in binary (Android, OHOS, etc), this upgrade of the C++ compiler is not required.
* The key type changes from `std::string` to `std::string_view`, to avoid unnecessary string construction and destruction when the key is a raw string.

### Android
* Use the latest ashmem API if possible.
* Use the latest API to get the device API level.

### Flutter
* MMKV will try to load libmmkv.so before Dart code, to reduce the error of loading library in Android.

### HarmonyOS NEXT
* Fix a bug that a `String` value might get truncated on encoding.
* MMKV returns `undefined` when a key does not exist, previously a default value of the type (`false` for `boolean`, `0` for `number`, etc) is returned.
* Add the feature to encode/decode a `float` value.
* Add the feature to encode/decode a `TypedArray` value.
* Support encoding a part of an `ArrayBuffer`.

### iOS/macOS
* Hide the default `NSObject.initialize()` from Swift/ObjC to prevent potential misuse.

### POSIX
* Support encode/decode `std::vector<T>` or `std::span<T>` values, with T as primitive types.
* Fix a compile error on Linux env.
* Fix a compile error on the GNU compiler.
* Fix a compile error with some old version of zlib (on CentOS).

### Python
* Python now runs on Windows. Check out the latest [wiki](https://github.com/Tencent/MMKV/wiki/python_setup) for instructions.

### Windows
* Support encode/decode `std::vector<T>` or `std::span<T>` values, with T as primitive types.
* Python now runs on Windows. Check out the latest [wiki](https://github.com/Tencent/MMKV/wiki/python_setup) for instructions.

## v1.3.5 / 2024-04-24
### HarmonyOS NEXT
* This is the first official support of HarmonyOS NEXT.
* Most things actually work!
* Checkout the [wiki](https://github.com/Tencent/MMKV/wiki/ohos_setup) for more.

### Flutter
* Migrate to federated plugins to avoid the iOS rename headache. From now on, no more renaming from `mmkv` to `mmkvflutter` is needed.
* Bump iOS Deployment Target to iOS 12.
* Bump Android minSdkVersion to 23.

### iOS & macOS
* Avoid using so-called privacy APIs (`lstat()`, `fstat()`, `NSUserDefaults`).
* Bump iOS Deployment Target to iOS 12.

### Android
* Bump minSdkVersion to 23.
* Drop armv7 & x86 support.

### POSIX
* Use the embedded libz when libz can not be found.
* Fix compile error when building with gcc.

### Windows
* Support x64 architecture.

## v1.3.4 / 2024-03-15
### Changes for All platforms
* Make `trim()` more robust in multi-process mode.

### iOS & macOS
* Support visionOS.

### POSIX
* Fix a compile error on `::unlink()`.

## v1.3.3 / 2024-01-25
### Changes for All platforms
* Add `removeStorage()` static method to safely delete underlying files of an MMKV instance.
* Add protection from a potential crash of a multi-process MMKV loading due to the MMKV file not being valid.
* Add back the lazy load feature. It was first introduced in v1.2.16. But it was rollbacked in v1.3.0 of potential ANR & file corruption. Now it's clear that the bug was caused by something else, it's time to bring it back.
* **Optimize loading speed** by using shared inter-process lock unless there's a need to truncate the file size, which is rare.
* Make these two lately added features **more robust**: customizing the initial file size & optimizing write speed when there's only one key inside MMKV.

### Android
* Fix a bug that `null` is returned when the value is in fact an empty ByteArray.
* Fix AGP >= 8 package namespace error.
* Fix the `FastNative` naming conflict.
* Upgrade to SDK 34.
* Upgrade `androidx.annotation`  to v1.7.1.

### iOS & macOS
* On the Xcode 15 build, an App will crash on iOS 14 and below. Previously we have recommended some workarounds (check the v1.3.2 release note for details). Now you can use Xcode 15.1 to fix this issue.
* Fix a bug that the multi-process mode won't configure correctly. It was introduced in v1.3.2.
* Fix a macro naming conflict.
* Avoid using a so-called privacy API when creating temp files.

### POSIX
* Fix a compile error on `memcpy()`.

### Golang
* Fix a compile error when `MMKV_DISABLE_CRYPT` is on.

## v1.3.2 / 2023-11-20
Among most of the features added in this version, the credit goes to @kaitian521.
### Changes for All platforms
* Add the feature of customizing the **initial file size** of an MMKV instance.
* **Optimize write speed** when there's only one key inside MMKV, the new key is the same as the old one, and MMKV is in `SINGLE_PROCESS_MODE`.
* **Optimize write speed** by overriding from the beginning of the file instead of append in the back, when there's zero key inside MMKV, and MMKV is in `SINGLE_PROCESS_MODE`.
* Add the feature of `clearAll()` with keeping file disk space unchanged, **reducing the need to expand file size** on later insert & update operations. This feature is off by default, you will have to call it with relative params or newly added methods. 
* Add the feature of **comparing values before setting/encoding** on the same key.
* Fix a potential bug that the MMKV file will be invalid state after a successful expansion but a failure `zeroFill()`, will lead to a crash.
* Fix a potential crash due to other module/static lib turn-off **RTTI**, which will cause MMKV to fail to catch `std::exception`.
* Fix several potential crash due to the MMKV file not being valid.

### Android
* Use the `-O2` optimization level by default, which will **reduce native lib size** and improve read/write speed a little bit.
* Experimantal use `@fastNative` annotation on `enableCompareBeforeCompare()` to speed up JNI call.

### iOS & macOS
* Optimize auto-clean logic to **reduce lock waiting time**.
* Turn-off mlock() protection in background on iOS 13+. We have **verified it on WeChat** that the protection is no longer needed from at least iOS 13. Maybe iOS 12 or older is also not needed, but we don't have the chance to verify that because WeChat no longer supports iOS 12.

#### Known Issue
* On Xcode 15 build, App will crash on iOS 14 and below. The bug is introduced by Apple's new linker. The official solutions provided by Apple are either:
  * Drop the support of iOS 14.
  * Add `-Wl,-weak_reference_mismatches,weak` or `-Wl,-ld_classic` options to the `OTHER_LDFLAGS` build setting of Xcode 15. Note that these options are **not recognized** by older versions of Xcode.
  * Use older versions of Xcode, or **wait for Xcode 15.2**.

## v1.3.1 / 2023-8-11
This is a hotfix version. It's **highly recommended** that v1.2.16 & v1.3.0 users upgrade as soon as possible.

### Changes for All platforms
* Fix a critical bug that might cause multi-process MMKV corrupt. This bug was introduced in v1.2.16.
* Add the ability to filter expired keys on `count()` & `allKeys()` methods when auto key expiration is turn on.
* Reduce the `msync()` call on newly created MMKV instances.

### iOS & macOS
* Fix a bug that NSKeyedArchive object might fail to decode when auto key expiration is turn on.

## v1.3.0 / 2023-06-14
### Changes for All platforms
* Add auto key expiration feature. Note that this is a breaking change, once upgrade to auto expiration, the MMKV file is not valid for older versions of MMKV (v1.2.16 and below) to correctly operate.
* Roll back the lazy load optimization due to reported ANR issues. It was introduced in v1.2.16.

### iOS & macOS
* Fix a potential memory leak on setting a new value for an existing key.
* Upgrade min support target to iOS 11 / macOS 10.13 / tvOS 13 / watchOS 4.

### Windows
* Fix a bug that might fail to truncate the file size to a smaller size in some cases.

### Flutter
* The version of MMKV for Flutter is now the same as the MMKV native library.
* Starting from v1.3.0, Flutter for Android will use `com.tencent:mmkv`. Previously it's `com.tencent:mmkv-static`. It's the same as `com.tencent:mmkv` starting from v1.2.11.

## v1.2.16 / 2023-04-20
### Changes for All platforms
* Optimization: The actual file content is lazy loaded now, saving time on MMKV instance creation, and avoiding lock waiting when a lot of instances are created at the same time.
* Fix a bug when restoring a loaded MMKV instance the meta file might mistakenly report corrupted.

### Android
* Optimization: Remove unnecessary binder call on main process instantiation.

### Flutter
* Fix a crash on decoding an empty list.
* Remove deprecated dependence.
* Make the script more robust to fix the iOS Flutter plugin name.

### Windows
* Fix a string format bug on the MinGW64 environment.

### golang
* Fix a build error on 32-bit OS.

## v1.2.15 / 2023-01-12
### Changes for All platforms
* Log handler now handles all logs from the very beginning, especially the logs in initialization.
* Log handler register method is now deprecated. It's integrated with `initialize()`.
* Fix a bug that `lock()`/`unlock()`/`try_lock()` is not thread-safe.

### Flutter
* Reduce the privacy info needed to obtain android `sdkInt`, avoid unnecessary risk on Android App Review.

### iOS & macOS
* Fix a compile error on macOS.
* Fix a bug that some ObjC exceptions are not being caught.
* Add assert on nil MMKV base path, protect from mis-using MMKV in global variable initialization.
* Starting from v1.2.15, one must call `+[MMKV initializeMMKV:]` manually before calling any MMKV methods.

### golang
* Fix a compile error on GCC.

### Windows
* Support CMake project on Windows.

## v1.2.14 / 2022-08-10
### Changes for All platforms
* Fix a bug that `MMKV.getXXX()` may return invalid results in multi-process mode.

### Android
* Return `[]` instead of `null` on empty `StringSet` from `MMKV.decodeStringSet()` methods.
* Upgrade Android Compile & Target SDK to `32`.

### iOS
* Protect from the crash in `-[MMKV getObject:forKey:]` method when the key-value doesn't exist.

## v1.2.13 / 2022-03-30

### Android
* Fix crash on using Ashmem while `MMKV_DISABLE_CRYPT` macro is defined.

### iOS
* Add ability to retrieve key existece while getting value, aka `-[MMKV getXXX:forKey:hasValue:]` methods.

### POSIX
* Add ability to retrieve key existece while getting value, aka `MMKV::getXXX(key, defaultValue, hasValue)` methods.

### Windows
* Add ability to retrieve key existece while getting value, aka `MMKV::getXXX(key, defaultValue, hasValue)` methods.

## v1.2.12 / 2022-01-17
### Changes for All platforms
* Fix a bug that a subsequential `clearAll()` call may fail to take effect in multi-process mode.
* Hide some OpenSSL symbols to prevent link-time symbol conflict, when an App somehow also static linking OpenSSL.

### Android
* Upgrade `compileSdkVersion` & `targetSdkVersion` from `30` to `31`.

## v1.2.11 / 2021-10-26

### Android
* Due to increasing report about crash inside STL, we have decided to make MMKV **static linking** `libc++` **by default**. Starting from v1.2.11, `com.tencent:mmkv-static` is the same as `com.tencent:mmkv`.
* For those still in need of MMKV with shared linking of `libc++_shared`, you could use `com.tencent:mmkv-shared` instead.
* Add backup & restore ability.

### iOS / macOS
* Add backup & restore ability.
* Support tvOS.
* Fix a compile error on some old Xcode.

### Flutter (v1.2.12)
* Add backup & restore ability.

### POSIX / golang / Python
* Add backup & restore ability.
* Fix a compile error on Gentoo.

### Windows
* Add backup & restore ability.

## v1.2.10 / 2021-06-25
This version is mainly for Android & Flutter.  

### Android
* Complete **JavaDoc documentation** for all public methods, classes, and interfaces. From now on, you can find the [API reference online](https://javadoc.io/doc/com.tencent/mmkv).
* Drop the support of **armeabi** arch. Due to some local build cache mistake, the last version (v1.2.9) of MMKV still has an unstripped armeabi arch inside. This is fixed.
* Change `MMKV.mmkvWithID()` from returning `null` to throwing exceptions on any error.
* Add `MMKV.actualSize()` to get the actual used size of the file.
* Mark `MMKV.commit()` & `MMKV.apply()` as deprecated, to avoid some misuse after migration from SharedPreferences to MMKV.

### Flutter (v1.2.11)
* Bug Fixed: When building on iOS, occasionally it will fail on symbol conflict with other libs. We have renamed all public native methods to avoid potential conflict.
* Keep up with MMKV native lib v1.2.10.

## v1.2.9 / 2021-05-26
This version is mainly for Android & Flutter.  

### Android
* Drop the support of **armeabi** arch. As has been mention in the last release, to avoid some crashes on the old NDK (r16b), and make the most of a more stable `libc++`, we have decided to upgrade MMKV's building NDK in this release. That means we can't support **armeabi** anymore. Those who still in need of armeabi can **build from sources** by following the [instruction in the wiki](https://github.com/Tencent/MMKV/wiki/android_setup).

We really appreciate your understanding.

### Flutter (v1.2.10)
* Bug Fixed: When calling `MMKV.encodeString()` with an empty string value on Android, `MMKV.decodeString()` will return `null`.
* Bug Fixed: After **upgrading** from Flutter 1.20+ to 2.0+, calling `MMKV.defaultMMKV()` on Android might fail to load, you can try calling `MMKV.defaultMMKV(cryptKey: '\u{2}U')` with an **encrytion key** '\u{2}U' instead.
* Keep up with MMKV native lib v1.2.9.

## v1.2.8 / 2021-05-06
This will be the last version that supports **armeabi arch** on Android. To avoid some crashes on the old NDK (r16b), and make the most of a more stable `libc++`, we have decided to upgrade MMKV's building NDK in the next release. That means we can't support **armeabi** anymore.  

We really appreciate your understanding.

### Android
* Migrate MMKV to Maven Central Repository. For versions older than v1.2.7 (including), they are still available on JCenter.
* Add `MMKV.disableProcessModeChecker()`. There are some native crash reports due to the process mode checker. You can disable it manually.
* For the same reason described above (native crashed), MMKV will now turn off the process mode checker on a non-debuggable app (aka, a release build).
* For MMKV to detect whether the app is debuggable or not, when calling `MMKV.initialize()` to customize the root directory, a `context` parameter is required now.

### iOS / macOS
* Min iOS support has been **upgrade to iOS 9**.
* Support building by Xcode 12.

### Flutter (v1.2.9)
* Support null-safety.
* Upgrade to flutter 2.0.
* Fix a crash on the iOS when calling `encodeString()` with an empty string value.

**Known Issue on Flutter**  

* When calling `encodeString()` with an empty string value on Android, `decodeString()` will return `null`. This bug will be fixed in the next version of Android Native Lib. iOS does not have such a bug.

### Windows
* Fix a compile error on Visual Studio 2019.

## v1.2.7 / 2020-12-25
Happy holidays everyone!
 
### Changes for All platforms
* Fix a bug when calling `sync()` with `false ` won't do `msync()` asynchronous and won't return immediately.

### Android
* Fix an null pointer exception when calling `putStringSet()` with `null`.
* Complete review of all MMKV methods about Java nullable/nonnull annotation.
* Add API for `MMKV.initialize()` with both `Context` and `LibLoader` parammeters.

### Flutter (v1.2.8)
* Fix a crash on the iOS simulator when accessing the default MMKV instance.
* Fix a bug on iOS when initing the default MMKV instance with a crypt key, the instance is still in plaintext.

### Golang
Add golang for POSIX platforms. Most things actually work!. Check out the [wiki](https://github.com/Tencent/MMKV/wiki/golang_setup) for information.

## v1.2.6 / 2020-11-27
### Changes for All platforms
* Fix a file corruption when calling `reKey()` after `removeKeys()` has just been called.

### Android
* Fix compile error when `MMKV_DISABLE_CRYPT` is set.
* Add a preprocess directive `MMKV_DISABLE_FLUTTER` to disable flutter plugin features. If you integrate MMKV by source code, and if you are pretty sure the flutter plugin is not needed, you can turn that off to save some binary size.

### Flutter (v1.2.7)
Add MMKV support for **Flutter** on iOS & Android platform.  Most things actually work!  
Check out the [wiki](https://github.com/Tencent/MMKV/wiki/flutter_setup) for more info.

## v1.2.5 / 2020-11-13
This is a pre-version for Flutter. The official Flutter plugin of MMKV will come out soon. Stay Tune!

### iOS / macOS
* Fix an assert error of encrypted MMKV when encoding some `<NSCoding>` objects.
* Fix a potential leak when decoding duplicated keys from the file.
* Add `+[MMKV pageSize]`, `+[MMKV version]` methods.
* Add `+[MMKV defaultMMKVWithCryptKey:]`, you can encrypt the default MMKV instance now, just like the Android users who already enjoy this for a long time.
* Rename `-[MMKV getValueSizeForKey:]` to `-[MMKV getValueSizeForKey: actualSize:]` to align with Android interface.

### Android
* Fix a potential crash when getting MMKV instances in multi-thread at the same time.
* Add `MMKV.version()` method.

## v1.2.4 / 2020-10-21
This is a hotfix mainly for iOS.

### iOS / macOS
* Fix a decode error of encrypted MMKV on some devices.

### Android
* Fix a potential issue on checking `rootDir` in multi-thread while MMKV initialization is not finished.

## v1.2.3 / 2020-10-16
### Changes for All platforms
* Fix a potential crash on 32-bit devices due to pointer alignment issue.
* Fix a decode error of encrypted MMKV on some 32-bit devices.

### iOS / macOS
* Fix a potential `crc32()` crash on some kind of arm64 devices.
* Fix a potential crash after calling `+[MMKV onAppTerminate]`.

### Android
* Fix a rare lock conflict on `checkProcessMode()`.

### POSIX
Add MMKV support for **Python** on POSIX platforms.  Most things actually work!  
Check out the [wiki](https://github.com/Tencent/MMKV/wiki/python_setup) for more info.

## v1.2.2 / 2020-07-30

### iOS / macOS
* Add auto clean up feature. Call `+[MMKV enableAutoCleanUp:]` to enable auto cleanup MMKV instances that not been accessed recently.
* Fix a potential crash on devices under iPhone X.

### Android
* Add multi-process mode check. After so many issues had been created due to mistakenly using MMKV in multi-process mode in Android, this check is added. If an MMKV instance is accessed with `SINGLE_PROCESS_MODE` in multi-process, an `IllegalArgumentException` will be thrown.

### POSIX
* Add support for armv7 & arm64 arch on Linux.

## v1.2.1 / 2020-07-03
This is a hotfix release. Anyone who has upgraded to v1.2.0 should upgrade to this version **immediately**.

* Fix a potential file corruption bug when writing a file that was created in versions older than v1.2.0. This bug was introduced in v1.2.0.
* Add a preprocess directive `MMKV_DISABLE_CRYPT` to turn off MMKV encryption ability once and for all. If you integrate MMKV by source code, and if you are pretty sure encryption is not needed, you can turn that off to save some binary size.
* The parameter `relativePath` (customizing a separate folder for an MMKV instance), has been renamed to `rootPath`. Making it clear that an absolute path is expected for that parameter.

### iOS / macOS
* `-[MMKV mmkvWithID: relativePath:]` is deprecated. Use `-[MMKV mmkvWithID: rootPath:]` instead. 
* Likewise, `-[MMKV mmkvWithID: cryptKey: relativePath:]` is deprecated. Use `-[MMKV mmkvWithID: cryptKey: rootPath:]` instead. 

## v1.2.0 / 2020-06-30
This is the second **major version** of MMKV. Everything you call is the same as the last version, while almost everything underneath has been improved. 

* **Reduce Memory Footprint**. We used to cache all key-values in a dictionary for efficiency. From now on we store the offset of each key-value inside the mmap-memory instead, **reducing memory footprint by almost half** for (non-encrypt) MMKV. And the accessing efficiency is almost the same as before. As for encrypted MMKV, we can't simply store the offset of each key-value, the relative encrypt info needs to be stored as well. That will be too much for small key-values. We only store such info for large key-values (larger than 256B).
* **Improve Writeback Efficiency**. Thanks to the optimization above, we now can implement writeback by simply calling **memmove()** multiple times. Efficiency is increased and memory consumption is down.
* **Optimize Small Key-Values**. Small key-values of encrypted MMKV are still cached in memory, as all the old versions did. From now on, the struct `MMBuffer` will try to **store small values in the stack** instead of in the heap, saving a lot of time from `malloc()` & `free()`. In fact, all primitive types will be store in the stack memory.

All of the improvements above are available to all supported platforms. Here are the additional changes for each platform.

### iOS / macOS
* **Optimize insert & delete**. Especially for inserting new values to **existing keys**, or deleting keys. We now use the UTF-8 encoded keys in the mmap-memory instead of live encoding from keys, cutting the cost of string encoding conversion.
* Fix Xcode compile error on some projects.
* Drop the support of iOS 8. `thread_local` is not available on iOS 8. We choose to drop support instead of working around because iOS 8's market share is considerably small.

### POSIX
* It's known that GCC before 5.0 doesn't support C++17 standard very well. You should upgrade to the latest version of GCC to compile MMKV.

## v1.1.2 / 2020-05-29

### Android / iOS & macOS / Windows / POSIX

* Fix a potential crash after `trim()` a multi-process MMKV instance.
* Improve `clearAll()` a bit.

## v1.1.1 / 2020-04-13

### iOS / macOS

* Support WatchOS.
* Rename `+[MMKV onExit]` to `+[MMKV onAppTerminate]`, to avoid naming conflict with some other OpenSource projects.
* Make background write protection much more robust, fix a potential crash when writing meta info in background.
* Fix a potential data corruption bug when writing a UTF-8 (non-ASCII) key.

### Android

* Fix a crash in the demo project when the App is hot reloaded.
* Improve wiki & readme to recommend users to init & destruct MMKV in the `Application` class instead of the `MainActivity` class.

### POSIX
* Fix two compile errors with some compilers.

## v1.1.0 / 2020-03-24
This is the first **major breaking version** ever since MMKV was made public in September 2018, introducing bunches of improvement. Due to the Covid-19, it has been delayed for about a month. Now it's finally here! 

* **Improved File Recovery Strategic**. We store the CRC checksum & actual file size on each sync operation & full write back, plus storing the actual file size in the same file(aka the .crc meta file) as the CRC checksum. Base on our usage inside WeChat on the iOS platform, it cuts the file **corruption rate down by almost half**.
* **Unified Core Library**. We refactor the whole MMKV project and unify the cross-platform Core library. From now on, MMKV on iOS/macOS, Android, Windows all **share the same core logic code**. It brings many benefits such as reducing the work to fix common bugs, improvements on one platform are available to other platforms immediately, and much more.
* **Supports POSIX Platforms**. Thanks to the unified Core library, we port MMKV to POSIX platforms easily.
* **Multi-Process Access on iOS/macOS**. Thanks to the unified Core library, we add multi-process access to iOS/macOS platforms easily.
* **Efficiency Improvement**. We make the most of armv8 ability including the AES & CRC32 instructions to tune **encryption & error checking speed up by one order higher** than before on armv8 devices. There are bunches of other speed tuning all around the whole project.

Here are the old-style change logs of each platform.

### iOS / macOS
* Adds **multi-process access** support. You should initialize MMKV by calling `+[MMKV initializeMMKV: groupDir: logLevel:]`, passing your **app group id**. Then you can get a multi-process instance by calling `+[MMKV mmkvWithID: mode:]` or `+[MMKV mmkvWithID: cryptKey: mode:]`, accessing it cross your app & your app extensions.
* Add **inter-process content change notification**. You can get MMKV changes notification of other processes by implementing `- onMMKVContentChange:` of `<MMKVHandler>` protocol.
* **Improved File Recovery Strategic**. Cuts the file corruption rate down by almost half. Details are above.
* **Efficiency Improvement**. Encryption & error checking speed are up by one order higher on armv8 devices(aka iDevice including iPhone 5S and above). Encryption on armv7 devices is improved as well. Details are ahead.
* Other speed improvements. Refactor core logic using **MRC**, improve std::vector `push_back()` speed by using **move constructors** & move assignments.
* `+[MMKV setMMKVBasePath:]` & `+[MMKV setLogLevel:]` are marked **deprecated**. You should use `+[MMKV initializeMMKV:]` or `+[MMKV initializeMMKV: logLevel:]` instead.
* The `MMKVLogLevel` enum has been improved in Swift. It can be used like `MMKVLogLevel.info` and so on.

### Android
* **Improved File Recovery Strategic**. Cuts the file corruption rate down by almost half. Details are above.
* **Efficiency Improvement**. Encryption & error checking speed are up by one order higher on armv8 devices with the `arm64-v8a` abi. Encryption on `armeabi` & `armeabi-v7a` is improved as well. Details are ahead.
* Add exception inside core encode & decode logic, making MMKV much more robust.
* Other speed improvements. Improve std::vector `push_back()` speed by using **move constructors** & move assignments.

### Windows
* **Improved File Recovery Strategic**. Cuts the file corruption rate down by almost half. Details are above.
* Add exception inside core encode & decode logic, making MMKV much more robust.
* Other speed improvements. Improve std::vector `push_back()` speed by using **move constructors** & move assignments.

### POSIX
* Most things actually work! We have tested MMKV on the latest version of Linux(Ubuntu, Arch Linux, CentOS, Gentoo), and Unix(macOS, FreeBSD, OpenBSD) on the time v1.1.0 is released.

## v1.0.24 / 2020-01-16

### iOS / macOS
What's new  

* Fix a bug that MMKV will fail to save any key-values after calling `-[MMKV clearMemoryCache]` and then `-[MMKV clearAll]`.
* Add `+[MMKV initializeMMKV:]` for users to init MMKV in the main thread, to avoid an iOS 13 potential crash when accessing `UIApplicationState` in child threads.
* Fix a potential crash when writing a uniquely constructed string.
* Fix a performance slow down when acquiring MMKV instances too often.
* Make the baseline test in MMKVDemo more robust to NSUserDefaults' caches.

### Android
What's new  

* Fix `flock()` bug on ashmem files in Android.
* Fix a potential crash when writing a uniquely constructed string.
* Fix a bug that the MMKVDemo might crash when running in a simulator.

### Windows
* Fix a potential crash when writing a uniquely constructed string or data.

## v1.0.23 / 2019-09-03

### iOS / macOS
What's new  

* Fix a potential security leak on encrypted MMKV.

### Android
What's new  

* Fix a potential security leak on encrypted MMKV.
* Fix filename bug when compiled on Windows environment.
* Add option for decoding String Set into other `Set<>` classes other than the default `HashSet<String>`, check `decodeStringSet()` for details.
* Add `putBytes()` & `getBytes()`, to make function names more clear and consistent.
* Add notification of content changed by other process, check the new `MMKVContentChangeNotification<>` interface & `checkContentChangedByOuterProcess()` for details.

### Windows
What's new  

* Fix a potential security leak on encrypted MMKV.
* Fix `CriticalSection` init bug.

## v1.0.22 / 2019-06-10

### iOS / macOS
What's new  

* Fix a bug that MMKV will corrupt while adding just one key-value, and reboot or clear memory cache. This bug was introduced in v1.0.21.

### Android
What's new  

* Fix a bug that MMKV will corrupt while adding just one key-value, and reboot or clear memory cache. This bug was introduced in v1.0.21.

### Windows
What's new  

* Fix a bug that MMKV will corrupt while adding just one key-value, and reboot or clear memory cache. This bug was introduced in v1.0.21.

## v1.0.21 / 2019-06-06
### iOS / macOS
What's new  

* Fix a bug that MMKV might corrupt while repeatedly adding & removing key-value with specific length. This bug was introduced in v1.0.20.

### Android
What's new  

* Fix a bug that MMKV might corrupt while repeatedly adding & removing key-value with specific length. This bug was introduced in v1.0.20.

### Windows
What's new  

* Fix a bug that MMKV might corrupt while repeatedly adding & removing key-value with specific length. This bug was introduced in v1.0.20.

## v1.0.20 / 2019-06-05
### iOS / macOS
What's new  

* Fix a bug that MMKV might crash while storing key-value with specific length.
* Fix a bug that `-[MMKV trim]` might not work properly.

### Android
What's new  

* Migrate to AndroidX library.
* Fix a bug that MMKV might crash while storing key-value with specific length.
* Fix a bug that `trim()` might not work properly.
* Fix a bug that dead-lock might be reported by Android mistakenly.
* Using `RegisterNatives()` to simplify native method naming.

### Windows
* Fix a bug that MMKV might crash while storing key-value with specific length.
* Fix a bug that `trim()` might not work properly.
* Fix a bug that `clearAll()` might not work properly.

## v1.0.19 / 2019-04-22
### iOS / macOS
What's new  

* Support Swift 5.
* Add method to get all keys `-[MMKV allKeys]`;
* Add method to synchronize to file asynchronously `-[MMKV async]`.
* Fix a pod configuration bug that might override target project's C++ setting on `CLANG_CXX_LANGUAGE_STANDARD`.
* Fix a bug that `DEFAULT_MMAP_SIZE` might not be initialized before getting any MMKV instance.
* Fix a bug that openssl's header files included inside MMKV might mess with target project's own openssl implementation.

### Android
What's new  

* Support Android Q.
* Add method to synchronize to file asynchronously `void sync()`, or `void apply()` that comes with `SharedPreferences.Editor` interface.
* Fix a bug that a buffer with length of zero might be returned when the key is not existed.
* Fix a bug that `DEFAULT_MMAP_SIZE` might not be initialized before getting any MMKV instance.


## v1.0.18 / 2019-03-14
### iOS / macOS
What's new  

* Fix a bug that defaultValue was not returned while decoding a `NSCoding` value.
* Fix a compile error on static linking MMKV while openssl is static linked too.

### Android
What's new  

* Introducing **Native Buffer**. Checkout [wiki](https://github.com/Tencent/MMKV/wiki/android_advance#native-buffer) for details.
* Fix a potential crash when trying to recover data from file length error.
* Protect from mistakenly passing `Context.MODE_MULTI_PROCESS` to init MMKV.


### Windows
* Fix a potential crash when trying to recover data from file length error.

## v1.0.17 / 2019-01-25
### iOS / macOS
What's new  

* Redirect logging of MMKV is supported now.
* Dynamically disable logging of MMKV is supported now.
* Add method `migrateFromUserDefaults ` to import from NSUserDefaults.

### Android
What's new  

* Redirect logging of MMKV is supported now.
* Dynamically disable logging of MMKV is supported now.  
  Note: These two are breaking changes for interface `MMKVHandler`, update your implementation with `wantLogRedirecting()` & `mmkvLog()` for v1.0.17. (Interface with default method requires API level 24, sigh...)
* Add option to use custom library loader `initialize(String rootDir, LibLoader loader)`. If you're facing `System.loadLibrary()` crash on some low API level device, consider using **ReLinker** to load MMKV. Example can be found in **mmkvdemo**.
* Fix a potential corruption of meta file on multi-process mode.
* Fix a potential crash when the meta file is not valid on multi-process mode.


### Windows
* Redirect logging of MMKV is supported now.
* Dynamically disable logging of MMKV is supported now.
* Fix a potential corruption of meta file on multi-process mode.

## v1.0.16 / 2019-01-04
### iOS / macOS
What's new  

* Customizing root folder of MMKV is supported now.
* Customizing folder for specific MMKV is supported now.
* Add method `getValueSizeForKey:` to get value's size of a key.

### Android
What's new  

* Customizing root folder of MMKV is supported now.
* Customizing folder for specific MMKV is supported now.
* Add method `getValueSizeForKey()` to get value's size of a key.
* Fix a potential crash when the meta file is not valid.


### Windows
MMKV for Windows is released now. Most things actually work!

## v1.0.15 / 2018-12-13
### iOS / macOS
What's new  

* Storing **NSString/NSData/NSDate** directly by calling `setString`/`getSring`, `setData`/`getData`, `setDate`/`getDate`.
* Fix a potential crash due to divided by zero.


### Android
What's new  

* Fix a stack overflow crash due to the **callback** feature introduced by v1.0.13.
* Fix a potential crash due to divided by zero.

### Windows
MMKV for Windows in under construction. Hopefully will come out in next release. For those who are interested, check out branch `dev_Windows` for the latest development.

## v1.0.14 / 2018-11-30
### iOS / macOS
What's new  

* Setting `nil` value to reset a key is supported now.
* Rename `boolValue(forKey:)` to `bool(forKey:)` for Swift.


### Android
What's new  

* `Parcelable` objects can be stored directly into MMKV now.
* Setting `null` value to reset a key is supported now.
* Fix an issue that MMKV's file size might expand unexpectly large in some case.
* Fix an issue that MMKV might crash on multi-thread removing and getting on the same key.


## v1.0.13 / 2018-11-09
### iOS / macOS
What's new  

* Special chars like `/` are supported in MMKV now. The file name of MMKV with special mmapID will be encoded with md5 and stored in seperate folder.
* Add **callback** for MMKV error handling. You can make MMKV to recover instead of discard when crc32 check fail happens.
* Add `trim` and `close` operation. Generally speaking they are not necessary in daily usage. Use them if you worry about disk / memory / fd usage.
* Fix an issue that MMKV's file size might expand unexpectly large in some case.

Known Issues

* Setting `nil` value to reset a key will be ignored. Use `remove` instead.

### Android
What's new  

* Add static linked of libc++ to trim AAR size. Use it when there's no other lib in your App embeds `libc++_shared.so`. Or if you already have an older version of `libc++_shared.so` that doesn't agree with MMKV.  
Add `implementation 'com.tencent:mmkv-static:1.0.13'` to your App's gradle setting to integrate.
* Special chars like `/` are supported in MMKV now. The file name of MMKV with special mmapID will be encoded with md5 and stored in seperate folder.
* Add **callback** for MMKV error handling. You can make MMKV to recover instead of discard when crc32 check fail happens.
* Add `trim` and `close` operation. Generally speaking they are not necessary in daily usage. Use them if you worry about disk / memory / fd usage.

Known Issues

* Setting `null` value to reset a key will be ignored. Use `remove` instead.
* MMKV's file size might expand unexpectly large in some case.

## v1.0.12 / 2018-10-18
### iOS / macOS
What's new  

* Fix `mlock` fail on some devices
* Fix a performance issue caused by mistakenly merge of test code
* Fix CocoaPods integration error of **macOS**

### Android / 2018-10-24
What's new  

* Fix `remove()` causing data inconsistency on `MULTI_PROCESS_MODE`


## v1.0.11 / 2018-10-12
### iOS / macOS
What's new  

* Port to **macOS**
* Support **NSCoding**  
You can  store NSArray/NSDictionary or any object what implements `<NSCoding>` protocol.
* Redesign Swift interface
* Some performance improvement

Known Issues

* MMKV use mmapID as its filename, so don't contain any `/` inside mmapID.
* Storing a value of `type A` and getting by `type B` may not work. MMKV does type erasure while storing values. That means it's hard for MMKV to do value-type-checking, if not impossible.

### Android 
What's new  

* Some performance improvement

Known Issues

* Getting an MMKV instance with mmapID that contains `/` may fail.  
MMKV uses mmapID as its filename, so don't contain any `/` inside mmapID.
* Storing a value of `type A` and getting by `type B` may not work.  
MMKV does type erasure while storing values. That means it's hard for MMKV to do value-type-checking, if not impossible.
* `registerOnSharedPreferenceChangeListener` not supported.  
This is intended. We believe doing data-change-listener inside a storage framework smells really bad to us. We suggest using something like event-bus to notify any interesting clients.

## v1.0.10 / 2018-09-21  

 * Initial Release
