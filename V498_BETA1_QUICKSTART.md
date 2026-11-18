# AIGDOS v4.98-beta.1 — 新用户介绍与上手指南

**AIGDOS（AI Game Dev OS）是一套配合 AI 编程助手使用的游戏开发框架**，用于组织游戏想法、GDD、开发任务、代码与美术资源、测试证据、独立审查和人工验收。它帮助你持续推进并追溯开发过程；模型、Godot 引擎和图像生成工具需要自行准备。

当前公开版本为 **v4.98-beta.1**，面向 **macOS 上的全新 Godot 项目**。只需下载一个完整包，无需先安装旧版：

- [下载完整 ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip)
- [下载 SHA-256 校验文件](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256)
- [用途与完整能力说明](https://github.com/markjsun1en-sketch/AIGDOS-Releases/blob/main/AIGDOS_CAPABILITIES.md)

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
