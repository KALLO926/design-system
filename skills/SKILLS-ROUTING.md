# 技能路由与冲突覆写总表（SKILLS-ROUTING）

> **阅读原则**：调用任意第三方技能前，**先读本表及该技能同目录的 `LOCAL-NOTE.md`**。当上游 `SKILL.md` 与 `contract/design-contract.md` 冲突时，一律以契约与 `LOCAL-NOTE.md` 为准。

---

## 一、常驻四步纯净流水线（默认启用）

| 步 | 技能 | 角色 | 本地覆写摘要 | 无脚本镜像降级方案 |
|---|---|---|---|---|
| 1 | `frontend-design` | 审美方向与排版主入口 | 英文标签默认 Sentence case；方向须三项实质分化 | 纯文档即用 |
| 1（附） | `gemini-frontend-design` | 方向弹药（Hero 4 原型＋3 字号纪律） | **禁用 L13316 `forest` 深绿底色**；素材优先级与字体自托管按契约覆写 | 纯文档即用 |
| 3 | `impeccable` | **前置 P0 机检门**（仅调用 detector hook/binary） | 不启用其 20+ 命令全流程总监路由，避免与第 1/4 步抢权 | 镜像缺二进制时，由 `unslop` 工艺清单＋真实浏览器实测替代 |
| 3 | `unslop`（局部转正） | **前置 Token 级工艺审计**（仅用 `references/signs.md` + `tokens.md`） | 补齐盲审对离阶字号、`600` 字重、超时动效的盲区；禁用其 `paper and ink` / 暖米底色倾向 | 纯文档即用（完整收录） |
| 4 | `better-interface` | **唯一后段语义与工程审查入口**（含 6 内部模块） | 覆写 `interface-review` 悬空转交逻辑改为直接审查；吸收 `web-design-guidelines` 独有工程项（`Intl.*`、`overscroll-behavior`、hydration safety、`tabular-nums`） | 纯文档即用（35 文件完整收录） |

---

## 二、流水线外 · 点名触发或专项技能

| 技能 | 定位 | 关键覆写（详见各自 `LOCAL-NOTE.md`） |
|---|---|---|
| `ui-pattern-learner` | 每日自学习唯一方法（五步法） | 停用 YAML 仅出完成报告；Step 5 强制对第 6 节落盘代码回跑校验；分层验收严禁资产层标 P0 |
| `unslop`（完整 9-pass） | 去 AI 味专项改造 | 点名触发；不覆盖契约配色与单文件中文系统栈规则 |
| `impeccable`（全命令模式） | 后台/重构/多模式专项 | 仅在点名或 Operate/Read 类复杂重设计时按需调子命令 |
| `gsap` | 复杂滚动/时间轴动效实现 | 点名触发；须遵守契约“全站唯一缓动性格”与 `prefers-reduced-motion` |
| `emil-motion` | Emil 动效组，**已合并（9 合 1，点名参考）** | 场景路由见其 `SKILL.md`；原 9 个技能全文存 `references/`；须遵守契约“全站唯一缓动性格”与 `prefers-reduced-motion` |
| `yu-apple-design` | 宇苹果风格网页参考包 | 点名触发 |
| `claude-skill-web-clone` | 网站复刻专项 | 点名触发 |
| `local-anatomy` | 术语/机制交互式等距解释图 | 专项讲解任务触发 |

---

## 三、已移出默认流水线 / 降级 / 待清退

| 技能 | 现行状态 | 原因与处理 |
|---|---|---|
| `taste-skill` | **移出常驻（降级为文案参考）** | 与契约存在 4 处正面冲突（`One section per screen` 诱发 PPT、`sand\|sky\|sage` 踩颜色禁令、随机发牌剥夺方向权、`Stop` Hook 强拦）；禁用其 `Stop` Hook，仅按需查阅 `copy-lint`/`copy-voice` |
| `ui-ux-pro-max` | **移出常驻（降级为点名查词典）** | 两次配色品味翻车；禁用 `--design-system` 与 `MASTER.md` 生成，仅按需查 `--domain ux/chart/icons`；不采纳其“禁 0ms 切换”条 |
| `web-design-guidelines` | **已由 `better-interface` 吸收（备查）** | 与 `better-interface` 重合 >70%；原 `SKILL.md` 强拉 GitHub 远端且要求 `Title Case` 冲突；已并入后段审查 |
| `local_self_hosted_fonts` | **已清退** | 强令打包全量中文字体与契约“单文件 `.html` 中文降级系统栈”正面相反，且残留 `/opt/muse/` 废话；由契约＋机检替代（2026-11-25 已移入回收站） |
