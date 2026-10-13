# React Native iOS Skills Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create five independently maintained Markdown Codex skills under `react-native-skill/` for iOS React Native and Expo workflows.

**Architecture:** Each skill gets a separate directory containing one concise `SKILL.md` with YAML frontmatter and focused workflow guidance. There is no shared router or app code. Validate each file's metadata and exercise realistic prompts before completion.

**Tech Stack:** Markdown, YAML frontmatter, Codex skill-authoring validator.

**Spec:** `docs/superpowers/specs/2026-10-13-ios-react-native-skills-design.md`

## Global Constraints

- Place skills under `react-native-skill/`, one directory and one `SKILL.md` per skill.
- Keep each skill self-contained, independently maintainable, and written in Markdown.
- Treat supplied commands as examples; inspect package manager and Expo/EAS configuration before recommending project-specific commands.
- Require authorization immediately before destructive file cleanup, Apple/EAS account mutations, signing changes, App Store submissions, or paid cloud builds.
- Do not encode credentials or personal account information as defaults.
- Do not implement or modify the React Native app.

## Review Focus

- Generic EAS failures can hide distinct dependency, configuration, and native build causes; the skill must request the phase logs before diagnosing.
- Expo prebuild can overwrite native edits; the recovery skill must inspect project ownership and Git state before recommending clean regeneration.
- Simulator, physical-device, Expo Go, and custom development-client workflows have different prerequisites; the runbook must identify the target first.
- Signing, account linking, builds, and submission affect external state or cost; the release skill must separate guidance from executing those actions.
- Xcode cache cleanup deletes local build data and can trigger rebuilds; the cleanup skill must inspect targets and get authorization before deletion.

---

### Task 1: EAS iOS Build Troubleshooter

**Files:** Create `react-native-skill/debugging-eas-ios-builds/SKILL.md`.

- [x] RED: Give an independent subagent a generic `Install dependencies` failure plus incomplete project evidence without this skill; record whether it guesses instead of requesting logs.
- [x] GREEN: Write discriminating frontmatter and guidance to inspect the failing phase, logs, package manager files, Expo config, and EAS profile; include one concise example.
- [x] Re-run the scenario with the skill and confirm it requests the relevant evidence before proposing a cause; validate the frontmatter.

### Task 2: Expo iOS Project Recovery

**Files:** Create `react-native-skill/recovering-expo-ios-projects/SKILL.md`.

- [x] RED: Give an independent subagent a prebuild failure with uncommitted native edits and a suggestion to delete `ios/`; record its baseline behavior without this skill.
- [x] GREEN: Write triggers and guidance to inspect Git state, native project ownership, Expo config, and Pods; make clean prebuild, deletion, and pod deintegration approval-gated.
- [x] Re-run the scenario with the skill and confirm it checks local edits and authorization before destructive repair; validate the frontmatter.

### Task 3: iOS Development Build Runbook

**Files:** Create `react-native-skill/running-ios-dev-builds/SKILL.md`.

- [x] RED: Give an independent subagent a request to run on a physical iPhone with no dev client installed and incomplete project setup; record its baseline guidance without this skill.
- [x] GREEN: Write triggers and guidance distinguishing Expo Go, simulator builds, and custom clients; cover device, Metro, and native prerequisites without assuming Yarn.
- [x] Re-run the scenario with the skill and confirm it identifies the target/client gap before giving commands; validate the frontmatter.

### Task 4: Apple Signing and App Store Release

**Files:** Create `react-native-skill/releasing-ios-apps/SKILL.md`.

- [x] RED: Give an independent subagent a production build request with no account/team choice and ask it to proceed; record its baseline response without this skill.
- [x] GREEN: Write triggers for EAS linking, profiles, signing, and submission; separate release guidance from external account changes, credential creation, paid builds, and submission.
- [x] Re-run the scenario with the skill and confirm it asks for the missing choice and authorization; validate the frontmatter.

### Task 5: Xcode Build Cache Cleanup

**Files:** Create `react-native-skill/cleaning-xcode-build-cache/SKILL.md`.

- [x] RED: Give an independent subagent a request to free disk space without naming removable data; record whether it recommends deletion without inspection or authorization.
- [x] GREEN: Write triggers and guidance to inspect disk use, identify targets and rebuild consequences, separate cache cleanup from unrelated build fixes, and require authorization before deletion.
- [x] Re-run the scenario with the skill and confirm it gathers scope and authorization before deleting data; validate the frontmatter.

### Task 6: Validate the Skill Set

**Files:** Review all five files under `react-native-skill/`.

- [x] Run the skill-authoring quick validator on each skill directory; if its PyYAML dependency is unavailable, parse YAML frontmatter with the system Ruby YAML parser and check required fields.
- [x] Review names and descriptions for distinct activation boundaries and check there are no scaffold placeholders or private credentials.
- [x] Review the five baseline/with-skill scenario outcomes, checking evidence gathering and authorization boundaries.
- [x] Review `git diff --check` and the final file list; leave unrelated existing changes untouched.
