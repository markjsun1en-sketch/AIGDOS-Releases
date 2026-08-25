# AI Game Dev OS v4.93 User Manual

这份手册面向当前 `AI_Game_Dev_OS_v4.93/` 的个人使用和 AI IDE 接入方式。
如果你使用 Codex、WorkBuddy、Cursor 或其他 agent runtime，先读
`IDE_INTEGRATION.md`，再按本手册执行具体项目流程。

当前 `AI_Game_Dev_OS_v4.93/` 已明确拆成 `os_layer/` 和 `pack_layer/`。
`os_layer/` 以 `AI_Game_Dev_OS_v4.92_Release/` 为完整 OS baseline 的来源记录，
但完整 v4.93 checkout 运行时不依赖外部 v4.92 目录；需要的 OS contract 已打包在
当前 `os_layer/` 内。`pack_layer/` 承载当前 v4.93
`end_to_end_production_pack`。这个物理拆分记录在 `OS_PACK_LAYER_MAP.md`。
入口、模块目录、调用方式、写入边界和命令清单统一放在根目录
`MODULES_AND_INVOCATION.md`，不再由各 layer README 分别承担。

本手册仍主要说明当前 production pack 怎么用。LTM、self-evolution、runtime DB、
hub、os_control_plane、agent_packs、schemas 等完整 OS 能力来自 `os_layer/`。
当前 v4.93 还包含 docs-only FixOps foundation：
`os_layer/docs/31_fixops/`；对应的 Problem/Fix 工作证据在
仓库根目录的 `.dev_os_meta/fixops/` 工作区治理区，而不在
`AI_Game_Dev_OS_v4.93/` 版本包内。memory-routing 基础设施变更、DB/RAG、自动
self-evolution merge 和 skill intake 正式吸收仍需通过 promotion pipeline。

本手册只说明当前 production pack 如何用于长期游戏项目：从游戏想法、GDD、
设计审查、生产规格、运行时交付、QA 证据，到真实项目 readiness 评估。

## 0. AI IDE 接入原则

AI IDE 不应该把 v4.93 当成普通文档目录，也不应该直接从 `os_layer/README.md`
或 `pack_layer/README.md` 开始判断整个系统。正确入口是：

1. `INDEX.md`
2. `MANIFEST.json`
3. `README.md`
4. `DESIGN_AND_CAPABILITY_ARCHITECTURE.md`
5. `MODULES_AND_INVOCATION.md`
6. `RUNTIME_INSTANCE_CONTRACT.md`
7. `REAL_PROJECT_ADOPTION_CONTRACT.md`
8. `ROOT_MIRROR_POLICY.md`
9. `IDE_INTEGRATION.md`
10. `adapters/AI_IDE_ADAPTER_BASE.md`
11. `USER_MANUAL.md`
12. `OS_PACK_LAYER_MAP.md`
13. `os_layer/os_control_plane/leads/orchestrator.capability.json`
14. `pack_layer/packs/game_system_packs/end_to_end_production/capability_pack.json`
15. `pack_layer/packs/game_system_packs/end_to_end_production/references/end_to_end_delivery_workflow.json`
16. `pack_layer/packs/game_system_packs/end_to_end_production/docs/roadmap/PHASE_STATUS.md`

当用户自然表达目标时，AI IDE 应先做路由判断：

```text
目标类型 -> OS/Pack 资源 -> owner agent/control capability -> skill/workflow
-> 允许写入范围 -> 证据 -> 验证命令 -> 停止条件
```

例如：

- “帮我做一个完整游戏 GDD” -> GDD orchestration 和 specialist GDD skills。
- “把 lessismore 接入 v493” -> project readiness、Shared Truth、
  Architecture Baseline、Beta Readiness。
- “这个问题以后别再发生” -> FixOps problem/fix/pattern workflow。
- “让 AI agents 分工做” -> Orchestrator routing、agent run plan、runtime
  adapter contract。

仓库里的 Python 脚本负责生成 contracts、briefs、reports、run plans 和验证结果；
它们不直接调用 LLM agents。真正的执行由 Codex、WorkBuddy、Cursor 或外部
runtime adapter 完成，并把 trace/evidence 回写给仓库验证。

