# UI 模式学习完成报告

## 1. 基础档案 (Meta)
* **Pattern ID**：`stripe-press-article`
* **来源网址**：https://press.stripe.com/tacit（301 跳转至 https://stripe.press/tacit）
* **所属类型**：`[编辑部排版 / 长文排版系统]`
* **风格标签**：`#编辑部` `#深色长文` `#流体字阶` `#单墨色`
* **验证状态**：✅ 已通过沙盒渲染与视觉找茬闭环（共迭代 3 轮：第 1 轮未通过→第 2 轮窄口径不通过（只卡星形高光）→第 3 轮结构层通过，可入库；终审评审未给分项分数）
* **沉淀路径**：`patterns/stripe-press-article.md`
* **学习日期**：2026-11-24；类别⑤ 编辑部与内容网站（排版系统／页面模式，非组件类）
* **页面构成**：全屏 Canvas 动画 Hero → Director's Note（引文＋17 段正文＋影片卡片）→ Bibliography（书目列表 84 条）

> 终审说明（据实记录）：第 3 轮评审的标题结论是"结构层通过，可入库"，同时在问题清单里把星形网格仍标为"P0 级观感问题"（星点过大过亮、光晕压字）。两处口径不一致；评审的收尾建议明确写"若不再开第 4 轮，问题 1、2 记入完成报告作「已知限制」即可入库"。按"最多 3 轮"的硬规则不再开第 4 轮，本报告按终审标题结论入库，并把星形问题与展示字代位问题如实记入已知限制（见附录），不淡化。

---

## 2. 审美解剖与克制逻辑 (Anatomy)
* **核心克制点（刻意去掉了什么）**：
  * 无容器 chrome：无边框、无阴影、无圆角、无卡片，层级全靠排版与留白
  * 单墨色 #cac8d1：全文只用一个文字色，不靠多色区分层级
  * 正文链接不换色：与正文同色、仅下划线，hover 也无任何计算样式变化（两次验证）
  * 无首字下沉、无装饰引文样式（引文无边框无背景）、无语义标题标签（标题全是 styled div，无 H1–H3）、无浅色模式、无脚注
* **主次层级手段**：三档字阶（展示 84px : 引文 40px : 正文 20px，约 4.2:2:1）+ 三字体分工（Cardinal Fruit 展示/引文、Ivar 正文、Suisse Mono 微标签）；字距随字号收紧（-0.03em → -0.06em）；不对称网格（488px 左栏 + 576px 右栏）+ 巨型留白（章节 margin-top:120vh）；展示标题 sticky；hover 靠反色（导航 #d8ec46、书目条目 #cac8d1 反白）而非加重；流体 calc 字号
* **适用与禁用边界**：
  * **适用**：慢节奏长文编辑部、有真实影像资产的深色电影感页面
  * **禁用**：高密度信息界面；需颜色表达语义的界面；<60px 的展示标题；没有影像资产时的"裸排版"（本页的留白与黑场全靠照片与 Canvas 动画兜底）

---

## 3. 核心 Design Tokens 与交互差值 (Tokens & Deltas)

