# AIGDOS v4.98-beta.1 — Beta Release Notes

[中文](V498_BETA1_RELEASE_NOTES.md) | **English**

Released on November 18, 2026, this version distributes the development package that passed the second validation iteration. Its public version is `v4.98-beta.1`; “second” refers to the validation iteration, not a beta.2 version.

Start with the [purpose and overview](README.en.md). The [full capability guide](AIGDOS_CAPABILITIES.en.md) covers functional areas, all 53 public skills, host responsibilities, and validation limits.

For a page you can share directly with new users, use the [introduction and quick start](V498_BETA1_QUICKSTART.en.md).

## Download and identity

New users need only this one Dev OS package. There is no need to download or install v4.97 separately. The archive includes the fixed v4.97 base with its original bytes, the v4.98 additions, and supporting tools.

| Item | Value |
| --- | --- |
| ZIP | [DevOS-v4.98-beta.1-candidate.zip](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip) |
| Size | 9,880,303 bytes |
| Build commit | `e6d09e203980f5f2df0183b794248ca3b261496e` |
| Checksum | [SHA-256 file](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256) |
| Build information | [Build receipt](V498_BETA1_BUILD_RECEIPT.json) |

SHA-256: `491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d`.

## Getting started

Required environment: macOS, Python 3.10+, Git, and Godot 4.4. The tested Python version is 3.13.2; other versions have not been validated individually. Godot should be discoverable as `godot`, or you can supply `--godot` and your executable path to both commands below.

Put the ZIP and checksum file in the same directory. Use extraction, virtual environment, and project directories that do not already exist. The checksum must report `OK`; stop if it fails. Run `run-request` only after `doctor` succeeds.

```sh
shasum -a 256 -c DevOS-v4.98-beta.1-candidate.zip.sha256
unzip DevOS-v4.98-beta.1-candidate.zip
cd DevOS-v4.98-beta-integration
python3 -m venv ../devos-beta-venv
. ../devos-beta-venv/bin/activate
python3 -m pip install -r tools/dev_os_candidate/requirements.txt
python3 -B dev_os.py doctor --profile godot
python3 -B dev_os.py run-request \
  --request tools/dev_os_candidate/examples/charge_meter.json \
  --workspace "$PWD/../new-game-run"
```

Use `tools/dev_os_candidate/README.md` inside the archive for complete operating instructions. `new-game-run` must be a new directory outside the package, with an existing parent. The example produces `project/`, project instructions, check evidence, and a review handoff, and creates local source commits in the new project. It ends at `CLAIMED_DONE`, pending independent review. It does not automatically complete human acceptance in Whiteboard.

Your own project requires reviewed request JSON and source code. `run-request` executes code supplied by its caller; it is not an embedded model or an automatic game generation service. Under the new project's instructions, the host opens or refreshes Console at the end of normal replies. The host must load those instructions and permit the required local operations.

## Included changes and existing validation

- Integrates the fixed r2 runtime repairs, independent review, formal development export, and Whiteboard acceptance repairs, while retaining the current Console and Art Pipeline Kit. New-project binding files are excluded from Godot export inputs.
- In a new isolated game project, real Godot import and gameplay checks were completed, followed by two byte-identical macOS formal development exports, signature verification, isolated PCK recovery verification, and exported-app execution and window interaction. Gameplay and export tasks were closed after independent review.
- Chrome checks covered the Console's Whiteboard, Runtime Graph, and Game Map tabs and consecutive refreshes within the same page. Whiteboard save, disk readback, and restart readback agreed.

These results reuse existing acceptance evidence for the same build commit and original ZIP. Publication reused the original package without rerunning product tests. The existing acceptance covers a bounded integration task. Public Beta distribution was separately authorized and does not establish full formal-release qualification.

## Environment and capability limits

- Tested: macOS, Python 3.13.2, Godot `4.4.stable.official.4c311cbee`, and Git 2.50.1. Python requirements are pinned to jsonschema 4.26.0 and PyYAML 6.0.3. Existing acceptance evidence does not bind a specific macOS minor version or full hardware matrix. Windows, Linux, and cross-machine reproducibility have not been qualified.
- Formal development export also requires matching Godot 4.4 macOS export templates and local signing tools. Local signature verification is not Developer ID notarization or public-release qualification. Two matching local builds do not establish cross-machine reproducibility.
- Art workflows may require Node.js, ffmpeg/ffprobe, Pillow, and NumPy; structural transfer additionally requires SciPy. Generation models and tools are supplied by the host, not installed with the package. Existing tool probes include Node.js 22.14.0 and ffmpeg/ffprobe 8.1; these do not establish acceptance of every art workflow.
- `doctor` checks package integrity and selected dependencies. It does not replace gameplay verification, independent review, or human acceptance. SHA-256 verifies bytes; it is not a publisher's digital signature.
- The fixed base retains one historical script syntax exception. Use the successor at `tools/dev_os_candidate/profile_boot_readset.py`. The recorded build syntax result is `PASS_WITH_FROZEN_EXCEPTIONS`.
- OBS65 and CAP16 remain blocked. There is no claim of full formal-release qualification, overall professional-efficacy acceptance, or general migration of existing projects. Existing projects, older versions, and fixed r2 installations are not automatically upgraded.
- Console Game Map displays static scene and script declarations, not a complete expansion of dynamically created objects. Undeclared development tiers and milestones remain unknown. Refreshing the page does not change runtime task or release state.
