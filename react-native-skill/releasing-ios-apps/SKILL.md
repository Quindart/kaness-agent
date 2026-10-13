---
name: releasing-ios-apps
description: Use when linking an Expo iOS app to EAS, configuring Apple signing, creating an iOS release build, or preparing App Store submission.
---

# Releasing iOS Apps

Treat project linking, Apple signing, production builds, and App Store submission as separate decisions. Account roles and a successful build do not establish which account, team, app record, or release action the user intends.

## Workflow

1. Confirm whether the user wants a development build, a production App Store build, or submission of an existing build. Identify the app root, package manager, bundle identifier, current EAS project link, `eas.json` profiles, and existing submit configuration.
2. Ask which EAS account owns the project and which Apple Developer team should sign it. Do not select an account from a list based only on Owner/Developer roles. Treat example emails and credentials in notes as placeholders unless confirmed.
3. Check the project's existing configuration before recommending `eas init`, `build:configure`, adding `eas-cli`, or changing scripts. Use the package manager and CLI version the project actually supports; verify command syntax from the installed CLI help or current official Expo documentation when needed.
4. Explain the selected profile, signing method, expected account changes, and any cloud build cost. Prepare a command sequence only for the confirmed account, app, team, and profile.
5. Before linking/changing an EAS project, creating or changing signing credentials, starting a paid build, or submitting to App Store Connect, state the exact action and get explicit authorization. Use interactive authentication flows; never ask the user to paste Apple passwords, private keys, or API secrets into chat.
6. After an authorized build, report its status and identifier. Treat App Store submission as its own user decision; do not infer submission permission from authorization to build.

## Guardrails

- Do not guess the Apple Developer team or EAS account from account names, roles, or the most recent login.
- Do not create signing credentials or change project links just to discover configuration.
- A development profile is not an App Store release profile. Confirm distribution and submission configuration in `eas.json` before using it.
- Do not start a cloud build or submit a build without authorization for that exact action.
- CLI syntax and EAS behavior can change. Prefer the project's installed CLI help and official Expo documentation over unverified command recipes from old notes.

## Example

If several EAS accounts are listed and the user asks to “publish tonight,” ask which account and Apple Developer team own the app. You may outline the release stages, but do not link an account, create credentials, start a paid build, or submit until the intended target and authorization are clear.
