# UI 模式学习完成报告

## 1. 基础档案 (Meta)
* **Pattern ID**：`shadcn-command`
* **来源网址**：https://ui.shadcn.com/docs/components/command （实测重定向至 /docs/components/base/command）
* **所属类型**：`高密度 Web App`（命令面板／交互与状态系统）
* **风格标签**：`#命令面板` `#微弱层级` `#键盘优先` `#零阴影`
* **验证状态**：✅ 已通过沙盒渲染与视觉找茬闭环（共迭代 `2` 轮）
* **沉淀路径**：`patterns/shadcn-command.md`

---

## 2. 审美解剖与克制逻辑 (Anatomy)
* **核心克制点（刻意去掉了什么）**：
  * 去掉了卡片阴影：整个命令面板 box-shadow 全为 none，层级只靠 1px 淡边框（明度 90.95）与 3.5% 的选中底明度差成立。
  * 去掉了焦点环：input 用 outline-hidden，鼠标点击与键盘 Tab 回到输入框时，输入框、外层容器、搜索框外包三处均无 outline、无 box-shadow、无边框色变化——焦点完全不画出来（与本契约「焦点必须可见」相冲突，只记录、不学，见第 5 节）。
  * 去掉了选中态的一切装饰：选中行只换底色（白 → 明度 96.52，差 3.5%），字重、字色、描边、位移全部不变；且切换是瞬时的，transition-duration 为 0s。注意：选中底与白底对比仅约 1.05:1，远低于契约「选中与周围 ≥3:1」——在弱光屏上几乎看不见选中行，这是本对象的第二处缺陷，只记录、不学。
  * 去掉了空态的弱化处理：空态文字 "No results found." 与正文同字号同字色（14px 纯黑），不降灰、不加图标，只靠居中＋24px 上下 padding 表达。
* **主次层级手段**：`字重 500 vs 400 ＋ 灰度（正文纯黑 vs 辅助 Lab 明度 48.5）＋ 3.5% 选中底明度差`；字号跨度极小（12 / 14 / 15px 三档，层级比仅 1.25），层级几乎全靠灰度与字重，不靠字号。
* **适用与禁用边界**：
  * **适用**：键盘优先的生产力工具（命令面板、搜索框、选择器）；需要高密度但不想吵的列表；浅色界面。
  * **禁用**：焦点可达性有硬要求的场景（本对象无可见焦点环，直接套用会违反无障碍基线）；需要一眼区分选中与未选中的低注意力场景（3.5% 明度差在弱光屏上几乎看不见）；深色模式未经验证（本次只采浅色）。

---

## 3. 核心 Design Tokens 与交互差值 (Tokens & Deltas)

| 维度 | 提取规范 / 精确参数 | 关键备注 |
| :--- | :--- | :--- |
| **色彩与层级** | 容器底 Lab 明度 100（纯白）／边框与分隔线 Lab 明度 90.95（约 #E5E5E5）／选中底 Lab 明度 96.52（约 #F5F5F5）／辅助文字 Lab 明度 48.5（约 #737373） | 白底到选中底只差 3.5% 明度；边框到白底差 9%；中性色占比 100%（零强调色） |
| **排版比例** | 分组标题 12px/16px/w500 · 条目与输入 14px/20px/w400 · 容器继承 15px/26.25px | 层级比 12:14:15 ≈ 0.86:0.93:1，极小跨度；字距默认 normal，仅快捷键字距 1.2px（0.1em） |
| **间距与圆角** | 步进 4px：容器 padding 4px、分组 padding 4px、条目 padding 6px 8px、gap 8px；外圆角 14px / 搜索框外包 10px / 条目 6px | 同心圆角成立：14 = 10 + 4（容器−搜索框）；条目 6 + 分组 4 + 容器 4 = 14 |
| **Hover 差值** | 悬停即选中：背景 透明 → 明度 96.52，字色/字重/描边/位移全不变，transition 0s（瞬时） | hover 与键盘选中是同一状态（data-selected），不是两套样式 |
| **键盘差值** | ArrowDown 沿启用项移动，跳过 disabled 项；选中行同上（仅底色变）；无 outline、无 box-shadow | 实测：Search Emoji 选中时按 ArrowDown → 跳过 disabled 的 Calculator → Profile；再按 → Billing |
| **Focus/边缘态** | 鼠标点击与键盘 Tab 均无可见焦点环（input/外包/容器三处 outline-style 均为 none，box-shadow none）；disabled 项 opacity 0.5，其余与正常项完全一致 | 禁用态做法与契约「禁用态统一降至 50%」逐字互证；焦点缺失与契约「焦点必须可见」冲突 |
| **空态** | "No results found." 14px/20px/w400 纯黑，居中，上下 padding 各 24px，高 68px；不匹配的条目从 DOM 卸载（非 display:none）；列表 265px → 68px，但外框因 size-full 撑满父容器保持 312px 不变 | 空态不降灰、不缩小，是「少即是多」的空态做法；外框高度由父容器定，组件自身不锁高 |

---

