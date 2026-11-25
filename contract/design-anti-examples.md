# 设计反例库

不好看、AI 味道重的设计案例存档。每条记：对象与来源、问题具体在哪、禁止的手法。
做设计时命中「禁止的手法」的产出不许交付。条目由每日设计任务搜集补充，kk 的否决优先入账。
全文件上限 100 行，过时条目删改不堆积。

## 内部反例（自家踩过的）

### 薯乐官网 v1（2026-11-15，内部产出）
- 问题：通用模板排版（大标题＋说明＋按钮的默认组合）、素材靠代码绘图、无品牌识别，kk 判「一坨」，盲审设计 4 分。
- 禁止：模板味首屏结构；用代码绘图冒充产品素材交付。

### 设计规范档案站 v1（2026-10-21，内部产出）
- 问题：纸面/档案/复古纸质风格（类纸张底色、发丝线、标本档案气质），kk 评「太丑」并明令禁止。
- 禁止：纸面档案风作为任何页面的默认视觉方向。

### PPT 式拼贴页面群（2026-11-17，内部产出，kk 明令拒绝）
- 问题：页面不像网站，像几页 PPT 拼在一起——每节自成一页、居中大标题＋说明＋卡片阵列、每节分量一样重；整体太满、无视觉重心、无编排，滚动只是翻页。
- 禁止：PPT 式设计——一节一页的幻灯片结构；每节同构、同重、互不衔接；用填满一屏代替编排与让步。

### DAR 阶段同质化页面群（2026-11，内部产出）
- 问题：同一套米黄底色＋圆角卡片反复套不同题材，页面之间只有标题不同——规则只禁不用、没给合法素材出路时的典型死法。
- 禁止：同一视觉母题无差别套多个不相关题材；圆角卡片阵列当默认布局。

### Huttter 海报 v1（2026-11-19，内部产出，kk 判定）
- 问题：kk 问「为啥都是瑞士风格呢」——v1 实为一张瑞士骨架（网格、无衬线大字、黑线分区、01/02/03 编号）加野蛮主义配色，四套皮只换配色；kk 明令取消「网格＋无衬线」为设计默认。
- 禁止：只换配色冒充新方向；三方向稿须在骨架/字体/排版/色彩/质感中至少三项实质分化。

## 外部反例（搜集补充区）

### Durable 官网首页（2026-11-17，外部）
- 对象与来源：Durable（AI 建站工具）官网首页 https://durable.co/ ；本体文字 2026-11-17 打开核实（新鲜抓取），本地镜像样式未加载，视觉细节未作断言。
- 问题：全页是 AI 落地页模板的活标本——首屏「大标题 The complete AI business builder＋一句话说明＋Start for free 按钮＋4.8 星 Trustpilot＋Trusted by 3 million」；其后每节同构（短标题＋一行说明＋按钮）：Built for your business / Launch and grow / 证言轮播 / $396 对 $25 价格对比 / FAQ / 结尾 CTA；整页没有一件自家独有的视觉资产，Hero 的图是 7 张轮播的"生成示例站"（Slide 1 of 7: Build your coaching business, fast），等于用自己的模板给自己做广告，换个名字就能套给任何一家 AI 工具。
- 禁止：大标题＋说明＋按钮＋信任徽章的万能模板首屏；用证言、星级、数字徽章代替品牌资产撑场面。

### Zarla 落地页功能页（2026-11-18，外部）
- 对象与来源：Zarla（AI 建站工具）落地页功能页 https://www.zarla.com/features/landing-page-builder ；页面正文本体 2026-11-18 打开核实（全文 14 个 h2），本地镜像样式未加载，视觉配色与字号未作断言。
- 问题：与 Durable 同一模板病的第二件标本——全页 14 节同构（短标题＋一行说明＋按钮）：首屏「The AI landing page builder built for small business owners.」＋一句话说明＋"Free to build · No credit card · Pay when you publish"；其后依次为行业认知段＋数字行（50,000+ / 30 sec / 4.9★）／"What is a landing page builder?" 定义段／How it works／6 张特性卡（AI campaign copy / Multi-step lead forms / Leads → CRM / Mobile-first / SEO / Unlimited）／Three steps 编号三步／对比表（Zarla vs Wix/Unbounce/Leadpages）／证言两条（Trustpilot、Capterra）／FAQ／结尾 CTA；全页唯一视觉资产是一段演示视频与一张模板缩略图，换个名字就能套给任何一家 AI 建站工具。
- 禁止：大标题＋说明＋按钮＋信任徽章的万能模板首屏；用证言、星级、数字徽章代替品牌资产撑场面。

### Mixo 官网首页（2026-11-19，外部）
- 对象与来源：Mixo（AI 建站工具）官网首页 https://www.mixo.io/ ；页面正文本体 2026-11-19 打开核实，视觉配色与字号未作断言。
- 问题：Durable、Zarla 之后同一模板病的第三件标本——首屏「Build a professional website with AI in minutes.」大标题＋一句话说明＋"Free plan available · No credit card required"＋"Get Started for Free" 按钮＋"4.8/5 on Trustpilot"；其后每节同构（短标题＋一行说明＋证言引用）：Built with Mixo / Everything your business needs / Get enquiries / Stay in touch / Trusted by（3M+ / 300+ / 40+ 数字行）／工具集成；全页唯一视觉资产是 3 张功能截图与一批用户示例站缩略图（即自己的模板产出）。另有拼贴失误：同一批 6 个示例站卡片（PayYou / Holistic Oncology / Bosworth Law Firm / Renovate Pro / CraftiPanda / Temen Bulu）在页内原样重复出现两次，换个名字就能套给任何一家 AI 建站工具。
- 禁止：大标题＋说明＋按钮＋信任徽章的万能模板首屏；用证言、星级、数字徽章代替品牌资产撑场面。

