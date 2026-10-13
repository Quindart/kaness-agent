---
name: running-ios-dev-builds
description: Use when running or installing an Expo or React Native development build on an iOS simulator or physical iPhone.
---

# Running iOS Development Builds

Choose the run path based on the app's native dependencies, the target device, and the project's existing scripts. Expo Go cannot load native modules that are not included in Expo Go; apps with custom native modules need a compatible development build.

## Workflow

1. Confirm the target: iOS simulator or physical device. Ask whether the app uses custom native modules and whether a development client is already installed.
2. Inspect the app root, package manager and lockfile, `package.json` scripts, Expo config, and whether an `ios/` project is present. Check whether `expo-dev-client` is already configured before suggesting an install.
3. For simulator runs, verify that Xcode and an available iOS simulator are installed. Use the project's existing iOS script when it is clear; otherwise give the Expo CLI command supported by the installed version. Explain if the command will generate or modify native project files.
4. For a physical device, confirm macOS/Xcode availability, device connection and trust, signing setup, and Developer Mode where required. If using EAS, establish the intended profile and device distribution/registration setup before starting a cloud build.
5. Start Metro using the project's existing script or the command appropriate for its development client. Make sure Metro is serving the same app/dev-client configuration that is installed on the device.
6. If required tooling or `expo-dev-client` is missing, explain the dependency or setup change and ask before making it. Do not present a command as guaranteed until it matches the detected package manager, scripts, and Expo setup.

## Guardrails

- Do not use Expo Go as the run target when the app depends on a custom native module unavailable in Expo Go.
- Do not assume `yarn ios --device` exists; inspect `package.json` scripts first. Do not mix `yarn`, `npx`, and `yarn dlx` without checking the repository's package manager and installed CLI.
- A physical-device build needs a valid iOS signing path. Do not promise installation from a simulator-only build or skip signing/device setup.
- Do not generate or clean the `ios/` project without checking whether it contains native changes; use the Expo iOS project recovery workflow for prebuild or CocoaPods failures.
- Keep development builds separate from production signing and App Store submission.

## Example

If the app has a custom native module and only Expo Go is installed, explain that Expo Go is not a compatible client. Check whether `expo-dev-client` and the project's iOS run script already exist, then recommend a local or EAS development build based on the user's Mac/Xcode and signing setup.
