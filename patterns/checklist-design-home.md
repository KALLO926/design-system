# UI 模式学习完成报告

## 1. 基础档案 (Meta)
* **Pattern ID**：`checklist-design-home`
* **来源网址**：https://checklist.design/
* **所属类型**：编辑部与内容网站（内容模式：清单目录型首页）
* **风格标签**：`#暖白底` `#明度阶梯` `#等明度分类色` `#无边框无阴影` `#目录索引`
* **验证状态**：✅ 已通过沙盒渲染与视觉找茬闭环（共迭代 2 轮）
* **沉淀路径**：`patterns/checklist-design-home.md`

**对象一句话**：首页不是一张清单，而是一个清单的目录——5 个分类色块面板（置顶清单药丸）+ 搜索区 + 12 张随机精选卡片 + AI 推广卡 + 订阅区 + 全量目录页脚（138 条清单、5 个分组）。首页本身没有勾选 / 进度 UI（无 checkbox、无进度条），勾选态在清单详情页，不在本对象范围内。

**采样方式**：2026-11-25，本机 Playwright + miniproxy，对 live 页 `getComputedStyle()` 实测；动效参数另从 JS 源码（HomePage / PageEnter 分片）交叉核对。monolith 单文件档不完整（见第 7 节），实测以 live 页为准。

---

## 2. 审美解剖与克制逻辑 (Anatomy)
* **核心克制点（刻意去掉了什么）**：
* 去掉了所有边框与阴影：卡片、药丸、面板、输入框没有任何 `border`（仅 header 一条 1px black/6 底线、订阅输入 1px black/10、页脚 CTA 1px black/7），没有任何卡片阴影；唯一的阴影在搜索浮层（输入条、结果面板）与下拉菜单三处。层级只用明度差 + 文字透明度扛。
* 去掉了清单本体：首页不展示任何一条清单内容、不做勾选交互，只做「目录」——分类、名字、搜索；把「做清单」推迟到详情页。
* 去掉了装饰：无插画、无渐变、无大图（AI 卡的两张产品截图是唯一的图）；分类面板不配图标，图标只出现在药丸内部 20px。
* 去掉了默认推荐：搜索浮层打开是空的，不推热门、不推历史；空态只有一行字 `Nothing found for "xxx"`。
* 去掉了数字：药丸上没有条目数、没有进度百分比；全页唯一的数字是 "Browse all 138 checklists"。
* 去掉了 hover 的一半：精选卡片与 Notify 按钮 hover 零反馈——不是漏做，是把「可点」的信号只留给药丸、搜索、刷新三类元素。
* **主次层级手段**：三级明度阶梯是唯一的层级语言——底面 #f4f3ee → 色块面板（−7~−12 pts）→ 白药丸 / 白卡（+7~+12 pts）；五档分类色只做「认路」不做「强调」（五色彼此几乎等明度，颜色用来区分分类，不拉主次）；主次由字号与字重拉（60px h1 → 32px h3 → 20px 卡名 → 16px 正文 → 12px 标签）；大标题统一 −3% 字距、正文统一 −1%；字重只有 400 / 500 / 600 三档，600 只给标题；大面积留白 + 固定档位间距，无分割线。
* **适用与禁用边界**：
* **适用**：内容目录、索引页、工具箱 / 资源库首页——条目多（138 条）、分类清晰（5 类）、用户目标是「找到某一条」而非「浏览某一条」；低密度、浅色、弱对比的气质；适合需要「一眼扫完分类、再搜索定位」的场景。
* **禁用**：需要一眼比较条目内容的场景（药丸上除名字什么都没有，没有描述、没有数量、没有状态）；需要视觉刺激或品牌个性的页面（五色等明度、零装饰，品牌感几乎为零）；深色主题（整套明度阶梯建立在 #f4f3ee 暖白底上，反色后 tint 逻辑要重做）；条目少于约 20 条的站（5 个色块面板 + 12 张随机卡 + 5 列页脚目录的排场会显得空）。
* **照搬风险**：white/70 药丸在面板上的 +12 pts 对比只在浅色 tint 上成立，换深色面板会直接失效；精选卡片随机抽取依赖足够大的内容池（138 条），池子小会频繁重复。

