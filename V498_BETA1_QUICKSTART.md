# AIGDOS v4.98-beta.1 — 新用户介绍与上手指南

**中文** | [English](V498_BETA1_QUICKSTART.en.md)

**AIGDOS（AI Game Dev OS）是一套配合 AI 编程助手使用的游戏开发框架**，用于组织游戏想法、GDD、开发任务、代码与美术资源、测试证据、独立审查和人工验收。它帮助你持续推进并追溯开发过程；模型、Godot 引擎和图像生成工具需要自行准备。

当前公开版本为 **v4.98-beta.1**，面向 **macOS 上的全新 Godot 项目**。只需下载一个完整包，无需先安装旧版：

- [下载完整 ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip)
- [下载 SHA-256 校验文件](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256)
- [用途与完整能力说明](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/AIGDOS_CAPABILITIES.md)

## AI 辅助配置：全新安装与更新

对于 **macOS 上的全新 Godot 项目**，可以让 AI 编程助手使用本 ZIP 配置 AIGDOS。请使用完整版本号 **v4.98-beta.1**，并提供本分发仓库或已下载 ZIP 的位置。这里的“安装”包括核对并解压开发包、准备依赖，以及将宿主的项目指令连接到开发包。

| 情况 | 应让助手做什么 |
| --- | --- |
| **全新安装 / 新项目** | 按下方步骤使用全新的解压目录、环境目录和工作区。开发包放在游戏项目之外；一个完整 ZIP 即可，无需先安装旧版 Dev OS。 |
| **已有项目 / 从旧版更新** | 先检查已安装版本、包与项目绑定、自定义指令、任务状态、证据和未提交改动。本 Beta **没有通用自动更新器**；解压新 ZIP 不会迁移项目，不能把覆盖旧安装或重新绑定项目当成普通安装步骤。包内 RandomHeroes 接入工具只适用于其文档限定的配置，不是通用迁移工具。 |

- **必要：** 助手需要能访问指定文件，并获得执行所需本机命令的权限。核对 SHA-256、准备依赖、运行 `doctor --profile godot`，并确认宿主实际加载了适用的项目指令。仅下载 ZIP 不会让规则自动生效。
- **建议先验证：** `doctor` 通过后，在包外新工作区运行自带蓄力条示例。其 `CLAIMED_DONE` 结果仍需独立审查和人工验收。
- **可选：** 项目需要时再配置美术工具、外部记忆与高级模块。

