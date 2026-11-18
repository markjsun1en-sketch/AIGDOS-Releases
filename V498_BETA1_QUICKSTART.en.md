# AIGDOS v4.98-beta.1 — Introduction and Quick Start

[中文](V498_BETA1_QUICKSTART.md) | **English**

**AIGDOS (AI Game Dev OS) is a game development framework for working with AI coding assistants.** It organizes game ideas, game design documents (GDDs), development tasks, code and art assets, test evidence, independent review, and human acceptance. It helps you continue and trace development over time. Models, the Godot engine, and image generation tools must be provided separately.

The current public version is **v4.98-beta.1**, intended for **new Godot projects on macOS**. You need only one complete package; no older Dev OS installation is required:

- [Download the complete ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip)
- [Download the SHA-256 checksum file](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256)
- [Purpose and full capability guide](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/AIGDOS_CAPABILITIES.en.md)

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
