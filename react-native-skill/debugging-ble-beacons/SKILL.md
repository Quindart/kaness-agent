---
name: debugging-ble-beacons
description: Use when a React Native or Expo app cannot discover, range, or monitor a BLE Beacon or iBeacon, or when Beacon permissions and runtime support are unclear.
---

# Debugging BLE Beacons

Diagnose Beacon scanning from the radio protocol and native API actually used. Generic BLE advertisements and iBeacon region monitoring have different APIs, filters, and permission requirements.

## Workflow

1. Establish the reproduction details before changing code: physical device or simulator, OS/version, Expo Go or development build, app foreground/background state, Beacon model/protocol, expected identifier, scanning library and API, and exact error/log output.
2. Classify the operation:
   - **Generic BLE scan:** The app scans Bluetooth LE advertisements through a BLE library. Identify the library's scan filters, native permissions, service UUID handling, and whether the Beacon is advertising the expected packet.
   - **iBeacon through Core Location:** The app uses an iBeacon-specific ranging or region-monitoring API. Confirm the expected UUID and any major/minor filters, the required Core Location authorization, location-services state, and relevant background configuration.
   Do not apply iBeacon UUID or Core Location assumptions to every generic BLE scan.
3. Check that the chosen library and native modules are available in the running app. Expo Go cannot load arbitrary custom native modules; when the BLE library requires one, use a compatible development build and confirm the installed binary includes the native module.
4. Compare required permissions with the actual app configuration and device prompt state:
   - On iOS, check the Bluetooth usage description and, for Core Location Beacon APIs, the authorization level and usage descriptions required by that API and OS version.
   - On Android, check Bluetooth scan/connect permissions for the target and OS versions. Location permission may be required by older Android versions or when scan results are used to derive physical location; do not request it automatically for every BLE use.
   - Distinguish a user's account login to the app from operating-system permission. App authentication is relevant only if the product's own feature/backend requires it.
5. Confirm the Beacon is powered, advertising, in range, and using a protocol the phone/library supports. A nearby independent scanner such as nRF Connect can help compare visibility and advertisement data. Treat one scanner's result as supporting evidence, not a definitive hardware diagnosis.
6. Follow the evidence: compare scan results with and without filters, check permission status and app lifecycle, inspect native logs, and verify the library's current setup guide. Ask before changing native project configuration, installing dependencies, regenerating iOS/Android directories, or changing device/account settings.

## Common failure patterns

- A scan button changes UI state but the native scan promise rejects or reports no permission: inspect the exact API error and granted permission state.
- The Beacon appears in a generic BLE scanner but not the app: compare service/manufacturer data, scan filters, runtime compatibility, and permissions.
- Generic BLE scanning works but iBeacon region monitoring does not: verify the iBeacon UUID/region parameters and Core Location authorization separately.
- Scanning works only while foregrounded: inspect the library's background support and platform background requirements; do not infer background capability from foreground success.
- The device is not visible to any scanner: verify power, advertising configuration, distance, and supported radio/protocol before changing app code.

## Official references

- [Apple Core Bluetooth](https://developer.apple.com/documentation/corebluetooth)
- [Apple Core Location](https://developer.apple.com/documentation/corelocation)
- [Android Bluetooth permissions](https://developer.android.com/develop/connectivity/bluetooth/bt-permissions)
- [Expo development builds](https://docs.expo.dev/develop/development-builds/introduction/)

For library-specific permission keys and background behavior, consult the documentation for the BLE library and Expo SDK version used in the project.
