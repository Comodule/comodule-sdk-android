# Changelog

All notable changes to this project will be documented in this file.

## [1.2.2](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.2) - 2024-06-26

### Fixed
- Fixed a regression introduced in version [1.2.0](https://central.sonatype.com/artifact/com.comodule/bluetooth/1.2.0) that caused some firmware updates to get stuck on the `Initiating` step

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