**品牌语境观察（L4，只记录不下结论）**：作者 George Hatzis；站内到处是「去 Figma 用」的出口（Figma Plugin / Figma Skill / Agent skill 三个 pill 在 hero、页脚、AI 卡 CTA 反复出现）；页脚赞助商 Mobbin、Rectangles。文案口吻是工具型直白，无故事、无宣言。视觉上与 Linear / Vercel 系的「暖白底 + 低透明度黑 + 单一强调色」同源，但这里连强调色都省了——唯一的饱和色是 Notify 按钮的紫 #684d95。

---

## 3. 核心 Design Tokens 与交互差值 (Tokens & Deltas)

| 维度 | 提取规范 / 精确参数 | 关键备注 |
|:--- |:--- |:--- |
| **色彩与层级** | 底面 `#f4f3ee`（相对明度 0.8951）；五色面板等明度 tint：Mobile 奶油 rgb(240,234,216) −7.18pts、Web app 浅蓝 rgb(220,230,240) −11.41pts、Website 淡紫 rgb(229,227,232) −12.08pts、Design system 蜜桃 rgb(240,227,219) −10.93pts、Flows 薄荷 rgb(227,232,228) −9.86pts；药丸白 white/70（面板上 +12.13pts、底面上 +7.26pts）；AI 卡纯白 +10.49pts；订阅区 black/4（−7.91pts）；View all 药丸 dark/10（最暗的灰，−18.69pts）；文字 dark rgb(38,33,28) → /70 → /60 → /50 → /40 五档透明度阶梯 | 全站无边框、无阴影（仅搜索浮层输入条 0 2px 16px rgba(0,0,0,0.10)、结果面板 0 8px 32px rgba(0,0,0,0.12)、下拉菜单 0 8px 40px rgba(0,0,0,0.13) 三处有阴影）；层级只靠三级明度阶梯 + 文字透明度阶梯 |
| **排版比例** | h1 60/66/600/−3%（窄屏 48/52.8）；面板 h3 32/40/600/−3%；搜索 h2 36/39.6/600/−3%；AI 卡 h2 48/50.4/600/−3%；订阅 h3 30/36/600/−3%；精选卡名 20/27.5/600/−1%；药丸 / 正文 / 页脚链接 16/24/400/−1%；标签药丸与工具药丸 12/500/−1%；AI 眉题 14/500/dark/40；View all 与 Browse all 14；订阅输入与 Notify 按钮 18；字体 TT Hoves Pro（专有），栈 `"TT Hoves", Arial, sans-serif` | 大标题统一 −3%，正文统一 −1%，是全站最稳定的两个值；字重只有 400 / 500 / 600 三档 |
| **间距与圆角** | 圆角三档 12（View all、搜索按钮、刷新按钮、订阅输入、Notify 按钮、浮层行）／16（药丸、精选卡片、订阅区、AI 卡 CTA）／24（分类面板、AI 卡、下拉菜单）；标签药丸唯一满圆角 24px；面板 radius 24 + padding 40 + 内部 gap 24；面板区间 gap 16；药丸容器 flex-wrap gap 8；精选网格 6 列 gap 8；搜索行 max-width 460px；药丸 padding 12px 16px、图标 20px、gap 12；精选卡 padding 24、内部 gap 24、图标 24px；搜索浮层 fixed top 56px 居中、max-width 500px | 面板 radius 24 + padding 40 + 药丸 radius 16，**不是同心圆角**（24 ≠ 16 + 40）：面板圆角与药丸圆角各自取档位值，不按公式联动 |
| **Hover 差值** | 清单药丸：白 70% → 纯白，外层 motion.div scale 1.00 → **1.04**，tap 0.98，transition-colors 150ms cubic-bezier(0.4,0,0.2,1)；View all：dark/10 → dark/15，文字 dark/70 → 纯 dark；搜索按钮：black/6 → black/8，scale **1.01**，tap 0.99；刷新按钮：black/6 → black/10，图标 dark/40 → dark/70，scale **1.05**，tap 0.95；Browse 按钮：文字 dark/60 → 纯 dark，chevron 不旋转；Browse all 链接：dark/50 → 纯 dark，无下划线；**精选卡片与 Notify 按钮 hover 零反馈**（卡片、卡名、标签、图标四项两次实测零差值） | 临摹中药丸缩放直接作用于药丸本身，原站缩放作用于外层 motion.div（视觉等价） |
| **Focus/边缘态** | 搜索浮层行：hover 背景 transparent → black/6，键盘 ↓ 选中行 black/**8**（比 hover 深一档），行 16px/400、padding 8px 16px、radius 12、mx-2，transition-colors **100ms**；下拉菜单项 hover：分类 tint 层 opacity 0 → 1、gap 8 → 0（150ms），菜单本身白底 radius 24 + 阴影 + p-6，hover 与键盘 focus 均可打开；搜索浮层输入 focus 无 ring、无阴影；订阅 email 输入 focus：2px #684d95 40% ring；header Search 按钮 focus-visible：UA 默认 outline（1px auto），无自定义样式 | 边缘态：加载骨架——药丸骨架 `h-58px w-32 rounded-2xl bg-white/40 animate-pulse`，精选卡骨架 `rounded-2xl bg-black/[0.05] animate-pulse min-height 156px`，列宽按 [3,2,1] 循环，数据未到时显示 "Browse all **0** checklists"；搜索空态一行文案 `Nothing found for "xxx"`（14px dark/40，padding 16px 20px）；搜索浮层打开时只有输入条（无默认推荐），Esc 关闭，遮罩 `fixed inset-0 bg-black/25 backdrop-blur-[2px]`；超长文本：窄屏药丸自然换行、面板 p-10 与 h3 32px 不收缩，390px 无横向溢出 |
| **入场动效** | framer-motion：`initial {opacity:0, y:16} → whileInView {opacity:1, y:0}`，viewport once，amount 0.8（搜索区）/ 0.4（AI 卡）；duration 0.55s / 0.6s，ease `[0.16,1,0.3,1]`；AI 文字列 x −24→0，图片 x +30→0（delay 0.1 / 0.22）；Hero variants staggerChildren 0.1、delayChildren 0.15 | 这些元素 CSS `transition-duration` 实测为 0s，动效由 framer-motion 的 JS 驱动，不走 CSS transition |
| **几何（1440px）** | 整页 4884px；header 57；h1 top 145 高 132；副文案 top 293；工具行 top 367 高 32；面板区 top 439 高 680；搜索 h2 top 1239；精选网格 top 1370 高 509（卡片 154 高）；Browse all top 1907；AI 卡 top 2039 高 497；订阅 top 2599 高 309；页脚 top 3036 高 1848。区段间距：面板→搜索 120、网格→Browse all 28、Browse all→AI 114、AI→订阅 64、订阅→页脚 128 | 精选卡内部是图标 + 文本块（名 + 标签 gap 6），不是三个直接子项 gap 24；AI 文字列是眉题 + h2 + 两段文案包一层（内 gap 20）再与 CTA 间 gap 40；页脚目录链接 pitch 38px（li 高 34 + mb 4） |
| **随机性** | 12 张精选卡每次加载随机抽取（两次加载集合不同）；刷新按钮重新抽取 | 不是固定精选；临摹取其中一次快照（见第 7 节） |

---

## 4. 沙盒临摹与视觉纠偏记录 (Critic Loop)

临摹 v2（68,027 字节，由 `build_replica.py` 生成），live vs 临摹整页高 4884 → 4890px（+6，0.12%），逐段 top 对齐到 ±1px：h1 145、副文案 293、工具行 367、面板区 439（高 680）、搜索 h2 1239、搜索行 1301（live 1302）、精选网格 1369（live 1370，高 509）、精选卡 154 高、Browse all 1906（live 1907）、AI 卡 2040（live 2039，高 497）、订阅 2600（live 2599，高 309）、页脚 3037（live 3036）。残差：页脚目录 +4px（38 条长列的亚像素舍入）、Browse all 矩形高 20 vs 18（计算行高两边同为 20px）、页脚总高 +5px。结构层已过线。

* **Round 1 发现的失真问题**（v1 盲审意见 8 条）：
1. **推广区右侧空壳视觉（P0）**：初稿是大白空卡＋顶部橙色粗线＋灰色骨架线＋小浮卡，占位图感 → **已修正为** Round 2 按原版两张截图用 CSS / 文字画实（见下）。
2. **浮卡描边＋投影发硬**：初稿浮卡 box-shadow 为原版 JS 里的 0 4px 12px rgba(0,0,0,.2)，另有行分隔线与圆点描边 → **已修正为** 全部去掉描边（border 0、无分隔线），只留极淡投影 0 12px 28px rgba(0,0,0,.08)；注：此投影比原版 JS 自带值更淡，是按盲审意见改的。
3. **十二宫格卡片描边**：核对后未改——临摹 `.feat-card` 本来就没有 border 属性；像素实测卡片内底色原版 (252,251,250) vs 临摹 (252,252,250)、卡外底面同为 (244,243,238)，边缘对比与原版一致；所谓「描边」只是白 70% 卡片在暖白底上的底色差，已是「只留底色差」的状态，无可降的边框透明度。
4. **分类卡内 chip 图标与文字间距偏紧**：核对后未改数值（见第 4 节末实测裁决①）。
5. **十二宫格卡片图标到标题偏紧、上沿留白略少**：核对后未改数值（见第 4 节末实测裁决②）。
6. **顶部导航 Search 字重偏粗**：v2 曾按评审意见把 `.header-search` 从 500 改为 400（与 Figma Plugin / Figma Skill 的常规字重一致）→ **已修正为** 复核实测原版确为 500 后改回 500（见实测裁决③）。
7. **Hero 副文案行高略紧、断行剩孤词**：行高未改（实测原版与临摹同为 20.8px）；孤词根因是改写文案断行，v1 文案断为「…browse or」/「search below.」（第 2 行仅 2 词约 90px）→ **已修正为** 文案改为「Covering sites, apps, mobile, components and flows — browse the lists or search below.」，实测断为「…browse」（425px）/「the lists or search below.」（169px），与原版第 2 行宽度（168px）基本一致，无孤词。
8. **十二宫格条目与原版不符**：v1 用了一次随机加载抽到的 12 条 → **已修正为** 恢复为 orig-full.png（盲审对照图）里的 12 条，跨度模式不变 [2,1,2,1 / 1,2,1,2 / 2,1,2,1]：Action Sheet / Mobile（span 2）、Login / Mobile（1）、Notifications / Web App（2）、Feed / Web App（1）、Pricing / Website（1）、Onboarding Checklist / Mobile（2）、Alert / Design System（1）、Timeline / Gantt View / Web App（2）、Uploading media / Flows（2）、Billing / Web App（1）、Canceling subscription / Flows（2）、Badge / Design System（1）；标签字面改回原版大小写（Web App）；图标仍用近似通用图标替代（已知限制）。
* **Round 2 的改动**：
* 推广区按原版用 CSS / 文字画实两张截图：后层账单截图（bill-shot：Frame 432098 标签＋橙色顶栏 Clarent Energy / Usage / Billing / Outages / Support / Account #CL-4829-391＋CURRENT BILL $127.34、Billing period Feb 1 – Feb 28, 2026、Due by Mar 15, 2026＋BILL BREAKDOWN 明细＋PAYMENT METHOD VISA •••• 6291＋PREVIOUS BILLS 四行），前层插件面板（plugin-panel：深色头栏 Checklist Design＋✕＋Quality Check / Checklists 标签＋1 frame selected / Restart＋What's working 与 Considerations 两张卡片，全文照原版 plugin-panel.png 内容）。几何依据：bill-shot width 100% / right −32% / top 5%（页内 left 860、top 2065，实测原版 860/2064）；橙条 #d97706 高 29px；plugin-panel width clamp(220px, 38%, 310px) / left 5% / top 13%（页内 left 681、top 2104，与原版一致）。
* 逐卡 tops 对齐 ±2px：账单卡 1 (2148, 102) → (2149, 102)；卡 2 (2263, 148) → (2261, 152)；卡 3 (2424, 57) → (2423, 59)；卡 4 top 2493 → 2492；插件面板卡 1 (2191, 141) → (2193, 142)、卡 2 (2340, 128) → (2345, 131)；面板总高 394 → 400（底边 2498 → 2504，+6px，在卡片 overflow 内，不影响其余区段）。
* 右缘裁切与原版一致（实测原版 PNG 证实）：Pay $127.34 按钮、账单金额、Change method、View PDF 在原版里本就超出 AI 卡右缘（x>1312）不可见，v2 同样裁掉；ai-visual 背景由 #f7f5f0 改为 #fff（原版视觉区是白色，像素实测 255,255,255）；bill-shot 与 plugin-panel 均无边框、无阴影冒充层次。
* 宫格条目恢复原版 12 条（见 Round 1 第 8 条）。
* **三条实测裁决（评审估算与实测冲突时以实测为准）**：
1. chip padding / gap 实测与原版完全一致（padding 12px 16px、gap 12px；药丸矩形 live 119×48 vs 临摹 117×48，图标左缘与文字左缘均在 x=216，分毫不差，2px 宽差是字体渲染亚像素级差异），未按评审「＋2px」改——加了会偏离实测值。
2. 宫格卡片 padding / 图标间距实测一致（padding 24px、图标→文本块 gap 24px、名→标签 gap 6px；图标 top live 1426 vs 临摹 1425、卡名 top 1474 vs 1473、标签 top 1508 vs 1507，整页 −1px 偏移、内部相对位置分毫不差；卡片高同为 154px），未按「＋4px」改——加了会偏离实测值并顶高网格、打乱其余区段 ±1px 对齐；卡片本无边框。
3. 导航 Search 字重：v2 曾按评审意见 500 → 400，复核实测原版确为 500（sampled.json headerSearchBtn）后改回 500；观感偏粗归因于占位字体渲染差异，记为已知限制，不再改数值。
* **最终视觉复核结论**：第 2 轮盲审结论——整页几何对齐到 ±1px、推广区两张截图已画实、四卡 tops 对齐 ±2px、浮卡只留极淡投影、宫格条目恢复原版；明度阶梯、字阶、圆角三档与原版的克制气质一致，结构层已过线。

---

## 5. 提取的视觉禁忌 (Anti-Patterns)
* ❌ 严禁给药丸 / 卡片 / 面板加边框或阴影来「分层」——原站全站无边框无阴影，层级只靠底面→面板（−7~−12pts）→白药丸/白卡（+7~+12pts）的明度阶梯；唯一的阴影只允许出现在搜索浮层与下拉菜单。
* ❌ 严禁把分类色加深来「强调」——五色是等明度的认路色（彼此明度差仅 7~12pts），主次只能由字号与字重拉，加深分类色会直接破坏等明度体系。
* ❌ 严禁在精选卡片或 Notify 按钮上加 hover 动效来「补交互」——原站两次实测均为零反馈，可点信号只留给药丸、搜索、刷新三类元素；补 hover 等于改写原站的交互分工。

---

## 6. 已验证的纯净代码 (Verified Code)

以下为 replica.html v2 的核心代码（已验证版本，经两轮盲审；全文 68,027 字节，完整版见沙盒目录 `patterns-sandbox/2026-11-25-checklist-design-home/replica.html`，由 `build_replica.py` 生成；图标为通用占位 glyph，品牌名与营销文案为脱敏改写）。

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Checklist Studio — Check every detail, ship better work</title>
<style>
:root {
--body: #f4f3ee; /* 底面 */
--dark: #26211c; /* 暖黑，非纯黑 */
--dark-70: rgba(38,33,28,.7);
--dark-60: rgba(38,33,28,.6);
--dark-50: rgba(38,33,28,.5);
--dark-40: rgba(38,33,28,.4);
--black-06: rgba(0,0,0,.06);
--black-04: rgba(0,0,0,.04);
--purple: #684d95; /* 全站唯一饱和色 */
--ease: cubic-bezier(.4,0,.2,1);
}
* { box-sizing: border-box; margin: 0; padding: 0;}
body { background: var(--body); color: var(--dark);
font-family: "TT Hoves", Arial, sans-serif;
font-size: 16px; line-height: 24px; letter-spacing: -.01em;
-webkit-font-smoothing: antialiased;}
.container { max-width: 1216px; margin: 0 auto; padding: 0 16px;}

