---
name: ui-pattern-learner
description: 指导 Agent 从优秀真实 UI 网站中逆向提取 Design Tokens、交互状态差值与审美逻辑，经沙盒临摹验证后沉淀为高纯度设计模式库。
version: 1.1.0
requires_tools:
  - browser_automation (Playwright / Puppeteer / Browser MCP)
  - vision_analysis (多模态视觉理解)
  - local_sandbox (本地代码渲染与截图)
triggers:
  - 学习指定网站或组件库的 UI 设计风格
  - 从标杆产品中提取 Design Tokens 与微交互规范
  - 扩充或更新本地私有 UI 模式库
---

定位（2026-11-21 kk 定，2026-11-25 校准）：本方法是设计自学习的**唯一方法**。沉淀物**只有一种形式**——通过沙盒视觉验证的《UI 模式学习完成报告》（存 `patterns/<pattern_id>.md`，原 YAML 模式已于 2026-11-21 停用）。未通过结构层验证的草稿直接丢弃、不入模式库，但须记一句失败卡点。模式之上的判断仍以 `contract/design-contract.md` 为正本；通用结论按晋升规则入 `contract/design-lessons.md` / 契约。

# 学习维度与边界

核心原则：学底层秩序与工程工艺，不学易过时的视觉表皮（特定配色模板、流行圆角、装饰特效）。

- **学习维度**：L1 秩序与微弱层级（排版比例、4/8 步进、同心圆角、低透明边框、Surface Delta）；L2 边缘状态与微交互（空/加载/报错/溢出态、状态差值与缓动）；L3 信息密度与渐进式披露；L4 语境、立场与设计决策。
- **人机边界**：L1/L2 由 Agent 主学；L3 由 Agent 学＋减法审查纠偏，信息架构取舍归 kk；**L4 归 kk**（Agent 只记观察，不下品牌立场结论）。
- **素材六类轮换**：①品牌官网与落地页；②高密度生产力工具；③金融与数据面板；④无头与原子组件库；⑤编辑部与内容网站；⑥真实上线优秀网站。选材先查 `research/learning-sources.md` 状态表，跳过已入库、强登录墙与无公开态对象，学完顺手回写状态。
- **每日节奏（v2.3 优化建议口径）**：每日 **1 个深度对象**走完整五步法并产出高质量入库报告 ＋ **2 个轻采样**（仅做 Step 1–3 Token 差值与解剖卡记日志，或按本周项目缺口定向采风）；若需三对象全走五步法，仍维持非组件类 ≥2、组件类 ≤1。
- **选材可复现性预筛（2026-11-23 kk 拍板）**：先判视觉主角是否可复现（专有字体、照片纹理、手绘/抠图等）；不可复现时优先换同类对象，确有结构价值要学的按 Step 4 分层验收执行。
- **避坑三条**：不学概念飞机稿；不学「静态成功态」；学关系不背死色值。

# 选材红线

- **优先学习**：真实高密度生产级产品、无头组件库与一线上线官网本体。
- **严禁学习**：Dribbble/Behance 飞机稿、重阴影/高饱和渐变展示页。
- **核心原则**：绝不把原始 HTML/CSS 源码塞进知识库；只沉淀数学比例、状态差值、克制意图与验证后的纯净代码。

---

# 标准化学习流水线 (Learning Pipeline)

### Step 0: 本地化存档 (Localize First)
外站对象开工先用 `monolith` 落单文件本地档（需逐资产分析时用 `curl` 镜像，禁用 `wget`），后续采样、状态采集、临摹对照全对本地档做，断网可复现。本地自有页面跳过本步。

### Step 1: 计算样式与比例采样 (Computed Style Extraction)
- **整页/整站先行底稿**：起本地 HTTP 服务后跑 `npx -y designlang extract <localhost-URL> --wait 3000 --motion-runtime`（重页面禁用 `--interactions` 防超时）出初稿；单对象或需登录态时跳过底稿直接采样。`designlang` 的 `fidelity` 分与 `verify-ui` 不可靠，禁用作验收依据。
- **强制复核四项**：底稿中的**前景色归纳、字体归属、字阶主值、scrollLinked 判断**必须逐项用 `window.getComputedStyle()` 实测复核并标注实测值后，才准进入 Step 3；未复核底稿数据不许写入完成报告。
- **采样维度**：排版比例（size/line-height/weight/tracking）、微弱层级（低透明边框、Surface Delta、阴影或显式 `none`）、间距与同心圆角。
- **登录墙降级**：遇登录墙切至公开 Demo/Sandbox/Storybook；无公开态则跳过并在 `research/learning-sources.md` 标为 `跳过-登录墙`，严禁空转。

### Step 2: 交互状态差值采集 (Interactive State Delta)
触发 `hover`、`focus-visible`、`active`、`disabled` 及明暗切换，记录前后样式差值与 `transition-property`/`duration`/`cubic-bezier`。
- **降级路（DOM 状态考古）**：自动交互采不到时，改读本地档 DOM 数据属性、隐藏节点、类名语义与 JS 状态机，并用截图对照验证，不许跳过状态差值。

### Step 3: 双模态审美解剖 (Bimodal Anatomy)
将渲染截图与 Step 1–2 实测数据对照，输出《组件解剖卡》，必答三问：①克制点（刻意去掉了什么）；②层级手段（靠什么拉开主次）；③适用与禁用边界。

### Step 4: 沙盒临摹与分层视觉找茬 (Replication & Layered Critic Loop)
- **临摹分级**：日常＝单对象级；深临摹＝整页/整站级（每周至多 1 次，周五定题、周六占当日 1 个名额）。
- **纯净重构**：用干净语义代码独立重写，剥离原文案与业务 DOM。
- **并排盲审（≤3 轮，物理分表防口径漂移）**：与原站截图并排提交无上下文评审，结论必须拆为两张独立表：
  - **表 A · 结构层判定（入库门槛，仅此表允许标 P0）**：tokens、布局、间距、状态交互、边框/padding/字重/阴影四大失真；数值冲突以 `getComputedStyle` 实测为准，评审不得用估算数值驳回。
  - **表 B · 资产层已知限制（不阻断入库，禁止标 P0）**：照片纹理、专有字体字形、手绘/视频替代品的还原度差距，仅如实记入报告。

### Step 5: 结构化沉淀与落盘代码回跑校验 (Pattern Commit & Self-Check)
1. **落盘代码回跑校验（2026-11-25 新增防损坏门）**：将纯净代码写入 `patterns/<pattern_id>.md` 第 6 节后，**必须从最终落盘的 `.md` 文件中抽出第 6 节代码块重新检查/渲染一遍**，至少验证：
   - 无 CSS 语法拼接损坏（如丢失空格导致 `.switch.thumb`、`transform.15s`、`background-color.15s` 等失效规则）；
   - 所有自定义样式选择器在自身 HTML 中至少命中 1 个节点（状态类除外）。
2. **沉淀与失败归档**：按 `patterns/REPORT-TEMPLATE.md` 落盘，并更新 `patterns/README.md` 索引与 `research/learning-sources.md` 状态表；未通过结构层验证的草稿丢弃不入库，但须在当日日志与短消息记一句**失败卡点**。