项目绑定、运行计划、execution trace 和本地报告属于目标项目
`.ai_dev_os/` 或显式 artifact root；不要写回 v4.93 的 canonical agents、
skills、workflow、schemas 或根文档。具体边界见 `RUNTIME_INSTANCE_CONTRACT.md`。
真实项目接入和生命周期提升必须按 `REAL_PROJECT_ADOPTION_CONTRACT.md`
准备证据；dry-run、fixture 和 synthetic trace 都不能直接提升 lifecycle。
日常调用优先使用根 `scripts/` 和 `tests/` 命令；`ROOT_MIRROR_POLICY.md`
说明这些兼容入口如何与 `pack_layer/` 保持一致。

## 1. 先理解它是什么

当前 v4.93 pack-layer 不是“一键自动生成游戏 demo”的工具。它更像一个
AI 辅助游戏开发的生产 pack：

- 用 agent 和 skill 合同定义谁负责什么。
- 用 schema 定义每个交付物必须长什么样。
- 用 workflow 定义 GDD、设计、实现、QA、审批的顺序。
- 用验证脚本检查 AI 输出是否能进入真实项目。
- 用 Shared Truth 和 Architecture Baseline 把设计、代码、数据、证据连起来。
- 用 beta readiness report 判断一个项目是否具备继续提升生命周期的证据。

简单说：AI 可以帮助你写、拆、审、改、查，但每一步都必须留下可验证证据。

## 2. 适合谁使用

### 游戏创意或制作人

你可以用它把一个游戏想法变成结构化 GDD、模块拆分、设计风险、生产切片和
审查路线。

### 设计师

你可以用它拆解核心循环、模式、系统、数值、经济、内容、关卡、进度奖励、
玩法可读性和 playtest 反馈。

### 程序和技术负责人

你可以用它把设计产物转成生产规格、runtime/data/art/QA handoff，并通过
Architecture Baseline 检查设计声明和项目文件是否对得上。

### AI agent runtime 或工具开发者

你可以用它生成 agent run plan，让外部 runtime adapter 执行 agent，再把
execution trace 交回仓库验证。

## 3. 主要目录

```text
AI_Game_Dev_OS_v4.93/
  README.md
  INDEX.md
  MANIFEST.json
  MODULES_AND_INVOCATION.md
  OS_PACK_LAYER_MAP.md
  USER_MANUAL.md
  os_layer/
  pack_layer/
  packs/game_system_packs/end_to_end_production/
    capability_pack.json
    agents/
    skills/
    references/
    docs/
  scripts/delivery/
  tests/delivery/
```

如果你需要完整 OS Layer 或 v4.93 生产能力，也先从根目录说明判断调用路径；
确认 owner 后再进入 `os_layer/`、`pack_layer/` 或兼容路径 `packs/`。

最常看的文件：

- `README.md`：当前版本状态和能力地图。
- `USER_MANUAL.md`：这份上手手册。
- `pack_layer/packs/game_system_packs/end_to_end_production/capability_pack.json`：pack
  总 manifest。
- `pack_layer/packs/game_system_packs/end_to_end_production/docs/roadmap/V493_ARCHITECTURE_FILE_AUDIT.md`：
  实际文件和架构审计说明。
- `pack_layer/packs/game_system_packs/end_to_end_production/docs/roadmap/CLI_COMMAND_MATRIX.md`：
  所有主要 CLI 的路径参数和写入边界。
- `pack_layer/packs/game_system_packs/end_to_end_production/docs/beta/REAL_PROJECT_ADOPTION_CHECKLIST.md`：
  真实项目接入 checklist。
- `pack_layer/packs/game_system_packs/end_to_end_production/docs/agent_harness/RUNTIME_ADAPTER_SPEC.md`：
  外部 agent runtime adapter 规范。

根目录 `packs/` 下的同名路径是兼容镜像；AI IDE 做 canonical resource
selection 时应优先引用 `pack_layer/` 路径。

## 4. 三种使用模式

### 模式 A：从一个游戏想法开始

