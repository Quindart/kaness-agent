---
name: debugging-ios-push-notifications
description: Use when iOS push notifications fail to register, produce an APNs or FCM token, arrive on device, or reach app presentation/background handlers in an Expo or React Native app.
---

# Debugging iOS Push Notifications

Trace the failing hop using the token type, build, logs, and Firebase/APNs configuration. Permission prompts, APNs registration, FCM token creation, backend authorization, and notification presentation are separate stages.

## Workflow

1. Capture the exact symptom and evidence: physical device or simulator, OS version, Expo Go or development/production build, foreground/background/terminated state, notification type (visible or data-only), error text, relevant redacted logs, and the app's messaging libraries.
2. Identify the path in use. Check whether the app uses `expo-notifications`, `@react-native-firebase/messaging`, another native library, or a combination. Determine which library requests permission, registers with APNs, retrieves APNs/FCM/Expo push tokens, handles messages, and presents notifications. Do not assume these libraries are interchangeable or conflict-free; inspect their current setup and native configuration.
3. Verify the client registration stages in order:
   - The user granted the notification authorization needed for the requested behavior. Permission to display alerts, sounds, or badges is distinct from remote token registration.
   - The built app registers with APNs and obtains the token expected by its library. If using Firebase, confirm the app obtains an FCM registration token and that the backend sends to that token type, not an APNs token or Expo push token.
   - Inspect the actual signed app's `aps-environment` entitlement and provisioning/signing configuration. A source entitlement file alone does not prove which entitlement is in the installed binary. The entitlement identifies the APNs environment associated with that build.
4. Verify Apple and Firebase configuration without changing it: the Apple App ID has Push Notifications enabled; the installed bundle identifier matches the intended Firebase iOS app; Firebase has a valid APNs authentication key or certificate for the correct Apple team; and the Firebase project and app configuration match the token being used. APNs authentication keys support APNs authentication; do not infer an environment mismatch from a generic FCM failure or assume deleting/re-uploading a key is the fix.
5. Trace the sender side:
   - Confirm the backend targets the correct Firebase project and token type, uses the supported FCM API, and records the full provider error with secrets and tokens redacted.
   - For service accounts, verify which Google Cloud/Firebase project owns the identity and which project receives the send. If cross-project IAM is involved, check the current Firebase documentation for the exact role and API enablement requirements; do not copy a role name from old notes without checking.
   - Separate provider acceptance from device display. If FCM/APNs accepts a message but the UI does not show it, inspect app state, notification payload, presentation delegate/handler, and OS authorization.
6. Reproduce with a minimal message and a known current token on a real device when possible. Use simulator evidence only for capabilities supported by that simulator/OS version. Compare foreground, background, and terminated behavior separately.
7. Recommend the smallest evidence-supported change. Ask before editing native capabilities, changing Apple/Firebase configuration, adding IAM roles, rotating or replacing APNs credentials, deleting credentials, reinstalling as a diagnostic reset, or sending a real test notification.

## Distinctions that prevent common misdiagnoses

- **APNs token vs FCM token vs Expo push token:** These are different identifiers for different sending routes. Identify which one the app logs and which provider the backend calls.
- **Push Notifications capability vs Background Modes:** Push capability and correct signing are needed for remote push registration. Background remote notification mode is relevant to specific background delivery behavior; it is not a universal fix for missing visible notifications or missing tokens.
- **Entitlement vs source setting:** Check the entitlement in the built, signed app and the profile used to sign it. Do not treat an Xcode source file or screenshot as proof of the installed binary's environment.
- **FCM authorization vs APNs setup:** A backend service-account IAM role can affect sending through FCM; it cannot replace the APNs credentials and app registration Firebase needs for iOS delivery.
- **Local notifications vs remote delivery:** A local notification can test presentation without proving APNs/FCM delivery. A remote token can exist even when notification presentation is denied or the handler is incorrect.

## Official references

- [Apple Push Notifications](https://developer.apple.com/documentation/usernotifications)
- [Apple `aps-environment` entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/aps-environment)
- [Firebase Cloud Messaging setup for Apple platforms](https://firebase.google.com/docs/cloud-messaging/ios/client)
- [Firebase Apple platform credentials](https://firebase.google.com/docs/cloud-messaging/ios/certs)
- [Firebase Admin SDK sending](https://firebase.google.com/docs/cloud-messaging/send/admin-sdk)
- [Firebase predefined IAM roles](https://firebase.google.com/docs/projects/iam/roles-predefined-product)
- [Expo Notifications](https://docs.expo.dev/versions/latest/sdk/notifications/)
- [React Native Firebase Messaging](https://rnfirebase.io/messaging/usage)
