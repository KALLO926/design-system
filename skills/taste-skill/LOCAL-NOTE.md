# 本地定位说明（2026-11-25 审计降级：移出常驻流水线）

- **现行定位**：**已移出常驻默认流水线**，降级为按需参考（仅在需要文案去套话时查阅 `references/copy-voice.md` / `copy-tells.md` / `copy-moves.md`）。上游 `SKILL.md` 与 `references/` 原样保留供查阅，**禁止按其默认脚手架流程执行**。
- **移出常驻的 4 项硬冲突（源码级实证）**：
  1. **PPT 骨架冲突**：House Rule #3 与 `gate.mjs` 强制 `One section per screen`（每节 1–2 屏、相邻标题不露头），直接违反 `design-contract.md`「禁止 PPT 式横条带堆叠、按连续滚动空间编排」。
  2. **颜色禁令冲突**：第 4 节默认随机发牌 `dark-band|brand-band|surface-pairs` 及 `<html data-ground>` 池中的 **`sand|sky|sage`（沙黄/鼠尾草绿）**，直接触碰「全页一底到底」与 kk 2026-11-24「禁墨绿/米黄底色」红线。
  3. **决策权冲突**：`Deal, never choose by taste` 随机发牌、强制固定 hover 抬升 2px / 缩放 1.02–1.03 且禁止覆盖、按行业硬套圆角，剥夺方向阶段的视觉锚点、形状成系与唯一缓动性格决策权。
  4. **工程形态与钩子冲突**：强绑定 `starter.html` / `assets/` 脚手架与 Claude Code `Stop` Hook（自动跑 `gate.mjs` 拦截退出），与单文件 `.html` 交付冲突。**本地使用前必须禁用其 `Stop` Hook**。
- **镜像环境说明**：本镜像未收录其 `scripts/*.mjs` 与 `starter.html`，仅可查阅文案参考文档。
