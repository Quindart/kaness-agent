---
name: running-expo-device-builds
description: Use when choosing or starting Expo Go, a local development build, or an EAS development build on a physical iOS or Android device, including USB and Expo sign-in questions.
---

# Running Expo Device Builds

Choose the runtime and build route from the app's native dependencies, target device, and available signing setup. Expo Go, a local native build, and an EAS cloud build have different requirements.

## Workflow

1. Inspect the app root, package manager and lockfile, Expo SDK, `package.json` scripts, app config, native `ios/` or `android/` directories, config plugins, and whether `expo-dev-client` is installed. Check repository changes before any command that may generate or rewrite native files.
2. Confirm target platform, physical device or simulator, and intended runtime:
   - **Expo Go:** Use only when the app's native modules and configuration are supported by the installed Expo Go version.
   - **Local development build:** Use `npx expo run:ios` or `npx expo run:android` when native modules or custom native configuration are needed and local Xcode/Android tooling is available. Add `--device` when the CLI should target a connected physical device. If the native project is absent, Expo may generate it; explain that before running the command.
   - **EAS development build:** Use when a cloud build or EAS-managed signing/device workflow is intended. Confirm the EAS project, build profile, target device, credentials path, and possible cost before starting.
3. Check sign-in and device prerequisites for the selected path:
   - Local `npx expo run:*` does not generally require an Expo account. iOS device installation still needs valid Xcode signing; Android needs USB debugging or an available emulator.
   - EAS cloud builds require Expo authentication. Physical iOS builds also need an eligible Apple signing setup; consult current Expo guidance for account and device-registration requirements.
   - Expo Go account requirements can change by platform, app version, and CLI/Go version. Check the current Expo troubleshooting page instead of assuming login is always or never required.
4. For Android USB, confirm the device is authorized with `adb devices`. If Metro is unreachable over USB, check the active Metro port and consider `adb reverse tcp:<metro-port> tcp:<metro-port>`. Do not copy port `8081` blindly if the project uses another port or connection mode.
5. Start Metro using the project script or the command appropriate for the selected client. Keep the Metro server and native app process separate when the workflow requires both.
6. If native module availability, signing, or runtime compatibility is uncertain, gather the exact CLI/Xcode/Gradle error before changing dependencies, regenerating native directories, registering a device, or changing credentials.

## Authorization boundaries

- Ask before linking or changing an EAS project, registering a device, creating or replacing signing credentials, changing Apple team configuration, or starting a cloud build that may incur cost. State the specific action and profile first.
- Do not ask the user to paste Apple passwords, private keys, provisioning profiles, or access tokens into chat. Use the platform's interactive authentication flow.
- A development build is not an App Store release. For EAS iOS build failures, recovery of an Expo-generated iOS project, or App Store signing/submission, use the corresponding focused guide: [debugging EAS iOS builds](../debugging-eas-ios-builds/SKILL.md), [recovering Expo iOS projects](../recovering-expo-ios-projects/SKILL.md), or [releasing iOS apps](../releasing-ios-apps/SKILL.md).

## Official references

- [Expo development builds](https://docs.expo.dev/develop/development-builds/introduction/)
- [Expo environment setup](https://docs.expo.dev/get-started/set-up-your-environment/)
- [Expo Go sign-in troubleshooting](https://docs.expo.dev/troubleshooting/expo-go-sign-in-required/)
- [EAS iOS development builds for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices/)
- [Android Debug Bridge](https://developer.android.com/tools/adb)