## 4. 沙盒临摹与视觉纠偏记录 (Critic Loop)
* **Round 1 发现的失真问题**：
  1. `顶部内边距偏窄`：临摹缺了原版搜索框外包那层 `p-1 pb-0` 的 4px 顶垫，全部内容行相对面板顶缘上移约 4px → **已修正为** 补一层 `padding: 4px 4px 0` 的外包 div，复核搜索框顶距 9px、宽 366px、列表高 265px，与原版实测逐项对上。
* **Round 2 复核结论**：新开无上下文盲审复核，四项全过（设计 9.5／可用性 10／创意 9／内容 10），像素平均差异默认态 2.35/255、空态 0.95/255，无 P0。唯一 nit：临摹字形略偏轻——原站 Geist/Inter vs 临摹 system-ui 回退字体所致，评审明言不构成返工理由；字体族本次未采样，留作已知偏差，不追。
* **最终视觉复核结论**：默认态与空态在边框、间距、字重、阴影四项上与原版一致，呼吸感与克制气质对齐；两轮收敛。

---

## 5. 提取的视觉禁忌 (Anti-Patterns)
* ❌ 严禁 选中态同时改底色＋字重＋描边——选中只换底色一个维度就够，叠加等于在喊。
* ❌ 严禁 命令面板加阴影——本对象证明 1px 淡边框＋3.5% 明度差已足够撑起浮层层级，阴影一加就廉价。
* ❌ 严禁 抄这套焦点做法——输入框无可见焦点环是本对象的真实缺陷（键盘用户找不到焦点），只学其克制、不学其缺失；自家落地必须补焦点环。
* ❌ 严禁 空态降灰＋缩小＋加图标三件套——空态与正文同字同色、只靠居中与留白就够。

---

