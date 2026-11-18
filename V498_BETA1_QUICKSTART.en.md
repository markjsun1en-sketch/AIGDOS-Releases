# AIGDOS v4.98-beta.1 — Introduction and Quick Start

[中文](V498_BETA1_QUICKSTART.md) | **English**

**AIGDOS (AI Game Dev OS) is a game development framework for working with AI coding assistants.** It organizes game ideas, game design documents (GDDs), development tasks, code and art assets, test evidence, independent review, and human acceptance. It helps you continue and trace development over time. Models, the Godot engine, and image generation tools must be provided separately.

The current public version is **v4.98-beta.1**, intended for **new Godot projects on macOS**. You need only one complete package; no older Dev OS installation is required:

- [Download the complete ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip)
- [Download the SHA-256 checksum file](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256)
- [Purpose and full capability guide](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/AIGDOS_CAPABILITIES.en.md)

## AI-assisted setup: fresh installation or update

For a **new Godot project on macOS**, you can ask your AI coding assistant to set up AIGDOS from this ZIP. Use the full version **v4.98-beta.1** and provide this release repository or the downloaded ZIP's location. Here, installation means verifying and extracting the development package, preparing dependencies, and connecting the host's project instructions to the package.

| Situation | What to ask the assistant to do |
| --- | --- |
| **Fresh installation / new project** | Follow the setup steps below using new extraction, environment, and workspace directories. Keep the development package outside the game project. One complete ZIP is sufficient; no older Dev OS installation is required. |
| **Existing project / update from an older version** | First inspect the installed version, package/project bindings, custom instructions, task state, evidence, and uncommitted changes. This Beta has **no general-purpose updater**. Extracting a new ZIP does not migrate a project; do not overwrite the old installation or rebind its project as a routine setup step. The packaged RandomHeroes adoption tool is limited to its documented profile and is not a general migration tool. |

- **Required:** the assistant needs access to the selected files and permission to run the necessary local commands. Verify SHA-256, prepare the dependencies, run `doctor --profile godot`, and make sure the host actually loads the applicable project instructions. Downloading the ZIP alone does not activate its rules.
- **Useful first check:** after `doctor` passes, run the bundled charge-meter example in a new external workspace. Its `CLAIMED_DONE` result still requires independent review and human acceptance.
- **Optional:** configure art tools, external memory, and advanced modules when the project needs them.

The package includes adaptation material for **Codex**, **Cursor**, and **`workbuddy_private`**, but not every host configuration was validated in this Beta. `workbuddy_private` describes a specific private profile; it does not establish compatibility with every WorkBuddy installation. The assistant should inspect the selected profile and confirm which instructions are active in the actual host. For Codex, this includes the game project's `AGENTS.md` and any applicable override. Preserve existing project rules when configuring a host. See the [host responsibilities and limits](AIGDOS_CAPABILITIES.en.md#responsibilities-of-people-the-ai-host-and-aigdos).

### Copyable prompt for a fresh setup

A short request such as “Set up AIGDOS v4.98-beta.1 for a new Godot project” can start the setup conversation when accompanied by the release repository or local ZIP path. The following complete prompt makes the setup checks explicit:

```text
Set up AIGDOS v4.98-beta.1 for a new Godot project on macOS.
Release repository: https://github.com/markjsun1en-sketch/AIGDOS-Releases

Download the complete ZIP and checksum, or use the copies I provide. Verify SHA-256 before extraction. Read the quick-start guide and tools/dev_os_candidate/README.md inside the package. Use new directories and keep the package outside the game project.

Prepare the documented dependencies, select the profile appropriate to this AI host, and configure/load the project instructions. Run doctor --profile godot. Only after it passes, inspect the supplied charge_meter.json and run it in a new external workspace.

Preserve existing installations, projects, instructions, task state, and uncommitted changes. Keep the package payload unchanged. If the target is an existing project, assess migration support first instead of treating it as a fresh installation.

Report the package/project paths, active host instructions, doctor result, execution evidence, and remaining review/acceptance steps. Do not describe CLAIMED_DONE as human acceptance or full-release qualification.
```

### Copyable prompt for an existing-project update assessment

For an existing project, start with inspection. An applicable migration procedure, validation plan, and rollback approach are needed before changing its bindings or state; this guide does not supply a generic upgrade command.

```text
Assess whether my existing project can adopt AIGDOS v4.98-beta.1.
Release repository: https://github.com/markjsun1en-sketch/AIGDOS-Releases

Read the current installation/project instructions and the target release documentation. Inspect versions, package/project bindings, task state and history, evidence, external memory references, custom host rules, and uncommitted changes.

Report whether a documented migration path applies, what would change, how to validate it, how rollback would work, and what remains unsupported. Do not assume the RandomHeroes-specific adoption tool applies to my project. This request is an assessment: leave the existing installation, project bindings, state, and work products unchanged.
```

## 1. Verify the download

Put the ZIP and checksum file in the same directory, open a terminal there, and run:

```sh
shasum -a 256 -c DevOS-v4.98-beta.1-candidate.zip.sha256
```

The result must report `OK`. Stop and check the download if verification fails. The ZIP is **9,880,303 bytes**. Its expected SHA-256 is:

```text
491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d
```

## 2. Prepare the environment and run the example

You need macOS, Python 3.10+, Git, and Godot 4.4. Validation used Python 3.13.2. Make sure the extraction, virtual environment, and project directories below do not already exist:

```sh
unzip DevOS-v4.98-beta.1-candidate.zip &&
cd DevOS-v4.98-beta-integration &&
python3 -m venv ../devos-beta-venv &&
. ../devos-beta-venv/bin/activate &&
python3 -m pip install -r tools/dev_os_candidate/requirements.txt
```

Once environment setup succeeds, use `doctor` to check the package and dependencies. Run the charge-meter example only after the check passes:

```sh
python3 -B dev_os.py doctor --profile godot &&
python3 -B dev_os.py run-request \
  --request tools/dev_os_candidate/examples/charge_meter.json \
  --workspace "$PWD/../new-game-run"
```

If Godot cannot be found, add `--godot` followed by the actual executable path to both commands. A common macOS path is `/Applications/Godot.app/Contents/MacOS/Godot`.

`new-game-run` must be a new directory outside the package, with an existing parent directory. On success, `new-game-run/project/` contains the project, and `new-game-run/evidence/` contains check results and the review handoff. The command creates local Git commits and finishes at **`CLAIMED_DONE` — completion claimed, independent review pending**. Independent review and human acceptance are still required.

For your own project, prepare and review the request JSON and source code. `run-request` executes code supplied by the caller. It is not a service that generates an entire game from a single sentence, and it does not provide an operating-system sandbox.

## 3. Understand the Beta limits

Existing tests cover Godot import, gameplay checks, development export and execution, related independent review, and the three Console tabs for a bounded new project on macOS. The 53 public skills in the package are working contracts for the host to execute; they have not all been individually accepted in real production.

**OBS65 and CAP16 remain blocked. Full formal-release qualification and overall professional efficacy have not passed acceptance.** This validation does not cover Windows/Linux, cross-machine reproducibility, general upgrades of existing projects, or complete backup and recovery. A successful example run does not mean your game has passed acceptance.

For complete instructions, read `tools/dev_os_candidate/README.md` inside the package or the [release notes](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/V498_BETA1_RELEASE_NOTES.en.md).
