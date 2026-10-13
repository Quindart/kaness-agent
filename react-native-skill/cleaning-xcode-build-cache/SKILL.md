---
name: cleaning-xcode-build-cache
description: Use when Xcode build artifacts are consuming disk space or low disk space is preventing an iOS build.
---

# Cleaning Xcode Build Cache

Free space by targeting confirmed, regenerable Xcode build data. “Clean up Xcode” does not authorize deleting archives, simulator data, project files, or unrelated caches.

## Workflow

1. Establish what is using space and whether an Xcode build is currently running. Inspect sizes and paths of likely build caches without reading file contents or deleting anything.
2. Identify each proposed target by its exact path, size, and likely rebuild impact. Distinguish DerivedData from Archives, simulator device data, device support files, project build outputs, CocoaPods, and source files.
3. If the user named and authorized a specific cache target, remove only that target after confirming the path and that it is safe to clean. If the requested cleanup is broad or the target is unclear, present the exact candidate path(s), size, and consequences, then ask which to remove before deleting.
4. Prefer Xcode's own cleanup controls when they can remove the selected data safely. If using shell commands, quote the exact path and avoid wildcards or broader parent directories.
5. After cleanup, report the paths removed and note that Xcode may need to rebuild derived artifacts. Check free space again only if requested or needed for the next authorized build.

## Guardrails

- Do not run `rm -rf` against Xcode directories before checking the exact target and user authorization.
- Never treat `.xcarchive` contents, simulator device data, signing material, source files, or project `ios/` files as disposable build cache.
- Do not delete caches while an Xcode build is actively using them; wait for the build to finish or ask how to proceed.
- Do not promise that clearing DerivedData will fix a build failure unless the error points to stale derived artifacts.
- Do not clean unrelated package-manager caches, Pods, or the whole Developer directory as a shortcut to disk space.

## Example

If the user only says “Xcode is taking space; clean it,” inspect cache sizes and active builds first. Report the exact candidate path and expected rebuild effect, then ask before removing it. If the user explicitly asks to remove a specific DerivedData folder, verify that exact path and remove only it.
