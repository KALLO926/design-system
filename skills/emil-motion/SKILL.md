---
name: emil-motion
description: Emil Kowalski 动效技能组（9 合 1，2026-11-25 kk 确认合并）：从零构建动效、动效评审、代码库级动效审计与改进、动效性能、动效词汇、手势驱动 UI、寻找动效机会、编排多元素运动、移除动效。点名参考，非默认触发。
---

定位（2026-11-25 kk 确认合并）：点名参考，非默认触发；仅 kk 点名或任务明确涉及动效设计/评审时调用。原 9 个独立技能（animate、animation-performance、animation-vocabulary、choreograph、find-animation-opportunities、gestures、improve-animations、remove-animations、review-animations）全文存于 references/<技能名>/，各 SKILL.md 保持上游原样（含各自的点名参考定位声明）。

冲突口径：与 design-contract.md 冲突时以契约为准——全站只许一种缓动性格、动效只为讲清状态变化或叙事、必须有 prefers-reduced-motion 静态等价态。

## 场景路由

| 场景 | 读哪份 |
|---|---|
| 从零给组件/页面加动效、做转场 | references/animate/SKILL.md（配方在同目录 RECIPES.md） |
| 评审已有动效好不好 | references/review-animations/SKILL.md（标准在同目录 STANDARDS.md） |
| 全代码库动效审计＋改进计划 | references/improve-animations/SKILL.md（附 AUDIT.md、PLAN-TEMPLATE.md） |
| 动效卡顿、性能优化 | references/animation-performance/SKILL.md |
| 动效术语、给感觉找准确的词 | references/animation-vocabulary/SKILL.md |
| 多元素编排、队列与节奏 | references/choreograph/SKILL.md（配方在同目录 RECIPES.md） |
| 找哪里值得加动效 | references/find-animation-opportunities/SKILL.md |
| 手势驱动交互（拖拽、滑动） | references/gestures/SKILL.md |
| 删减/移除动效 | references/remove-animations/SKILL.md |