适合你只有初始概念，需要 AI 帮你规划完整 GDD 和后续生产路线。

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/run_gdd_orchestrator.py start \
  --intent "一个疲惫怪物需要玩家帮它们穿过反向塔防迷宫回家的游戏" \
  --project-id monster_commute \
  --artifact-root /tmp/monster_commute_gdd_run \
  --scope full_game_gdd \
  --depth-profile production \
  --quality-profile production
```

这个命令会生成一个 GDD orchestration run package。它不会自动调用 LLM。
你或外部 runtime 需要读取生成的 brief、run state 和 artifact contracts，
再让对应 agent/skill 工作。

适合的后续动作：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/run_gdd_orchestrator.py validate \
  --artifact-root /tmp/monster_commute_gdd_run \
  --depth-profile production \
  --quality-profile production
```

### 模式 B：把已有项目接入 v4.93

适合你已经有一个 Godot 或类 Godot 结构的项目，希望知道它离 v4.93 beta
readiness 还差什么。

先准备：

- 一个项目根目录。
- 推荐的 readiness registry：
  `.ai_dev_os/readiness_binding_registry.json`。如果没有，则使用
  `capability_bindings.json` 或 `.ai_dev_os` 下的兼容 bindings 文件。
- 一个 delivery artifact root，里面放已经完成的 v4.93 JSON artifacts。
- GDD、spec、QA、rollback、RawData、runtime/data/art 文件证据。

先跑 dry run：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/evaluate_pack_beta_readiness.py \
  --project-root <project_root> \
  --artifact-root <delivery_artifact_root> \
  --dry-run \
  --write-report /tmp/v493_beta_readiness_dry_run.json
```

渲染成 Markdown：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/render_beta_readiness_report.py \
  --report /tmp/v493_beta_readiness_dry_run.json \
  --write-markdown /tmp/v493_beta_readiness_dry_run.md
```

第一次真实项目 dry run 出现 `BLOCKED` 是正常的。重点看报告里的：

- `blocking_checks`
- `warning_checks`
- `promotion_blockers`
- `remediation_items`
- `owner`
- `next_action`

如果项目已经有 `.ai_dev_os/readiness_binding_registry.json`，Codex/OS 会优先
按这个 registry 理解项目 readiness。它不是简单的 pass/fail 文件，而是把这些
gate 分开：

- `sprint_closure_gate`
- `focused_qa_gate`
- `human_review_gate`
- `supported_adoption_gate`
- `os_demo_gate`
- `production_release_gate`

所以一个真实状态可以是：SP120 sprint closure 和 focused QA 都是 `PASS`，
但 SP121 art human review 是 `REVIEW_NEEDED`，supported adoption 和
production release 仍然 `BLOCKED`。这不是互相矛盾，而是 v4.93 应该保留的
长期项目状态。

当人工 review 有新变化，例如 placeholder art 被拒绝后，刷新方式是重新跑
beta readiness 和 Markdown renderer：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/evaluate_pack_beta_readiness.py \
  --project-root <project_root> \
  --artifact-root <delivery_artifact_root> \
  --agent-trace <project_root>/.ai_dev_os/agent_execution_trace.json \
  --trace-provenance <project_root>/.ai_dev_os/agent_execution_trace.provenance.json \
  --adoption-mode supported_project_adoption \
  --write-report <delivery_artifact_root>/beta_readiness_report_refreshed.json

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/render_beta_readiness_report.py \
  --report <delivery_artifact_root>/beta_readiness_report_refreshed.json \
  --write-markdown <delivery_artifact_root>/beta_readiness_report_refreshed.md
```

读刷新报告时，不要把 `human_review_gate: REVIEW_NEEDED` 写成“项目 sprint
失败”，也不要把 `provisional_slice_human_accepted_not_production_approval`
写成“生产批准”。

### 模式 C：让外部 agent runtime 执行任务

适合你已经有外部 Codex/agent runtime，希望它按 v4.93 合同执行，而不是让仓库
脚本直接调用 LLM。

先生成 run plan：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/generate_agent_run_plan.py \
  --artifact-root <delivery_artifact_root> \
  --write-plan /tmp/v493_agent_run_plan.json
```

