## 0.9.10
- 补充了必须的ACCESS_FINE_LOCATION权限

## 0.9.9
- 移除了无用的权限申请

## 0.9.8
- 移除了Registrar

## 0.9.7
- 增加了安卓通过mac直连设备的方法

## 0.9.6
- 修复flutter_bluex.podspec文件名

## 0.9.5
- 修复方法名

## 0.9.3
- 增加了README内容

## 0.9.0
- Fixed the crash issue caused by the UUID class on higher Android versions.
- Fixed the issue with the example not running on higher Android versions.
- Added a method to retrieve already bonded devices.
- Added more intuitive functions for getting device connection status and MTU.
- Fixed the issue where the timeout setting in the original connect method would not throw in the current await process.

- 修复了在高版本安卓上由于UUID类导致的闪退问题。
- 修复了示例在高版本安卓上运行的问题。
- 增加了获取已绑定设备的方法。
- 增加了更直观的获取设备连接状态和MTU的函数。
- 修复了原来的connect方法中的timeout设置并不会在当前的await流程中抛出异常的问题。

## 0.8.0
* Migrate the plugin to null safety.

## 0.7.3
* Fix Android project template files to be compatible with protobuf-lite.
* Add experimental support for MacOS.

## 0.7.2
* Add `allowDuplicates` option to `startScan`.
* Fix performance issue with GUID initializers.

## 0.7.1+1
* Fix for FlutterBlue constructor when running on emulator.
* Return error when attempting to `discoverServices` while not connected.

## 0.7.1
* Fix incorrect value notification when write is performed.
* Add `toString` to each bluetooth class.
* Various other bug fixes.

## 0.7.0
* Support v2 android embedding.
* Various bug and documentation fixes.

## 0.6.3+1
* Fix compilation issue with iOS.
* Bump protobuf version to 1.0.0.

## 0.6.3
* Update project files for Android and iOS.
* Remove dependency on protoc for iOS.

## 0.6.2
* Add `mtu` and `requestMtu` to BluetoothDevice.

## 0.6.0+4
* Fix duplicate characteristic notifications when connection lost.
* Fix duplicate characteristic notifications when reconnecting.
* Add minimum SDK version of 18 for the plugin.
* Documentation updates.

## 0.6.0
* **Breaking change**. API refactoring with RxDart (see example).
* Log a more detailed warning at build time about the previous AndroidX migration.
* Ensure that all channel calls to the Dart side from the Java side are done on the UI thread.
  This change allows Transactions to work with upcoming Engine restrictions, which require
  channel calls be made on the UI thread. Note this is an Android only change,
  the iOS implementation was not impacted.

## 0.5.0
* **Breaking change**. Migrate from the deprecated original Android Support
  Library to AndroidX. This shouldn't result in any functional changes, but it
  requires any Android apps using this plugin to [also
  migrate](https://developer.android.com/jetpack/androidx/migrate) if they're
  using the original support library.

## 0.4.2+1
* Upgrade Android Gradle plugin to 3.3.0.
* Refresh iOS build files.

## 0.4.2
* Set the verbosity of log messages with `setLogLevel`.
* Updated iOS and Android project files.
* `autoConnect` now configurable for Android.
* Various bug fixes.

## 0.4.1
* Fixed bug where setNotifyValue wasn't properly awaitable.
* Various UI bug fixes to example app.
* Removed unnecessary intl dependencies in example app.

## 0.4.0
* **Breaking change**. Manufacturer Data is now a `Map` of manufacturer ID's.
* Service UUID's, service data, tx power level packets fixed in advertising data.
* Example app updated to show advertising data.
* Various other bug fixes.

## 0.3.4
* Updated to use the latest protobuf (^0.9.0+1).
* Updated other dependencies.

## 0.3.3
* `scan` `withServices` to filter by service UUID's (iOS).
* Error handled when trying to scan with adapter off (Android).

## 0.3.2
* Runtime permissions for Android.
* `scan` `withServices` to filter by service UUID's (Android).
* Scan mode can be specified (Android).
* Now targets the latest android SDK.
* Dart 2 compatibility.

## 0.3.1
* Now allows simultaneous notifications of characteristics.
* Fixed bug on iOS that was returning `discoverServices` too early.

## 0.3.0
* iOS support added.
* Bug fixed in example causing discoverServices to be called multiple times.
* Various other bug fixes.

## 0.2.4
* **Breaking change**. Upgraded to Gradle 4.1 and Android Studio Gradle plugin
  3.0.1. Older Flutter projects need to upgrade their Gradle setup as well in
  order to use this version of the plugin. Instructions can be found
  [here](https://github.com/flutter/flutter/wiki/Updating-Flutter-projects-to-Gradle-4.1-and-Android-Studio-Gradle-plugin-3.0.1).

## 0.2.3
* Bug fixes

## 0.2.2
* **Breaking changes**:
* `startScan` renamed to `scan`.
* `ScanResult` now returns a `BluetoothDevice`.
* `connect` now takes a `BluetoothDevice` and returns Stream<BluetoothDeviceState>.
* Added parameter `timeout` to `connect`.
* Automatic disconnect on deviceConnection.cancel().

## 0.2.1
* **Breaking change**. Removed `stopScan` from API, use `scanSubscription.cancel()` instead.
* Automatically stops scan when `startScan` subscription is canceled (thanks to @brianegan).
* Added `timeout` parameter to `startScan`.
* Updated example app to show new scan functionality.

## 0.2.0

* Added state and onStateChanged for BluetoothDevice.
* Updated example to show new functionality.

## 0.1.1

* Fixed image for pub.dartlang.org.

## 0.1.0

* Characteristic notifications/indications.
* Merged in Guid library, removed from pubspec.yaml.

## 0.0.1 - September 1st, 2017

* Initial Release.