| 维度 | 提取规范 / 精确参数 | 关键备注 |
| :--- | :--- | :--- |
| **色彩与层级** | 页面底 #1a1a1a（designlang 底稿误报 #000000，已纠正）/ 主墨 #cac8d1 / 导航 #f5f5f5 / hover 亮色 #d8ec46；阴影全 none；圆角全 0（导航 logo 白色小 pill 约 3px，估算） | 单墨色 + 字号/字距/留白拉层级；中性色占比 >95% |
| **排版比例** | 展示标题 84.08px/73.99px（行高 0.88，字重 300，字距 -0.05em，大写，sticky top:0，padding-top:10vh）· 引文 40px/38px（字重 400，字距 -0.06em，mb 16px）· 正文 20px/24px（字重 400，字距 -0.04em，mb 24px）· 书目标题 48px/48px（斜体，字重 300，字距 -1.44px）· 微标签 12px 大写 | 字阶比约 4.2:2:1；展示标题为流体 calc（300–1600px 间插值，1440/390 两档精确复核通过）；栏宽 576px（x=518，不对称偏右），每行 69 字符；390px 下正文不降字号，栏 358px、每行 38 字符、无横向溢出 |
| **间距与圆角** | 章节 margin-top:120vh；padding-bottom:30vh；段间距 24px；署名 mb 32px；书目条目 1px dashed #cac8d166 分隔，padding 24px 0，标题 padding-right 100px | 圆角全 0；页面总高 14622px @1440、20936px @390 |
| **Hover 差值** | 导航链接：背景 transparent → #d8ec46，文字 #f5f5f5 → #000，transition: color 430ms ease；正文链接：无任何计算样式变化（下划线常驻、同色；样式包虽有全局 `a:hover` 亮色规则，但同特异度后置规则覆盖了颜色）；书目条目：背景 → #cac8d1、文字 → #000（整行反色，CSS 考古） | 正文链接 hover 不生效的层叠原因未完全定位（CDP 溯源被代理故障阻断），临摹以实测值为准：无变化 |
| **Focus/边缘态** | focus-visible：outline 1px solid #f7f7ef，offset 2px（3 个 Tab 停靠点一致）；::selection：bg #f5f5f5 / 字 #1a1a1a；无脚注（sup=0）、无 H1–H3、无明暗切换 | 390px 窄屏：正文死守 20px/24px；展示标题流体缩小（84.08→72.95、82.60→61.78）；无横向溢出 |

### designlang 底稿复核（四项不可信数据逐项核对）
* **前景色归纳**：底稿 #cac8d1 ✅ 实测一致
* **字体归属统计**：角色归属 ✅（展示+引文 Cardinal Fruit、正文 Ivar、微标签 Suisse Mono）；但底稿 "Body 16px" ❌ 错误，实测正文 20px
* **字阶主值**：✅ 基本一致（display 偏差系 1280 vs 1440 视口的流体字号）
* **scrollLinked 判断**：底稿未给判定；实测以 sticky 为准（展示标题 `position:sticky;top:0`），无 scroll-timeline 声明
* **底稿其他误报**：背景 #000000（实测 #1a1a1a）、行长 ~157 字符（实测 69）；fidelity 分数与 verify-ui 未作验证依据

---

## 4. 沙盒临摹与视觉纠偏记录 (Critic Loop)
* **Round 1 发现的失真问题**：
  1. `[书目取景]`：临摹截图取景过浅（滚动仅到标题上方 100px，sticky 标题未贴顶，与原站取景不对齐）→ **已修正为** 重拍（滚动至标题后 500px，sticky 标题贴顶对齐原站）
  2. `[星形纹理]`：星形在深色底上略重 → **已修正为** 保持（属资产层纹理占位，不追原站辉光；见第 4–5 轮的后续处理）
  3. `[字体代位]`：Georgia 比 Cardinal Fruit 粗，断行不同 → **已修正为** 记录为资产层已知限制，不改（结构数值一致）