外部 runtime adapter 读取 `/tmp/v493_agent_run_plan.json`，逐个执行
`handoff_lanes`。每个 lane 都会声明：

- `owner_agent`
- `owner_skill`
- `input_artifact_refs`
- `expected_output_refs`
- `expected_output_paths`
- `allowed_write_surfaces`
- `downstream_routes`
- `completion_policy`

执行后，adapter 必须产出 `agent_execution_trace.json`。

验证 trace：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/validate_agent_execution_trace.py \
  --run-plan /tmp/v493_agent_run_plan.json \
  --trace <agent_execution_trace.json>
```

只有 runtime-produced trace 才能作为真实 beta promotion 证据。sample trace 和
synthetic trace 只能用于测试。

## 5. 推荐的完整工作流

```text
1. 写清楚游戏想法或项目问题
2. run_gdd_orchestrator.py start 或 plan
3. AI 根据 agents/skills/contracts 生成 GDD 和设计 artifacts
4. validate_delivery_artifacts.py 检查 artifact 和文件证据
5. design_review_verdict 和 approval_gate_decision 关闭设计权威问题
6. game_design_production_spec_skill 把 approved GDD 转成生产规格
7. RawData / Godot runtime / UI art binding 按最窄 skill 执行
8. focused QA 产出 qa_evidence_bundle
9. Shared Truth 和 Architecture Baseline 建立设计到项目文件的证据链
10. controlled refactor 只处理已批准的小范围修改
11. agent harness 记录外部 runtime 的执行 trace
12. beta readiness 汇总 blockers、warnings、promotion evidence
13. 人工 review 后再决定是否提升 lifecycle_status
```

## 6. GDD 之后怎么继续

GDD 不是终点。v4.93 的核心价值在于把 approved GDD 继续推进到真实项目的
生产交付证据。后续流程按下面的顺序走。

### 6.1 从 GDD 到生产规格

当 GDD 已经通过质量评估、设计审查，并且需要的审批已经完成时，下一步不是
直接改代码，而是生成生产规格。

使用的能力：

- `game_design_production_spec_skill`

输入应该包括：

- approved GDD module 或 accepted gameplay feature brief；
- `design_review_verdict`；
- 必要时的 `approval_gate_decision`；
- production 或 exhaustive 深度下需要 `canonical_gdd_index`；
- 当前 source-of-truth surfaces；
- target player-visible behavior；
- no-go boundaries。

输出应该包括：

- `game_design_production_spec.json`；
- 人类可读 production spec；
- source trace；
- accepted behavior；
- data contracts；
- runtime contracts；
- QA scenarios；
- downstream handoff routes。

这一步的判断标准是：工程师或 runtime agent 能不能只看 production spec 就知道
要改哪些数据、哪些 runtime surface、哪些测试必须通过、哪些范围不能碰。

### 6.2 中途设计变化怎么处理

如果项目已经在制作中，不能让新的讨论、CoFlow note 或 playtest 想法直接改
代码。中途变化应该走 delta 和 patch。

推荐顺序：

```text
design_change_request
  -> gdd_design_change_delta_skill
  -> design_review_verdict_skill
  -> approval_gate_decision_skill when needed
  -> gdd_delta_merge_skill
  -> production_spec_patch_skill
```

使用的关键能力：

- `gdd_design_change_delta_skill`
- `gdd_delta_merge_skill`
- `production_spec_patch_skill`

`production_spec_patch_skill` 只应该从 merged canonical GDD change 生成 patch。
它不能从一段聊天、一个未合并 delta、一个未审批想法直接生成 implementation
patch。

输出应该包括：

- `production_spec_patch.json`；
- 人类可读 spec patch；
- source merge record；
- accepted behavior delta；
- data/runtime contract patches；
- QA regression cases；
- rollback plan；
- downstream handoff routes。

### 6.3 数据链路怎么落地

如果 production spec 里有数据字段、表格、配置、文本 key、资源生成或运行时数据
消费，走 RawData 全链路，而不是在代码里硬编码绕过去。

使用的能力：

- `rawdata_full_chain_execute_skill`
- `gamestring_formal_chain_repair_skill`，当问题是文本或 GameString 链路时使用

典型输入：

- accepted data field 或 row contract；
- RawData workbook 路径；
- importer command；
- schema 或 resource definition；
- runtime repository/loader；
- focused validation target。

输出应该包括：

- 更新后的 source table 或 no-table-change proof；
- regenerated runtime data resources；
- `rawdata_change_manifest.json`；
- importer validation；
- focused Godot smoke；
- rollback note。

完成标准不是“表改了”，而是：

```text
RawData source
  -> importer
  -> generated resource / JSON
  -> runtime repository or loader
  -> focused smoke evidence
