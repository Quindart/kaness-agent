---
name: debugging-eas-ios-builds
description: Use when an Expo or React Native iOS build on EAS fails, especially in Install dependencies, or when a generic EAS error does not identify the cause.
---

# Debugging EAS iOS Builds

Use the failing build phase and its logs to identify the cause before changing project configuration. A summary such as “Unknown error” is not a diagnosis.

## Workflow

1. Identify the build ID or log URL, platform, selected EAS profile, and exact failed phase. If the logs are inaccessible, ask the user to share the relevant phase output with secrets removed.
2. Inspect the project’s package manager evidence: `package.json`, the lockfile, `packageManager`, Yarn configuration such as `.yarnrc.yml`, and relevant scripts. Check Expo app configuration and the selected `eas.json` profile as well.
3. Compare the failing log with the project’s actual package manager and EAS configuration. Treat Yarn/Corepack, linker, Node version, native dependency, and cache issues as hypotheses until the log supports one.
4. Recommend the smallest change supported by the evidence. Explain which file or setting it changes and why. Ask before changing project files, installing dependencies, or starting a paid cloud build.
5. Retry once with the same platform and profile after the fix. If the same phase fails, gather the new log and revise the diagnosis instead of repeating the same change.

## Guardrails

- Do not infer a Yarn, Corepack, Node, CocoaPods, or cache problem from the generic error alone.
- Do not delete lockfiles, switch package managers, change `nodeLinker`, add `eas-cli`, or add package scripts as a first response. First confirm the project’s current setup and the failing log.
- Treat commands in user notes as examples. Match the repository’s package manager and existing scripts; avoid mixing `yarn dlx`, `npx`, and project scripts without a reason.
- Avoid `--clear-cache` as a reflex. Use it only when the evidence points to stale cached build state, since it can increase build time or cost.
- Redact tokens, environment values, signing material, and private URLs before sharing logs.

## Example

For “EAS iOS build failed: Unknown error, Install dependencies,” ask for that build’s Install dependencies log and inspect the package manager files and EAS profile. Do not prescribe Corepack or a linker change until the evidence supports it.