* **Round 2（盲审第 1 轮返工，第 4 轮）**：盲审 12 条意见，逐条"盲审意见 → 实测值 → 改成什么"：
  1. 竖排 TACIT 隐形 → 实测：原站无该元素的 DOM（Canvas 光栅资产）；像素采样字母近黑 #000–#000053、底中位亮度约 150 → 临摹 .giant 由 `rgba(202,200,209,.16)` 改为实色 #cac8d1、字重 700（像素推断，非 CSS 实测值）
  2. 首屏缺标语与字幕条 → 两者均为光栅资产文字层（tagline.png / marquee-sprite.png），全文不可恢复 → 补文字层代位（标语 "WE KNOW MORE THAN WE CAN TELL" + 字幕条可辨认片段），置于巨字列右侧
  3. "丢绿色前景色" → **未按评审字面改**：像素采样原站与临摹引文/正文最常见亮色均为 rgb(202,200,209) = #cac8d1，临摹 --ink 从一开始就是该值，不存在"纯白"；"浅绿"是绿色星形＋深色影像底语境下的同时对比错觉，色值不动，以实测为准
  4. 占位块廉价 → 背景 #3a3a3a 改为近黑 #141414＋1px 淡描边＋淡字；中央 note 框去虚线与深底
  5. 星形偏暗偏机械 → 改亮石灰绿 #c8f542＋近白芯＋径向辉光晕，双层底做亮度起伏；首版过亮压字后微调（晕 r 30→24、层透明度 .8→.65）
  6. 书目区疏、标题重 → **字重未按评审字面改**：书目标题 fw 300 已实测（10 条全为 300，无更轻档可降）；条目 padding 与分隔线与实测一致，不动；改的是真正的实测偏差（meta 行 margin-top 8px→0、标题补 padding-right:100px、致谢 intro margin-top 24px→10px、meta 行重构为 flex 两端）
  7. 缺条目与致谢段 → 书目补全为原站 84 条、原站顺序；致谢段换为全文（含六人）；biblio 重拍取景对齐原站（"7 Days Out" 置顶为首个可见条目）
  8. 分隔线略重 → **未改**：重采 li border-bottom 为 1px dashed rgba(202,200,209,0.4)，与临摹 --hairline 完全一致，以实测为准
  9. 390px 顶栏 TACIT 被裁 → 根因不是宽度溢出（285px＜368px 可用宽），而是 fixed 导航透明、sticky 展示标题滚动经过时巨字笔画横穿导航文字 → 临摹 .nav 改为全宽 76px＋同形渐晕（原站 NavContainer 实测渐晕，按 #1a1a1a 底色转译），品牌区补 ⋮ 分隔符；窄屏字号保持实测 12px 不缩
  10. 星形压字（窄屏）→ ≤700px 时 .stars 透明度降至 .3
  11. 引文高对比感弱 → blockquote 字重 400→300，仅为代位字体的观感补偿（非实测值），字号保持实测 40px
  12. 引文为占位文案 → 只记报告、不改临摹（见已知限制）
* **Round 3（盲审第 2 轮返工，第 5 轮，最后一轮）**：只改星形一处——整层透明度 .65→.35；星形路径等比 ×0.65（主星 40px→26px、次层 44px→约 28.6px）；辉光晕 r 24→16、晕心 stop-opacity .5→.3；次层晕 r 30→20、stop-opacity .25→.2、星芯 fill-opacity .3→.2。星色 #c8f542 与近白芯保留，其余一律不动；只重拍 compare-top / compare-quote
* **最终视觉复核结论**：第 3 轮终审结论"结构层通过，可入库"——导航、引文、正文、书目结构（字号/栏宽/间距/分隔线/390px 无溢出）均已逼近原站；剩余偏差（星形网格仍偏亮偏大、展示字因 Georgia 代位偏粗偏宽致断行不同、正文略密略重）记入已知限制；终审评审同时把星形标为"P0 级观感问题"，见第 1 节终审说明

---

## 5. 提取的视觉禁忌 (Anti-Patterns)
* ❌ 严禁深色长文页用多色区分层级：本页证明单墨色 + 字号/字距/留白已足够，多色会先破坏电影感
* ❌ 严禁正文链接 hover 变色/变底色：本页正文链接刻意保持与正文同色、仅下划线；hover 上亮色会打断阅读流（导航 hover 用亮色是另一回事：导航是 UI 声音，正文是阅读声音）
* ❌ 严禁展示标题用 400 以上字重或正字距：300 + -0.05em 是该标题成立的条件，换成常规字重/字距会立刻变"普通大标题"
* ❌ 严禁窄栏正文降字号：本页 390px 下正文死守 20px，降字号是"为塞进更多字而牺牲可读性"的反模式

---

