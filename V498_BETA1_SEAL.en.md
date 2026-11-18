# AIGDOS v4.98-beta.1 — Release Seal Record

[中文](V498_BETA1_SEAL.md) | **English**

**Status: Beta distribution baseline sealed.** Seal date: November 18, 2026. The version owner explicitly authorized the freeze after publication and documentation completion. The version remains `v4.98-beta.1`, scoped to new Godot projects on macOS.

“Sealed” means a traceable snapshot of the published package and its documentation. OBS65 and CAP16 remain blocked. This Beta still does not establish full formal-release qualification, overall professional efficacy, general existing-project migration, cross-platform/cross-machine reproducibility, or complete backup and recovery.

## Pinned identity

| Item | Sealed baseline |
| --- | --- |
| Distribution repository | [AIGDOS-Releases](https://github.com/markjsun1en-sketch/AIGDOS-Releases) |
| Distribution tag | [`v4.98-beta.1`](https://github.com/markjsun1en-sketch/AIGDOS-Releases/tree/v4.98-beta.1), an annotated tag pinning the distribution commit containing this record |
| Completed-documentation baseline | [`ccf0dd9416028b9dc775aac70a02ba5f5218b075`](https://github.com/markjsun1en-sketch/AIGDOS-Releases/commit/ccf0dd9416028b9dc775aac70a02ba5f5218b075); the seal commit adds only seal records and status links |
| Product build commit | `e6d09e203980f5f2df0183b794248ca3b261496e`; see the [original build receipt](V498_BETA1_BUILD_RECEIPT.json) |
| Original ZIP | `DevOS-v4.98-beta.1-candidate.zip` |
| Bytes | `9880303` |
| SHA-256 | `491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d` |

The existing [download URL](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip) and [checksum file](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256) remain in use. The same original bytes are available through the [tagged ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/v4.98-beta.1/DevOS-v4.98-beta.1-candidate.zip) and [tagged checksum](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/v4.98-beta.1/DevOS-v4.98-beta.1-candidate.zip.sha256). The filename retains `candidate` as part of the original release identity. GitHub's automatically generated repository source archive is not this development package.

## Completed closeout

- Public distribution task `V498-BETA-PUBLISH-01` was checked as `accepted`, revision `7`. The local Whiteboard human confirmation is dated `2026-11-18T11:50:32+00:00`, with confirmation snapshot `d32e51714d36de9599c02a3c377efa0593a0232f42a4acf20e89ff9e5e7054a1`. This summarizes the existing public-distribution acceptance without expanding its scope.
- Chinese and English READMEs, quick-start guides, capability documentation, architecture guides, and execution sequence diagrams are complete. Codex, Cursor, and WorkBuddy each have fresh-project setup and existing-project update-assessment prompts with host and migration limits.
- The distribution repository contains a root MIT `LICENSE`, linked from both READMEs. Adding it did not modify the published ZIP.
- The package retains its original bytes, filename, and build identity. Sealing reuses existing product validation; it records release status and a pinned snapshot without claiming new product-test results.

The original build receipt's `publication.tag_created: false` describes the initial public publication. This seal adds a distribution tag and records that later action here. It does not rewrite the original receipt, acceptance history, or package manifests.

## Changes after sealing

1. Preserve the snapshot referenced by `v4.98-beta.1`. Do not move or recreate that tag, or replace this version's original ZIP or checksum file.
2. Subsequent features, product fixes, or package-content changes require a new version and package, with separate validation and publication. Necessary documentation corrections use separate commits without rewriting the sealed snapshot.
3. Existing installations and projects do not upgrade automatically. Loaded host rules, a passing `doctor`, or a successful new-directory example do not replace migration evidence, independent review, or human acceptance.
4. Keep OBS65 and CAP16 blocked. Clearing either gate or granting full formal-release qualification requires its own evidence and acceptance. This seal does not change those states.

Start here: [English quick start](V498_BETA1_QUICKSTART.en.md) · [Capability guide](AIGDOS_CAPABILITIES.en.md) · [Release notes](V498_BETA1_RELEASE_NOTES.en.md) · [MIT License](LICENSE)