### Bluehost AI Builder 落地页（2026-11-21，外部）
- 对象与来源：Bluehost AI Builder 落地页 https://www.bluehost.com/ai-builder ；页面正文本体 2026-11-21 打开核实，视觉配色与字号未作断言。
- 问题：Durable、Zarla、Mixo 之后同一模板病的第四件标本——首屏「Describe the website or app your business needs.」＋一句话说明＋提示词输入框（按钮换成输入框，骨架未变）；其后每节同构（短标题＋一行说明）：No code. Just describe it.／24/7 human support／Everything on one plan／三档定价（Free $0／Lite $16／Standard $32）／Watch it build 演示对话／与 Lovable、Bolt、Hostinger 的对比表／SEO 功能清单／FAQ／结尾 CTA「Describe it. We build it.」；全页唯一视觉资产是 6 个虚构示例站轮播（Northline、Lumen Studio、Form Club、Bright Smile、Hollis & Reed、Nor-Tex，页内自标 "illustrative website concept"）加三张通用职业 stock 照，等于是用自家模板产出给自家模板做广告，换个名字就能套给任何一家 AI 建站工具。
- 禁止：大标题＋说明＋按钮＋信任徽章的万能模板首屏；用证言、星级、数字徽章代替品牌资产撑场面。

### shadcn/ui Command 输入框无可见焦点环（2026-11-22，外部，getComputedStyle 实测）
- 对象与来源：shadcn/ui Command 组件文档首个 Base UI 演示 https://ui.shadcn.com/docs/components/command ；2026-11-22 实时页实测（浅色模式）：鼠标点击与键盘 Tab 回到输入框时，input、外包、容器三处 outline-style 均为 none、box-shadow 均为 none、边框色不变；选中行底色与白底对比仅约 1.05:1。
- 问题：一个被广泛复制的命令面板，键盘用户按 Tab 进输入框后看不到焦点落在哪，选中行在弱光屏上也几乎看不见——克制过了头，把状态本身省掉了。不是「极简」，是漏了状态。
- 禁止：把「无焦点环」当克制来抄；自家命令面板／搜索框落地时，焦点环与选中对比（≥3:1）必须另行补足，不许以本对象为依据省略。

## 二阶默认（2026-11-22，源自 AI 设计调研简报）

「去 AI 味」的流行解法本身已收敛成新模板，与一阶 tells 同罪：流水线审查与视觉盲审对照核查时，命中下列任一即列为问题。

- 奶油底＋陶土色（Claude 官网 look 的整体搬运）——约 2026 年成为默认
- 近黑底＋单一酸绿信号色——约 2026 年成为默认
- 全大写 mono 字体当模板铬件——约 2026 年成为默认
- 标题里只给一个词上色——约 2026 年成为默认
- 装饰性 01/02/03 编号（无真实顺序含义时）——约 2026 年成为默认

维护规则：每条 tell 标注成为默认的大致时间；每周五设计复盘滚动检查自家流水线的高频解法有没有新上榜，上榜即补进本节。（来源：avoid-ai-design 的二阶默认目录，Anthropic 与 OpenAI 官方前端指南互证）

### Stripe Press〈Tacit〉临摹星形网格（2026-11-24，内部临摹）
- 对象与来源：Stripe Press〈Tacit〉文章页临摹（patterns/stripe-press-article.md）；原站星星是嵌在照片纹理里的细小暗星，临摹把星形抽成独立 SVG 图层放在纯黑底上。
- 问题：黑底放大了星形对比度——亮石灰绿大星＋辉光晕满屏均匀铺开，压在引文与正文字形上，三轮返工（透明度 .65→.35、星形缩小 35%、晕 r 24→16）终审仍判过亮，记入已知限制。
- 禁止：装饰纹理代位时只抄形状、不降存在感；透明度/尺寸/辉光须三档同降，让代位纹理退回底纹层，不许它与正文抢视线。

### 墨绿＋米黄默认配色（2026-11-24，kk 明令）
- 对象与来源：贾维斯近期多个自然/食品题材产出（含渴茶官网灰绿底）；kk 指出「为什么你的设计还在用墨绿米黄」后当场下禁令。
- 问题：「向世界染色」滑成模板默认——题材沾自然就整套染绿、沾温暖食物就染米黄，底面一眼能叫出颜色名；这是 AI 生成设计的统计平均脸，与瑞士网格同病，只是换了层皮。单页数值审查（占比/对比）查不出跨项目同质化。
- 禁止：墨绿、米黄（奶油/米色系）及近似深绿、暖米色调用作底色与主色调（已同步入契约「颜色」节）；产品实物与包装实拍的真实颜色不在此限。

（由每日设计任务按格式追加：对象＋来源链接、问题具体到位置与手法、禁止的手法一条。）