/* header：sticky，高 57px，只靠 1px black/6 底线分隔 */
header { position: sticky; top: 0; z-index: 40;
background: var(--body); border-bottom: 1px solid var(--black-06);}
.header-in { max-width: 1216px; margin: 0 auto; padding: 0 16px;
height: 56px; display: flex; align-items: center; justify-content: space-between;}
.header-search { display: inline-flex; align-items: center; gap: 6px;
font-size: 16px; font-weight: 500; color: var(--dark-70);
background: none; border: 0; cursor: pointer; font-family: inherit; padding: 6px;}

/* hero：h1 60/66/600/−3%，副文案 16/20.8/dark/50 */
.hero { text-align: center;}
.hero h1 { font-size: 60px; line-height: 66px; font-weight: 600; letter-spacing: -.03em;}
.hero.sub { max-width: 448px; margin: 0 auto; font-size: 16px;
line-height: 20.8px; color: var(--dark-50);}

/* 分类面板：radius 24 + padding 40 + gap 24；圆角三档 12/16/24，非同心圆角 */
.panels { margin-top: 40px; display: flex; flex-direction: column; gap: 16px;}
.panel-row { display: flex; gap: 16px;}
.panel { flex: 1; min-width: 0; display: flex; flex-direction: column;
gap: 24px; padding: 40px; border-radius: 24px;}
/* 五色等明度 tint：奶油 #f0ead8 / 浅蓝 #dce6f0 / 淡紫 #e5e3e8 / 蜜桃 #f0e3db / 薄荷 #e3e8e4 */
.panel-head { display: flex; align-items: baseline; justify-content: space-between;}
.panel h3 { font-size: 32px; line-height: 40px; font-weight: 600; letter-spacing: -.03em;}
.view-all { font-size: 14px; font-weight: 500; color: var(--dark-70);
background: rgba(38,33,28,.1); padding: 4px 10px; border-radius: 12px;}
.view-all:hover { background: rgba(38,33,28,.15); color: var(--dark);}

