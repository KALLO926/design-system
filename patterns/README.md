# patterns/ · 已验证 UI 模式库

定位：设计自学习（ui-pattern-learner 五步法，唯一方法）的沉淀物。只有通过沙盒临摹＋视觉找茬闭环（≤3 轮）的组件才准入库；未通过的草稿直接丢弃，不在此留存。

## 结构
- 每条模式一个文件：`<pattern_id>.md`，即《UI 模式学习完成报告》（按 REPORT-TEMPLATE.md 模板填写：基础档案／审美解剖／Tokens 与差值表／纠偏记录／视觉禁忌／纯净代码）。pattern_id = `<source>-<对象名>`，对象不限于组件——区块、页面模式、排版系统、交互/状态系统都算，如 `linear-command-menu`、`stripe-pricing-section`。
- 沉淀形式只有完成报告（2026-11-21 kk 定，YAML 不用了）。
- 临摹沙盒与截图属过程文件，不入库（放 goal-3 hidden_files/design-learning/ 下按日期存）。

## 检索与使用
- 做生产级工具类界面（SaaS、面板、命令菜单、表单、数据表）时，先查本库命中的模式，按其 tokens 与 state_deltas 落地。
- 本库管「真实产品怎么做的」；做法卡（component-recipes）管动效机制；判断仍以 design-contract.md 为正本，冲突时契约优先。
- 模式里的通用结论（克制点、层级手段）可按契约晋升规则提进 design-lessons.md；anti_patterns 里的禁止手法可入 design-anti-examples.md。

## 索引
（由每日自学习入库时追加：pattern_id — 来源 — 一句话适用场景）

- `shadcn-command` — shadcn/ui 命令面板 — 键盘优先的命令菜单：选中只换 3.5% 明度底色、hover 即选中、空态不降灰（2026-11-22，五步法首轮）
- `radix-accordion` — Radix UI 手风琴 — 无边框折叠：分隔线是 1px 阴影＋间隙透底色、明度三档分层、焦点环框住整行、展开动效只动高度与箭头（2026-11-24，第 1 轮盲审即通过）
- `stripe-press-article` — Stripe Press〈Tacit〉文章页排版系统 — 深色长文：单墨色 #cac8d1、三档字阶 4.2:2:1、流体 calc 标题、正文链接 hover 零变化、390px 下正文死守 20px（2026-11-24，3 轮盲审通过；星形网格与 Georgia 代位问题记已知限制）
- `base-ui-switch` — Base UI Switch 开关 — 方形直角开关：轨道 36×20px、圆角 0、阴影 none、开/关黑白互换、滑块行程 16px、hover/active/disabled 零差值、focus-visible 2px outline（2026-11-25，第 1 轮盲审即通过）
- `checklist-design-home` — Checklist Design 首页清单列表 — 全站无边框无阴影：三级明度阶梯＋文字透明度阶梯分层、五色面板等明度 tint、药丸 hover 白 70%→纯白＋scale 1.04、圆角三档 12/16/24（2026-11-25，2 轮盲审通过；专有字体与图标占位、精选卡随机抽取记已知限制）
- `ramp-pricing` — Ramp 定价区三档卡片 — 全 weight 400 字阶、卡片圆角 8px 阴影 none、Plus 整圈 3px 柠檬黄边框、CTA hover 只变背景明度、原版 H1 源码显式 `<br>` 断行（2026-11-25，3 轮盲审结构层通过；脱敏改写、logo 标记缺失、卡高 +102px 记已知限制）
