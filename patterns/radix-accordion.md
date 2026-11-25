# UI 模式学习完成报告

## 1. 基础档案 (Meta)
* **Pattern ID**：`radix-accordion`
* **来源网址**：https://www.radix-ui.com/primitives/docs/components/accordion
* **所属类型**：`[原子组件 / 无头组件库]`
* **风格标签**：`#无头组件` `#折叠` `#微弱层级` `#键盘优先`
* **验证状态**：✅ 已通过沙盒渲染与视觉找茬闭环（共迭代 1 轮，第 1 轮盲审即通过，无需返工；评审未给分项分数，只给了通过结论）
* **沉淀路径**：`patterns/radix-accordion.md`
* **学习日期**：2026-11-24；类别④ 无头与原子组件库（今日 3 个对象中唯一组件类对象）

---

## 2. 审美解剖与克制逻辑 (Anatomy)
* **核心克制点（刻意去掉了什么）**：
  * 去掉了真实边框：分隔线是 1px 底部阴影 + 项间 1px 间隙透出的根背景色，不是 border
  * hover 只压一级背景（#FDFCFD → #FAF9FB），active 无独立样式
  * 触发器与内容区同字号同字重（15px/400），主次只靠颜色与 2.6% 明度差
  * 无手形光标（cursor: auto）；箭头是唯一图形元素
  * 触发器 transition-duration 实测 0s——hover 变色是瞬切，无过渡
* **主次层级手段**：明度三档（触发器 #FDFCFD → 内容区 #FAF9FB → 根背景 #DBD8E0）+ 颜色分工（紫触发器文字 / 灰紫内容文字 / 更深的箭头 #654DC4）+ 动效只动两样（内容高度、箭头旋转，同条缓动同时长）+ 焦点环框住整行
* **适用与禁用边界**：
  * **适用**：FAQ、设置分组、文档章节等低密度折叠
  * **禁用**：需一眼扫读全部内容的场景；项数多（>6）或单项内容很长；需要强烈分区感的场景；品牌色非紫系时须换 accent token

---

## 3. 核心 Design Tokens 与交互差值 (Tokens & Deltas)

| 维度 | 提取规范 / 精确参数 | 关键备注 |
| :--- | :--- | :--- |
| **色彩与层级** | 触发器 #FDFCFD / 内容区 #FAF9FB（低 2.6% 明度）/ 根背景 #DBD8E0（低 28.7%，只在 1px 缝里露出来当分隔线）；触发器文字 #6550B9（对比 6.03:1）、内容文字 #65636D（5.62:1）、箭头 #654DC4（6.01:1） | 中性色占比 >95%，强调色只在箭头与触发器文字上 |
| **排版比例** | 触发器 15px/15px/w400 · 内容 15px/22.5px/w400 · 字距 normal | 触发器与内容同字号同字重，不靠字重拉层级 |
| **间距与圆角** | 根 300px 固定宽、圆角 6px、阴影 `0 2px 10px rgba(0,0,0,0.2)`；触发器高 45px、左右 padding 各 20px；内容内层 padding 15px 20px；首项上圆角 4px、末项下圆角 4px、中间 0 | 根 6px vs 首末项 4px，间隙 0，非严格同心圆角 |
| **Hover 差值** | 背景 #FDFCFD → #FAF9FB（与内容区同色）；字色/阴影/箭头全不动；transition-duration 0s 瞬切 | 缓动：无（瞬切） |
| **展开差值** | 内容高 75px → 0，项总高 120px → 45px；箭头 180° → 0°（120ms 时约 43°）；内容 `slideDown/slideUp 0.3s cubic-bezier(0.87, 0, 0.13, 1)`，箭头 `transform 0.3s` 同款缓动 | 高度走 `@keyframes height: 0 ↔ var(--radix-accordion-content-height)` |
| **Focus/边缘态** | 触发器自身 outline: none；所属项 `:focus-within` 得 `0 0 0 2px #211F26` + relative/z-index 1；Tab 到触发器时 `:focus-visible` 为 true；鼠标点按后环也出现（:focus-within 不区分鼠标/键盘） | 390px 窄屏全部 token 不变，300px 固定宽无横向溢出；disabled 演示中无此项，未采到；深色模拟下演示容器强制浅色，token 不变 |

