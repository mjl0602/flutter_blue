[![pub package](https://img.shields.io/pub/v/flutter_bluex.svg)](https://pub.dartlang.org/packages/flutter_bluex)

## THIS IS A FORK VERSION OF FLUTTER_BLUE(这是一个 fork 版本，使用前请注意！)

Since the original flutter_blue is no longer maintained, I forked the original repository and continued its maintenance for newer versions of Flutter:

- Added a method to retrieve bound (paired) devices, allowing these devices to connect without scanning(Android ONLY).
- Introduced more intuitive functions to directly obtain the device connection status and MTU, instead of getting the first status from a stream.
- Fixed a crash issue on higher versions of Android caused by a redundant UUID class.
- Corrected the issue where the timeout setting in the original connect method would not throw an exception in the current await process.
- Resolved the problem with the example not running on higher versions of Android.

由于原先的 flutter_blue 不再维护，我 fork 了原仓库并针对新版本 flutter 继续维护：

- 增加了获取已绑定设备(已配对设备)的方法，已配对设备可以不需要扫描就能连接（仅安卓）。
- 增加了更直观的直接获取设备连接状态和 MTU 的函数，而不是从流获得第一个状态。
- 修复了在高版本安卓上由于一个无用的 UUID 类导致的闪退问题。
- 修复了原来的 connect 方法中的 timeout 设置并不会在当前的 await 流程中抛出异常的问题。
- 修复了示例在高版本安卓上运行的问题。

<br>
<p align="center">
<img alt="FlutterBlue" src="https://github.com/pauldemarco/flutter_blue/blob/master/site/flutterblue.png?raw=true" />
</p>
<br><br>

## Introduction

FlutterBlue is a bluetooth plugin for [Flutter](https://flutter.dev), a new app SDK to help developers build modern multi-platform apps.

## Alpha version

This library is actively developed alongside production apps, and the API will evolve as we continue our way to version 1.0.

**Please be fully prepared to deal with breaking changes.**
**This package must be tested on a real device.**

Having trouble adapting to the latest API? I'd love to hear your use-case, please contact me.

## Cross-Platform Bluetooth LE

FlutterBlue aims to offer the most from both platforms (iOS and Android).

Using the FlutterBlue instance, you can scan for and connect to nearby devices ([BluetoothDevice](#bluetoothdevice-api)).
Once connected to a device, the BluetoothDevice object can discover services ([BluetoothService](lib/src/bluetooth_service.dart)), characteristics ([BluetoothCharacteristic](lib/src/bluetooth_characteristic.dart)), and descriptors ([BluetoothDescriptor](lib/src/bluetooth_descriptor.dart)).
The BluetoothDevice object is then used to directly interact with characteristics and descriptors.

## Usage

### Obtain an instance

```dart
FlutterBlue flutterBlue = FlutterBlue.instance;
```

### Scan for devices

```dart
// Start scanning
flutterBlue.startScan(timeout: Duration(seconds: 4));

// Listen to scan results
var subscription = flutterBlue.scanResults.listen((results) {
    // do something with scan results
    for (ScanResult r in results) {
        print('${r.device.name} found! rssi: ${r.rssi}');
    }
});

// Stop scanning
flutterBlue.stopScan();
```

### Connect to a device

```dart
// Connect to the device
await device.connect();

// Disconnect from device
device.disconnect();
```

### Discover services

```dart
List<BluetoothService> services = await device.discoverServices();
services.forEach((service) {
    // do something with service
});
```

### Read and write characteristics

```dart
// Reads all characteristics
var characteristics = service.characteristics;
for(BluetoothCharacteristic c in characteristics) {
    List<int> value = await c.read();
    print(value);
}

// Writes to a characteristic
await c.write([0x12, 0x34])
```

### Read and write descriptors

```dart
// Reads all descriptors
var descriptors = characteristic.descriptors;
for(BluetoothDescriptor d in descriptors) {
    List<int> value = await d.read();
    print(value);
}

// Writes to a descriptor
await d.write([0x12, 0x34])
```

### Set notifications and listen to changes

```dart
await characteristic.setNotifyValue(true);
characteristic.value.listen((value) {
    // do something with new value
});
```

### Read the MTU and request larger size

```dart
final mtu = await device.mtu.first;
await device.requestMtu(512);
```

Note that iOS will not allow requests of MTU size, and will always try to negotiate the highest possible MTU (iOS supports up to MTU size 185)

## Getting Started

### Change the minSdkVersion for Android

Flutter_blue is compatible only from version 19 of Android SDK so you should change this in **android/app/build.gradle**:

```dart
Android {
  defaultConfig {
     minSdkVersion: 19
```

### Add permissions for Bluetooth

We need to add the permission to use Bluetooth and access location:

#### **Android**

In the **android/app/src/main/AndroidManifest.xml** let’s add:

```xml
    <!-- Request legacy Bluetooth permissions on older devices. -->
    <uses-permission android:name="android.permission.BLUETOOTH"
                     android:maxSdkVersion="30" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"
                     android:maxSdkVersion="30" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>
    <!-- Needed only if your app looks for Bluetooth devices.
         You must add an attribute to this permission, or declare the
         ACCESS_FINE_LOCATION permission, depending on the results when you
         check location usage in your app. -->
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN" />

    <!-- Needed only if your app makes the device discoverable to Bluetooth
         devices. -->
    <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />

    <!-- Needed only if your app communicates with already-paired Bluetooth
         devices. -->
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
 <application
```

#### **IOS**

In the **ios/Runner/Info.plist** let’s add:

```dart
	<dict>
	    <key>NSBluetoothAlwaysUsageDescription</key>
	    <string>Need BLE permission</string>
	    <key>NSBluetoothPeripheralUsageDescription</key>
	    <string>Need BLE permission</string>
	    <key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
	    <string>Need Location permission</string>
	    <key>NSLocationAlwaysUsageDescription</key>
	    <string>Need Location permission</string>
	    <key>NSLocationWhenInUseUsageDescription</key>
	    <string>Need Location permission</string>
```

For location permissions on iOS see more at: [https://developer.apple.com/documentation/corelocation/requesting_authorization_for_location_services](https://developer.apple.com/documentation/corelocation/requesting_authorization_for_location_services)

## Reference

### FlutterBlue API

|             |      Android       |        iOS         | Description                                        |
| :---------- | :----------------: | :----------------: | :------------------------------------------------- |
| scan        | :white_check_mark: | :white_check_mark: | Starts a scan for Bluetooth Low Energy devices.    |
| state       | :white_check_mark: | :white_check_mark: | Stream of state changes for the Bluetooth Adapter. |
| isAvailable | :white_check_mark: | :white_check_mark: | Checks whether the device supports Bluetooth.      |
| isOn        | :white_check_mark: | :white_check_mark: | Checks if Bluetooth functionality is turned on.    |

### BluetoothDevice API

|                  |      Android       |        iOS         | Description                                                                                       |
| :--------------- | :----------------: | :----------------: | :------------------------------------------------------------------------------------------------ |
| connect          | :white_check_mark: | :white_check_mark: | Establishes a connection to the device.                                                           |
| disconnect       | :white_check_mark: | :white_check_mark: | Cancels an active or pending connection to the device.                                            |
| discoverServices | :white_check_mark: | :white_check_mark: | Discovers services offered by the remote device as well as their characteristics and descriptors. |
| services         | :white_check_mark: | :white_check_mark: | Gets a list of services. Requires that discoverServices() has completed.                          |
| state            | :white_check_mark: | :white_check_mark: | Stream of state changes for the Bluetooth Device.                                                 |
| mtu              | :white_check_mark: | :white_check_mark: | Stream of mtu size changes.                                                                       |
| requestMtu       | :white_check_mark: |                    | Request to change the MTU for the device.                                                         |

### BluetoothCharacteristic API

|                |      Android       |        iOS         | Description                                              |
| :------------- | :----------------: | :----------------: | :------------------------------------------------------- |
| read           | :white_check_mark: | :white_check_mark: | Retrieves the value of the characteristic.               |
| write          | :white_check_mark: | :white_check_mark: | Writes the value of the characteristic.                  |
| setNotifyValue | :white_check_mark: | :white_check_mark: | Sets notifications or indications on the characteristic. |
| value          | :white_check_mark: | :white_check_mark: | Stream of characteristic's value when changed.           |

### BluetoothDescriptor API

|       |      Android       |        iOS         | Description                            |
| :---- | :----------------: | :----------------: | :------------------------------------- |
| read  | :white_check_mark: | :white_check_mark: | Retrieves the value of the descriptor. |
| write | :white_check_mark: | :white_check_mark: | Writes the value of the descriptor.    |

## Troubleshooting

### When I scan using a service UUID filter, it doesn't find any devices.

Make sure the device is advertising which service UUID's it supports. This is found in the advertisement
packet as **UUID 16 bit complete list** or **UUID 128 bit complete list**.
