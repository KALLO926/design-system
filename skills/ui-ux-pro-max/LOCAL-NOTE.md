# 本地定位说明（2026-11-25 审计降级：移出常驻流水线）

- **现行定位**：**已移出常驻默认流水线**，降级为点名参考词典（仅在需要查特定行业的 UX 交互模式、图表选型或图标规范时按需调用 `--domain ux / chart / icons`）。上游 `SKILL.md` 原样保留不改。
- **强制本地覆写（违者即违规）**：
  1. **禁用 `--design-system` 与 `--persist MASTER.md`**：教训录已两次实锤其配色建议翻车（黑金奢华、食品类给品红 `#D946EF` 与沙底）；`design-contract.md` 是唯一判断正本，禁止本技能另立 `MASTER.md` 作为真相源。
  2. **无配色、字体、审美定向决定权**：若其返回结果与契约或方向稿冲突，一律直接否决。
  3. **覆写 `Instant state changes (0ms)` 反模式条**：本技能 Priority 2 将 0ms 瞬时切换列为反模式，但模式库 `patterns/shadcn-command.md` 已实证高密度键盘列表的 0s 瞬时切换是刻意克制；该条以场景与契约为准，不机械套用。
- **镜像环境说明**：本镜像未收录 `scripts/search.py` 与 CSV 数据表，外部 Agent 仅可按需阅读 `references/quick-reference.md` 与 `references/pro-rules.md`。