## 6. 已验证的纯净代码 (Verified Code)
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Tacit — Replica (study copy)</title>
<style>
  :root{
    --page:#1a1a1a;        /* measured: .tacit__Page background rgb(26,26,26) */
    --ink:#cac8d1;         /* measured: body text rgb(202,200,209) */
    --nav-ink:#f5f5f5;     /* measured: nav links rgb(245,245,245) */
    --lime:#d8ec46;        /* measured: nav hover background rgb(216,236,70) */
    --hairline:#cac8d166;  /* measured: biblio dashed separator rgba(202,200,209,.4) */
    --serif-display: Georgia, "Times New Roman", serif;   /* stand-in for Cardinal Fruit */
    --serif-body: Georgia, "Times New Roman", serif;      /* stand-in for Ivar */
    --mono: ui-monospace, "SF Mono", Menlo, Consolas, monospace; /* stand-in for Suisse Mono */
  }
  *{margin:0;padding:0;box-sizing:border-box}
  html{background:var(--page)}
  body{background:var(--page);color:var(--ink);font-family:var(--serif-body);
       -webkit-font-smoothing:antialiased}
  ::selection{background:#f5f5f5;color:#1a1a1a}            /* measured */
  :focus-visible{outline:1px solid #f7f7ef;outline-offset:2px} /* measured */

  /* ---- star-grid texture: evokes the canvas scene, pure CSS placeholder ----
     Orig star cores sample near-white rgb(245,245,245) with dark-green mids.
     REV 3 values: layer opacity .35, stars scaled x0.65, halo r 16 / stop-opacity .3. */
  .stars{position:fixed;inset:0;pointer-events:none;z-index:0;opacity:.35;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='96' height='96'%3E%3Cdefs%3E%3CradialGradient id='g'%3E%3Cstop offset='0%25' stop-color='%23c8f542' stop-opacity='.3'/%3E%3Cstop offset='100%25' stop-color='%23c8f542' stop-opacity='0'/%3E%3C/radialGradient%3E%3C/defs%3E%3Ccircle cx='48' cy='48' r='16' fill='url(%23g)'/%3E%3Cpath d='M48 35 L50.9 45.1 L61 48 L50.9 50.9 L48 61 L45.1 50.9 L35 48 L45.1 45.1 Z' fill='%23c8f542'/%3E%3Cpath d='M48 42.8 L49.3 46.7 L53.2 48 L49.3 49.3 L48 53.2 L46.7 49.3 L42.8 48 L46.7 46.7 Z' fill='%23f4ffd6'/%3E%3C/svg%3E"),
      url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='192' height='192'%3E%3Cdefs%3E%3CradialGradient id='h'%3E%3Cstop offset='0%25' stop-color='%23c8f542' stop-opacity='.2'/%3E%3Cstop offset='100%25' stop-color='%23c8f542' stop-opacity='0'/%3E%3C/radialGradient%3E%3C/defs%3E%3Ccircle cx='96' cy='96' r='20' fill='url(%23h)'/%3E%3Cpath d='M96 81.7 L98.6 93.4 L107.7 96 L98.6 98.6 L96 110.3 L93.4 98.6 L84.3 96 L93.4 93.4 Z' fill='%23c8f542' fill-opacity='.2'/%3E%3Cpath d='M96 90.8 L97.3 94.7 L101.2 96 L97.3 97.3 L96 101.2 L94.7 97.3 L90.8 96 L94.7 94.7 Z' fill='%23f4ffd6' fill-opacity='.35'/%3E%3C/svg%3E");
    background-size:96px 96px,192px 192px;
    background-position:0 0,48px 96px}

  /* ---- nav: full-width 76px scrim (measured NavContainer gradient, translated
     from the original's dark-purple rgba(34,1,37,.6) to the replica's #1a1a1a) ---- */
  .nav{position:fixed;top:0;left:0;right:0;height:76px;z-index:30;display:flex;
       justify-content:space-between;align-items:flex-start;pointer-events:none;
       padding:14px 12px 0 10px;
       background:linear-gradient(0deg, rgba(26,26,26,0), rgba(26,26,26,0.6))}
  .nav>*{pointer-events:auto}
  .brand{display:flex;align-items:center;gap:10px;flex-shrink:0}
  .logo-pill{background:#f5f5f5;color:#000;font-family:var(--mono);font-size:12px;
       letter-spacing:-.03em;text-transform:uppercase;padding:4px 10px;border-radius:3px;
       line-height:12px;display:inline-flex;align-items:center;gap:6px}
  .logo-pill::before{content:"";width:8px;height:8px;background:#000;border-radius:50%}
  .nav-div{font-family:var(--mono);font-size:12px;color:var(--nav-ink);opacity:.7;
       line-height:12px;margin:0 2px}
  .brand .section-name{font-family:var(--mono);font-size:12px;letter-spacing:-.03em;
       text-transform:uppercase;color:var(--nav-ink);line-height:12px}
  .nav-links{display:flex;flex-direction:column;align-items:flex-end;gap:6px;line-height:1;flex-shrink:0}
  .nav-links div{font-family:var(--mono);font-size:12px;letter-spacing:-.03em;
       text-transform:uppercase;color:var(--nav-ink);cursor:pointer;
       transition:color 430ms ease}
  .nav-links div:hover{background:var(--lime);color:#000}  /* measured hover */

  /* ---- hero placeholder (asset layer: full-viewport canvas scene, not reproduced) ---- */
  .hero{position:relative;z-index:1;height:900px;display:flex;align-items:flex-end;
        padding:0 0 40px 10px;overflow:hidden}
  .hero .giant{font-family:var(--serif-display);font-weight:700;text-transform:uppercase;
       font-size:280px;line-height:.8;letter-spacing:-.05em;color:var(--ink);
       writing-mode:vertical-rl;transform:rotate(180deg);user-select:none}
  /* tagline + credits strip: text-layer stand-ins for the raster tagline.png /
     marquee-sprite.png artwork in the original hero canvas. */
  .hero .tagline{position:absolute;left:270px;top:330px;max-width:220px;
       font-family:var(--mono);font-size:12px;line-height:16px;letter-spacing:-.03em;
       text-transform:uppercase;color:var(--ink)}
  .hero .credits{position:absolute;left:270px;right:12px;bottom:36px;
       font-family:var(--mono);font-size:10px;line-height:14px;letter-spacing:.04em;
       text-transform:uppercase;color:rgba(202,200,209,.8);
       white-space:nowrap;overflow:hidden}
  .hero .note{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);
       font-family:var(--mono);font-size:12px;letter-spacing:-.03em;text-transform:uppercase;
       color:rgba(202,200,209,.6);border:1px solid rgba(202,200,209,.18);padding:14px 18px;text-align:center;
       line-height:1.8;background:transparent}
  .hero .hero-img{position:absolute;right:120px;top:120px;width:391px;height:186px;
       background:#141414;border:1px solid rgba(202,200,209,.22);display:flex;align-items:center;justify-content:center;
       font-family:var(--mono);font-size:11px;letter-spacing:.05em;color:rgba(202,200,209,.55);
       text-transform:uppercase;text-align:center}

  /* ---- article grid: left rail 488px + text column 576px, 20px gap, 10px left inset ---- */
  main{position:relative;z-index:1}
  .note-section{display:grid;grid-template-columns:488px 576px;gap:20px;
       padding:0 0 0 10px}
  .rail{position:relative}
  .display-title{position:sticky;top:0;padding-top:10vh;font-family:var(--serif-display);
       font-weight:300;font-size:84.0752px;line-height:73.9862px;letter-spacing:-4.20376px;
       text-transform:uppercase;color:var(--ink)}   /* measured 84.08/73.99, ls -0.05em, lh .88 */
  .watch{margin-top:48px}
  .watch .card{margin-bottom:40px}
  .watch .thumb{width:391px;height:186px;background:#141414;border:1px solid rgba(202,200,209,.22);display:flex;
       align-items:center;justify-content:center;font-family:var(--mono);font-size:11px;
       letter-spacing:.05em;color:rgba(202,200,209,.55);text-transform:uppercase}
  .watch .label{font-family:var(--serif-display);font-style:italic;font-size:20px;
       line-height:24px;margin-top:12px}
  .watch .name{font-family:var(--serif-display);font-weight:300;font-size:40px;
       line-height:40px;letter-spacing:-.02em;text-transform:uppercase}
  .textcol{padding-top:10vh}
  blockquote{font-family:var(--serif-display);font-weight:300;font-size:40px;
       line-height:38px;letter-spacing:-2.4px;color:var(--ink);margin:0 0 16px 0;
       hanging-punctuation:first last}              /* measured 40/38, ls -0.06em, lh 95%;
       weight 300 is a stand-in-font perceptual compensation, NOT measured:
       measured quote weight is 400 */
  .attribution{font-family:var(--mono);font-size:12px;line-height:14.4px;
       letter-spacing:-.72px;text-transform:uppercase;margin-bottom:32px} /* measured 12/14.4, ls -0.06em */
  .textcol p{font-family:var(--serif-body);font-size:20px;line-height:24px;
       letter-spacing:-.8px;margin-bottom:24px}     /* measured 20/24, ls -0.04em */
  .textcol p i{font-style:italic}
  .textcol p a{color:var(--ink);text-decoration:underline}  /* measured: underline, no hover change */
  .byline{margin-top:20px}

  /* ---- bibliography ---- */
  .biblio{display:grid;grid-template-columns:488px 1fr;gap:20px;padding:120px 12px 0 10px}
  .biblio .display-title{font-size:71px;line-height:62.48px;letter-spacing:-3.57px}
       /* Georgia renders "BIBLIOGRAPHY" 565px wide at the measured 82.6px, overflowing
          the 488px rail (Cardinal Fruit fits 488px); scaled to 71px to fit — stand-in
          compensation, desktop tokens otherwise unchanged */
  .biblio .intro{position:sticky;top:163px;font-family:var(--mono);font-size:12px;line-height:14.4px;
       letter-spacing:-.72px;text-transform:uppercase;margin-top:10px;max-width:293px}
       /* sticky below the stuck heading: measured original intro top 171.66px */
  .bib-list{list-style:none}
  .bib-list li{border-bottom:1px dashed var(--hairline);padding:24px 0} /* measured: 24px 0, 1px dashed rgba(202,200,209,.4) */
  .bib-list li:hover{background:var(--ink);color:#000}                  /* measured hover invert */
  .bib-list li:hover .by{color:#000}
  .bib-list li:hover .kind{color:#000}
  .bib-list .btitle{font-family:var(--serif-display);font-style:italic;font-weight:300;
       font-size:48px;line-height:48px;letter-spacing:-1.44px;padding-right:100px}
       /* measured: 48/48, fw 300, ls -1.44px, italic, padding-right 100px */
  .bib-list .meta{display:flex;justify-content:space-between;align-items:baseline;margin-top:0}
  .bib-list .by{font-family:var(--mono);font-size:12px;line-height:14.4px;
       letter-spacing:-.72px;text-transform:uppercase;color:var(--ink)}
  .bib-list .kind{font-family:var(--mono);font-size:12px;line-height:14.4px;
       letter-spacing:-.72px;text-transform:uppercase;color:var(--ink);
       white-space:nowrap;padding-left:24px}
  footer{position:relative;z-index:1;font-family:var(--mono);font-size:12px;
       letter-spacing:-.03em;text-transform:uppercase;color:var(--ink);
       padding:80px 12px 40px 10px}

  /* 窄屏：原站 390px 下展示标题 72.95px / 61.78px 且无横向溢出；代位字体
     比 Cardinal Fruit 宽约 15–22%，按同比例缩小至 68px / 50px 以消除溢出。
     属字体代位调整，非 token 改动；桌面端保持实测值不变。 */
  @media (max-width:700px){
    .stars{opacity:.3}  /* narrow: keep the bright stars from pressing on the text */
    .hero{height:844px}
    .hero .giant{font-size:150px}
    .hero .hero-img{display:none}
    .hero .tagline{left:170px;top:500px;max-width:200px}
    .hero .credits{left:170px;bottom:36px;white-space:normal}
    .note-section,.biblio{grid-template-columns:minmax(0,1fr);padding-left:12px;padding-right:12px}
    .note-section>*,.biblio>*{min-width:0}
    .display-title{font-size:68px;line-height:59.84px;letter-spacing:-3.4px}
    .biblio .display-title{font-size:50px;line-height:44px;letter-spacing:-2.5px}
    .textcol{padding-top:24px}
    .watch .thumb{width:100%}
    .bib-list .kind{display:none}
    .bib-list .btitle{padding-right:0}
  }
</style>
</head>
<body>
<div class="stars" aria-hidden="true"></div>
<nav class="nav">
  <div class="brand"><span class="logo-pill">Stripe Press</span><span class="nav-div" aria-hidden="true">&#8942;</span><span class="section-name">Tacit</span><span class="nav-div" aria-hidden="true">&#8942;</span></div>
  <div class="nav-links"><div>Watch the films</div><div>Bibliography</div><div>Director&rsquo;s note</div></div>
</nav>
<header class="hero">
  <div class="giant">Tacit</div>
  <div class="tagline">We know more<br>than we can<br>tell</div>
  <div class="credits">A Stripe Press production &middot; Tamara Winter &middot; Edward Allerton &middot; Benjamin Shearn &middot; Julian Cisneros &middot; Jeremy Robinson &middot; Cardboard Spaceship</div>
  <div class="hero-img">Hero still<br>photo placeholder</div>
  <div class="note">Hero scene &mdash; full-viewport canvas animation<br>Asset layer placeholder &middot; not reproduced</div>
</header>
<main>
  <section class="note-section">
    <div class="rail">
      <h1 class="display-title">Director&rsquo;s<br>note</h1>
      <div class="watch">
        <div class="card">
          <div class="thumb">Film still placeholder</div>
          <div class="label">Watch</div>
          <div class="name">Fingal<br>Ferguson</div>
        </div>
        <div class="card">
          <div class="thumb">Film still placeholder</div>
          <div class="label">Watch</div>
          <div class="name">Christophe<br>Laudamiel</div>
        </div>
      </div>
    </div>
    <div class="textcol">
      <blockquote>&ldquo;We know more than we can tell. The craft lives in the hands, not in the manual, and it is passed on by watching, not by reading.&rdquo;</blockquote>
      <div class="attribution">Placeholder quote &mdash; after Polanyi, The Tacit Dimension</div>
      <p>（正文为占位改写文案，4 段，见沙盒 replica.html 全文；原站正文见 sampled.json）</p>
      <p class="byline">Tamara Winter, Commissioning Editor, Stripe Press</p>
    </div>
  </section>
  <section class="biblio">
    <div class="rail">
      <h2 class="display-title">Bibliography</h2>
      <p class="intro">Thanks to everyone who inspired or contributed to this repository of our favorite demonstrations of tacit knowledge in theory and in practice, including Cedric Chin, Parker Conley, Andy Matuschak, Brie Wolfson, Dwarkesh Patel, and Eloise Davenport.</p>
    </div>
    <ul class="bib-list">
      <li><div class="btitle">The Tacit Dimension</div><div class="meta"><span class="by">by Michael Polanyi</span><span class="kind">Book</span></div></li>
      <li><div class="btitle">Personal Knowledge</div><div class="meta"><span class="by">by Michael Polanyi</span><span class="kind">Book</span></div></li>
      <!-- 其余 82 条按原站顺序全量见沙盒 replica.html（书目数据为事实性目录信息，非设计 token） -->
    </ul>
  </section>
</main>
<footer>Stripe Press &mdash; study replica &middot; structure layer only</footer>
</body>
</html>
```
（书目 84 条全量、引文改写占位文案 4 段正文全文，见沙盒 replica.html；本节只收 tokens 与结构骨架。）

---

## 附：已知限制与采样阻塞（资产层差距，不阻断入库）
1. **字体**：Cardinal Fruit / Ivar / Suisse Mono 均为 Stripe 私有字体，未复现；临摹用 Georgia（展示/正文）与 ui-monospace（微标签）代位。Georgia 笔画更粗、字怀更宽，断行位置与原站不同（同栏宽下行数略有出入），但字号/行高/字距/栏宽等结构数值一致。终审评审指出展示字（书目标题、引言、FINGAL FERGUSON、竖排 TACIT）因此"大一档、偏重"，属代位固有差距。
2. **Hero 场景**：全屏 Canvas 动画（拼贴影片画面 + 绿色四角星网格 + 巨型竖排 TACIT）是动态影像资产，无法用 CSS 复现；临摹用灰色占位块 + 星形网格纹理 + 标注框代替。终审评审仍把星形网格标为 P0 级观感问题（星点过大过亮、光晕压字，390px 下最明显）；按"最多 3 轮"规则不再返工，记为已知限制。
3. **照片**：影片卡片剧照、书封/剧照全部用灰色占位块（391×186）代替，未复现任何照片。
4. **WATCH 卡片**：卡片内 "WATCH" 标签与人名字号为估算（20px 斜体标签 + 40px 大写人名），未及实测（采样后期代理故障阻断）；纵向位置亦为估算。
5. **窄屏类型标签**：书目 kind 标签在 390px 下的显隐未实测；临摹暂按 700px 以下隐藏处理，标为待核实。
6. **书目展示标题代位补偿**：临摹桌面端 BIBLIOGRAPHY 为 71px 而非实测 82.6px——Georgia 在 82.6px 下宽 565px、溢出 488px 栏，属代位字体补偿，非 token 改动。
7. **窄屏展示标题代位补偿**：临摹在 ≤700px 媒体查询内把展示标题从实测值（72.95px / 61.78px）缩至 68px / 50px——代位字体比 Cardinal Fruit 宽约 15–22%，不缩会在 390px 下横向溢出。桌面端保持实测值不变；属字体代位调整，非 token 改动。
8. **引文为占位文本**：临摹引文为改写占位文案，非 Polanyi 原文（原站引文见 sampled.json，开头 "I shall reconsider human knowledge by starting from the fact that we can know more than we can tell…"）。
9. **首屏标语与片尾字幕条为文字层代位**：原站两者均为 Canvas 光栅资产（tagline.png / marquee-sprite.png），无 DOM 文本；2026-11-24 原站取回失败（curl 经 miniproxy 返回 000），字幕条全文不可恢复，临摹仅复现可辨认片段；标语位置因巨字列占位右移至 left:270px，属布局代位。
10. **竖排 TACIT 字重 700 为像素推断**：原站无该元素的 CSS 实测值（Canvas 光栅）；700 取自海报字笔画粗重的像素观感，非 computed style。亮字压黑底与原站深字压拼贴的层级相反，记为代位代价。
11. **导航顶部渐晕为转译**：原站 NavContainer 实测渐晕为紫色 rgba(34,1,37,0.6)（海报双色调）；临摹按 #1a1a1a 底色转译为 rgba(26,26,26,0.6)，形状（0deg、76px 高）与实测一致。
12. **引文字重 300 为观感补偿**：实测引文字重为 400；临摹改为 300 仅因 Georgia 400 代位显重，非实测值。
* **采样阻塞**：Playwright `page.screenshot()` 在本站上因等待字体加载超时，改走 CDP `Page.captureScreenshot`；定位器 hover 在动画导航上超时，改走 `mouse.move`；2026-11-24 约 03:41 起 Chromium 经 miniproxy 访问 stripe.press 连续失败（ERR_EMPTY_RESPONSE，curl 亦超时 000），仅阻断 CDP 层叠溯源与 WATCH 卡片补采；designlang v13.4.1 无 `extract` 子命令（正确用法 `designlang <url>`）；monolith 存档时 2 个 woff2 字体请求失败，本地档回退系统字体，采样以原站实测为准。
* **过程文件**：`~/workspace/goals/goal-3/hidden_files/design-learning/patterns-sandbox/2026-11-24-stripe-press-article/`（monolith 档、sampled.json、designlang-out/、orig-*.png、replica.html、compare-*.png、anatomy-draft.md、report-draft.md）
