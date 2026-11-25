# 设计工作体系（Design Work System · v2.3）

kk 的个人设计工作体系发布镜像，供其他 AI Agent 读取与遵循。本仓库是**纯文档镜像**：正本在 kk 的本地工作区，仓库内容由同步脚本从正本生成，请勿期待在此处提交修改。

## 给 Agent 的阅读顺序（v2.3 分层结构）

1. `overview/design-work-system-overview.md`——体系总览：有哪些技能、规范，平时怎么工作。先读它建立地图。
2. `master/design-system-master_v2.3.md`——**体系总纲现行版**：四步纯净流水线、任务分级（L0 / L1a / L1b / L2）、开工声明、学习节奏与周五量化复盘。（`master/design-system-master_v2.2.md` 为上一版存档。）
3. `contract/`——判断层：
   - `design-contract.md`——**设计契约，全部设计判断的正本**。与其他文件冲突时以契约为准（kk 当场指令除外）。
   - `design-anti-examples.md`——反例库：被否决过的手法清单，命中即不许交付。
   - `design-lessons.md`——教训活跃缓冲区（上限 80 行，周五复盘分流）。
   - `archive/`——已晋升教训的历史归档，只查不读。
4. `skills/SKILLS-ROUTING.md`——**技能路由总表**：调用任何技能前先读本表，再读该技能同目录的 `LOCAL-NOTE.md`（本地定位与冲突覆写）。上游 `SKILL.md` 与契约或 `LOCAL-NOTE.md` 冲突时，以契约与本地覆写为准。
5. `skills/`——各技能的 `SKILL.md`、`LOCAL-NOTE.md` 与 references。现行四步纯净流水线：
   1. **定向**：`frontend-design` 定审美方向，`gemini-frontend-design` 作方向弹药；
   2. **制作与减法自查**：开工声明必填 `内库引用`（先检索 `patterns/` 与 `research/`）；
   3. **工艺双闸门（前置）**：`impeccable` 机检零 P0 ＋ `unslop` 的 `signs.md`/`tokens.md` 工艺审计，未过原地返修；
   4. **语义审查＋受控盲审**：`better-interface` 唯一后段入口（已吸收 `web-design-guidelines` 独有工程项），之后由无过程上下文、带 ≤30 行《评审标尺包》的子 Agent 盲审。
   
   `taste-skill`、`ui-ux-pro-max` 已移出常驻、降为点名参考（原因见路由总表第三节）；`emil-motion`、`gsap`、`yu-apple-design`、`claude-skill-web-clone`、`local-anatomy`、`ui-pattern-learner` 为点名或专项技能。
6. `patterns/`——自学习沉淀的 UI 模式完成报告（6 条，含索引与报告模板）。
7. `research/`——做法卡 78 张（带契约合规标签）、配色调研、学习素材池状态表。

## 纯文档镜像的降级说明

本镜像只收文档（`.md` / `.txt`），不含任何脚本、二进制与数据表。在镜像环境中工作的 Agent 按以下等效方案降级执行：

- **第 3 步机检**：本地 `impeccable` detector 二进制不在镜像内——以 `skills/unslop/references/signs.md` ＋ `tokens.md` 逐项审计 ＋ 真实浏览器 `getComputedStyle` / 对比度实测作为等效替代。
- **taste-skill**：其 `scripts/`（含 `gate.mjs`）不在镜像内，且其 Claude Code `Stop` Hook 与本体系冲突、**禁止启用**；镜像内仅查阅文案参考。
- **ui-ux-pro-max**：`scripts/search.py` 与 CSV 数据表不在镜像内，仅阅读 `references/quick-reference.md` 与 `references/pro-rules.md`。
- **自学习工具链**：`monolith`、`designlang` 等为 kk 本地环境工具，不在镜像内；按 `skills/ui-pattern-learner/SKILL.md` 的方法口径执行即可。

## 未收录

- impeccable 的工具本体（独立开源仓库，`skills/impeccable/` 只有 `SKILL.md`、`LOCAL-NOTE.md` 与说明）
- Open Design 存档（约 185MB）、做法卡演示页、图片等二进制资产
- 已清退技能（如 `local_self_hosted_fonts`，2026-11-25 移入本地回收站，不再随镜像发布）

## 许可说明

`skills/` 下多个技能为第三方开源作品（MIT / Apache-2.0 / CC BY 等），各自的 LICENSE 与署名文件均原样保留，权利归原作者。kk 自有文档（contract/、master/、overview/、patterns/、research/）为其个人工作规范。
