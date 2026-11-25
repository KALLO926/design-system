# 本地定位说明（2026-11-25 升级：后段语义与工程审查唯一入口）

- **现行定位**：四步纯净流水线**第 4 步唯一语义与工程审查入口**（内含 `better-accessibility` / `better-layout` / `better-writing` / `better-typography` / `better-colors` / `better-ui` 六个完整子模块），在通过第 3 步工艺双闸门（`impeccable` 机检＋`unslop` Token 审计）之后执行。上游 `SKILL.md` 保持原样不改。
- **本地覆写与增补**：
  1. **覆写第 2 节 `interface-review` 转交逻辑**：上游 `SKILL.md` 指示遇到未提交改动/分支/PR 时拒绝审查并让用户调用 `interface-review`。本工作体系未配置 `interface-review`——**一律直接按本技能 6 大领域模块完成本地变更审查**，不因找不到 `interface-review` 而中止。
  2. **吸收原 `web-design-guidelines` 独有工程核查项**（避免重复跑两轮审查）：
     - 日期/时间/货币使用 `Intl.DateTimeFormat` / `Intl.NumberFormat`，数字列启用 `font-variant-numeric: tabular-nums`；
     - 弹窗/抽屉加 `overscroll-behavior: contain`，触控交互加 `touch-action: manipulation`；
     - 品牌名/代码标识按需加 `translate="no"`；表单不阻断 `onPaste`，框架项目检查 Hydration 一致性。
  3. **仲裁口径**：本技能 13 条 Escalation Triggers 与工程/无障碍硬底线为 **P1（必改或留痕豁免）**；主观审美微调标 **CANDIDATE**，只采纳高价值项，绝不与 `contract/design-contract.md` 硬禁区冲突。
