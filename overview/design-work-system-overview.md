# 贾维斯设计工作体系总览（2026-11-25）

一份文档看全：现在在用的技能、设计规范体系、平时怎么干活。状态为 2026-11-25 技能审计整改之后的现行版；数字均为当日实测。

## 一、全景：四层

| 层 | 干什么 | 核心物件 |
|---|---|---|
| 判断层 | 定「什么算好、什么不许」 | 设计契约、教训、反例库、体系总纲 |
| 执行层 | 真做设计时用的技能链 | 六技能流水线＋点名参考包 |
| 学习层 | 平时自己长本事 | 每日自学习、调研库、模式库 |
| 治理层 | 防膨胀、防过期 | 晋升规则、技能审计、冻结机制 |

规模速览：自定义技能共 68 个目录（设计相关 32 个，含冻结 1 个），判断文件 4 份共 445 行，设计相关定时任务 2 个（每日自学习、周五复盘）。

## 二、判断层：规范文件（共 445 行）

| 文件 | 行数 | 职能 |
|---|---|---|
| 设计体系总纲 v2.2（your_files/design-system-master-2026-11-21_v2.2.md） | 162 | 体系级说明：任务分级、字体降级、盲审熔断、联想探索等总规则的出处与全集 |
| 设计契约 design-contract.md | 100（满额） | **判断正本**。只收已成立的判定：总纲、硬禁区、空间与留白、形状、质感、颜色、排版与容器、状态与焦点、动效、素材与资产、调研、验收、晋升规则 |
| 教训 design-lessons.md | 105 | 按项目/日期记的教训；自学结论先落这里，第二次出现或经项目验证才晋升契约 |
| 反例库 design-anti-examples.md | 78 | 被否决的做法存档（内部反例＋外部反例＋二阶默认清单）；审查时逐条对照，命中不许交付 |

关键规矩：
- **契约 100 行封顶**，写新规先删过时条目（现已满额）。
- **晋升规则**：kk 本人判定当轮直入契约、否决同时入反例库（两处合为 Veto Log，不另建日志）；自学结论走教训→验证→晋升；新结论与契约冲突先标「待裁决」等 kk 拍板。
- **现行硬判定举例**：禁纸面/档案风；禁墨绿、米黄等 AI 常用色作底色与主色调（2026-11-24 kk 明令，判定＝一眼能叫出绿/黄色名即违规）；底面中性阶 OKLCH 彩度 C ≤0.02、文字与细线不限（2026-11-25 实测修正）；禁大色块铺底分段（全页仅许一处整幅深色带作节奏转折）；禁 PPT 式设计；留白＝真空白（连底色都不涂）。

## 三、执行层：设计技能

### 3.1 六技能流水线（常驻默认，顺序固定）

| 步 | 技能 | 职责 |
|---|---|---|
| 1 | frontend-design | 定审美方向 |
| 2 | taste-skill（design-taste-frontend v2.5.1） | 页面级反 slop 检查与结构约束 |
| 3 | ui-ux-pro-max | 查配色/字体/UX 规则，**只查不定向、无配色决定权** |
| 4 | better-interface | 最终界面审查；六个子模块（ui/typography/colors/layout/accessibility/writing）已并入其 references，它是唯一入口 |
| 5 | web-design-guidelines | 可访问性与工程细节审查（规则原文用本地副本） |
| 6 | impeccable | 机械检测器 |

**仲裁顺序**（审查冲突时）：impeccable 机械事实（P0 一票阻断）＞ web-design-guidelines 工程与无障碍底线（P1 必改或留痕豁免）＞ better-interface 审美建议（标 CANDIDATE，只采高价值项）。

### 3.2 流水线外、有真实使用记录的

