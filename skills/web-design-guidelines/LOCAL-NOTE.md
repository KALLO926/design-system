# 本地定位说明（2026-11-25：已由 better-interface 吸收，降为备查）

- **现行定位**：常规流水线不再单独跑本技能，其 70%+ 规则已由 `better-interface`（35 文件完整子模块）更深入覆盖，剩余独有工程核查项已并入 `skills/better-interface/LOCAL-NOTE.md`。本目录仅作速查参考保留。
- **若被点名单独调用，必须遵守以下 2 条本地覆写**：
  1. **禁止 `WebFetch` 远端拉取**：上游 `SKILL.md` 要求每次从 `raw.githubusercontent.com` 拉取最新规则——本机与国内网络下**一律强制直接读取同目录本地副本 `command.md`**。
  2. **覆写英文大小写规则**：`command.md` 要求 `Title Case for headings/buttons (Chicago style)`，与 `contract/design-contract.md`、`frontend-design`、`unslop`、`gemini-frontend-design` 的 **`Sentence case`** 冲突——一律以 **Sentence case** 为准。
