# iOS React Native Skills Design

## Goal

Create five standalone Codex skills for recurring iOS React Native and Expo workflows reflected in the supplied notes. Each skill has its own directory and `SKILL.md`, and can be discovered or invoked independently. The skills should give practical guidance without assuming every project uses the same Expo, Yarn, EAS, or CocoaPods setup.

## Audience and success criteria

The audience is a developer working on an Expo or React Native iOS app. A skill is successful when it activates for its specific problem, gathers the relevant project state before recommending changes, and provides actionable guidance without applying risky cleanup or release actions without authorization.

## Proposed skills

1. `debugging-eas-ios-builds` — use for failed EAS iOS builds, including dependency installation failures. Inspect build logs, package manager metadata, Expo/EAS configuration, and the failing phase before suggesting a fix. Do not guess a cause from a generic failure message.
2. `recovering-expo-ios-projects` — use for Expo prebuild, generated `ios/` projects, CocoaPods, workspace, or Xcode target issues. Inspect repository state and native-project ownership before recommending regeneration. Treat clean prebuild, deleting `ios/`, and pod deintegration as potentially destructive; explain their impact and ask for authorization before performing them.
3. `running-ios-dev-builds` — use when setting up or troubleshooting a development build on an iOS simulator or physical device. Distinguish Expo Go from a custom development client, and cover simulator availability, device selection, Metro, and local native build prerequisites.
4. `releasing-ios-apps` — use for EAS project linking, development/production build profiles, Apple signing, and App Store submission preparation. Keep account selection, credentials, build profile, and submission as explicit decisions; do not submit or change external account state without authorization.
5. `cleaning-xcode-build-cache` — use when Xcode build data is consuming disk space. Inspect or estimate affected cache locations first, explain what rebuilding may be required, and ask before deleting files. Do not treat cache cleanup as a generic build-failure fix.

## Shared constraints

- Install as personal skills under `${CODEX_HOME:-$HOME/.codex}/skills/`, with one folder per skill. Keep automatic discovery enabled.
- Write skill instructions in English for consistent tool and error-message matching; retain the user's supplied account names, project ID, email placeholder, and build URL only as examples when useful. Do not encode credentials or personal account information as defaults.
- Treat the supplied commands as examples, not universal instructions. In particular, inspect the project's package manager and `eas.json` before recommending Yarn invocation or adding `eas-cli`; do not add dependencies or scripts unless the task calls for it.
- Prefer read-only inspection and diagnosis first. Before destructive filesystem changes, external account changes, signing changes, App Store submission, or paid cloud builds, explain the concrete action and obtain the required authorization.
- Each skill stays self-contained and focused. Do not create a master router or shared dependency between skills.

## Validation

Validate each skill's frontmatter, naming, and completeness with the skill-authoring validator. Review descriptions for distinct activation boundaries. Exercise each skill against a realistic request and check that it asks for required evidence and respects authorization boundaries. Do not run app builds, modify Apple/EAS accounts, submit an app, or delete user data as part of validation.

## Out of scope

- Implementing or modifying the React Native app itself.
- Installing `eas-cli`, changing project dependencies, generating native projects, or configuring Apple credentials.
- Automating live EAS builds or App Store submissions.
- Providing a full Xcode or App Store manual beyond these five workflows.
