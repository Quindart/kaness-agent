---
name: recovering-expo-ios-projects
description: Use when Expo prebuild, an iOS native project, CocoaPods, an Xcode workspace, or an iOS target fails or needs regeneration in a React Native app.
---

# Recovering Expo iOS Projects

Diagnose the generated or maintained iOS project before changing it. Expo prebuild and CocoaPods commands can rewrite files under `ios/`; first establish which files are generated and which contain developer changes.

## Workflow

1. Find the app root and inspect `package.json`, Expo app configuration, relevant config plugins, `ios/Podfile`, and the lockfiles that exist. Identify whether the project commits and maintains its native iOS directory or generates it from Expo config.
2. Inspect Git state for `ios/`, including tracked, modified, and untracked files. If there are local native edits, identify and preserve them before proposing any regeneration. Ask the user about unclear native changes.
3. Read the actual Xcode, prebuild, or CocoaPods error and identify the failing stage. Check the workspace, scheme, and target in Xcode configuration before advising which file to open or build.
4. Recommend the smallest repair that matches the error. For Pods, prefer a normal `pod install` from `ios/` when dependencies are already described by the Podfile and lockfile. Use repo updates, deintegration, or prebuild only when the failure evidence supports them.
5. Before running a clean prebuild, deleting `ios/`, or running `pod deintegrate`, explain what files may change and ask for authorization. Preserve native edits and review the resulting diff before continuing.

## Guardrails

- Do not treat `ios/` as disposable just because Expo can generate it. Native modules, config plugins, signing settings, and handwritten changes may depend on it.
- Do not recommend `rm -rf ios` or `expo prebuild --clean` as a generic repair. A clean prebuild can replace native project files.
- Do not run `pod deintegrate` as a routine step. It removes CocoaPods integration from the Xcode project and can change local files.
- Open the iOS `.xcworkspace` when CocoaPods is integrated. Confirm the actual workspace path and scheme instead of assuming a wildcard matches one project.
- Keep `Podfile.lock` and the project's selected package manager consistent; do not delete lockfiles to make installation errors disappear.

## Example

If prebuild failed and `git status --short -- ios` shows modified native files, stop before clean regeneration. Review the diff and identify whether the files are generated or maintained; preserve the edits and ask before an operation that may overwrite them.
