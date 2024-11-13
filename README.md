[![Maven Central Version](https://img.shields.io/maven-central/v/com.comodule/bluetooth)](https://central.sonatype.com/artifact/com.comodule/bluetooth)

# Comodule Android SDK

The Comodule SDK is a versatile framework designed for native Android applications, enabling seamless interaction with Comodule modules over Bluetooth Low Energy. It offers an efficient approach to access and modify vehicle information, alter settings, as well as perform data-to-cloud. The SDK supports over-the-air firmware updates of the module firmware.

The SDK supports Android 10 and above.

## Table of contents
- [Important resources](#important-resources)
  - [Changelog](#changelog)
  - [API documentation](#api-documentation)
- [Setup](#setup)
  - [Adding dependency](#adding-dependency)
  - [Permissions](#permissions)
- [Getting started](#getting-started)
  - [Initializing the SDK](#initializing-the-sdk)
  - [Discovering modules](#discovering-modules)
  - [Connecting to module and module status](#connecting-to-module-and-module-status)
  - [Disconnecting from a module](#disconnecting-from-a-module)
  - [Vehicle properties](#vehicle-properties)
    - [Discovering what the module is able to do](#discovering-what-the-module-is-able-to-do)
    - [Subscribing to all available properties](#subscribing-to-all-available-properties)
    - [Subscribing to a specific property](#subscribing-to-a-specific-property)
    - [Writing properties](#writing-properties)
  - [Firmware update](#firmware-update)
    - [Preparations and current firmware info](#preparations-and-current-firmware-info)
    - [Firmware update status](#firmware-update-status)
    - [Starting Firmware update](#starting-firmware-update)
  - [What else can the SDK do?](#what-else-can-the-sdk-do)
    - [Logging](#logging)
    - [Clearing cache](#clearing-cache)
- [Need help or encountering issues?](#need-help-or-encountering-issues)

## Important resources
### Changelog
The Changelog can be found [here](https://github.com/Comodule/comodule-sdk-android/blob/main/CHANGELOG.md).

### API documentation
The API documentation is an essential resource for working with Comodule SDK. It provides comprehensive details on all exposed methods, including descriptions of states and corner cases. Please refer to it for the most accurate and up-to-date information. The documentation can be found [here](https://comodule.github.io/comodule-sdk-android/index.html) or viewed in Android Studio when `Cmd`/`Ctrl` clicking declarations.

## Setup

### Adding dependency
Comodule SDK is available as a Maven Central package. To add the Comodule SDK into the project, use the following dependency in your app module's `build.gradle` file:
```kotlin
implementation("com.comodule:bluetooth:<version>")
```
You can check available versions on [Maven Central Portal](https://central.sonatype.com/artifact/com.comodule/bluetooth). We recommend always using the latest version of the SDK.

### Permissions
Comodule SDK takes care of declaring permissions required for its operation in the Manifest, so you don't need to include them youself.

However, Comodule SDK does not handle requesting Bluetooth permissions. The following permissions must be requested by your application before initiating discovery or connection:
- `android.permission.BLUETOOTH_SCAN` on Android 12+
- `android.permission.BLUETOOTH_CONNECT` on Android 12+
- `android.permission.BLUETOOTH` on Android 10..11
- `android.permission.BLUETOOTH_ADMIN` on Android 10..11
- `android.permission.ACCESS_FINE_LOCATION` on Android 10..11
- `android.permission.ACCESS_COARSE_LOCATION` on Android 10..11

## Getting started

The following section provides simple examples on using the Comodule Bluetooth SDK. Please address the [API documentation](#api-documentation) for more detailed information.

### Initializing the SDK

Comodule SDK is intended to be used as a singleton, and can be created by calling:
[`CmSdk.create(context, apiKey)`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/-companion/create.html). The API key is provided by Comodule.

NB: Please, make sure that the `create` method is called only once, as calling it repeatedly will produce different instances each time and may cause unpredictable results.

```kotlin
val cmSdk = CmSdk.create(context = context, apiKey = API_KEY)
```

If necessary, the API key can be updated later by calling [`CmSdk.setApiKey`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/set-api-key.html).

All the SDK functionalities can be accessed using the `CmSdk` instance. The available methods can be found [here](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html).

### Discovering modules

Call [`CmSdk.discoverModules`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/discover-modules.html) and collect the result:
```kotlin
cmSdk.discoverModules(timeOut = CmScanDuration.MEDIUM).collect { modules ->
	// handle discovered modules
}
```

The discovery will complete after the specified `timeOut`. Alternatively, it is possible to cancel discovery after a custom timeout:
```kotlin
val discoverJob = launch {
  cmSdk.discoverModules().collect {
    // handle discovered modules
  }
}
delay(2.seconds)
discoverJob.cancel()
```

### Connecting to module and module status

It is possible to connect to a module using the serial obtained via [`CmSdk.discoverModules`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/discover-modules.html) using [`CmSdk.connect`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/connect.html).

Optionally you can subscribe to the connection state using [`CmSdk.observeConnectionState`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-558194595/Functions/237758736).

Any connection errors will be returned as a result of `CmSdk.connect`, but it is also possible to subscribe to them as a Flow using [`CmSdk.observeConnectionError`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#2046744262/Functions/237758736).

```kotlin
// optionally subscribe to the connection state
cmSdk.observeConnectionState().collect { connectionState ->
	when (connectionState) {
		is Disconnected -> TODO()
		is Connecting.BluetoothDisabled -> TODO()
		is Connecting.Searching -> TODO()
		is Connecting.Handshaking -> TODO()
		is Connecting.FirmwareUpdateOngoing -> TODO()
		is Connected -> TODO()
	}
}

// optionally subscribe to the connection error
cmSdk.observeConnectionError().collect { connectionError ->
	// handle errors
}

// connect
when (val result = cmSdk.connect(serial)) {
	is CmConnectResult.Success -> TODO()
	is CmConnectResult.Failure -> TODO()
}
```

### Disconnecting from a module

Module can be disconnected using : [`CmSdk.disconnect`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-855044744/Functions/237758736).

### Vehicle properties

#### Discovering what the module is able to do

Mainly used to discover which properties the vehicle has available for implementation: [`CmSdk.observePropertiesInfo`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-52135319/Functions/237758736).
```kotlin

cmSdk.observePropertiesInfo().collect { result ->
	when (result) {
		CmPropertiesInfoResult.Available -> TODO()
		CmPropertiesInfoResult.NotAvailable -> TODO()
	}
}
```

#### Subscribing to all available properties

You can observe module's available properties with corresponding values using: [`CmSdk.observeProperties`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-509453193/Functions/237758736).

```kotlin
cmSdk.observeProperties().collect { result ->
	when (result) {
		CmPropertiesResult.Available -> TODO()
		CmPropertiesResult.NotAvailable -> TODO()
	}
}
```
#### Subscribing to a specific property

You can observe a specific property with its value using: [`CmSdk.observeProperty`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#1555774927/Functions/237758736).

```kotlin
cmSdk.observeProperty().collect { result ->
	when (result) {
		CmPropertyResult.Available -> TODO()
		CmPropertyResult.WriteOnly -> TODO()
		CmPropertyResult.NotAvailable.ModuleNotConnected -> TODO()
		CmPropertyResult.NotAvailable.Missing -> TODO()
	}
}
```

#### Writing properties

A property value can be updated using [`CmSdk.setPropertyValue`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-682457749/Functions/237758736).

```kotlin
// set range property value
val result = cmSdk.setPropertyValue("<identifier>", CmPropertyValue.Range(10.5))
// or set states property value
// cmSdk.setPropertyValue("<identifier>", CmPropertyValue.States("<state-identifier>"))
// or set raw property value
// cmSdk.setPropertyValue("<identifier>", CmPropertyValue.Raw(bytes))
when (result) {
	is CmWriteResult.Success -> TODO()
	is CmWriteResult.Failure -> TODO()
}
```

### Firmware update
It is possible to use the SDK to update the module or vehicle firmware.

#### Preparations and current firmware info

Firmware update has to be prepared and validated by the Comodule team. If no firmware update has been set up, the SDK will say that the module is up to date. Firmware updates are mostly provided for Comodule modules but in some cases also to update the firmware of vehicle components. 

To access current firmware info, use: [`CmSdk.observeFirmwareInfo`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-933029343/Functions/237758736).

```kotlin
cmSdk.observeFirmwareInfo().collect { result ->
	when (result) {
		is CmFirmwareInfo.Available -> TODO()
		is CmFirmwareInfo.NotAvailable -> TODO()
	}
}
```

#### Firmware update status

Every time a valid BLE connection is made the SDK makes sure if an update is available.
To access this info, use: [`CmSdk.observeFirmwareUpdateStatus`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#-715024556/Functions/237758736).

```kotlin
cmSdk.observeFirmwareUpdateStatus().collect { status ->
	when (status) {
		is CmFirmwareUpdateStatus.ModuleNotConnected -> TODO()
		is CmFirmwareUpdateStatus.FetchingUpdateInfo -> TODO()
		is CmFirmwareUpdateStatus.UpToDate -> TODO()
		is CmFirmwareUpdateStatus.Ongoing -> TODO()
		is CmFirmwareUpdateStatus.Available -> TODO()
	}
}
```

#### Starting Firmware update

If a firmware update is available for a given connected module, it is possible to start the update using: [`CmSdk.updateFirmware`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#247722695/Functions/237758736)
```kotlin
when (val result = cmSdk.updateFirmware(MyActivity::class.java)) { result ->
	is CmFirmwareUpdateResult.Success -> TODO()
	is CmFirmwareUpdateResult.Failure -> TODO()
}
```

### What else can the SDK do?

#### Logging

It is possible to enable crashlytics logging when instantiating the SDK. In that case non-fatal errors such as failed connection, failed firmware updates and failed write operations will be reported to your application's Crashlytics non-fatal errors section alongside with the logs.

The logs are not collected by Comodule and are only meant for your application.

#### Clearing cache

Comodule SDK caches some vehicle related info to provide a faster handshake during the connection and allow offline use within reason. 

NB! In most cases clearing cache is never required. The SDK performs its own cache maintenance periodically while the SDK is being used. 

In some edge cases, it might be necessary to clear this cache. To do so, use: [`CmSdk.clearCache`](https://comodule.github.io/comodule-sdk-android/-comodule%20-s-d-k/com.comodule.bluetooth/-cm-sdk/index.html#1430594303/Functions/237758736).

## Need help or encountering issues?
Got questions about Comodule solutions or the SDK implementation? Just fill out our [Contact form](https://www.comodule.com/contact).