---

## 4. 沙盒临摹与视觉纠偏记录 (Critic Loop)
* **Round 1 发现的失真问题**：
  1. `[内容区行数]`：初稿占位答案太长，内容区 3 行、根高 233px（原站 212px）→ **已修正为** 三条答案各缩到 27 字以内，内容区回到 2 行、根高 210px
  2. `[项间间隙]`：初稿漏了项间 1px 间隙（触发器 rect y 值 368/489/535，项高 120+45+45=210，根高 212，差 2px）→ **已修正为** 补 `.acc-item + .acc-item { margin-top: 1px }`，根高回到 212px，与原站逐像素对齐
* **最终视觉复核结论**：第 1 轮盲审即通过——默认态与展开态的分隔线位置与颜色、触发器/内容区行高、箭头方向、背景明度三档均与原站一致；展开态对照图的原站一侧带考古伪影（见已知限制），评审只对照了触发器背景、箭头方向与整体结构
* **临摹实测复核**（replica-measured.json）：根 300×212px 与原站完全一致；触发器 15px/15px/400、padding 0 20px、背景 #FDFCFD、分隔阴影 `0 1px 0 #DBD8E0`、焦点环 `0 0 0 2px #211F26` 全部复现；临摹自有 JS 下点击/Tab/Enter 全部跑通

---

## 5. 提取的视觉禁忌 (Anti-Patterns)
* ❌ 严禁手风琴分隔线用真实 border：1px 阴影 + 间隙透底色的"线"比 border 更轻；border 一旦上色就显廉价、显重
* ❌ 严禁触发器与内容拉字号/字重差：同字号同字重、只靠颜色与 2% 级明度差分层；一加粗触发器，整个组件立刻变"菜单"
* ❌ 严禁焦点环只圈按钮：键盘焦点看不见等于没有，描边要框住整行才够用
* ❌ 严禁 hover 加阴影/变色/位移三件套：只压一级背景就够了，多一样都显闹

---

## 6. 已验证的纯净代码 (Verified Code)
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>radix-accordion — 沙盒临摹</title>
<style>
  /* Tokens 实测自 Radix Primitives Accordion 文档演示（2026-11-24，浅色态） */
  :root {
    --trigger-bg: #FDFCFD;
    --trigger-bg-hover: #FAF9FB; /* hover 与内容区同色 */
    --trigger-text: #6550B9;
    --content-bg: #FAF9FB;
    --content-text: #65636D;
    --separator: #DBD8E0;        /* 根背景即分隔线色 */
    --chevron: #654DC4;
    --focus-ring: #211F26;       /* :focus-within 项级描边 */
    --ease: cubic-bezier(0.87, 0, 0.13, 1);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    background: #fff;
    min-height: 100vh;
    display: flex; align-items: center; justify-content: center;
  }
  /* 根：300px 固定宽，圆角 6px，外层柔和阴影；分隔线色即根背景 */
  .acc {
    width: 300px;
    background: var(--separator);
    border-radius: 6px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    overflow: hidden; /* 原站根 overflow:visible、首末项圆角 4px；此处收 clip 以保视觉一致，见已知限制 */
  }
  .acc-item { overflow: hidden; }
  .acc-item + .acc-item { margin-top: 1px; } /* 实测：项与项之间有 1px 间隙，透出根背景色形成分隔线 */
  .acc-item:first-child { border-radius: 4px 4px 0 0; }
  .acc-item:last-child { border-radius: 0 0 4px 4px; }
  /* 焦点态：描边画在整项上（:focus-within），不是触发器 outline */
  .acc-item:focus-within { position: relative; z-index: 1; box-shadow: 0 0 0 2px var(--focus-ring); }
  .acc-header { display: flex; }
  /* 触发器：45px 高，flex 两端对齐，左右各 20px；字 15px/行高 15px、字重 400；
     无真实边框，分隔线是 1px 底部阴影；hover 只变背景；cursor 保持默认 */
  .acc-trigger {
    flex: 1;
    display: flex; align-items: center; justify-content: space-between;
    height: 45px;
    padding: 0 20px;
    font-size: 15px; line-height: 15px; font-weight: 400; letter-spacing: normal;
    font-family: inherit;
    color: var(--trigger-text);
    background: var(--trigger-bg);
    border: 0;
    box-shadow: 0 1px 0 var(--separator);
    cursor: default;
    text-align: left;
  }
  .acc-trigger:hover { background: var(--trigger-bg-hover); }
  .acc-trigger:focus { outline: none; } /* 焦点由项级描边承担 */
  .acc-chevron {
    width: 15px; height: 15px;
    flex: none;
    color: var(--chevron);
    transition: transform 0.3s var(--ease);
  }
  .acc-item[data-state="open"] .acc-chevron { transform: rotate(180deg); }
  /* 内容区：原站用 @keyframes height: 0 ↔ var(--radix-accordion-content-height)，
     此处用 grid-rows 0fr/1fr 等效实现（同 300ms、同缓动、同视觉） */
  .acc-content {
    display: grid;
    grid-template-rows: 0fr;
    background: var(--content-bg);
    overflow: hidden;
    transition: grid-template-rows 0.3s var(--ease);
  }
  .acc-item[data-state="open"] .acc-content { grid-template-rows: 1fr; }
  .acc-content-inner { overflow: hidden; }
  .acc-content-text {
    padding: 15px 20px;
    font-size: 15px; line-height: 22.5px; font-weight: 400;
    color: var(--content-text);
  }