## 6. 已验证的纯净代码 (Verified Code)
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Command menu — sandbox replica (tokens measured from shadcn/ui Command docs, light mode, 2026-11-22)</title>
<style>
  /* Tokens below are measured computed values from the live docs demo.
     White bg / border Lab L 90.95 (#e5e5e5) / selected bg Lab L 96.52 (#f5f5f5)
     / muted text Lab L 48.5 (#737373). No shadows anywhere. Radii concentric: 14 / 10 / 6. */
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: #fff; font-family: system-ui, -apple-system, "Segoe UI", sans-serif;
         display: flex; justify-content: center; padding: 40px 0; }
  .cmd { width: 384px; background: #fff; border: 1px solid #e5e5e5; border-radius: 14px;
         padding: 4px; font-size: 15px; line-height: 26.25px; height: 312px; /* measured demo frame height:
         the docs demo root is size-full, so its 312px height comes from the preview parent, not content;
         the list inside collapses 265px -> 68px in the empty state while the frame stays 312px */ }
  .cmd-input-outer { padding: 4px 4px 0; } /* 原版 p-1 pb-0 外包：盲审 Round 1 指出临摹顶部紧了 4px，根因即缺了这层 4px */
  .cmd-input-wrap { display: flex; align-items: center; height: 32px;
         background: rgba(229,229,229,.3); border: 1px solid rgba(229,229,229,.3);
         border-radius: 10px; padding: 0 8px;
         transition: color .15s cubic-bezier(.4,0,.2,1), background-color .15s cubic-bezier(.4,0,.2,1),
                     border-color .15s cubic-bezier(.4,0,.2,1); }
  .cmd-input-wrap svg { width: 16px; height: 16px; flex-shrink: 0; color: #737373; opacity: .5; }
  .cmd-input { flex: 1; height: 20px; border: 0; outline: none; background: transparent;
         font: inherit; font-size: 14px; line-height: 20px; font-weight: 400; color: #000;
         padding-left: 6px; min-width: 0; }
  .cmd-input::placeholder { color: rgba(0,0,0,.5); }
  .cmd-list { max-height: 288px; overflow-y: auto; overflow-x: hidden; outline: none; scrollbar-width: none; }
  .cmd-list::-webkit-scrollbar { display: none; }
  .cmd-group { padding: 4px; overflow: hidden; }
  .cmd-heading { padding: 6px 8px; font-size: 12px; line-height: 16px; font-weight: 500; color: #737373; }
  .cmd-item { display: flex; align-items: center; gap: 8px; height: 32px; padding: 6px 8px;
         border-radius: 6px; font-size: 14px; line-height: 20px; font-weight: 400; color: #000;
         cursor: default; user-select: none; }
  .cmd-item svg { width: 16px; height: 16px; flex-shrink: 0; }
  .cmd-item[data-selected="true"] { background: #f5f5f5; }
  .cmd-item[data-disabled="true"] { opacity: .5; }
  .cmd-shortcut { margin-left: auto; font-size: 12px; line-height: 16px; font-weight: 400;
         letter-spacing: 1.2px; color: #737373; }
  .cmd-separator { height: 1px; background: #e5e5e5; margin: 0 -4px; }
  .cmd-empty { display: none; padding: 24px 0; text-align: center; font-size: 14px;
         line-height: 20px; font-weight: 400; color: #000; }
  .cmd-empty.show { display: block; }
  .hidden { display: none !important; }
</style>
</head>
<body>
<div class="cmd" id="cmd">
  <div class="cmd-input-outer">
  <div class="cmd-input-wrap">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>
    <input class="cmd-input" id="cmdInput" type="text" placeholder="Type a command or search..." autocomplete="off" spellcheck="false">
  </div>
  </div>
  <div class="cmd-list" id="cmdList">
    <div class="cmd-empty" id="cmdEmpty">No results found.</div>
    <div class="cmd-group" data-group>
      <div class="cmd-heading">Suggestions</div>
      <div class="cmd-item" data-value="calendar" data-selected="true">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="18" x="3" y="4" rx="2"/><path d="M16 2v4"/><path d="M8 2v4"/><path d="M3 10h18"/></svg>
        <span>Calendar</span>
      </div>
      <div class="cmd-item" data-value="search emoji">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M8 14s1.5 2 4 2 4-2 4-2"/><line x1="9" x2="9.01" y1="9" y2="9"/><line x1="15" x2="15.01" y1="9" y2="9"/></svg>
        <span>Search Emoji</span>
      </div>
      <div class="cmd-item" data-value="calculator" data-disabled="true">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="16" height="20" x="4" y="2" rx="2"/><line x1="8" x2="16" y1="6" y2="6"/><line x1="16" x2="16" y1="14" y2="18"/><path d="M16 10h.01"/><path d="M12 10h.01"/><path d="M8 10h.01"/><path d="M12 14h.01"/><path d="M8 14h.01"/><path d="M12 18h.01"/><path d="M8 18h.01"/></svg>
        <span>Calculator</span>
      </div>
    </div>
    <div class="cmd-separator" data-sep></div>
    <div class="cmd-group" data-group>
      <div class="cmd-heading">Settings</div>
      <div class="cmd-item" data-value="profile">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
        <span>Profile</span><span class="cmd-shortcut">⌘P</span>
      </div>
      <div class="cmd-item" data-value="billing">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="20" height="14" x="2" y="5" rx="2"/><line x1="2" x2="22" y1="10" y2="10"/></svg>
        <span>Billing</span><span class="cmd-shortcut">⌘B</span>
      </div>
      <div class="cmd-item" data-value="settings">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"/><circle cx="12" cy="12" r="3"/></svg>
        <span>Settings</span><span class="cmd-shortcut">⌘S</span>
      </div>
    </div>
  </div>
</div>
<script>
(function () {
  const input = document.getElementById('cmdInput');
  const empty = document.getElementById('cmdEmpty');
  const items = [...document.querySelectorAll('.cmd-item')];
  const groups = [...document.querySelectorAll('[data-group]')];
  const sep = document.querySelector('[data-sep]');
  const enabled = () => items.filter(i => !i.hidden && i.dataset.disabled !== 'true');
  function select(el) {
    items.forEach(i => i.dataset.selected = 'false');
    if (el) el.dataset.selected = 'true';
  }
  items.forEach(i => i.addEventListener('mousemove', () => {
    if (i.dataset.disabled === 'true') return;
    select(i);
  }));
  input.addEventListener('keydown', e => {
    if (e.key !== 'ArrowDown' && e.key !== 'ArrowUp') return;
    e.preventDefault();
    const list = enabled();
    if (!list.length) return;
    const cur = list.findIndex(i => i.dataset.selected === 'true');
    const next = e.key === 'ArrowDown'
      ? list[(cur + 1) % list.length]
      : list[(cur - 1 + list.length) % list.length];
    select(next);
  });
  input.addEventListener('input', () => {
    const q = input.value.trim().toLowerCase();
    let visible = 0;
    items.forEach(i => {
      const hit = !q || (i.dataset.value || '').includes(q);
      i.hidden = !hit;
      if (hit) visible++;
    });
    groups.forEach(g => {
      g.classList.toggle('hidden', ![...g.querySelectorAll('.cmd-item')].some(i => !i.hidden));
    });
    const anyGroup = groups.some(g => !g.classList.contains('hidden'));
    sep.classList.toggle('hidden', !anyGroup || groups.filter(g => !g.classList.contains('hidden')).length < 2);
    empty.classList.toggle('show', visible === 0);
    if (visible > 0) {
      const first = enabled()[0];
      if (first) select(first);
    } else select(null);
  });
})();
</script>
</body>
</html>
```

---

### 采样说明（诚实边界）
- 全部数值为 2026-11-22 对 ui.shadcn.com 实时页的 getComputedStyle() 实测（浅色模式，1440×900 视口，首个 Base UI 演示）。
- 本机 Playwright 直连出口代理三次隧道失败，后经本地转发代理（给 Chromium 预置 Proxy-Authorization）打通；live browser 任务无 JS 求值入口，只复用了它的交互观察（默认选中 Calendar、ArrowDown 跳 disabled 项）作交叉验证，两处结论一致。
- 首轮采样曾撞上未水合（hydration）状态：空态元素可见、交互无响应；以 `[cmdk-item][data-selected="true"]` 出现为水合门，重新采样后数据才有效。未水合那轮的截图与数值已废弃，不引用。
- 字体族未采样（原站用 Geist Sans，临摹用 system-ui 栈，视觉接近但非同一字体）；深色模式未采。
