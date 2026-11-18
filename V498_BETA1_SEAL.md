# AIGDOS v4.98-beta.1 — 封版记录

**中文** | [English](V498_BETA1_SEAL.en.md)

**状态：Beta 分发基线已封存。** 封版日期：2026-11-18。依据为版本所有者在完成发布及文档补齐后明确授权封版。版本名保持 `v4.98-beta.1`，适用范围保持 macOS 上的全新 Godot 项目。

这里的“封版”是固定已发布包及配套文档的可追溯快照。OBS65、CAP16 继续阻断；完整正式版本资格、整体专业效能、通用旧项目迁移、跨平台/跨机器复现和完整备份恢复仍未获本次 Beta 证明。

## 固定身份

| 项目 | 封版基线 |
| --- | --- |
| 分发仓库 | [AIGDOS-Releases](https://github.com/markjsun1en-sketch/AIGDOS-Releases) |
| 分发标签 | [`v4.98-beta.1`](https://github.com/markjsun1en-sketch/AIGDOS-Releases/tree/v4.98-beta.1)，带注释标签，固定含本记录的分发提交 |
| 文档补齐基线 | [`ccf0dd9416028b9dc775aac70a02ba5f5218b075`](https://github.com/markjsun1en-sketch/AIGDOS-Releases/commit/ccf0dd9416028b9dc775aac70a02ba5f5218b075)；封版提交仅补充封版记录与状态链接 |
| 产品构建提交 | `e6d09e203980f5f2df0183b794248ca3b261496e`，见[原构建收据](V498_BETA1_BUILD_RECEIPT.json) |
| 原 ZIP | `DevOS-v4.98-beta.1-candidate.zip` |
| 字节数 | `9880303` |
| SHA-256 | `491387e002ab37f2db1533e6dc3b38b5854933ddb1833f97ba89f2d57f76fc7d` |

继续使用原有[下载入口](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip)和[校验文件](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/main/DevOS-v4.98-beta.1-candidate.zip.sha256)。按标签取得同一原包可使用[封版 ZIP](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/v4.98-beta.1/DevOS-v4.98-beta.1-candidate.zip)与[封版校验文件](https://raw.githubusercontent.com/markjsun1en-sketch/AIGDOS-Releases/v4.98-beta.1/DevOS-v4.98-beta.1-candidate.zip.sha256)；文件名中的 `candidate` 保留原发布身份。GitHub 自动生成的仓库源码归档不是这个开发包。

## 已完成的收尾

- 公众发布任务 `V498-BETA-PUBLISH-01` 已核对为 `accepted`，revision `7`。本地 Whiteboard 人工确认时间为 `2026-11-18T11:50:32+00:00`，确认快照为 `d32e51714d36de9599c02a3c377efa0593a0232f42a4acf20e89ff9e5e7054a1`。这是既有公众分发验收的摘要，不扩大原验收范围。
- 中英文 README、快速指南、完整能力说明、架构及执行时序说明已补齐。Codex、Cursor、WorkBuddy 均有新项目安装与已有项目更新评估提示词，并注明宿主适配和迁移限制。
- 分发仓库根目录已包含 MIT `LICENSE`，两种语言的 README 均已链接；已发布 ZIP 没有因此改包。
- 原包保持原字节、原文件名和原构建身份。封版复用既有产品验证；本次只补充发布状态与固定快照，不重新宣称产品测试通过。

既有构建收据中的 `publication.tag_created: false` 记录首次公众发布时的事实。本次封版新增分发标签，单独记录于此，不追改原收据、原验收历史或原包内清单。

## 封版后变更规则

1. 保留 `v4.98-beta.1` 标签所指快照；不移动或重新打此标签，不替换该版本原 ZIP 或校验文件。
2. 后续功能、产品修复或包内容变化使用新版本号与新包，另行验证和发布。必要文档勘误以独立提交记录，不改写封版快照。
3. 已有安装与项目不自动升级。宿主规则可加载、`doctor` 通过或新目录示例成功，均不能代替迁移依据、独立审查和人工验收。
4. 继续保留 OBS65、CAP16 阻断；解除阻断或授予完整正式版本资格需要各自的证据与验收。本次封版不改变这些状态。

操作入口：[中文快速指南](V498_BETA1_QUICKSTART.md) · [能力说明](AIGDOS_CAPABILITIES.md) · [版本说明](V498_BETA1_RELEASE_NOTES.md) · [MIT 许可证](LICENSE)