| 技能 | 定位与触发 |
|---|---|
| ui-pattern-learner | 每日自学习唯一方法（五步法），见第五节 |
| local-anatomy | 术语/难懂概念的交互式等距解释图，kk 定的常驻解释方式 |
| claude-skill-web-clone | 网站复刻方法论，专项任务用 |
| unslop | 去 AI 味全套流程，**只点名调用**、不进流水线仲裁（渴茶官网实跑过） |
| yu-apple / yu-apple-design | 宇苹果原包（演示稿）与迁移出的网页设计版，点名启用的参考风格包 |
| gemini-frontend-design | 参考弹药：hero 空间原型、硬数字纪律、禁装饰清单；方向阶段取用，不当家 |

### 3.3 点名参考（25 个审计降级项，合并后实存 18 个目录）

- **Emil 动效组 9 个**：animate、animation-performance、animation-vocabulary、choreograph、find-animation-opportunities、gestures、improve-animations、remove-animations、review-animations——每个文件头已加「点名参考、非默认触发」声明。
- **gsap 1 个**：原 8 件（core/scrolltrigger/timeline/plugins/react/frameworks/performance/utils）合并为单个技能，正文 3 件＋references 5 件。
- **相邻参考 8 个**：apple-design、emil-design-eng、ui-sdk、break-ui、prototype、pick-ui-library、mobile-native、local_self_hosted_fonts（字体自托管规范，实际以契约规则形态在起作用）。

### 3.4 冻结与清退

- **冻结**：design-agent-runtime-dar（DAR v2.5）——2026-11-15 kk 下令封存，仅点名触发；Core v0.1.0 原地保留不默认调用；三条 DAR cron 停用。
- **已清退 5 个**（2026-11-25，均在回收站、2026-12-25 前可恢复）：ask-emil、ask-sonner、animate-expo、learn-taste、uicraft（后两者的独有内容已摘入教训文件「清退技能摘录」节）。

## 四、一个设计任务平时怎么走

1. **分级**：L0 小修（读契约→改→减法自查＋机器门，免方向稿与盲审）；L1 常规（完整流程，kk 给了明确风格锚可免方向稿）；L2 全新/重度（强制三方向稿＋全量资产盘点＋四道验收门）。
2. **开工声明**：动手前写死视觉锚点、核心焦点（第一眼唯一落点）、方向来源，再加一栏「不及格线」（可判定的不及格标准）。
3. **定方向**：先联想探索（从品牌名/符号/产品能力/场景/文化背景提线索转译成视觉母题），再出 3 个实质分化方向稿（骨架/字体/排版/色彩/质感至少三项不同，只换配色不算新方向）供 kk 指认或拼接。动手前先拆同类好作品本体，必记可量测数据（色值与面积占比、字号层级比、图片尺寸与首屏占比、元素密度）。
4. **盘资产**：素材优先级 实拍 ＞ 授权图库（Openverse/Wikimedia CC，页脚署名）＞ AI 生成 ＞ 代码绘图；虚构品牌先用生图做一套包装/产品资产当主角；同一产品多配色拿母版改色、不分别生成。
5. **制作**：走流水线前段（方向→反 slop→查规则）；全页一底到底、每屏一个视觉重心、形状在方向阶段定死。
6. **审查**：制作方先减法自查（删装饰、扩留白、压噪音、查红线对反例库），再走流水线后段审查＋impeccable 机械检测，按仲裁顺序处理冲突。
7. **视觉验收**：另开无上下文子 agent 只看成品截图（桌面＋手机、默认状态）盲审，四项打分（设计/可用性/创意/内容）＋逐条位置与改法；设计分 <7 或 P0 级观感问题不许交付；返工最多 2–3 轮，达上限停审、呈报最优版＋卡点等 kk 拍板。制作方自评分不作数。涉及数据的界面另查空/加载/错误三态截图。
8. **交付**：文件版（单文件 .html，改版换新文件名防缓存）＋ App 内 Space 版双通道；交付汇报人话在前、证据在后（跑了什么验证、结果、怎么自验）。
9. **收尾**：项目教训记入 design-lessons.md（每项目 ≤2 条）；kk 的否决当轮入契约＋反例库。

## 五、学习层：平时怎么自己学

