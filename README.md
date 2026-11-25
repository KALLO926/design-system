# 设计工作体系（Design Work System）

kk 的个人设计工作体系发布镜像，供其他 AI Agent 读取与遵循。本仓库是**镜像**：正本在 kk 的本地工作区，仓库内容由同步脚本从正本生成，请勿期待在此处提交修改。

## 给 Agent 的阅读顺序

1. `overview/design-work-system-overview.md`——体系总览：有哪些技能、规范，平时怎么工作。先读它建立地图。
2. `contract/design-contract.md`——**设计契约，全部设计判断的正本**。与其他文件冲突时以契约为准（kk 当场指令除外）。
3. `contract/design-anti-examples.md`——反例库：被否决过的手法清单，命中即不许交付。
4. `contract/design-lessons.md`——教训缓冲区：待验证的经验，先看再决定是否采纳。
5. `master/design-system-master_v2.2.md`——体系总纲：任务分级、验收门、流程元规则。
6. `skills/`——各技能的 SKILL.md 与 references。核心是六技能流水线：frontend-design 定审美方向 → taste-skill 反 slop 与结构约束 → ui-ux-pro-max 查配色/字体/UX 规则（只查不定向）→ better-interface 最终界面审查 → web-design-guidelines 可访问性与工程审查 → impeccable 机械检测。
7. `patterns/`——自学习沉淀的 UI 模式完成报告（6 条，含索引与报告模板）。
8. `research/`——做法卡 78 张、配色调研、学习素材池。

## 未收录

- impeccable 的工具本体（独立开源仓库，`skills/impeccable/` 只有 SKILL.md 与说明）
- ui-ux-pro-max 的数据表 CSV、各技能的脚本与示例文件（本镜像只收文档：.md/.txt）
- Open Design 存档（约 185MB）、做法卡演示页、图片等二进制资产

## 许可说明

`skills/` 下多个技能为第三方开源作品（MIT / Apache-2.0 / CC BY 等），各自的 LICENSE 与署名文件均原样保留，权利归原作者。kk 自有文档（contract/、master/、overview/、patterns/、research/）为其个人工作规范。
