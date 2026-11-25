# 设计工作体系（Design Work System · v2.3 优化版）

kk 的个人设计工作体系镜像，供其他 AI Agent 读取与遵循。

## 给 Agent 的分层阅读顺序（防上下文淹没）

### 第一层：核心启动包（每次任务必读，总计约 20KB）
1. `overview/design-work-system-overview.md` — 体系地图与四步流水线总览。
2. `contract/design-contract.md` — **唯一判断正本**。与其他任何技能或文档冲突时以本契约为准（kk 当场指令除外）。
3. `contract/design-anti-examples.md` — 反例库与二阶默认，命中即不交付。
4. `skills/SKILLS-ROUTING.md` — 技能路由表、冲突覆写总览与无脚本环境降级路径。

### 第二层：按需挂载（仅在对应环节读，切勿一次全量灌入）
- 流程模板、四级任务分级、受控盲审标尺包与周五指标：`master/design-system-master_v2.3.md`
- 活跃待验证教训：`contract/design-lessons.md`（历史已晋升项见 `contract/archive/lessons-promoted-2026-11-25.md`）
- 技能原文与本地覆写：`skills/<skill>/LOCAL-NOTE.md`（**先读 `LOCAL-NOTE.md` 再读 `SKILL.md`**）
- 已验证模式库（6 条，含索引与模板）：`patterns/README.md`
- 调研资产：`research/color-research-2026-11-24.md`、`research/component-recipes-2026-11-20.md`（已标栈与契约高危标签）、`research/learning-sources.md`（状态表）

---

## 本镜像的技能可执行性说明（纯文档环境必看）

本仓库仅收录文档（`.md`/`.txt`），不含脚本、CSV 数据表与二进制检测器：

| 分类 | 技能 | 外部 Agent 在本镜像中如何用 |
|---|---|---|
| **纯文档即用（完整收录）** | `frontend-design`、`better-interface`（含 6 子模块）、`unslop`（含 `signs.md` + `tokens.md`）、`gemini-frontend-design`、`gsap`、`ui-pattern-learner`、`yu-apple-design`、`claude-skill-web-clone`、`local-anatomy` | 直接按 `LOCAL-NOTE.md` + `SKILL.md` 执行 |
| **依赖本地工具（仅作参考/降级）** | `impeccable`（缺检测器二进制与 `reference/`）、`ui-ux-pro-max`（缺 `search.py` 与 CSV）、`taste-skill`（缺 `scripts/gate.mjs` 与 `starter.html`，且已移出默认流水线） | 见 `skills/SKILLS-ROUTING.md` 的纯文本降级方案（用 `unslop` 工艺表＋浏览器实测替代 `impeccable` 机检） |

---

## 同步脚本维护建议（供本地正本同步器参考）

1. **增补 4 份漏同步的核心引用正本**：`DESIGN-ASSETS.md`、`associative-exploration-2026-11-21.md`、`ui-learning-curriculum-2026-11-21.md`、`design-skill-audit-2026-11-25_v1.md`。
2. **排除冗余社区/空脚手架杂项**：同步时过滤 `skills/local-anatomy/.github/`、`CODE_OF_CONDUCT.md`、`CONTRIBUTING.md`、`SECURITY.md`、`CHANGELOG.md` 以及已退出默认链的 `taste-skill/assets/` 脚手架文档，镜像体积可缩减 >50%。
3. **路径自动映射**：同步导出时将 `~/workspace/custom-skills/` 与 `~/workspace/design-research-library/` 映射为仓库相对路径，避免断链。

## 许可说明

`skills/` 下多个技能为第三方开源作品（MIT / Apache-2.0 / CC BY 等），各自 LICENSE 与署名均原样保留；第三方技能的上游 `SKILL.md` 尽量保持原样，所有本地规则调整沉淀在同目录 `LOCAL-NOTE.md`。
