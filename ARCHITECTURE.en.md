# AIGDOS Architecture: Layers, Packages, and Runtime

[中文](ARCHITECTURE.md) | **English**

[Home](README.en.md) · [Full capabilities](AIGDOS_CAPABILITIES.en.md) · [Quick start](V498_BETA1_QUICKSTART.en.md)

**AIGDOS has six logical responsibility layers. Its physical OS/Pack split is a different view.** The v4.97 and v4.98 directories identify versioned build inputs; they are not architectural layers. A diagram of runtime connections alone cannot explain these distinctions.

This guide applies to the published **v4.98-beta.1** package. It explains the inherited architecture and where the current Beta components fit, without changing their implementation or qualification status.

## 1. The six logical layers

![AIGDOS logical architecture: six layers from control and authority to observation and delivery](docs/diagrams/layers.en.svg)

[Open the full-size layer diagram](docs/diagrams/layers.en.svg)

| Layer | Responsibility | Representative components and artifacts | Authority boundary |
| --- | --- | --- | --- |
| **L1 — Control and authority** | Decide goals, authorized scope, acceptance, and lifecycle promotion | Human owner/approver, governance owners, Orchestrator control capability | The Orchestrator coordinates within its contract; human approval remains the authority for promotion |
| **L2 — Planning and governance** | Translate intent into plans and operating constraints | Intent baseline, roadmap/phase status, change control, adoption/lifecycle rules, mirror and release policy | Plans and routing recommendations do not create new permissions |
| **L3 — Execution and capability** | Perform scoped design and production work | Production packs, agent/skill contracts, workflows, delivery scripts, Worker and runtime adapters; AI host and engine/tool calls | The host executes the work; role definitions do not start autonomous processes or grant write access |
| **L4 — Runtime core and sources of truth** | Maintain the concrete state and artifacts that work and checks use | Code, schemas, manifests, State Service task state, project bindings, Shared Truth, run plans, handoffs, traces, project artifacts | Package definitions, project state, and external systems keep distinct owners; no single new registry replaces them |
| **L5 — Validation and evidence** | Check results and prepare reviewable verdicts | Schema/semantic checks, focused QA, source/commit bindings, independent review evidence, reconciliation, readiness reports | A check or readiness PASS does not automatically grant human acceptance or release promotion |
| **L6 — Observation and delivery** | Present results and deliver or retain approved outputs | Console views, rendered reports, development-build receipts, release archives, retention/compaction outputs | Views and archives are outputs, not new authorities; retention and disposal keep their original conditions |

These are **responsibility layers**, not six services, processes, or directories. A capability can span layers: a Godot feature uses L3 implementation, L4 source/state, L5 checks, and L6 delivery, under L1/L2 constraints. Evidence returns to the appropriate reviewer or human decision maker; it does not approve itself.

## 2. How logical layers map to the package

The physical split separates reusable OS foundations, game-production capability, host/engine integration, and project data. It is not a one-to-one mapping to L1–L6.

### OS / Pack modules and runtime

![OS and Pack modules connected to adapters, external tools, project runtime, and Console](docs/diagrams/os-pack-runtime.en.svg)

[Open the full-size OS / Pack diagram](docs/diagrams/os-pack-runtime.en.svg)

Read the connections as follows:

1. **OS → Pack:** shared contracts, schemas, and base capability providers support game-specific production methods. The module groups summarize responsibilities within the physical directories; each box is not a separate service.
2. **OS / Pack → adapter integration:** adapters use OS contracts and runtime APIs together with selected Pack skills, workflows, and validators. The candidate entry, host/engine adapters, and production composition are selectable integration surfaces; not every command traverses all three.
3. **Adapters → project runtime and tools:** explicit bindings direct scoped writes and tool calls. Source, task state, handoffs, traces, and evidence belong to the selected project or artifact root outside the package. Godot, Git, and image tools are provided separately.
4. **Project → checks and views:** state and evidence return through the selected integration/validation entries. Console reads records and static structure. Neither a view nor a successful check substitutes for independent review or human acceptance.

The diagram combines the fixed base and selected v4.98 additions by responsibility. In particular, the current production methods and Art Pipeline Kit are in the v4.98 Pack area; the fixed base still supplies the task runtime and inherited providers. User LTM uses an explicit external corpus. OBS65 and CAP16 remain blocked.