</style>
</head>
<body>
  <div class="acc" id="acc">
    <div class="acc-item" data-state="open">
      <h3 class="acc-header">
        <button class="acc-trigger" aria-expanded="true" aria-controls="panel-1" id="trigger-1">
          <span>占位问题一：它支持键盘操作吗？</span>
          <svg class="acc-chevron" viewBox="0 0 15 15" fill="none" aria-hidden="true">
            <path d="M3.13523 6.15803C3.3241 5.95657 3.64052 5.94637 3.84197 6.13523L7.5 9.56464L11.158 6.13523C11.3595 5.94637 11.6759 5.95657 11.8648 6.15803C12.0536 6.35949 12.0434 6.67591 11.842 6.86477L7.84197 10.6148C7.64964 10.7951 7.35036 10.7951 7.15803 10.6148L3.15803 6.86477C2.95657 6.67591 2.94637 6.35949 3.13523 6.15803Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path>
          </svg>
        </button>
      </h3>
      <div class="acc-content" id="panel-1" role="region" aria-labelledby="trigger-1">
        <div class="acc-content-inner"><p class="acc-content-text">占位答案：支持。方向键移动焦点，回车或空格展开收起。</p></div>
      </div>
    </div>
    <div class="acc-item" data-state="closed">
      <h3 class="acc-header">
        <button class="acc-trigger" aria-expanded="false" aria-controls="panel-2" id="trigger-2">
          <span>占位问题二：它自带样式吗？</span>
          <svg class="acc-chevron" viewBox="0 0 15 15" fill="none" aria-hidden="true">
            <path d="M3.13523 6.15803C3.3241 5.95657 3.64052 5.94637 3.84197 6.13523L7.5 9.56464L11.158 6.13523C11.3595 5.94637 11.6759 5.95657 11.8648 6.15803C12.0536 6.35949 12.0434 6.67591 11.842 6.86477L7.84197 10.6148C7.64964 10.7951 7.35036 10.7951 7.15803 10.6148L3.15803 6.86477C2.95657 6.67591 2.94637 6.35949 3.13523 6.15803Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path>
          </svg>
        </button>
      </h3>
      <div class="acc-content" id="panel-2" role="region" aria-labelledby="trigger-2">
        <div class="acc-content-inner"><p class="acc-content-text">占位答案：不带。只给行为与状态机，外观全由使用者定。</p></div>
      </div>
    </div>
    <div class="acc-item" data-state="closed">
      <h3 class="acc-header">
        <button class="acc-trigger" aria-expanded="false" aria-controls="panel-3" id="trigger-3">
          <span>占位问题三：它能做展开动画吗？</span>
          <svg class="acc-chevron" viewBox="0 0 15 15" fill="none" aria-hidden="true">
            <path d="M3.13523 6.15803C3.3241 5.95657 3.64052 5.94637 3.84197 6.13523L7.5 9.56464L11.158 6.13523C11.3595 5.94637 11.6759 5.95657 11.8648 6.15803C12.0536 6.35949 12.0434 6.67591 11.842 6.86477L7.84197 10.6148C7.64964 10.7951 7.35036 10.7951 7.15803 10.6148L3.15803 6.86477C2.95657 6.67591 2.94637 6.35949 3.13523 6.15803Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path>
          </svg>
        </button>
      </h3>
      <div class="acc-content" id="panel-3" role="region" aria-labelledby="trigger-3">
        <div class="acc-content-inner"><p class="acc-content-text">占位答案：能。高度动画 300 毫秒，箭头同步旋转 180 度。</p></div>
      </div>
    </div>
  </div>
