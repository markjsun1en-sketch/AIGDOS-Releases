# AIGDOS v4.98-beta.1 — Beta 版本说明

本版本发布于 2026-11-18，对应第二轮验证通过的开发包。公开版本名为 `v4.98-beta.1`；“第二轮”指验证轮次。

首次了解 AIGDOS，请先读[用途与能力总览](README.md)；完整的功能分类、53 项公开技能、宿主分工与验证边界见[能力说明](AIGDOS_CAPABILITIES.md)。

## 下载与身份

新用户只需这一个 Dev OS 包，不需要先下载或安装 v4.97。包内包含保持原字节的固定 v4.97 基座、v4.98 增量和配套工具。

| 项目 | 对应值 |
| --- | --- |
| ZIP | [DevOS-v4.98-beta.1-candidate.zip](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip) |
| 大小 | 9,880,303 字节 |
| 构建提交 | `e6d09e203980f5f2df0183b794248ca3b261496e` |
| 校验和 | [SHA-256 文件](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256) |
| 构建信息 | [构建收据](V498_BETA1_BUILD_RECEIPT.json) |

SHA-256：`491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d`。

## 新用户开始

必要环境：macOS、Python 3.10+、Git、Godot 4.4。实测 Python 为 3.13.2；其余 Python 版本未逐版验证。Godot 应可由命令 `godot` 找到，或向下面两条命令同时提供 `--godot` 和自己的可执行文件位置。

把 ZIP 与校验文件放在同一目录，选择尚不存在的解压目标、虚拟环境和项目目录：

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

以包内 `tools/dev_os_candidate/README.md` 为完整操作入口。`new-game-run` 必须是包外全新目录，父目录已存在。示例会生成 `project/`、项目指令、检查证据及审查交接，并在新项目创建本地源码提交；结束状态为 `CLAIMED_DONE`，还需独立审查。它不会自动完成白板人工验收。

自己的项目需提供经过审阅的请求 JSON 和源码；`run-request` 执行调用者提供的代码，不是内置模型或自动游戏生成服务。Console 按新项目指令在正常回复结束时打开或刷新；宿主须加载项目指令并允许所需本机操作。

## 本次包含与已有实测

- 集成固定 r2 的运行时修复、独立审查、正式开发导出与白板验收修复，保留当前 Console 和 Art Pipeline Kit。新项目绑定文件已移出 Godot 导出输入。
- 在隔离的新游戏项目完成实际 Godot 导入与玩法检查，完成两次内容一致的 macOS 正式开发导出、签名核验、隔离 PCK 恢复核验、导出程序运行与窗口操作；玩法和导出任务经独立审查关闭。
- 在 Chrome 验证 Console 的 Whiteboard、Runtime Graph、Game Map 三个页签及同页连续刷新；白板保存、磁盘读回和重启读回一致。

以上复用同一构建提交与原 ZIP 的现有验收结果，本轮复用原包发布，未重新执行产品测试。已有验收针对限定集成任务；本次公众 Beta 分发另经授权，不构成完整正式版本资格。

## 环境与能力边界

- 实测：macOS、Python 3.13.2、Godot `4.4.stable.official.4c311cbee`、Git 2.50.1。包内 Python 依赖固定为 jsonschema 4.26.0、PyYAML 6.0.3。现有验收材料未绑定 macOS 小版本及完整硬件矩阵；Windows、Linux 与跨机器复现未资格验证。
- 正式开发导出另需匹配的 Godot 4.4 macOS 导出模板及本机签名工具。现有本机签名核验不构成面向下载用户的 Developer ID 公证或发布资格；本地双构建一致不证明跨机器可复现。
- 美术功能按需准备 Node.js、ffmpeg/ffprobe、Pillow、NumPy，结构转移另需 SciPy；生成模型和工具由宿主提供，未随包安装。现有工具探测记录含 Node.js 22.14.0、ffmpeg/ffprobe 8.1，不代表所有美术流程逐项验收。
- `doctor` 检查包完整性和所选依赖；不能替代玩法、独立审查或人工验收。SHA-256 用于核对字节，不是发布者数字签名。
- 包含固定基座的一处历史脚本语法例外；请使用包内 `tools/dev_os_candidate/profile_boot_readset.py` 后继入口。已有构建语法检查结果为 `PASS_WITH_FROZEN_EXCEPTIONS`。
- OBS65、CAP16 继续阻断。没有完整正式版本资格、专业效能整体通过或通用旧项目迁移承诺。已有项目、旧版本与固定 r2 不自动升级。
- Console Game Map 展示静态场景和脚本声明，不完整展开动态生成对象；未声明的开发档位和里程碑显示未知。页面刷新也不改变运行时任务或发布状态。
