# Expo, Beacon, and Push Notification Skills Implementation Plan

> **For agentic workers:** Implement each task in order in this session. Each task creates one standalone Markdown skill and receives its own focused content review before the next task.

**Goal:** Add three standalone Expo device, BLE Beacon, and iOS push-notification skills under `react-native-skill/`.

**Architecture:** Keep each workflow in its own `SKILL.md`. Preserve the existing specialized iOS build, EAS, recovery, release, and cache skills; link to one only when its workflow is directly relevant. Use evidence-based troubleshooting and official documentation links for details that change by SDK, OS, or service.

**Tech Stack:** Markdown, YAML frontmatter, Expo, React Native, iOS Core Location/APNs, Android BLE/ADB, Firebase Cloud Messaging.

**Spec:** `docs/superpowers/specs/2026-10-13-expo-ble-push-skills-design.md`

## Global Constraints

- Create exactly the three skills named in the spec under `react-native-skill/`.
- Keep each skill independently useful and avoid duplicating existing iOS release, signing, EAS failure, recovery, and cache guidance.
- Do not copy private account, email, team, app, device, key, token, or project identifiers from the supplied notes.
- Do not perform external account, signing, Firebase, Apple Developer Portal, EAS project, or paid-build changes.
- Verify version-sensitive details against current official documentation; avoid encoding stale service quotas as fixed facts.
- Do not add or run application tests. Review each Markdown artifact directly against its task requirements.

## Review Focus

- Expo account requirements differ between local `expo run:*`, EAS cloud builds, Expo Go, and platform/version; each path must be distinguished.
- Beacon APIs differ: generic BLE advertisement scanning must not inherit iBeacon UUID or location assumptions.
- iOS notification issues can occur at different layers; logs and built entitlements must identify the failing layer before suggesting a fix.
- Service-account roles, APNs credentials, quotas, command syntax, and platform permissions may change; use current official sources for these details.
- Examples must not imply authorization to modify Apple/Firebase accounts, rotate credentials, register devices, or start paid builds.

---

### Task 1: Expo device development skill

**Files:**
- Create: `react-native-skill/running-expo-device-builds/SKILL.md`

**Interfaces:**
- Consumes: The Expo device-build scope and constraints in the spec; existing `running-ios-dev-builds`, `releasing-ios-apps`, and `debugging-eas-ios-builds` skill scopes.
- Produces: A standalone skill for choosing and launching Expo Go, local development builds, or EAS development builds on physical iOS and Android devices.

- [x] Write frontmatter with `name: running-expo-device-builds` and a concise trigger-focused description.
- [x] Explain the decision among Expo Go, local dev build, and EAS; distinguish account/signing needs by route and note version-sensitive Expo Go requirements.
- [x] Cover Android USB discovery/ADB reverse only as relevant; inspect package manager, scripts, app config, native project, and device prerequisites before commands.
- [x] Add authorization boundaries for project linking, device registration, signing changes, and paid cloud builds; point to existing iOS skills for their specialized workflows.
- [x] Review the completed skill against all five Review Focus items and the spec's acceptance criteria.

**Review result:** Confirm standalone scope, valid frontmatter, accurate official Expo references, no duplicated iOS signing procedure, and no private identifiers.

---

### Task 2: BLE Beacon debugging skill

**Files:**
- Create: `react-native-skill/debugging-ble-beacons/SKILL.md`

**Interfaces:**
- Consumes: The Beacon scope and constraints in the spec.
- Produces: A standalone diagnostic skill that separates generic BLE advertisements from iBeacon/Core Location behavior across iOS and Android.

- [x] Write frontmatter with `name: debugging-ble-beacons` and a trigger-focused description.
- [x] Start by identifying library/API, device/runtime, platform/OS, scan mode, permissions, and expected Beacon protocol.
- [x] Separate app authentication from OS permissions; make location and UUID requirements conditional on the API/platform rather than universal.
- [x] Include a bounded hardware/advertising cross-check using an independent scanner such as nRF Connect, and interpret results as evidence rather than certainty.
- [x] Link to current official Apple, Android, Expo, and selected-library references where relevant; review against the spec.

**Review result:** Confirm the workflow does not prescribe Expo login as a BLE permission, assumes no GPS requirement for all BLE scans, and does not claim every scan requires a known UUID.

---

### Task 3: iOS push notification debugging skill

**Files:**
- Create: `react-native-skill/debugging-ios-push-notifications/SKILL.md`

**Interfaces:**
- Consumes: The APNs/FCM diagnostic scope and constraints in the spec.
- Produces: A standalone diagnostic skill tracing iOS push delivery from app registration through APNs/FCM to the backend sender.

- [x] Write frontmatter with `name: debugging-ios-push-notifications` and a trigger-focused description.
- [x] Identify notification libraries and distinguish permission/presentation, token registration, APNs credentials/entitlements, Firebase configuration, and backend authorization.
- [x] Require inspection of the built `aps-environment` entitlement, app/bundle/project identity, token type, relevant logs, and service-account project/role before selecting a fix.
- [x] Treat `expo-notifications` and `@react-native-firebase/messaging` as potentially distinct layers; verify current integration and library configuration rather than asserting universal compatibility.
- [x] Avoid guesses about sandbox/production, key replacement, app deletion/reinstall, role names, limits, or delivery guarantees; use current official Apple/Firebase/Expo references.
- [x] Review the completed skill against the spec and all five Review Focus items.

**Review result:** Confirm diagnostic ordering follows observed evidence, portal/key/account mutations require explicit authorization, and no supplied private identifiers appear.

---

### Task 4: Final review

**Files:**
- Review: `react-native-skill/running-expo-device-builds/SKILL.md`
- Review: `react-native-skill/debugging-ble-beacons/SKILL.md`
- Review: `react-native-skill/debugging-ios-push-notifications/SKILL.md`

- [x] Compare all three documents with the spec's acceptance criteria and check for conflicting instructions, overly broad claims, duplicated workflows, and private identifiers.
- [x] Confirm the three frontmatter names match their directory names and descriptions route their intended requests.
- [x] Report completion with the created file links and any remaining factual limits; do not claim tests were run.
