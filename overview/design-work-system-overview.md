# 贾维斯设计工作体系总览（2026-11-25 v2.3 优化版）

一份文档看全：现行技能、判断规范、四步纯净流水线与学习治理闭环。本版基于 2026-11-25 仓库源码级审计完成重构。

## 一、全景：四层

| 层 | 职责 | 核心物件 |
|---|---|---|
| 判断层 | 定义「什么算好、什么不许」 | `contract/design-contract.md`、`design-lessons.md`、`design-anti-examples.md`、`master/design-system-master_v2.3.md` |
| 执行层 | 实际设计交付链 | 四步纯净流水线 ＋ `skills/SKILLS-ROUTING.md` ＋ 各技能 `LOCAL-NOTE.md` |
| 学习层 | 持续积累已验证模式与资产 | `ui-pattern-learner`、模式库（6 条）、做法卡（78 张带风险/栈标签）、状态化素材池 |
| 治理层 | 防膨胀、防冲突、量化闭环 | 契约双上限、规则代码化下沉、每周五 4 指标复盘、回收站清退 |

## 二、判断层：分层加载（不再全量灌入）

| 文件 | 现行定位 | 常规任务加载时机 | 容量约束 |
|---|---|---|---|
| `contract/design-contract.md` | **唯一判断正本**（仅收审美与验收红线） | 每次任务＋每日自学习**必读** | ≤95 行且 ≤4,500 字（留 ≥20 行余量） |
| `contract/design-anti-examples.md` | 否决手法速查表（含聚合标本＋二阶默认） | 每次任务＋盲审标尺包**必读** | ≤80 行 |
| `contract/design-lessons.md` | 活跃教训缓冲区（已晋升项归档至 `archive/`） | 开工按需扫读，周五强制分流 | ≤80 行 |
| `master/design-system-master_v2.3.md` | 流程、分级、模板与工具手册 | L2 启动、改流程、周五复盘**按需查阅** | 不随 L0/L1a 全量载入 |

## 三、执行层：四步纯净流水线（替代原六步冲突链）

| 步 | 环节 | 技能 / 工具 | 说明 |
|---|---|---|---|
| 1 | 定向与原型 | `frontend-design` ＋ `gemini-frontend-design` 原型附录 | 定方向、骨架、唯一缓动；遵循 `LOCAL-NOTE.md` 禁用 `forest` 绿底 |
| 2 | 制作与减法自查 | 制作方（先查内库＋填开工声明＋7 格色板） | 全页一底到底；减法四查后进门 |
| 3 | 工艺双闸门（前置！） | `impeccable`（仅机检 Hook）＋ `unslop`（仅 `signs.md` + `tokens.md`） | **先拦低成本确定性与 Token 级 slop**，未过原地返修；镜像无脚本时用 `unslop` 清单＋实测替代 |
| 4 | 语义审核＋受控盲审 | `better-interface` ＋ 带 30 行标尺包盲审 | 吸收 `web-design-guidelines` 独有工程项；盲审隔离过程但对齐 kk 红线 |

**降级/退出常驻项**（详情见 `skills/SKILLS-ROUTING.md`）：
- `taste-skill`：因 `One section per screen`（PPT 冲突）、`sand|sky|sage` 底色发牌与 `Stop` Hook 冲突，移出常驻流水线，仅按需取用 `copy-lint` / `copy-voice`。
- `ui-ux-pro-max`：移出常驻，降为点名查 UX/图表/图标词典，禁用 `--design-system` 与 `MASTER.md`。
- `web-design-guidelines`：规则已由 `better-interface` 覆盖并消除 `WebFetch` / `Title Case` 冲突，降为备查。
- `local_self_hosted_fonts`：与契约单文件中文降级系统栈规则冲突，标记待清退。
- Emil 动效组（9 目录）：建议参照 `gsap` 合并为单个 `emil-motion` 技能（正文路由＋`references/`）。

## 四、任务流与分级（L0 / L1a / L1b / L2）

1. **分级**：L0 微调（免方向与盲审）；**L1a 体系内延展**（复用母版，免方向稿，单轮受控盲审）；**L1b 常规新设计**（2 个首屏＋核心区高保真切片）；L2 旗舰级（3 个实质分化切片＋全量资产盘点＋四门验收）。
2. **消费闭环**：L1a+ 开工前先查内库，在《开工声明》必填 `内库引用`，L1a+ 必附 7 格 OKLCH 色板功能表（强调色标资产采样部位）。
3. **验收**：减法自查 → 工艺双闸门前置 → `better-interface` 审核 → 受控盲审（熔断 ≤3 轮）→ 四态/三态实测 → 双通道交付（单文件 `.html` 换版本号防缓存 ＋ Space 版）。

## 五、学习层与治理闭环

- **模式库（`patterns/`）**：已正式入库 **6 条**（`shadcn-command`、`radix-accordion`、`stripe-press-article`、`base-ui-switch`、`checklist-design-home`、`ramp-pricing`）；Step 5 新增「第 6 节落盘代码回跑校验」防 CSS 选择器损坏，分层验收将结构层 P0 与资产层已知限制物理分表。
- **每日节奏**：1 个精学入库 ＋ 2 个轻采样或项目定向采风；素材池 `research/learning-sources.md` 改为状态追踪表，避开强登录墙与已失证资产陷阱。
- **做法卡（`research/component-recipes-2026-11-20.md`）**：全部 78 张补齐 `[HTML原生可用]` / `[React/Motion耦合]` 栈标签，17 张高危装饰卡标红 `[⚠️ 契约高危]`。
- **周五复盘**：固定追踪 4 指标——kk 一次通过率、盲审平均返工轮次、规则代码化率、内库消费率。
