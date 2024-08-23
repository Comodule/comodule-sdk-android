# Changelog

All notable changes to this project will be documented in this file.

## [1.3.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.3.0) - 2024-08-23

### Breaking changes
- `disconnect()` has been updated to a suspend function
### Fixed
- Fixed an intermittent crash when losing BLE connection

## [1.2.6](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.6) - 2024-08-02

### Fixed
- Fixed Data to Cloud running while an external firmware update is ongoing

## [1.2.5](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.5) - 2024-07-26

### Fixed
- Fixed faulty proguard configuration

## [1.2.4](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.4), [1.2.4-compat](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.4-compat) - 2024-07-24

### Fixed
- Fixed an issue introduced in [1.2.3](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.3) that caused properties to not receive updates after connecting to the module more than once

## [1.2.3-compat](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.3-compat) - 2024-07-22

### Changed
- Downgraded appcompat and kotlinx.serialization dependencies to same as in version [1.2.2](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.2)

## [1.2.3](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.3) - 2024-07-16

### Changed
- Updated dependencies

### Fixed
- Fixed an issue where Data to Cloud failed to send big batches of data

## [1.2.2](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.2) - 2024-06-26

### Changed
- Improved documentation and API for SDK creation by adding `CmSdk.create` method and deprecating `CmSdkBuilder.getInstance`

### Fixed
- Fixed a regression introduced in version [1.2.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.0) that caused some firmware updates to get stuck on the `Initiating` step
- Removed unnecessary `com.google.android.gms.permission.AD_ID` permission declared in the Manifest
- Improved DTC stability

## [1.2.1](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.1) - 2024-06-07

### Fixed
- Removed a faulty error log

## [1.2.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.0) - 2024-06-07

### Changed
- Added more structured logging for Data to Cloud
- Remove periodic checks for external versions for modules that do not have external versions
- Removed second query for external verions when parsing firmware info

### Fixed
- Fixed a bug where values were not written correctly if multiple properties were assigned to the same register position
- Fixed connection error not being cleared timely
- Fixed Data to Cloud not sending data in some cases
- Fixed a crash that was sometimes happening when performing Data to Cloud

## [1.1.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.1.0) - 2024-06-04

### Changed
- Improved connection speed by caching module's MAC address after the first scan
- Exposed properties that failed to initialize for debugging purposes

## [1.0.6](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.6) - 2024-05-27

### Fixed
- Fixed a bug where connection state Disconnected was emitted too early after a failed connection attempt

## [1.0.5](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.5) - 2024-05-22

### Changed
- Allow to connect to modules when no BLE config is present

### Fixed
- Fixed a bug where Bluetooth pairing dialog did not appear again if it was ignored by user the first time
- Fixed a bug where wrong module name was sometimes outputted during Connecting-Searching stage

## [1.0.4](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.4) - 2024-05-17

### Added
- Added additional error handling for Bluetooth errors
- Added additional logging

### Changed
- Improved connection speed by using only direct connection

### Fixed

- Fixed a bug where the SDK connected to the module during an ongoing firmware update.

## [1.0.3](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.3) - 2024-05-10

### Fixed

- Fixed a bug where some properties do not update.

## [1.0.2](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.2) - 2024-04-29

### Fixed

- Fixed a bug when the SDK remains connected to the module after calling `disconnect()` during `Connecting.Searching` stage

## [1.0.1](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.1) - 2024-04-26

### Fixed

- Patially fixed a bug when the SDK remains connected to the module after calling `disconnect()` during `Connecting.Searching` stage

## [1.0.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.0.0) - 2024-04-19

Initial release