/* 清单药丸：白 70%、radius 16、padding 12/16、图标 20、gap 12；hover 纯白 + scale 1.04 */
.pills { display: flex; flex-wrap: wrap; gap: 8px;}
.pill-motion { transition: transform.15s var(--ease);}
.pill-motion:hover { transform: scale(1.04);}
.pill-motion:active { transform: scale(.98);}
.pill { display: inline-flex; align-items: center; gap: 12px;
padding: 12px 16px; border-radius: 16px;
background: rgba(255,255,255,.7); color: var(--dark);
font-size: 16px; line-height: 24px;
transition: background-color.15s var(--ease);}
.pill:hover { background: #fff;}

/* 搜索区：h2 36/39.6/600/−3%；搜索行 max-width 460px */
.search-sec { padding-top: 96px; text-align: center;}
.search-sec h2 { font-size: 36px; line-height: 39.6px; font-weight: 600; letter-spacing: -.03em;}
.search-row { display: flex; gap: 12px; justify-content: center; margin-top: 24px;}
.search-trigger { width: 460px; max-width: 100%; display: flex; align-items: center; gap: 12px;
padding: 12px 16px; border: 0; border-radius: 12px; background: rgba(0,0,0,.06);
font-size: 16px; color: var(--dark-70); cursor: pointer;
transition: background-color.15s var(--ease);}
.search-trigger:hover { background: rgba(0,0,0,.08);}
.refresh { width: 48px; height: 48px; border: 0; border-radius: 12px;
background: rgba(0,0,0,.06); color: var(--dark-40); cursor: pointer;
transition: background-color.15s var(--ease), transform.15s var(--ease);}
.refresh:hover { background: rgba(0,0,0,.1); color: var(--dark-70); transform: scale(1.05);}
.refresh:active { transform: scale(.95);}

/* 精选网格：6 列 gap 8；卡片白 70%、radius 16、padding 24、gap 24；hover 零反馈 */
.feat-grid { margin-top: 8px; padding-top: 32px; display: grid;
grid-template-columns: repeat(6, 1fr); gap: 8px; text-align: left;}
.feat-card { background: rgba(255,255,255,.7); border-radius: 16px; padding: 24px;
display: flex; flex-direction: column; gap: 24px; align-items: flex-start;}
.feat-text { display: flex; flex-direction: column; gap: 6px; align-items: flex-start;}
.feat-name { font-size: 20px; line-height: 27.5px; font-weight: 600; letter-spacing: -.01em;}
.feat-tag { font-size: 12px; line-height: 16px; font-weight: 500;
letter-spacing: -.01em; color: rgba(38,33,28,.8); padding: 4px 10px; border-radius: 24px;}

/* AI 卡：纯白、radius 24；文字列 h2 48/50.4/600/−3% */
.ai-card { margin-top: 114px; background: #fff; border-radius: 24px; overflow: hidden;
display: grid; grid-template-columns: minmax(0, 520px) 1fr;}
.ai-text { padding: 64px; display: flex; flex-direction: column; gap: 40px; justify-content: center;}
.ai-copy { display: flex; flex-direction: column; gap: 20px;}
.ai-text h2 { font-size: 48px; line-height: 50.4px; font-weight: 600; letter-spacing: -.03em;}
.ai-text p { font-size: 18px; line-height: 23.4px; color: var(--dark-60);}
.ai-visual { position: relative; min-height: 480px; background: #fff; overflow: hidden;}
/* 推广区两张截图用 CSS/文字画实，无图片、无边框；
bill-shot: width 100% / right −32% / top 5%，橙条 #d97706 高 29px；
plugin-panel: clamp(220px, 38%, 310px) / left 5% / top 13%，
只留极淡投影 0 12px 28px rgba(0,0,0,.08) */
.bill-shot { position: absolute; width: 100%; right: -32%; top: 5%; border-top: 1px solid #3f8ae2;}
.plugin-panel { position: absolute; width: clamp(220px, 38%, 310px); left: 5%; top: 13%;
background: #f0eee6; border-radius: 10px; box-shadow: 0 12px 28px rgba(0,0,0,.08); overflow: hidden;}

/* 订阅区：black/4 底、radius 16；输入 focus 用 2px #684d95 40% ring */
.news { margin: 64px 0; background: var(--black-04); border-radius: 16px;
padding: 80px 24px; text-align: center;}
.news h3 { font-size: 30px; line-height: 36px; font-weight: 600; letter-spacing: -.03em;}
.news input { flex: 1; min-width: 0; padding: 8px 20px; font-size: 18px; line-height: 28px;
font-family: inherit; letter-spacing: -.01em; background: #fff;
border: 1px solid rgba(0,0,0,.1); border-radius: 12px; color: var(--dark);}
.news input:focus { outline: none;
box-shadow: 0 0 0 2px color-mix(in oklab, #684d95 40%, transparent);}
.news button { padding: 16px 24px; font-size: 18px; line-height: 28px; font-weight: 500;
font-family: inherit; letter-spacing: -.01em; background: var(--purple);
color: #fff; border: 0; border-radius: 12px; cursor: pointer;}

/* 搜索浮层：fixed top 56px 居中、max-width 500px；行 hover black/6，键盘选中行 black/8 */
.overlay-wrap { position: fixed; top: 56px; left: 50%; transform: translateX(-50%);
z-index: 70; width: 100%; max-width: 500px; padding: 0 16px; display: none;}
.overlay-bar { background: #fff; border-radius: 12px;
box-shadow: 0 2px 16px rgba(0,0,0,.10); overflow: hidden;
display: flex; align-items: center; gap: 12px; padding: 16px 20px;}
.overlay-panel { margin-top: 8px; background: #fff; border-radius: 12px;
box-shadow: 0 8px 32px rgba(0,0,0,.12); overflow: hidden; display: none; padding: 8px 0;}
.overlay-row { display: flex; align-items: center; gap: 12px; padding: 8px 16px;
margin: 0 8px; border-radius: 12px; font-size: 16px;
transition: background-color.1s var(--ease);}
.overlay-row:hover { background: rgba(0,0,0,.06);}
.overlay-row.active { background: rgba(0,0,0,.08);}
</style>
</head>
<body>
<header><!-- logo + nav（Home / Browse 下拉 / Figma Plugin / Figma Skill）+ Search 按钮 --></header>
<main>
<section class="hero">
<h1>Sweat every detail,<br>ship better work.</h1>
<p class="sub">Covering sites, apps, mobile, components and flows — browse the lists or search below.</p>
<!-- 工具药丸：Agent skill / Figma skill -->
</section>
<section class="panels" aria-label="Checklist categories">
<!-- 5 个分类面板：Mobile app / Web app / Website / Design system / Flows，
每面板 = h3 + View all + 清单药丸（8/7/8/10/6 个） -->
<section class="panel" style="background:#f0ead8" aria-label="Mobile app">
<div class="panel-head"><h3>Mobile app</h3><a class="view-all" href="#">View all</a></div>
<div class="pills">
<span class="pill-motion"><a class="pill" href="#">Paywall</a></span>
<!-- 其余药丸同结构，图标为通用 20px 描边占位 glyph -->
</div>
</section>
</section>
<section class="search-sec">
<h2>Find exactly what you need</h2>
<div class="search-row">
<button class="search-trigger" type="button">Search checklists…</button>
<button class="refresh" type="button" aria-label="Refresh checklists"></button>
</div>
<div class="feat-grid">
<!-- 12 张精选卡：图标 + 文本块（卡名 + 标签 gap 6），跨度模式 [2,1,2,1 / 1,2,1,2 / 2,1,2,1] -->
<a class="feat-card" style="grid-column: span 2" href="#">
<span class="feat-text"><span class="feat-name">Action Sheet</span>
<span class="feat-tag" style="background:#f0ead8">Mobile</span></span>
</a>
</div>
<a class="browse-all" href="#">Browse all 138 checklists →</a>
</section>
<section class="ai-card">
<div class="ai-text">
<div class="ai-copy"><!-- 眉题 + h2 + 两段文案 --></div>
<a class="ai-cta" href="#">Try the plugin</a>
</div>
<div class="ai-visual">
<div class="bill-shot"><!-- 账单截图：CURRENT BILL $127.34 等，全文 --></div>
<div class="plugin-panel"><!-- 插件面板：What's working / Considerations，全文 --></div>
</div>
</section>
<section class="news"><!-- 订阅：h3 + 文案 + email 输入 + Notify 按钮 --></section>
</main>
<footer><!-- 顶行品牌 + CTA，5 列全量目录（138 条），底栏 --></footer>
<!-- 搜索浮层：遮罩 bg-black/25 + blur(2px)，输入条 + 结果面板，Esc 关闭 -->
</body>
</html>
```

---

## 7. 已知限制（资产层差距，不阻断入库）
* **存档不完整**：monolith 单文件档 `checklist-design-home.html`（185KB）不完整——CSS 拉取失败、Vite 懒加载分片缺失，本地打开渲染为空；所有计算样式实测值均来自 live 页，采样以 live 页为准。另有 curl 补建的文件夹镜像 `mirror/`（42 个 JS/CSS 资产 + 字体 + `api/checklists/grouped` JSON，138 条清单、5 个分组）存档；镜像可渲染外壳但清单数为 0（API 数据未随镜像生效）。
* **字体**：TT Hoves Pro 为专有字体（Regular / Medium / DemiBold woff2 已随镜像存档，仅本地临摹对照用；临摹用其 @font-face 引用，fallback Arial）。
* **图标**：药丸 / 卡片图标是每条清单的专有线图标（Supabase 托管，114 个）；临摹用通用 20px / 24px 描边占位 glyph。导航 Search 字重观感偏粗归因于占位字体渲染差异，不再改数值。
* **推广区图片**：AI 卡两张产品截图（plugin-mockup.png / plugin-panel.png）为真实产品图；临摹用同几何的 CSS / 文字画实，未用图片。
* **精选卡随机**：精选卡每次加载随机抽取，临摹取其中一次快照——v2 固定为 orig-full.png（盲审对照图）里的 12 条（Action Sheet、Login、Notifications、Feed、Pricing、Onboarding Checklist、Alert、Timeline / Gantt View、Uploading media、Billing、Canceling subscription、Badge）。
* **脱敏改写不一致**：Hero / 品牌名 / 页脚为脱敏改写（页面 chrome 去品牌化，如品牌名改写为 "Checklist Studio"、Hero 标题改写为 "Sweat every detail, ship better work."），但推广区 mockup 内仍留原版产品字样（如 Checklist Design），属去品牌化不一致，记为已知限制。分类名与清单名属通用 UI 词汇，保留以保结构保真。
* **残差**：页脚目录 +4px（38 条长列的亚像素舍入）、Browse all 矩形高 20 vs 18（计算行高两边同为 20px）、页脚总高 +5px；窄屏（390px）已重拍 replica-narrow.png（高 9462），未逐项复核窄屏推广区。

**过程文件**：沙盒目录 `~/workspace/goals/goal-3/hidden_files/design-learning/patterns-sandbox/2026-11-25-checklist-design-home/`（STATUS.md、STATUS-v2.md、sampled.json、anatomy-draft.md、replica.html v2、compare-default.png、compare-full.png、mirror/、探针脚本与日志）。