```

整条链都能证明。

### 6.4 Godot runtime 怎么落地

如果 production spec 已经有 accepted behavior，并且需要改 Godot 脚本、场景、
资源或测试，使用窄 runtime slice。

使用的能力：

- `godot_runtime_feature_slice_skill`
- `godot_scene_resource_writer_skill`，当重点是 scene/resource 写入时使用

输入应该包括：

- accepted feature behavior；
- affected runtime scripts and scenes；
- affected data or resource paths；
- focused smoke target；
- no-go boundaries。

输出应该包括：

- modified runtime source or scene wiring；
- `qa_evidence_bundle.json`；
- focused runtime validation result；
- regression risk list；
- rollback note。

关键原则：

- 改最小 surface。
- 不做无关 polish 或大重构。
- 至少有一个 focused smoke 能证明行为。
- 如果需要数据或美术决策，回到对应 owner，不要在 runtime 里伪造。

### 6.5 UI 和美术资源怎么绑定

如果源美术、UI 图、icon、状态图等已经被人类或 Art Direction 接受，才能进入
runtime binding。未接受的图不能直接绑定进项目。

使用的能力：

- `creative_asset_acceptance_skill`
- `ui_source_art_runtime_binding_skill`

典型流程：

```text
source art review
  -> creative_asset_acceptance_report
  -> cleaned runtime assets
  -> asset_binding_manifest
  -> runtime binding
  -> screenshot or crop evidence
```

输出应该包括：

- runtime PNG assets；
- cleanup/residue report；
- `asset_binding_manifest.json`；
- runtime binding change 或 no-binding-needed note；
- screenshot/crop evidence；
- rollback note。

不要把 review image、带水印图、带 baked text 的图直接当 runtime asset。

### 6.6 QA 证据怎么关闭

任何 production execution 都应该由 focused QA evidence 关闭，而不是只靠口头
说明。

使用的能力：

- `focused_qa_evidence_runner_skill`

输入应该包括：

- task id；
- acceptance criteria；
- changed file list；
- validators；
- focused Godot smoke；
- visual crop matrix when visual evidence is required。

输出应该包括：

- focused validation command list；
- `qa_evidence_bundle.json`；
- command exit codes；
- PASS tokens 或失败输出；
- screenshot/report paths；
- failure triage；
- closure recommendation。

推荐状态：

- `DONE`：must-pass evidence 全部通过。
- `REVIEW_NEEDED`：需要人类视觉、玩法或产品判断。
- `BLOCKED`：必须修复的问题存在。
- `DEFER`：非阻塞风险明确延期。

### 6.7 什么时候停止继续打磨

长期项目最容易陷入无边界 polish。v4.93 用 stop/close 决策来阻止 AI 自动扩张
范围。

使用的能力：

- `visual_polish_stop_close_skill`
- `visual_stop_state_decision`

适用场景：

- placeholder 已经足够当前迭代；
- visual slice 需要人类 signoff；
- 当前 evidence 只支持 review-ready，不支持 production-pass；
- 继续 polish 需要新任务和新 owner。

输出应该明确：

- close；
- defer；
- reopen named sub-scope；
- escalate to human。

不能一边标记 DONE，一边暗示还会自动继续改。

### 6.8 Shared Truth 和 Architecture Baseline 在后续做什么

GDD 之后，Shared Truth 和 Architecture Baseline 用来防止设计、代码、数据各说
各话。

Shared Truth 负责回答：

- 哪些 facts 是 accepted？
- 哪些只是 proposed assumptions？
- 哪个 artifact 是 canonical owner？
- 哪些 evidence refs 支撑这个事实？

Architecture Baseline 负责回答：

- production spec 声明的 runtime/data surfaces 是否真实存在？
- script、scene、resource、RawData、data file 是否能对上？
- 缺失 surface 应该由哪个 owner 修？
- remediation item 指向哪个 source artifact？

常用命令：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/generate_shared_truth_index.py \
  --artifact-root <delivery_artifact_root> \
  --write-index /tmp/v493_shared_truth_index.json

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/inspect_architecture_baseline.py \
  --project-root <project_root> \
  --artifact-root <delivery_artifact_root> \
  --write-baseline /tmp/v493_architecture_baseline.json
```

