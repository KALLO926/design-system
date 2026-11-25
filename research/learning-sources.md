# 学习素材池（ui-pattern-learner 状态化选材表）

定位：每日自学习的动态选材池。六类框架为开放清单，Agent 遇到真实上线、公开可达（或带公开 Demo / Storybook）的优秀对象可随时核实入池。
**维护纪律（2026-11-25 升级）**：选材前先看「状态」与「可复现性备注」，跳过已入库与强登录墙对象；每次学习结束或周五复盘同步回写本表，禁止盲目重复踩坑。

---

## 一、活跃可选与已学状态表

| 对象与网址 | 类别 | 当前状态 | 可复现性与学习备注 |
|---|---|---|---|
| Vercel (`vercel.com`) | ① 品牌官网与落地页 | 未学 | 公开可达，结构层易复现 |
| Linear (`linear.app`) | ① 品牌官网与落地页 | 未学 | 官网公开可达（内部工作台需登录，见暂缓区） |
| Stripe (`stripe.com`) | ① 品牌官网与落地页 | 未学 | 官网公开可达，WebGL 渐变属资产层 |
| Apple (`apple.com`) | ① 品牌官网与落地页 | 未学 | Open Design 已有系统包参考，实拍图属资产层 |
| teenage engineering (`teenage.engineering`) | ① 品牌官网与落地页 | 已做拆解（旧法） | 自有字体＋实拍为主，若走五步法须按分层验收 |
| Nothing (`nothing.tech`) | ① 品牌官网与落地页 | ⚠️ 资产高风险 | **2026-11-23 实证卡资产层（点阵专有字体）**；若再学仅验结构层 |
| Raycast (`raycast.com`) | ② 高密度生产力工具 | 未学 | 官网与公开交互演示可达 |
| Amie (`amie.so`) | ② 高密度生产力工具 | 未学 | 先核实是否存在公开 Demo，无公开态则移入暂缓区 |
| Stripe Docs (`docs.stripe.com`) | ③ 金融与数据面板 | 进行中（11-24 拆过 API 页） | 公开可达，暗色边框先像素取样 |
| Vercel Geist (`vercel.com/geist`) | ③ 金融与数据面板 | 未学 | 公开设计系统文档，极佳结构层对象 |
| Mercury (`mercury.com`) | ③ 金融与数据面板 | 未学 | 官网＋公开 Demo 面板可达 |
| Ramp (`ramp.com`) | ③ 金融与数据面板 | ✅ 已入库（`ramp-pricing`） | 2026-11-25 定价区三档卡片 3 轮通过；其他模块可另选 |
| Wise (`wise.com`) | ③ 金融与数据面板 | ⚠️ 资产高风险 | **2026-11-23 实证卡资产层（照片纹理主角）**；优先选其汇率计算器组件 |
| shadcn/ui (`ui.shadcn.com`) | ④ 无头与原子组件库 | ✅ 已入库（`shadcn-command`） | 2026-11-22 入库；其余原子组件可继续轮换 |
| Radix UI (`radix-ui.com`) | ④ 无头与原子组件库 | ✅ 已入库（`radix-accordion`） | 2026-11-24 第 1 轮通过；其余组件可继续轮换 |
| Base UI (`base-ui.com`) | ④ 无头与原子组件库 | ✅ 已入库（`base-ui-switch`） | 2026-11-25 第 1 轮通过；注意本地 monolith 档未水合需走 DOM 考古 |
| React Aria (`react-spectrum.adobe.com/react-aria`) | ④ 无头与原子组件库 | 未学 | 公开交互文档完整，A11y 与键盘状态机标杆 |
| Ark UI (`ark-ui.com`) | ④ 无头与原子组件库 | 未学 | 公开无头组件文档 |
| Headless UI (`headlessui.com`) | ④ 无头与原子组件库 | 未学 | 公开交互示例完整 |
| Stripe Press (`press.stripe.com`) | ⑤ 编辑部与内容网站 | ✅ 已入库（`stripe-press-article`） | 2026-11-24 3 轮通过；装饰星形纹理属资产层已知限制 |
| Checklist Design (`checklist.design`) | ⑤ 编辑部与内容网站 | ✅ 已入库（`checklist-design-home`） | 2026-11-25 2 轮通过；专有字体/图标记已知限制 |
| Are.na (`are.na`) | ⑤ 编辑部与内容网站 | 未学 | 公开频道可达，低噪内容网格 |
| Substack (`substack.com`) | ⑤ 编辑部与内容网站 | 未学 | 公开长文阅读页可达 |
| The Pudding (`pudding.cool`) | ⑤ 编辑部与内容网站 | ⚠️ 资产高风险 | **2026-11-23 实证卡资产层（照片抠图/手绘视觉）**；仅作排版或滚动结构参考 |
| 真实上线一线官网（汽车/家居/消费品等） | ⑥ 真实上线优秀网站 | 持续轮换 | 从本体现选导航、定价表、产品选择器、表单等真实区块 |

---

## 二、暂缓区（强登录墙 / 付费墙，按规则不进日常轮换）

> 依据入池规则：强登录且无公开 Demo 的产品不进每日自动轮换，留待 kk 提供已登录态或截图时再专项拆解。

- **Superhuman** (`superhuman.com`)：核心邮件工作台强登录。
- **Notion** (`notion.so`)：工作台内部需登录（公开文档页除外）。
- **Linear 内部工作台** (`linear.app` App 端)：需登录；官网营销页归上方 ① 类。

---

## 三、发现渠道（仅用于发现新对象，严禁拿索引站截图当学习本体）

- **Mobbin** (`mobbin.com`) — 仅作发现渠道（且存在登录/付费墙，不再列于生产力工具池）
- **Component Gallery** (`component.gallery`) — 无头/设计系统组件目录索引
- **Minimal Gallery** (`minimal.gallery`) — 极简品牌官网发现渠道
