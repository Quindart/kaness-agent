---
name: running-ios-dev-builds
description: Use when deciding between Expo Go and a custom Expo development client, or running an Expo or React Native app on an iOS simulator or physical iPhone.
---

# Running iOS Development Builds

Choose by intended client, target device, and project setup. Expo Go cannot load native modules that are not included in it; incompatible apps need a custom development client.

## Workflow

1. Confirm simulator or physical device and the intended client: Expo Go, an installed client, local development build, or EAS build. If unsure about Expo Go compatibility, inspect the Expo SDK, config plugins, native packages, and project setup; ask if still unclear.
2. Inspect the app root, package manager/lockfile, `package.json` scripts, Expo config, and `ios/`. Check for `expo-dev-client` before suggesting installation.
3. For a simulator, verify Xcode and an available iOS simulator. Use a confirmed project script or the installed Expo CLI; explain when a command generates or changes native files.
4. For a physical device, confirm Mac/Xcode, connection and trust, signing, and Developer Mode as needed. For EAS, identify the account, project, profile, and device registration path.
5. Before EAS device registration, account/project or signing changes, or a cloud build, state the exact action/profile and possible cost, then get explicit authorization.
6. Start Metro with the project script or command for the installed client. If required tooling or `expo-dev-client` is missing, explain the change and ask before making it.

## Guardrails

- No custom native modules does not by itself prove Expo Go compatibility. Do not assume `yarn ios --device` exists or mix Yarn, `npx`, and `yarn dlx` without checking the project.
- A physical build needs valid signing; do not promise installation from a simulator build. Check for native edits before generating or cleaning `ios/`; use the iOS recovery skill for prebuild or CocoaPods failures.
- Keep development builds separate from production release and submission.

## Example

If compatibility is unclear, inspect the Expo config and native packages before choosing a client. For a custom module, check `expo-dev-client` and the project script, then choose local or EAS based on Mac/Xcode and signing readiness.