### 6.9 最终 readiness 怎么判断

当 GDD、spec、runtime/data/art、QA、Shared Truth、Architecture Baseline 和
agent trace 都有证据后，才进入 beta readiness。

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/evaluate_pack_beta_readiness.py \
  --project-root <project_root> \
  --artifact-root <delivery_artifact_root> \
  --agent-trace <agent_execution_trace.json> \
  --write-report /tmp/v493_beta_readiness_report.json

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/render_beta_readiness_report.py \
  --report /tmp/v493_beta_readiness_report.json \
  --write-markdown /tmp/v493_beta_readiness_report.md
```

只有当 final report 满足这些条件，才可以考虑 lifecycle promotion：

- `verdict: "PASS"`；
- `beta_promotion_allowed: true`；
- `trace_evidence_mode: "supplied"`；
- `promotion_blockers` 为空；
- 人工 review 同意。

## 7. 常用命令

### 验证所有 v4.93 合同

```bash
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short
```

### 只验证文档路径

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/validate_markdown_local_paths.py
python3 AI_Game_Dev_OS_v4.93/pack_layer/scripts/delivery/validate_markdown_local_paths.py
```

### 只跑某个能力域测试

```bash
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k shared_truth
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k architecture_baseline
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k refactor
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k agent
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k beta_readiness
python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k docs_path
```