包内包含 **Codex**、**Cursor** 和 **`workbuddy_private`** 的适配资料，但本 Beta 没有验证所有宿主组合。`workbuddy_private` 描述特定私有配置，不能据此认定适配了每一种 WorkBuddy 安装。助手应检查选定配置，并确认当前宿主实际启用了哪些指令。对于 Codex，这包括游戏项目的 `AGENTS.md` 及适用的覆盖文件；配置宿主时应保留已有项目规则。详见[人员、宿主与 AIGDOS 的职责边界](AIGDOS_CAPABILITIES.md#人ai-宿主和-aigdos-如何分工)。

安装提示词：[通用](#可复制的全新安装提示词) · [Cursor](#cursor-新项目安装提示词) · [WorkBuddy](#workbuddy-新项目安装提示词) · [已有项目更新评估](#可复制的已有项目更新评估提示词)

### 可复制的全新安装提示词

附上分发仓库或本地 ZIP 路径后，一句“请为全新 Godot 项目配置 AIGDOS v4.98-beta.1”就可以发起安装。下面的完整提示词明确了配置与检查步骤：

```text
请为 macOS 上的全新 Godot 项目配置 AIGDOS v4.98-beta.1。
分发仓库：https://github.com/markjsun1en-sketch/AIGDOS-Releases

下载完整 ZIP 和校验文件，或使用我提供的副本。解压前核对 SHA-256。阅读快速指南和包内 tools/dev_os_candidate/README.md。使用全新目录，并将开发包放在游戏项目之外。

按文档准备依赖，选择适合当前 AI 宿主的配置，并配置/加载项目指令。运行 doctor --profile godot。只有通过后，才检查自带 charge_meter.json，并在包外新工作区运行示例。

保留已有安装、项目、指令、任务状态与未提交改动，保持开发包内容不变。如果目标是已有项目，先评估迁移支持，不要按全新安装处理。

报告开发包/项目路径、当前生效的宿主指令、doctor 结果、执行证据，以及剩余的独立审查和人工验收步骤。不要把 CLAIMED_DONE 描述成人工验收通过或取得完整正式版本资格。
```

### Cursor 新项目安装提示词

**适配边界：** `cursor` 是 AI 宿主配置，`doctor --profile godot` 中的 `godot` 是引擎检查配置，两者不能互换。包内 Cursor 资料以项目规则和协调者管理的并行代理为模型；是否可用仍需按当前宿主核对。基础安装不要求启用并行代理、hook 或外部平台同步，也不能从其他宿主照搬这些机制。Beta 未逐一验证所有 Cursor 版本及配置。

```text
请在 Cursor 中为 macOS 上的全新 Godot 项目
配置 AIGDOS v4.98-beta.1。
分发仓库：
https://github.com/markjsun1en-sketch/AIGDOS-Releases

1. 阅读快速指南，下载完整 ZIP 和校验文件，或使用我的副本。
   解压前核对 SHA-256；不匹配则停止。
   使用全新的解压目录、虚拟环境和游戏工作区，开发包放在项目外。
   若目标已有内容，先做更新评估，不覆盖已有项目或安装。

2. 阅读包内 tools/dev_os_candidate/README.md。
   按指南准备 Python、Git、Godot 和包内声明的依赖，
   再从包根目录打印 Cursor 的阅读清单：
   python3 -B tools/dev_os_candidate/profile_boot_readset.py \
     --runtime cursor
   输出路径相对于 AI_Game_Dev_OS_v4.97/os_layer/；
   阅读列出的文件，尤其是
   docs/06_runtime_profiles/cursor/RULE_LOADING.md。
   这条命令只列出文档，不会安装或激活规则。
   根据当前 Cursor 的规则机制确定适用指令及其加载位置，
   不照搬其他宿主的私有 Team Mode 或 hook 约定。

3. 在包根目录运行 python3 -B dev_os.py doctor --profile godot。
   通过后审阅 tools/dev_os_candidate/examples/charge_meter.json，
   按指南运行 run-request，使用包外尚不存在的工作区，父目录需存在。
   不要为了预先放置规则而创建 run-request 的目标工作区。
   示例创建 project/ 后，保留生成的项目指令与包绑定，
   配置适用的 Cursor 项目规则并确认实际加载情况。

4. 保留已有规则、安装、项目、任务状态和未提交改动；
   保持 ZIP 及解压后的开发包内容不变。
   报告包/项目路径、生效规则及加载依据、doctor 结果、
   evidence/ 中的实际证据和未验证的宿主功能。
   示例成功不代表完整 Cursor 适配已验收。
   CLAIMED_DONE 之后仍需独立审查和白板上的人工验收。
   OBS65、CAP16 保持阻断，不宣称完整正式版本资格。
```

### WorkBuddy 新项目安装提示词

**适配边界：** `workbuddy_private` 记录特定项目的私有协作语义，不是所有公开 WorkBuddy 安装的兼容性承诺。先核对当前宿主和项目是否满足该配置的条件；不匹配或无法确认时，可以在具备文件与终端能力的前提下验证基础 CLI 流程，但应将私有配置适配标为未确认。基础流程不要求启用 Team Mode、代理同步或外部回写；不要把私有模板中的项目标识、注册表或历史状态复制到新项目。

```text
请在 WorkBuddy 中为 macOS 上的全新 Godot 项目
配置 AIGDOS v4.98-beta.1。
分发仓库：
https://github.com/markjsun1en-sketch/AIGDOS-Releases

1. 阅读快速指南，下载完整 ZIP 和校验文件，或使用我的副本。
   解压前核对 SHA-256；不匹配则停止。
   使用全新的解压目录、虚拟环境和游戏工作区，开发包放在项目外。
   若目标已有内容，先做更新评估，不覆盖已有项目或安装。

2. 阅读包内 tools/dev_os_candidate/README.md。
   若文件和终端能力允许，按指南准备依赖，
   再从包根目录打印私有配置的阅读清单：
   python3 -B tools/dev_os_candidate/profile_boot_readset.py \
     --runtime workbuddy_private
   输出路径相对于 AI_Game_Dev_OS_v4.97/os_layer/；
   阅读清单文件，并补读
   docs/06_runtime_profiles/workbuddy_private/LIMITATIONS.md。
   这条命令只列出文档，不会安装或激活规则。
   核对当前宿主的规则载体、文件/终端能力和协作语义，
   不要仅凭 WorkBuddy 名称选择私有配置。
   仅在适用条件确认后加载适用的项目指令；
   不匹配或无法确认时报告缺口，仅做已支持的基础 CLI 配置与示例。
   不默认启用 Team Mode、代理同步或外部回写，
   不复制私有项目标识、注册表或历史状态。

3. 在具备所需能力的前提下，从包根目录运行
   python3 -B dev_os.py doctor --profile godot。
   通过后审阅 tools/dev_os_candidate/examples/charge_meter.json，
   按指南运行 run-request，使用包外尚不存在的工作区，父目录需存在。
   不要为了预先放置规则而创建 run-request 的目标工作区。
   保留生成的 project/ 指令与包绑定；能加载的规则须核实生效。
   缺少必需能力时报告阻碍，不伪造命令结果或规则加载状态。

4. 保留已有规则、安装、项目、任务状态和未提交改动；
   保持 ZIP 及解压后的开发包内容不变。
   分别报告基础 CLI 验证结果与宿主适配结果，列出包/项目路径、
   生效指令及依据、doctor 结果、evidence/ 证据和未支持或未验证项。
   示例成功不代表 WorkBuddy 私有配置已适配。
   CLAIMED_DONE 之后仍需独立审查和白板上的人工验收。
   OBS65、CAP16 保持阻断，不宣称完整正式版本资格。
```

### 可复制的已有项目更新评估提示词

已有项目应从检查开始。在改变绑定或状态前，需要适用的迁移流程、验证方案和回退办法；本指南不提供通用升级命令。

```text
请评估我的已有项目能否接入 AIGDOS v4.98-beta.1。
分发仓库：https://github.com/markjsun1en-sketch/AIGDOS-Releases

阅读当前安装/项目指令和目标版本文档。检查版本、包与项目绑定、任务状态与历史、证据、外部记忆引用、自定义宿主规则和未提交改动。

说明是否存在适用的已记录迁移路径、会改动哪些内容、如何验证、如何回退，以及仍不支持什么。不要假定 RandomHeroes 专用接入工具适用于我的项目。本次请求仅做评估：保持现有安装、项目绑定、状态和工作产物不变。
```

## 1. 校验下载文件

把 ZIP 和校验文件放在同一目录，在该目录打开终端执行：

```sh
shasum -a 256 -c DevOS-v4.98-beta.1-candidate.zip.sha256
```

应显示 `OK`；失败时停止，重新核对下载。ZIP 大小为 **9,880,303 字节**，预期 SHA-256：

```text
491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d
```

## 2. 准备环境并运行示例

需要 macOS、Python 3.10+、Git、Godot 4.4；实际验证使用 Python 3.13.2。确保下面的解压目录、虚拟环境和项目目录尚不存在：

```sh
unzip DevOS-v4.98-beta.1-candidate.zip &&
cd DevOS-v4.98-beta-integration &&
python3 -m venv ../devos-beta-venv &&
. ../devos-beta-venv/bin/activate &&
python3 -m pip install -r tools/dev_os_candidate/requirements.txt
```

环境准备成功后，先用 `doctor` 检查包和依赖，通过后再执行蓄力条示例：

```sh
python3 -B dev_os.py doctor --profile godot &&
python3 -B dev_os.py run-request \
  --request tools/dev_os_candidate/examples/charge_meter.json \
  --workspace "$PWD/../new-game-run"
```

若找不到 Godot，向两条命令都添加 `--godot` 和实际可执行文件路径；macOS 常见路径为 `/Applications/Godot.app/Contents/MacOS/Godot`。

`new-game-run` 必须是包外全新目录，父目录已存在。成功后，`new-game-run/project/` 保存项目，`new-game-run/evidence/` 保存检查证据与审查交接。命令会创建本地 Git 提交，结束状态为 **`CLAIMED_DONE`（声明完成、待独立审查）**，还需独立审查和人工验收。

自己的项目需要准备并审阅请求 JSON 与源码；`run-request` 执行调用者提供的代码，不是输入一句话就自动生成完整游戏的服务，也不提供操作系统沙箱。

## 3. 了解 Beta 边界

已有实测覆盖限定 macOS 新项目的 Godot 导入、玩法检查、开发导出与运行、相关独立审查，以及 Console 三个页签。包内 53 项公开技能是供宿主执行的工作契约，不代表全部能力均已完成实战验收。

**OBS65、CAP16 继续阻断，完整正式版本资格与整体专业效能尚未通过。** 本次验证不覆盖 Windows/Linux、跨机器复现、通用旧项目升级或完整备份恢复。示例成功也不等于你的游戏已经验收通过。

完整操作请阅读包内 `tools/dev_os_candidate/README.md` 或[版本说明](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/V498_BETA1_RELEASE_NOTES.md)。