<script>
  // single 模式：一次只展开一项；回车/空格由原生 button 触发 click；上下方向键移动焦点
  const items = [...document.querySelectorAll('.acc-item')];
  const triggers = [...document.querySelectorAll('.acc-trigger')];
  function setOpen(idx) {
    items.forEach((item, i) => {
      const open = i === idx;
      item.setAttribute('data-state', open ? 'open' : 'closed');
      triggers[i].setAttribute('aria-expanded', String(open));
    });
  }
  triggers.forEach((btn, i) => {
    btn.addEventListener('click', () => {
      const isOpen = items[i].getAttribute('data-state') === 'open';
      if (isOpen) { items[i].setAttribute('data-state', 'closed'); btn.setAttribute('aria-expanded', 'false'); }
      else setOpen(i);
    });
    btn.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowDown') { e.preventDefault(); triggers[(i + 1) % triggers.length].focus(); }
      if (e.key === 'ArrowUp') { e.preventDefault(); triggers[(i - 1 + triggers.length) % triggers.length].focus(); }
    });
  });
</script>
</body>
</html>
```

---

## 附：已知限制与采样限制（资产层差距，不阻断入库）
* **字体**：原站用 untitledSans（Radix 专有字体），临摹用系统字体栈；字形有细微差别，不影响结构与观感判定。
* **展开动画机制**：原站用 `@keyframes height: 0 ↔ var(--radix-accordion-content-height)`（依赖 JS 写入该变量）；临摹用 `grid-template-rows: 0fr → 1fr` 等效实现，同为 300ms、同条缓动，视觉一致，机制不同。
* **根圆角**：原站根 `overflow: visible`、首项圆角 4px < 根 6px（非严格同心）；临摹根用 `overflow: hidden` 收 clip 以保视觉一致，是一处有意的小偏差。
* **compare-open.png 的原站一侧带考古伪影**：强制改 `data-state` 后，首项内容因子节点未卸载仍可见（React 未运行所致），不是真实展开态；该图的可用对照点只有箭头方向与触发器背景，内容位置以默认态对照图为准。
* **采样限制**：页面 JS 未水合——线上页与本地 monolith 档两次加载中，真实鼠标点击与主题切换均无反应（`data-state` 不变；多个预加载请求 `ERR_ABORTED`）。展开/收起、Enter/Space、快速连续切换的真实交互未经验证，改走 DOM 状态考古（手动改 `data-state` 后采样 + 截图对照 + 关键帧规则原文核对）。考古验证成立的：箭头旋转两端态与中途角度、hover/active 背景、焦点环、内容区静态 token。考古不能验证的：收起项内容子节点是否卸载、真实点击时序、disabled 态。
* **过程文件**：`~/workspace/goals/goal-3/hidden_files/design-learning/patterns-sandbox/2026-11-24-radix-accordion/`（monolith 档、sampled.json、orig-*.png、replica.html、compare-*.png、anatomy-draft.md、report-draft.md）