### 验证 delivery artifacts

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/validate_delivery_artifacts.py \
  --pack-root AI_Game_Dev_OS_v4.93/packs/game_system_packs/end_to_end_production \
  --artifact-root <delivery_artifact_root> \
  --verify-files \
  --require-workflow-closure \
  <delivery_artifact_root>/*.json
```

### 生成和验证 Shared Truth

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/generate_shared_truth_index.py \
  --artifact-root <delivery_artifact_root> \
  --write-index /tmp/v493_shared_truth_index.json

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/validate_shared_truth_index.py \
  /tmp/v493_shared_truth_index.json
```

### 扫描和验证 Architecture Baseline

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/inspect_architecture_baseline.py \
  --project-root <project_root> \
  --artifact-root <delivery_artifact_root> \
  --write-baseline /tmp/v493_architecture_baseline.json

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/validate_architecture_baseline.py \
  /tmp/v493_architecture_baseline.json
```

## 8. 如何让 AI 正确工作

给 AI/Codex 的任务不要只说“帮我做游戏”。更好的任务格式是：

```text
项目目标：
<一句话说明游戏和当前阶段>

要使用的 v4.93 能力：
<例如 GDD orchestration / core gameplay loop / economy design / architecture baseline>

输入材料：
<已有 GDD、代码目录、RawData、art、QA 记录、用户反馈>

期望输出：
<具体 artifact、Markdown、JSON、spec、QA evidence、report>

限制：
必须遵守 AI_Game_Dev_OS_v4.93 的 agents、skills、workflow、allowed writes。
不要自动调用外部 tracker、Git remote 或 LLM agents。
先跑对应验证命令，再给结论。
```

如果任务涉及真实项目修改，应该要求 AI：

- 先读取 `capability_pack.json`、相关 skill contract 和 workflow。
- 明确 owner agent 和 owner skill。
- 明确允许写入和禁止写入边界。
- 生成或更新对应 artifact。
- 跑对应验证脚本。
- 保留 rollback 和 QA evidence。

## 9. 什么情况下算完成

一个 v4.93 任务不应该只因为“AI 写了一段内容”就完成。更合理的完成条件是：

- artifact JSON schema 合法；
- artifact refs 可以解析；
- 人类可读 GDD/spec 文档存在；
- workflow closure 通过；
- QA evidence 存在；
- rollback 或 repair route 明确；
- design review 和 approval gate 满足要求；
- Architecture Baseline 没有未解释的缺失 surface；
- beta readiness report 没有 promotion blockers；
- 对应 pytest selector 或总入口测试通过。

## 10. 不应该怎么用

不要这样使用：

- 让仓库脚本直接调用 LLM agents。
- 把 fixture 成功当成真实项目 adoption evidence。
- 绕过 `approval_gate_decision` 修改核心玩法、经济、胜负、奖励或商业化。
- 让设计 agent 直接改 runtime 文件。
- 让 runtime agent 自己批准设计变化。
- 把 `/tmp` 里的探索输出直接提交成正式证据。
- 提交未评审的 generated GDD 输出目录。
- 自动更新 `lifecycle_status`。

## 11. 文件提交建议

通常应该提交：

- pack manifest、agent、skill、reference、schema、docs 的有意改动；
- delivery scripts 的有意改动；
- 测试和固定 regression fixtures；
- 明确用于验证的 sample artifacts。

通常不应该提交：

- `/tmp` 或本地临时输出；
- 未评审的探索性 GDD 生成结果；
- `__pycache__`、`.pyc`、`.DS_Store`；
- 没有被测试引用的临时报告。

当前仓库特别说明：`tests/delivery/fixtures/gdd_formal_output_tests/` 是本地生成
输出目录，除非单独决定把其中部分文件提升为 regression fixture，否则不要提交。

## 12. 常见问题

### 为什么 lifecycle_status 还是 draft？

因为 v4.93 现在是 beta candidate scaffold。它已经有完整的合同、验证器和
fixtures，但还缺真实项目 adoption evidence 和 runtime-produced agent trace。

### dry run 返回 BLOCKED 是不是失败？

不一定。对真实项目第一次接入来说，`BLOCKED` 往往是正常诊断结果。你应该读取
report 里的 blockers 和 remediation_items，然后按 owner 路由修复。

### 可以只用 GDD 能力，不接入真实项目吗？

可以。你可以只用 `run_gdd_orchestrator.py`、GDD playbooks、quality evaluator
和 GDD validators 来做高质量设计文档。但这不会构成完整 beta promotion 证据。

### 可以直接让 Codex 改项目代码吗？

可以，但推荐先让 Codex 走 v4.93 的 production spec、Architecture Baseline、
allowed writes、QA evidence 和 rollback route。这样改动才适合长期项目维护。

### release 目录怎么用？

`AI_Game_Dev_OS_v4.93_Release/` 是发布快照。日常开发和文档更新应该改
`AI_Game_Dev_OS_v4.93/`。如果要重新发布，需要重新切 release snapshot 并打 tag。

## 13. 最小上手路径

如果你只是想试一次完整流程，按这个顺序：

```bash
python3 AI_Game_Dev_OS_v4.93/scripts/delivery/run_gdd_orchestrator.py start \
  --intent "一个一句话游戏想法" \
  --project-id first_v493_try \
  --artifact-root /tmp/first_v493_try \
  --scope full_game_gdd \
  --depth-profile production \
  --quality-profile production

python3 AI_Game_Dev_OS_v4.93/scripts/delivery/run_gdd_orchestrator.py validate \
  --artifact-root /tmp/first_v493_try \
  --depth-profile production \
  --quality-profile production

python3 -m pytest AI_Game_Dev_OS_v4.93/tests/delivery/test_v493_end_to_end_skill_contracts.py -q --tb=short -k docs_path
```

然后读取 `/tmp/first_v493_try` 里的 run package，把它交给 AI/Codex 继续按
agent/skill 合同补齐 GDD 和 production artifacts。