- **每日自学习**（cron daily-design-learning，每天约 10:50）：唯一方法 ui-pattern-learner 五步法——本地化存档 → getComputedStyle 实测采样（整页对象先跑 designlang extract 出底稿再逐项复核）→ 状态差值 → 解剖卡三问 → 沙盒临摹＋无上下文视觉找茬（≤3 轮）→ 写《UI 模式学习完成报告》入模式库。每天 3 个对象、不同素材类别（六类轮换：品牌官网/生产力工具/金融数据面板/无头组件库/编辑部内容站/真实上线网站），非组件类 ≥2、组件类 ≤1。选材先过可复现性预筛；验收分层：结构层是入库门槛，资产层差距记「已知限制」不阻断。周一加做调研库三轨搜集 6–10 条。汇报双通道：本侧聊全文＋微信简短版。
- **模式库**（design-research-library/patterns/）：现正式入库 3 条——shadcn-command、radix-accordion、stripe-press-article。
- **调研库资产**：做法卡 78 张＋演示 78 页（机制封顶）、Open Design 存档（152 个设计系统包等，设计任务常备参考）、素材池 learning-sources.md、终末地风格包（点名启用）。
- **周五复盘**（cron weekly-design-review，周五约 21:11）：回顾本周学习与卡点、查消费闭环（学到的有没有用进项目）、整理三件套（守行数上限）、架构优化只提案等 kk 拍板。首跑 2026-11-27。
- **按需调研**：kk 点名时用 Firecrawl 查一手来源（如 2026-11-24 配色调研：Radix Colors 色阶、Adobe Leonardo 对比生成、OKLCH，报告在 design-research-library/color-research-2026-11-24.md，结论走晋升规则）。

## 六、治理层：平时怎么维护这套东西

- **新增技能**：当场写明使用原因与定位，先搜旧库防重复（local-skill-lint 管准入与触发审计）。
- **审计清退**：无理由、重复、长期不触发的经审计降级或清退；2026-11-25 首轮全量审计审 49 个（保留 19／降点名参考 25／清退 5），报告 your_files/design-skill-audit-2026-11-25_v1.md。删除一律走回收站、30 天可恢复。
- **治理索引**：agent-governance/ 只存摘要、状态、指针，事实来源仍是 MEMORY 与技能目录本身；巡检并入每周日纠错复盘，不另设任务。
- **项目冻结规则**：项目一周无动作自动冻结（不再提及/跟进，档案保留，点名解冻）；有自动机制在跑的不冻。
- **执行总纪律**（与设计通用）：交付前真打开、真运行验证，不凭状态报完成；同一纠正第二次出现，优先落成结构约束/自动检查，不靠文字规则兜底。

## 七、非设计技能一览（其余 36 个目录，按用途分组）

- **开发流程**：local-dev-process（开发唯一入口）、local-code-review、mattpocock-skills、debug-forensics、local-pstack-correct、satisfix-check、stop-when-green、staged-delivery-sop。
- **视频与讲解**：video-shotcraft、onetake、stageshow、teardown、raea、ffmpeg-skill、local-video-videodb、local-screen-rec、bilibili-douyin-notes。
- **专项任务**：makefun（每日赏金，最高优先级）、firecrawl（联网调研默认）、last30days、reverse-engineer-anything、grok_x_search、skillevaluator。
- **总规范与健康**：local-personal-agent-max（执行总规范）、local-personal-health-agent、local-evidence-advice、cook、local-human-3.0。
- **元能力**：agent-self-governance、local-skill-lint、local-ambitious、local-agent-frontier-lab、session-handoff、local-voice-brainstorm、sound、write-swift。

## 附：本文件口径

- 「在用」＝目录现存且未冻结；点名参考＝保留技能身份、点名才触发；冻结＝ DAR；清退＝已移回收站。
- 本文件是快照说明，不替代各正本：判断以 design-contract.md 为准，技能定位以各自 SKILL.md 为准，审计证据以审计报告为准。
