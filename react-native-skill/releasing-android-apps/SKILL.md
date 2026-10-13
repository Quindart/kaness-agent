---
name: releasing-android-apps
description: Use when preparing a React Native or Expo Android app for a signed Google Play release, internal Play testing, or direct APK distribution.
---

# Releasing Android Apps

Choose the artifact and signing path from the intended destination. A Play Console release, an installable staging APK, and an EAS internal build are different deliverables.

## Workflow

1. Inspect the app root, package manager and lockfile, Expo SDK, `android.package`/Gradle `applicationId`, app version settings, `eas.json`, Android project state, and current signing setup. In an Expo prebuild project, determine which native files are generated and whether manual Gradle edits will survive prebuild. Preserve existing local changes.
2. Confirm the target:
   - **Google Play Console:** Build a signed Android App Bundle (AAB) for the intended Play track. New Play apps use Play App Signing. Google Play processes the bundle to deliver device-specific APKs.
   - **Direct install or EAS internal distribution:** Build an APK when testers need a file/link they can install directly. This APK path does not publish the app to Google Play. An AAB is not directly installable; distribute it through Google Play.
   - **Play internal testing track:** This is still a Play Console release. Use the Play upload artifact and track, not an EAS direct-install APK profile.
3. Confirm app identity and version before building. The package/application ID must match the existing Play app and cannot be casually changed after publication. Set `versionCode` above the highest code already used for the app; increment it for every Play upload. `versionName` is the user-facing label and does not replace `versionCode`.
4. Confirm signing ownership and storage:
   - With Play App Signing, distinguish the private **upload key** used to sign the uploaded AAB from Google's **app signing key** used for delivered APKs. Keep the upload keystore, alias, and passwords in an approved secret store or EAS credentials; never commit them or hard-code passwords in Gradle files.
   - Back up the upload keystore securely. If Play App Signing is enabled, a lost upload key can generally be reset through Play Console; a lost self-managed app signing key for an app not enrolled in Play App Signing can prevent updates under the same app identity.
   - Determine whether EAS-managed or local Gradle signing is intended. Do not create, replace, download, or upload credentials without explicit authorization.
5. Check Android build tool versions as a compatible set: Expo SDK/template, Android Gradle Plugin, Gradle wrapper, Kotlin plugin, and any Compose compiler configuration. Do not copy Gradle `8.14.3`, Kotlin `2.2.20`, or a Compose compiler value from another project without checking compatibility. `expo-build-properties` configures native files during Expo Prebuild; confirm the installed plugin version supports each option and that the project uses Prebuild before relying on it. Avoid adding Compose compiler configuration to a React Native app unless the project actually uses Compose.
6. Build the artifact for the selected route using the project's package manager, installed Expo/EAS CLI, and profile. For an existing local Android project, the Gradle task is typically `cd android && ./gradlew app:bundleRelease` for an AAB or `./gradlew app:assembleRelease` for an APK. Confirm the output path from the build log. For EAS, inspect `eas.json`: Play profiles should produce an app bundle; `distribution: internal` commonly selects a directly installable APK unless overridden. Use `eas build --platform android --local --profile <profile>` only when a local EAS build is intended and its current authentication/prerequisites are satisfied.
7. Before upload, inspect the built artifact and confirm package ID, version code, release signing, and expected build variant. Smoke-test the release artifact or a Play test-track install. AAB does not mean unlimited size: Google Play applies compressed download and module/asset limits; check the current Play Console size analysis if the app is large.
8. For Play Console, upload the AAB to the explicitly selected internal, closed, or production track. Review Play Console errors, store listing, content rating, Data safety, privacy policy, target API requirements, and release notes. Stop before review submission or rollout unless the user authorized that release action. `eas submit` also uploads to Play; confirm the app, track, service account, and artifact before running it.

## Guardrails

- Do not assume switching an oversized APK to AAB removes all size limits. Check Play's current per-device download and module/asset limits; consider Play Asset Delivery or feature delivery when appropriate.
- Do not set `usesCleartextTraffic: true` as a generic build fix. Prefer HTTPS; enable cleartext only for an explicitly approved, narrowly scoped development need.
- Do not edit generated Gradle files when the same change belongs in Expo config or a config plugin without checking the project's prebuild workflow.
- Never infer the target Play account, app, signing key, or release track from sample identifiers. Do not upload, submit, or roll out a release without authorization for that exact action.
- Avoid printing secrets when inspecting Gradle properties, EAS credentials, keystores, CI variables, or build logs.

## Official references

- [Expo: Create a release build locally](https://docs.expo.dev/guides/local-app-production/)
- [Expo: EAS internal distribution](https://docs.expo.dev/build/internal-distribution/)
- [Expo: Build APKs for devices](https://docs.expo.dev/build-reference/apk/)
- [Expo: `expo-build-properties`](https://docs.expo.dev/versions/latest/sdk/build-properties/)
- [Android: Sign your app](https://developer.android.com/studio/publish/app-signing)
- [Android: Upload your app to Play Console](https://developer.android.com/studio/publish/upload-bundle)
- [Google Play: Set up your app](https://support.google.com/googleplay/android-developer/answer/9859152)
- [Google Play: App size limits](https://support.google.com/googleplay/android-developer/answer/9859372)
- [Kotlin: Compose compiler migration](https://kotlinlang.org/docs/compose-compiler-migration-guide.html)
