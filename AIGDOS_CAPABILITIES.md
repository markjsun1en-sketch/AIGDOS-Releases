# AIGDOS 用途与完整能力说明

适用分发版本：**v4.98-beta.1**，2026-11-18。本文按已发布 ZIP 的实际内容整理，包含系统用途、工作方式、生产包全部 53 项公开技能，以及随包工具和进阶能力索引。

[返回首页](README.md) · [新用户指南](V498_BETA1_QUICKSTART.md) · [版本说明](V498_BETA1_RELEASE_NOTES.md) · [构建身份与验证摘要](V498_BETA1_BUILD_RECEIPT.json)

阅读导航：[用途与分工](#1-它解决什么问题) · [包组成与验证口径](#2-本包组成与验证口径) · [能力地图](#3-从想法到交付的能力地图) · [使用顺序](#4-推荐使用顺序) · [命令速查](#5-当前命令入口速查) · [Beta 边界](#6-beta-能力边界与证据) · [53 项技能](#7-全部-53-项公开生产技能) · [包内文档](#8-包内进一步阅读)

## 1. 它解决什么问题

AIGDOS（AI Game Dev OS）把 AI 参与的游戏开发组织成可继续、可检查的工作过程：需求有来源，设计有版本，任务有状态，改动有范围，结果有源码和运行证据，交付有独立审查与人工决定。

它尤其适合以下工作：

- **从想法走向可实施的设计**：把概念、玩法、数值、UI、内容与资源需求拆开研究，再合成能指导生产的 GDD（游戏设计文档）和规格。
- **持续开发一个游戏**：跨多次会话保留任务、设计事实、变更、交接与有证据的经验，减少重新解释项目和找错版本的成本。
- **交付一项实际功能**：限定 Godot 功能切片，组织代码、场景、数据与美术绑定，保留检查结果和实现提交，交接给独立审查人。
- **看清工作是否完成**：通过 Console 和结构化证据区分“正在做”“执行者声称完成”“独立审查通过”“人工接受”。

以上描述用途，不承诺开发速度或质量提升幅度。完整专业效能仍未通过本版本的整体验收。

### 人、AI 宿主和 AIGDOS 如何分工

| 参与方 | 负责的事情 |
| --- | --- |
| 人 / 项目负责人 | 提出目标与约束，选择设计方向，授权项目操作，确认视觉与玩法效果，作出人工接受和发布决定 |
| AI 宿主 / 编程助手 | 阅读项目指令，调用模型完成分析、设计与编码，选择真实工具，提供执行者和审查者身份，执行获准操作 |
| AIGDOS | 提供角色与技能契约、上下文组织、任务与交接入口、产物格式、验证器、证据与记忆工具、项目 Console |
| Godot、Git、图像工具等 | 实际运行引擎、管理代码、生成或处理图像、完成导出与外部验证 |

宿主可以按所提供的适配资料接入；包内保留 Codex、Cursor、Claude Code 与 WorkBuddy 的配置/读集。**存在适配资料不代表所有宿主组合都经本次 Beta 验证。** AIGDOS 本身不提供模型订阅、内置大模型、自动代理调度服务或文件系统权限隔离。

## 2. 本包组成与验证口径

一个 ZIP 包含以下部分，不要求再下载旧版：

```text
DevOS-v4.98-beta-integration/
  dev_os.py                    # 当前候选包命令入口
  candidate_manifest.json      # 文件清单、来源与 SHA-256
  AI_Game_Dev_OS_v4.97/         # 保持原字节的运行时与基础能力
  AI_Game_Dev_OS_v4.98/         # 当前开发增量、生产包与美术工具
  tools/                       # 候选包接入、Console 等工具
```

`AI_Game_Dev_OS_v4.98/` 单独是增量目录，没有完整任务运行时。应使用整个解压包及 `dev_os.py`。包内历史文档、`draft` 标记和案例身份保留原语义；公开分发整个 Beta 不会自动把每个组件升级为正式产品。

本文使用四种明确口径，它们可以同时适用于一个模块：

| 口径 | 含义 | 不能据此推导 |
| --- | --- | --- |
| **本次集成实测** | 同一构建已有隔离新项目与相关 GUI/导出证据，摘要绑定在构建收据 | 任意项目、所有平台或全部技能均已通过 |
| **包内可执行工具** | 交付了脚本、输入格式、检查逻辑或适配器；按依赖和前置条件调用 | 一条命令可代替所有业务判断和人工评审 |
| **宿主执行契约** | 交付了工作方法、角色职责、允许写集、输出与校验要求 | 技能 JSON 会自行启动模型、创建代理或取得权限 |
| **有界研发 / 继承能力** | 保留有限范围的实现、实验或历史验证；需按各模块原契约使用 | 完整生产资格、旧项目迁移资格或通用灾难恢复 |

## 3. 从想法到交付的能力地图

| 领域 | 能做什么、产生什么 | 当前入口与适用条件 |
| --- | --- | --- |
| GDD 与 CoFlow | 从意图生成分阶段计划和上下文包；组织概念、世界、机制、模式、数值、UI、资源与独立设计评审 | `run_gdd_orchestrator.py` 与对应技能；CLI 组织工作，宿主实际创作 |
| 专项设计 | 设计核心循环、系统深度、玩法模式、经济、数值、关卡、内容、成长奖励与玩家可读性 | 产出设计文档和结构化契约；专业判断与玩法验证仍必需 |
| 活文档与变更 | 维护事实和术语、模块依赖、GDD 索引、分块覆盖、审计、变更 delta、批准后的合并与生产规格补丁 | 关联设计来源与批准记录；一致性检查不能发现所有未声明事实 |
| 任务与独立审查 | 创建/恢复任务、记录进度、声明完成、交接和独立审查；绑定实际 Git 提交 | 当前 successor 入口配合固定 State Service；不另造任务状态权威 |
| 代码、场景与数据 | 受约束的 Worker 写入；Godot 功能切片、场景/资源写入与加载检查、RawData 全链和 GameString 修复 | 宿主供给实际实现和项目工具绑定；合作式写锁需要所有写入者遵守 |
| 美术、UI 与资源 | 概念稿、场景、氛围、受控局部修图、透明装饰、图层预览、UI 绑定及视觉接受记录 | 外部生成工具 + 本地处理 + 独立评审；实际素材仍按用途验收 |
| QA 与开发构建 | 定向行为检查、证据包、导出准备、PCK 字节核对、正式开发构建与交付记录 | 当前新项目示例和限定 macOS 正式开发链已有集成实测 |
| 项目 Console | 三个页签查看任务、关系、场景/脚本；显示 GDD、数值表、美术位置与项目阶段 | 本机浏览器；静态展示和记录投影，不改变原验收权威 |
| 记忆与知识检索 | 包外经验捕获、不可变修订、隔离不合格记录、查询及候选索引检查 | 显式输入与证据；不会自动获得全部聊天或私有项目内容 |
| 架构与追溯 | Shared Truth 关联设计、实现、Git 证据；Architecture Baseline 描述模块、场景、资源与设计代码关系 | 继承的 schema、验证器和产物协议；调用者需提供真实且完整的关系 |
| 受控重构与运行记录 | 小范围重构提案、批准交接、前后对照、回归；Agent Harness 运行计划、执行轨迹与交接 | 宿主执行、显式范围与证据；不存在凭计划自动授予的改动权限 |
| 可观察性与资源 | 读取审计、上下文引用、任务时长、显式模型用量；有界容量、数据库与恢复诊断 | 按选择的模块逐项接入；不自动收集所有工作，也不自动通过门禁 |

### 设计与 GDD 的具体用法

GDD 工具支持从游戏意图建立计划、准备某一阶段的上下文、校验阶段产物和评审关系。可用 `brief`、`standard`、`production`、`exhaustive` 深度档组织工作。CoFlow 的八类阶段技能见第 7 节。

实际工作需要宿主读取方法与直接上游材料，形成文档及结构化证据，再由独立评审给出结论。词汇、结构、引用与 schema 诊断可以帮助发现缺项，但不能证明设计好玩或达到专业质量。当前 GDD 消费入口还要求审核证据一致，不能凭一份机器报告自行批准设计。

包内入口：`AI_Game_Dev_OS_v4.98/CF498.md`、`AI_Game_Dev_OS_v4.98/pack_layer/scripts/delivery/run_gdd_orchestrator.py`。旧运行绑定原方法和来源，不能通过改写历史绑定把旧结果当成新方法的验证。

### 美术能做到什么

Art Pipeline Kit 把用途、结构、可变项、来源、生成预算、原始输出、加工记录、视觉评审和收尾放在同一流程中。新任务使用 `tools/workflow_v2.py`；已有图层操作使用 `tools/asset_layers.py`，路径均相对于 `AI_Game_Dev_OS_v4.98/pack_layer/art_pipeline_kit/`。

- 场景、氛围和概念稿：支持从 brief 生成候选、受约束编辑、规范化与评审包；生成由宿主真实图像工具完成。
- 局部修改和素材接受：把执行修图与评审分开，按目标显示尺寸、轮廓、结构、材质、光影和既定用途判断。
- 可复用透明装饰：支持独立装饰路径；精确位置和材质贴合可按任务声明相应测量要求。
- 已有素材图层：给定遮罩进行提取、按原层序显隐、组合预览与跨底图复用。没有自动语义分割或补画被遮挡底图。
- UI 运行时绑定：将已接受的资源连接到项目实际场景/脚本，并要求检查证据；仅有图片接受记录不能证明游戏内效果。

本包保留 Kit 自身的 0.2.5 与增量身份。图层工具虽随本 Beta 提供，不能据此改写旧固定 Kit 的发布状态。严格地图拓扑、多视图一致性、一次生成稳定交付没有保证；角色分层仍为 `experimental / not enabled`，动画、绑定和正式游戏资源替换走各自专项流程。

### 任务、写入与独立审查

`run-request` 接收调用者编写的请求和源码，在包外全新目录创建项目，运行实际 Godot 检查，创建本地源码提交并交接。它结束于 `CLAIMED_DONE`。当前任务操作使用 `tools/dev_os_candidate/task_runtime.py`，源码写入使用 `production_worker.py` 后继入口；固定 State Service 保留状态与历史权威。

创建者、执行者、源码写入者、进度/工作贡献者和完成声明者都不能通过此入口独立批准同一任务。宿主必须提供可信、区分明确的身份；这套记录不是针对恶意调用者绕过入口访问原始 API 的身份认证。

替换已有源码采用 `isolated_cooperative.v1` 协议，比较与发布在同一项目锁下进行。保障范围要求隔离工作区内的所有写入者合作；直接编辑文件或旧式写入者不在该协议的冲突保护范围内。

独立审查者可以先查收：

```sh
python3 -B dev_os.py review \
  --project "/absolute/new-game-run/project" \
  --actor candidate-independent-review --action queue
```

之后按包内 README 的 JSON 输入要求执行 `accept-handoff`、`complete-handoff`，再 `pass` 或 `reject`，提交真实任务、实现提交及证据引用。开放交接仍阻断关闭，审查动作不能顺带修改工作产物。白板人工接受另行保存；历史任务缺少完整参与记录时不能靠该入口强行补成独立通过。

### QA、导出与交付

这里的“正式开发构建”是受检查的开发交付流程，不代表游戏已具备商店发行资格。

| 层次 | 入口 / 产物 | 实际证明的内容 |
| --- | --- | --- |
| 安装检查 | `dev_os.py doctor` | 包清单、依赖、固定 provider 和运行时导入来源等检查 |
| 功能执行 | `dev_os.py run-request`、定向 QA | 指定源码在所选环境的导入与行为断言；另有待审查交接 |
| 导出准备 | `dev_os.py prepare-export` | 按 SHA-256 明确选择源文件，复制到全新工作区，绑定 Godot 4.4 macOS 模板与调试预设 |
| PCK 原始字节核对 | `dev_os.py verify-export` | 已声明 PNG 原件及 GDScript/JSON 字节在独立未加密 PCK v2 中的身份 |
| 正式开发构建 | 基座 `scripts/development/run_godot_development_build.py` | 干净固定 Git 来源、两次导出一致性、签名、恢复、交付与启动门禁，按实际收据判断 |
| 最终确认 | 独立审查 + 白板人工接受 | 具体实现、运行效果和已声明验收条件是否得到接受 |

`prepare-export` 不调用引擎或 Git，也不修改源项目；支持的原始格式和 importer 模式有限。`verify-export` 不证明场景二进制等价、渲染、玩法、签名或 GUI。引擎报错不能被一个 PCK PASS 覆盖。导出程序仍需实际运行与观察。本机 ad-hoc 签名核验不是面向下载用户的 Developer ID 公证。

### Console 与项目可视化

包内 `tools/dev_os_console/README.md` 描述三个页签：

- **Whiteboard**：项目任务、验收条件、证据、下一步，以及已连接的运行时任务，保留历史来源标记。
- **Runtime Graph**：执行者、依赖、交接、问题与门禁引用之间的实际记录关系。
- **Game Map**：从 Godot 主场景和自动加载对象展开 Hierarchy，以 Scratch 式视图查看事件、条件、动作和已记录信号调用，复制文件与行号。

Game Map 基于 Godot 文本场景与 GDScript 静态声明；动态生成对象、读取上限和未知项目阶段如实显示。Console 仅监听本机 `127.0.0.1`。手动打开/刷新：

```sh
python3 -B tools/dev_os_console/session_complete.py \
  --project-root "/absolute/new-game-run/project"
```

新项目指令要求主模型在正常回复结束时调用该入口。需要宿主实际加载指令且具备本机工具权限；强制中断等情况可能没有收尾动作。自定义初始化可用 `dev_os.py model-instructions --project ...` 输出相应文本。

### 记忆、观察与进阶运行能力

**包外长期记忆（LTM）**：`dev_os.py ltm` 支持 `capture`、`capture-sprint` 与 `consult`，请求以 JSON stdin 提供。项目/域/记录身份及内容按完整 UTF-8 SHA-256 区分，修订保留历史，重复重试复用已核对字节。缺少证据或批准的捕获进入隔离记录；查询不返回隔离内容，也不猜测哪一版已被接受。固定包内语料不被修改，不自动迁移旧记忆。

**候选索引与检索**：有公开记忆索引健康检查、显式快照候选构建、只读试查、查询改写、来源摘录和评估入口。候选搜索按所选快照与 SQLite 全文检索规则工作，不是自动读取所有项目的通用语义搜索，也不会自动启用索引。

**观察与度量**：读取审计、任务状态时长、生命周期、恢复动作、交付检查、CoFlow 检查记录、显式模型用量以及选定生产观察总体。缺测和未运行保留为未知，状态停留时间不是人工工时；导入的 token 记录不是自动账单接入。P0/P1 生产度量读端的验证仅使用合成输入，不能拿来宣传实际效率收益。

**继承的生产组合能力**：固定 v4.97 包含 Production Cell、多个任务通道的 DAG 协调、隔离工作区与受控集成、Production Assembly、质量契约、Pack Registry/复用预览、Evidence Continuity、Project Memory、权限感知检索与默认关闭的本机 Team Memory。它们需精确项目绑定及原门禁，保留原 `draft` / 有界验证状态；本 Beta 没有替这些能力完成全项目资格认证。

**OS 治理与扩展基础**：固定基座还保留控制面、Orchestrator、能力协议、运行时数据库、配置/挂载/Hub 路由和 self-evolution 治理入口。FixOps 问题处理基础仍有 docs-only 范围；规则或经验的正式吸收需要原 promotion 流程与批准，不能理解为系统会自行修改和升级自己。完整模块导航见基座 `MODULES_AND_INVOCATION.md`。

**容量、数据库与恢复**：随包包含证据采集、封闭批次共享恢复、流式恢复、执行容量、数据库负载/中断恢复/消费者复核、只读审计与显式保留/退役工具。它们有各自固定来源、资源预算、消费者、证据与授权条件，不是自动清理器或完整文件系统备份。收录 `REFUSAL_RECOVERY.md` 不解除 **CAP16 blocked**；OBS65 同样保持阻断。

## 4. 推荐使用顺序

1. **必要：下载并校验**。按[上手说明](V498_BETA1_RELEASE_NOTES.md)准备环境，运行 `doctor --profile godot`。失败先处理明确依赖或包身份问题。
2. **有用：先跑新项目示例**。选择包外尚不存在的目录，阅读 `charge_meter.json`，执行 `run-request`，查看项目、证据和待审查状态。
3. **必要：为自己的功能明确输入**。向宿主提供游戏意图、当前设计、允许改动范围和验收条件；按对应技能准备请求、源码或其他产物。示例请求不是任意自然语言游戏生成接口。
4. **必要：执行并评审实际结果**。保留失败尝试；对当前实现运行相应测试、导出或 GUI 观察，再办理独立审查与人工接受。
5. **有用：让 Console 和设计索引伴随开发**。持续查看任务、依赖和设计变更，避免依据旧状态继续工作。
6. **可选：按项目需要接入进阶模块**。美术、外部记忆、正式开发导出、生产度量和容量工具分别准备依赖和输入；首次体验无需启用全部模块。

## 5. 当前命令入口速查

以下从解压根目录执行。参数、JSON 格式和前置条件以包内 `tools/dev_os_candidate/README.md` 及各命令 `--help` 为准。

| 命令 | 用途 |
| --- | --- |
| `python3 -B dev_os.py doctor --profile godot` | 检查所选依赖与包身份；还可选 `core`、`art`、`all` |
| `python3 -B dev_os.py capabilities` | 检查 53 项公开技能、路由矩阵、流程 owner 与定义一致性，输出来源路径及哈希 |
| `python3 -B dev_os.py capabilities --skill SKILL_ID` | 查看某项技能的负责人、写集与校验模板；不执行技能 |
| `python3 -B dev_os.py run-request --request REQUEST --workspace NEW_DIR` | 执行调用者请求，在全新隔离项目产生证据与审查交接 |
| `python3 -B dev_os.py task --project PROJECT --action ACTION` | `create` / `resume` / `progress` / `claim` / `handoff`，以 JSON stdin 提供操作载荷 |
| `python3 -B dev_os.py review --project PROJECT --actor ACTOR --action ACTION` | 查队列、办理交接及独立 `pass` / `reject` |
| `python3 -B dev_os.py model-instructions --project PROJECT` | 输出绑定项目的 Console 指令片段，不直接修改项目 |
| `python3 -B dev_os.py ltm --root EXTERNAL_CORPUS --action ACTION` | 以 JSON stdin 捕获或查询包外记忆 |
| `python3 -B dev_os.py prepare-export --request REQUEST --workspace NEW_DIR --template TEMPLATE` | 准备明确绑定来源和模板的新导出项目 |
| `python3 -B dev_os.py verify-export --project PROJECT --pck PCK` | 检查声明的原始文件在实际 PCK 中的字节身份 |

新项目命令的目录必须是包外新目录，父目录已存在。固定 package、已有项目与历史运行不自动升级。包内 RandomHeroes 采用入口只覆盖其指定档案与契约，不是通用迁移工具。

## 6. Beta 能力边界与证据

本版本复用同一构建与同一原 ZIP 的验收。已实测的新项目执行、正式开发导出、相关独立审查、Console 三页签与白板保存/重启读回见[构建收据](V498_BETA1_BUILD_RECEIPT.json)。这次说明文档的补充没有重新运行产品测试或扩大验收范围。

- 实测为 macOS、Python 3.13.2、Godot `4.4.stable.official.4c311cbee`、Git 2.50.1；完整 macOS 小版本/硬件矩阵、Windows、Linux及跨机器复现未资格验证。
- Python 声明最低 3.10，固定依赖为 jsonschema 4.26.0、PyYAML 6.0.3。美术按需另备 Pillow/NumPy、Node.js、ffmpeg/ffprobe，结构转移另需 SciPy；不是每条美术流程都需要相同工具。
- 固定基座保留一处历史脚本语法例外，构建记录为 `PASS_WITH_FROZEN_EXCEPTIONS`。profile 读集使用 `tools/dev_os_candidate/profile_boot_readset.py` 后继入口。
- `doctor`、技能目录、schema PASS、PCK PASS、独立审查、人工接受各自证明不同事情，不能互相代替。
- 生成、创作、角色路由和审查仍依赖宿主正确执行；角色名称不是进程、权限或强身份认证。
- 完整专业效能、通用旧项目升级、一般备份恢复、全部模块长期生产可靠性未由本次 Beta 证明。
- **OBS65、CAP16 继续阻断；公开 Beta 分发不构成完整正式版本资格。**

## 7. 全部 53 项公开生产技能

本清单覆盖当前 v4.98 `end_to_end_production_pack` 对外列出的全部技能，不把兼容镜像重复计数；OS 工具和继承的其他 pack 另列在本说明其他章节。

每一行均为**宿主执行契约**。名称表示任务用途，负责人来自路由矩阵；具体写集、必需输入、产物、失败条件和校验模板须读取技能定义。当前 `capability_pack.json` 的生命周期仍为 `draft`。

包内共同路径：`AI_Game_Dev_OS_v4.98/pack_layer/packs/game_system_packs/end_to_end_production/`。技能定义为该目录下 `skills/<技能 ID>.json`，权威路由为 `references/cross_pack_routing_matrix.json`，工作流视图为 `references/end_to_end_delivery_workflow.json`。`art_asset_preparer` 和 `design_spec_author` 的定义继承自固定 v4.97 对应 pack；`capabilities` 会输出精确路径。

### CoFlow 分阶段设计

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `coflow_concept_draft_skill` | 概念与体验设计 | `gdd_creative_director` |
| `coflow_world_draft_skill` | 世界规则与可玩叙事 | `content_progression_designer` |
| `coflow_mechanics_draft_skill` | 机制、交互与系统深度 | `gameplay_systems_designer` |
| `coflow_modes_draft_skill` | 模式、操作配置与会话生命周期 | `gameplay_systems_designer` |
| `coflow_numeric_draft_skill` | 数值模型、经济与敏感性 | `balance_economy_designer` |
| `coflow_ui_draft_skill` | 前期 UI、信息架构、状态与交互可读性 | `gdd_uiux_bridge_author` |
| `coflow_resources_draft_skill` | 资源需求、规格、库存与生产范围 | `coflow_resource_planner` |
| `coflow_review_draft_skill` | 独立设计审查与生产可行性审查 | `gdd_review_steward` |

### GDD 规划与创作

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `coflow_gdd_synthesis_skill` | 把 CoFlow 决策整理为 GDD 与规格交接 | `gdd_production_author` |
| `coflow_style_longform_gdd_authoring_skill` | 编写 CoFlow 风格长篇 GDD 主稿 | `gdd_creative_director` |
| `gdd_orchestrator_skill` | 组织 GDD 计划、上下文与阶段校验 | `gdd_production_author` |
| `gdd_depth_profile_skill` | 明确 GDD 深度档及交付要求 | `gdd_production_author` |
| `gdd_module_generation_plan_skill` | 从意图规划模块生成顺序与范围 | `gdd_production_author` |
| `gdd_creative_expansion_skill` | 扩展初始意图，形成创意草案 | `gdd_production_author` |
| `gdd_creative_direction_selection_skill` | 比较并选择创意方向 | `gdd_creative_director` |
| `game_research_synthesis_skill` | 整理研究依据，支持设计决策 | `game_researcher` |
| `gdd_module_authoring_skill` | 按生产要求编写 GDD 模块 | `gdd_module_author` |

### 玩法、数值与内容设计

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `core_gameplay_loop_design_skill` | 设计核心玩法循环 | `gameplay_systems_designer` |
| `systems_depth_design_skill` | 设计系统深度与有意义的选择 | `gameplay_systems_designer` |
| `game_mode_design_skill` | 设计可玩的游戏模式 | `gameplay_systems_designer` |
| `numeric_balance_modeling_skill` | 在数据执行前建立数值平衡模型 | `balance_economy_designer` |
| `game_economy_design_skill` | 设计经济循环与约束 | `balance_economy_designer` |
| `gdd_content_framework_design_skill` | 设计内容框架与生产切片 | `content_progression_designer` |
| `gdd_encounter_level_design_skill` | 设计遭遇和关卡模式 | `content_progression_designer` |
| `gdd_progression_reward_design_skill` | 设计成长与奖励 | `content_progression_designer` |
| `gameplay_readability_design_skill` | 从玩家视角说明玩法、反馈与可读性 | `gameplay_systems_designer` |
| `gameplay_option_evaluation_skill` | 在设计锁定前比较玩法方案 | `gameplay_systems_designer` |

### 设计质量、试玩与变更

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `design_fact_registry_skill` | 维护已批准设计事实与术语 | `gdd_review_steward` |
| `playtest_session_record_skill` | 记录结构化试玩证据 | `playtest_session_recorder` |
| `playtest_feedback_synthesis_skill` | 把试玩反馈整理为设计行动 | `playtest_design_synthesizer` |
| `gdd_output_quality_eval_skill` | 生成词汇诊断并消费独立专业评审 | `gdd_review_steward` |
| `design_review_verdict_skill` | 记录生产路由前的设计审查结论 | `gdd_review_steward` |
| `approval_gate_decision_skill` | 持久记录设计批准门禁决定 | `approval_gate_steward` |
| `gdd_uiux_pipeline_bridge_skill` | 把 GDD 反馈要求交接到 UI/UX 流程 | `gdd_uiux_bridge_author` |
| `gdd_chunk_assembly_skill` | 规划和组装分块 GDD，核对覆盖范围 | `gdd_module_author` |
| `gdd_module_dependency_map_skill` | 维护 GDD 模块依赖关系 | `gdd_review_steward` |
| `canonical_gdd_index_skill` | 初始化并维护正式 GDD 索引 | `gdd_review_steward` |
| `gdd_consistency_auditor_skill` | 在进入生产前审计模块一致性 | `gdd_review_steward` |
| `gdd_design_change_delta_skill` | 记录开发中的设计变更差异 | `gdd_production_author` |
| `gdd_delta_merge_skill` | 将已批准变更合入正式 GDD | `gdd_review_steward` |
| `game_design_production_spec_skill` | 把设计意图转为生产契约 | `design_spec_author` |
| `production_spec_patch_skill` | 从已合并设计变更生成实现规格补丁 | `design_spec_author` |

### 美术与运行时绑定

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `scene_art_workflow_skill` | 使用 Kit 制作或修改场景、氛围与概念稿 | `art_asset_preparer` |
| `art_local_edit_execute_skill` | 执行有明确范围的局部修图 | `art_asset_preparer` |
| `creative_asset_acceptance_skill` | 评审资源是否适合指定运行时用途 | `art_asset_preparer` |
| `ui_source_art_runtime_binding_skill` | 将已接受 UI 原始美术绑定到运行时 | `ui_asset_runtime_binder` |
| `visual_polish_stop_close_skill` | 判断视觉打磨何时停止或关闭 | `visual_stop_steward` |

### 实现、数据与 QA

| 技能 ID | 用途 | 负责角色 |
| --- | --- | --- |
| `pipeline_binding_resolver_skill` | 解析项目实际生产工具与路径绑定 | `delivery_readiness_runner` |
| `rawdata_full_chain_execute_skill` | 执行 RawData 到运行时的全链改动 | `rawdata_chain_executor` |
| `godot_runtime_feature_slice_skill` | 实现限定 Godot 功能并交付验证证据 | `godot_runtime_slice_engineer` |
| `focused_qa_evidence_runner_skill` | 执行定向 QA 并组织证据包 | `focused_qa_evidence_runner` |
| `godot_scene_resource_writer_skill` | 写入 Godot 场景/资源并执行加载检查 | `godot_runtime_slice_engineer` |
| `gamestring_formal_chain_repair_skill` | 修复 GameString 与正式文本链 | `rawdata_chain_executor` |

## 8. 包内进一步阅读

下列是**下载并解压后**的相对路径。本公开仓库提供分发包；包内文件没有逐一展开为网页链接。

| 需要了解的内容 | 包内入口 |
| --- | --- |
| 当前候选包操作、审查、导出与采用边界 | `tools/dev_os_candidate/README.md` |
| 项目 Console、三个页签与宿主收尾 | `tools/dev_os_console/README.md` |
| v4.98 增量范围 | `AI_Game_Dev_OS_v4.98/README.md` |
| GDD / CoFlow 阶段编排 | `AI_Game_Dev_OS_v4.98/CF498.md` |
| 美术工作流与图层工具 | `AI_Game_Dev_OS_v4.98/pack_layer/art_pipeline_kit/README.md` |
| 固定基座整体架构与模块调用 | `AI_Game_Dev_OS_v4.97/DESIGN_AND_CAPABILITY_ARCHITECTURE.md`、`MODULES_AND_INVOCATION.md`（后者在同一目录） |
| 继承的生产组合及资格范围 | `AI_Game_Dev_OS_v4.97/V497_DEVELOPMENT_STATUS.md`、同目录 `README.md` |

<details>
<summary>展开 v4.98 观察、记忆、容量与恢复模块文档索引</summary>

以下文档均在包内 `AI_Game_Dev_OS_v4.98/` 根目录。它们记录有界工具和各自前置条件，收录不等于整项正式资格通过。部分历史证据只保存在原开发环境，不随 ZIP 提供；引用路径不能代替可重放证据。

| 包内文件 | 文档主题 |
| --- | --- |
| `ARCHIVE_RETIREMENT.md` | 持久归档退役契约 v1（CAP13） |
| `CODEX_EXEC_CAPTURE.md` | 自动保存一次 Codex 公开输出 |
| `CODEX_EXEC_COMPAT_READER.md` | 读取含缓存写入用量的 Codex 捕获 |
| `COFLOW_AUTHOR_RECEIPT.md` | 作者交接声明与稿件核对 |
| `COFLOW_BATCH_SUMMARY.md` | 汇总一组 CoFlow 检查 |
| `COFLOW_OBSERVATION.md` | 保留每次 CoFlow 检查 |
| `COFLOW_TASK_CHECK.md` | 将一次 CoFlow 检查对应到任务 |
| `CONTEXT_REVIEW.md` | Dev OS 上下文引用与验收检查（OBS55） |
| `DATABASE_ATTRIBUTION.md` | 连续登记回执缺失的持久归属证明（OBS50） |
| `DATABASE_CONSUMER_GUARD.md` | 消费者提交前的恢复证据重验（OBS52） |
| `DATABASE_ENROLLMENT.md` | 消费者首次登记中断恢复（OBS49） |
| `DATABASE_PARENT_RECOVERY.md` | Database workload controller-loss recovery (OBS46) |
| `DATABASE_PROOF_BINDING.md` | 未提交诊断绑定的归属证明恢复（OBS51） |
| `DATABASE_REVIEW.md` | 数据库恢复后的消费者收尾续接（OBS48） |
| `DATABASE_SOAK.md` | Dev OS 持续数据库负载验证（OBS45） |
| `DATABASE_TAKEOVER.md` | Enrolled database takeover (OBS47) |
| `DATABASE_WORKLOAD.md` | Dev OS 数据库负载与恢复验证（OBS44） |
| `DELIVERY_CLAIM_LINK.md` | 核对一次交付声明对应的文件检查 |
| `DELIVERY_REVIEW_SUMMARY.md` | 用直白文字阅读交付检查 |
| `EVIDENCE_CAPACITY.md` | 测试证据容量：v4.98 有界开发切片 |
| `EXECUTION_CAPACITY.md` | 固定交付检查入口的容量接入 |
| `LIFECYCLE_CAPTURE.md` | OBS-03：显式生命周期捕获与结果配对 |
| `LTM_CANDIDATE_SEARCH.md` | 在候选索引中试查并找到来源 |
| `LTM_INDEX_CANDIDATE.md` | 生成可复核的公开记忆索引候选 |
| `LTM_INDEX_HEALTH.md` | 检查记忆索引是否遗漏或仍在引用旧内容 |
| `LTM_QUERY_REWRITE.md` | 保留原问题，试用关键词候选 |
| `LTM_RETRIEVAL_EVAL.md` | 检查搜索结果是否找到了问题需要的记忆 |
| `LTM_SOURCE_EXCERPT.md` | 按搜索来源读取原文 |
| `MODEL_USAGE_SUMMARY.md` | 汇总任务的模型用量 |
| `OBSERVABILITY.md` | V498-OBS-02: read-only audit summaries |
| `OBSERVED_CHECK.md` | 自动记录一次生命周期日志检查 |
| `OBSERVED_DELIVERY_CHECK.md` | 保存一次交付文件检查的证据 |
| `PRODUCTION_MEASUREMENT.md` | P0 生产测量：显式事件协议 |
| `PRODUCTION_MEASUREMENT_P1.md` | V498 P1 并发、worktree 与模型观察读端 |
| `PRODUCTION_OBSERVATION.md` | 完整观察窗口与数据库诊断入口 |
| `READONLY_AUDIT.md` | 只读审计与原始证据退役索引（OBS53） |
| `RECOVERY_OBSERVATION.md` | 看清失败后的重试与后续检查 |
| `REFUSAL_DIAGNOSTICS.md` | 真实 RSS 拒绝诊断（CAP15，有界实验入口） |
| `REFUSAL_RECOVERY.md` | CAP16：超预算停止后诊断中断的有界恢复；CAP16 继续 blocked |
| `RETENTION_CLOSEOUT.md` | 最小保留集与 hold 任务收尾（OBS54） |
| `RETENTION_PARTIAL.md` | 已保留测试材料的部分回收 |
| `SEALED_RECOVERY.md` | 封存恢复来源（CAP14） |
| `SHARED_RECOVERY.md` | 跨 attempt 共享恢复材料：封闭批次 |
| `SHARED_TASK_TIMING.md` | 从任务记录查看状态时间 |

</details>

本文的能力归属依据已发布包的清单、路由矩阵、技能定义和各模块 README。包内原始材料与验证身份保持原样；具体版本以[构建收据](V498_BETA1_BUILD_RECEIPT.json)中的 ZIP SHA-256 和构建提交为准。
