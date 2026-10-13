# Expo, Beacon, and Push Notification Skills Design

## Goal

Add three standalone Markdown skills under `react-native-skill/` so an agent can independently handle Expo device development, BLE Beacon scanning, or iOS push-notification diagnosis using current project evidence and platform documentation.

## Context and constraints

- The repository already has separate skills for running iOS development builds, EAS iOS build failures, iOS release/signing, Expo iOS project recovery, and Xcode cache cleanup.
- Keep each new skill independently useful. Do not combine build setup, Beacon scanning, and push notification delivery into one general Expo guide.
- Preserve account names, emails, team IDs, bundle identifiers, device IDs, private keys, and project-specific tokens from the supplied notes as private examples; do not copy them into reusable guidance.
- Treat tool commands and service limits in the notes as examples that may age. Check the repository's installed CLI and current official docs when a task depends on exact syntax or quotas.
- Do not perform account, signing, Firebase, Apple Developer Portal, EAS project, or paid-build changes without explicit authorization for that action.

## Skills

### `running-expo-device-builds`

Create `react-native-skill/running-expo-device-builds/SKILL.md` for selecting and launching Expo apps on physical iOS and Android devices. Cover Expo Go, local development builds, and EAS development builds; distinguish the Expo account requirements of local commands, Expo Go, and EAS; cover USB device selection and Android ADB reverse where relevant. Inspect the project's package manager, scripts, Expo config, native directories, installed client, and signing prerequisites before recommending commands. Route iOS-specific signing or release work to the existing focused iOS skills rather than duplicating their procedures.

### `debugging-ble-beacons`

Create `react-native-skill/debugging-ble-beacons/SKILL.md` for diagnosing Beacon scan failures on iOS and Android. First identify whether the app uses generic BLE advertisements or iBeacon/Core Location region monitoring, the library/API, OS version, and physical-device/runtime type. Distinguish app authentication from OS permissions. Treat location and UUID requirements as API- and platform-specific; verify the selected library's current setup and permissions. Use an independent scanner such as nRF Connect as a hardware/advertising cross-check, not as sole proof of a code defect.

### `debugging-ios-push-notifications`

Create `react-native-skill/debugging-ios-push-notifications/SKILL.md` for tracing iOS push failures across app registration, APNs, Firebase Cloud Messaging, and the sending backend. Identify whether the app uses `expo-notifications`, `@react-native-firebase/messaging`, or both and which layer owns each observed behavior. Check the built app's actual `aps-environment` entitlement and signing profile, Push Notifications capability, Firebase app/bundle ID and APNs credential configuration, token type, and backend service-account project/role. Use logs and observed values to locate the failing hop before proposing changes. Do not guess a sandbox/production mismatch or recommend key replacement, credential deletion, reinstall, or portal changes without evidence and authorization.

## Accuracy requirements

- Local `npx expo run:ios` / `npx expo run:android` does not generally require Expo account login. EAS cloud builds require Expo authentication; Expo Go's account requirements depend on the current platform/version and should be checked against current Expo documentation.
- Do not state that all BLE scans require location services or a known UUID. Explain the difference between generic BLE scanning and iBeacon/Core Location region operations, and verify platform/library-specific permission requirements.
- Treat Firebase delivery, APNs registration/credentials, app entitlements, notification presentation, and backend authentication as distinct layers. Do not make universal claims about service quotas or delivery guarantees from old notes.
- Prefer current official Expo, Apple, Android, and Firebase documentation for version-sensitive commands, permissions, entitlements, roles, and service limits.

## Out of scope

- Editing an application's runtime code, Expo config, native project, Firebase/Apple account, or production credentials.
- Shipping a complete app-specific APNs/FCM setup guide with the supplied private identifiers.
- Adding executables, extra reference files, or broad Expo documentation unless an identified need requires them.

## Acceptance criteria

- Exactly three new self-contained `SKILL.md` files exist in the agreed `react-native-skill/` folder.
- Each file has valid skill frontmatter (`name`, discriminating `description`) and is independently applicable to its named workflow.
- The three workflows do not collapse into a single Expo skill and avoid duplicating existing iOS signing/release/EAS procedures.
- Instructions preserve authorization boundaries, avoid unsupported universal claims, and point to official documentation for changing details.
- No project-specific secrets or account/device identifiers from the supplied notes are included.