| Physical area | What it contains | Relationship to the logical layers |
| --- | --- | --- |
| `os_layer/` | Control-plane contracts, Orchestrator/governance foundations, runtime and database surfaces, memory policies, capability protocols, OS skills, and observation foundations | Supplies control/governance and shared runtime/evidence foundations across several layers |
| `pack_layer/` | Game-production agents, skills, GDD/CoFlow methods, workflows, artifact schemas, delivery validators, and art tools where included | Specializes execution and artifact/evidence production for game development, mainly across L3–L6 |
| Base `adapters/`, `engine_profiles/`, `production_runtime/`, `production_packs/` | Host and engine bindings, Worker/execution/build integration, production composition and its contracts | Connect capability execution to runtime state and validation; inherited development limits still apply |
| Top-level `tools/` and `dev_os.py` | Current candidate entry points, selected successor adapters, packaging checks, and project Console | Expose and connect existing capabilities; they are not a separate authority layer |
| Base `schemas/` and compatibility `packs/`, `scripts/`, `tests/` | Root contracts and compatibility mirrors of pack-layer content | Schemas participate in truth and validation; mirrored files are not additional independent capabilities |
| Project/workspace outside the package | Source, scenes, data, assets, Git commits, `.ai_dev_os` runtime state, and selected evidence/artifact roots | Holds the active project's L4 state and the inputs/outputs used by L3, L5, and L6 |
| External memory and systems | Explicit LTM corpus, Godot, Git/CI, image tools, and any explicitly connected issue/document systems | Supply execution or specialized sources of truth; these are not bundled autonomous services |

`os_layer/` does more than policy: it also contains shared runtime foundations. `pack_layer/` does more than prompts: it includes contracts, scripts, schemas, and validators. Host adapters execute against those definitions rather than redefining them.

## 3. What v4.97 and v4.98 mean here

```text
DevOS-v4.98-beta-integration/
  dev_os.py
  candidate_manifest.json
  AI_Game_Dev_OS_v4.97/       # Fixed base, original bytes
    os_layer/
    pack_layer/
    adapters/
    engine_profiles/
    production_runtime/
    production_packs/
    schemas/
  AI_Game_Dev_OS_v4.98/       # Selected additions, not a standalone runtime
    os_layer/
    pack_layer/
  tools/                    # Candidate entries, successors, Console
```

Both version directories contain OS and Pack areas. The complete Beta combines the fixed base with selected additions and tools. The v4.98 directory alone does not supply the full task runtime. Compatibility mirrors and other package files are omitted from this abbreviated tree.

Project state and user memory live outside this package. Historical `.dev_os_meta` evidence and governance materials from the development checkout are not a required bundled runtime dependency.

## 4. Execution and data flow

The following is the runtime-connection view. Read it after the layer map: boxes here are components and data locations, not L1–L6 layers.

![AIGDOS execution and data flow between the host, package, tools, project, Console, and memory](docs/diagrams/architecture.en.svg)

[Open SVG](docs/diagrams/architecture.en.svg) · [Mermaid source](docs/diagrams/architecture.en.mmd)

The host reads methods and invokes selected entries and external tools. The fixed base supplies task runtime and write mechanisms; the additions supply selected production methods and tools. Console reads project records and static structure. LTM capture/query uses an explicit external corpus. These arrows do not mean every module runs automatically or that one component can take over another's authority.

The separate [delivery flow](AIGDOS_CAPABILITIES.en.md#delivery-flow) explains why `run-request` ending at `CLAIMED_DONE` still leaves independent review and human acceptance to be completed.

## 5. Sources and limits

The six-layer model and OS/Pack separation are grounded in these files inside the published ZIP:

- `AI_Game_Dev_OS_v4.97/DESIGN_AND_CAPABILITY_ARCHITECTURE.md`
- `AI_Game_Dev_OS_v4.97/OS_PACK_LAYER_MAP.md`

These inherited documents retain historical status sections. For current included components and bounded implementation status, read them alongside the base `README.md` and `V497_DEVELOPMENT_STATUS.md`, the increment `AI_Game_Dev_OS_v4.98/README.md`, and `tools/dev_os_candidate/README.md`. Current public limits are summarized in the [release notes](V498_BETA1_RELEASE_NOTES.en.md).

This architecture describes responsibilities and implementation locations, not proof that every path is fully qualified. **OBS65 and CAP16 remain blocked; the Beta has not achieved full formal-release qualification.**
