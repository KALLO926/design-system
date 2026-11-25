# UI 模式学习完成报告

## 1. 基础档案 (Meta)
* **Pattern ID**：`base-ui-switch`
* **来源网址**：https://base-ui.com/react/components/switch
* **所属类型**：`原子组件`
* **风格标签**：`#极简` `#黑白反转` `#方形开关` `#无头组件` `#工程文档气质`
* **验证状态**：✅ 已通过沙盒渲染与视觉找茬闭环（共迭代 1 轮）
* **沉淀路径**：`patterns/base-ui-switch.md`

> 注：学的是文档站演示样式，Base UI 组件本身无头不带样式。方形是文档站的选择，不是组件默认主题。同页 Tailwind 写法（h-5 w-9、p-0.5、data-checked:bg-neutral-950、focus-visible:outline-2/offset-2）与 CSS Modules 版几何与焦点数值完全一致，可互证。

---

## 2. 审美解剖与克制逻辑 (Anatomy)
* **核心克制点（刻意去掉了什么）**：
* 去掉了圆角——轨道与滑块圆角均为 0，方形开关，市面开关几乎全是胶囊
* 去掉了阴影——轨道与滑块 `box-shadow: none`
* 去掉了 hover / active / disabled 的一切视觉反馈（实测零差值，CSS 模块中无 `:hover` / `:active` 规则，演示样式未给 disabled 任何处理）
* 去掉了 `cursor: pointer`，光标为 `default`
* 去掉了可见标签——演示开关本身无可见标签文字，仅 `aria-label`
* 去掉了色彩——只有黑白反转；状态切换只靠轨道背景＋滑块位置/颜色两个维度，过渡只有 0.15s ease 一种
* **主次层级手段**：开态本身就是强调——整轨翻黑、滑块翻白，全页唯一的实心黑块；关态刻意退到只剩 1px 边框（与底色 surface delta 为 0），不抢注意力；焦点环是唯一的描边，2px＋2px 外扩，与轨道同墨
* **适用与禁用边界**：
* **适用**：极简／工程文档气质、密集设置列表里「开关不能喧宾夺主」的场景
* **禁用**：需要一眼可发现操作点的消费级产品（无 hover、无 pointer、方形易被误读为复选框）；无障碍只靠焦点环兜底，弱视场景需额外处理；暗色模式成立只因黑白反转是对称的，换成彩色强调色后需重验

---

## 3. 核心 Design Tokens 与交互差值 (Tokens & Deltas)

| 维度 | 提取规范 / 精确参数 | 关键备注 |
|:--- |:--- |:--- |
| **色彩与层级** | 墨色 `oklch(14.5% 0 0)`，canvas 实测 sRGB `#0A0A0A`（10, 10, 10）；关态轨道白底、开态轨道 `#0A0A0A`；滑块关态 `#0A0A0A`、开态白；页面底色白，文字 rgb(46, 46, 46) | 关态轨道与页面底色 surface delta 为 0，只靠 1px 边框与页面区分；学的是文档站演示样式，Base UI 组件本身无头不带样式 |
| **排版比例** | 演示开关本身无可见标签文字，仅 `aria-label`；页面正文实测 14px/20px/400 | 无标签是克制点之一，未实测开关专属字阶 |
| **间距与圆角** | 轨道 36×20px，1px 实线边框，padding 2px；滑块 14×14px；轨道与滑块圆角均为 0，阴影 none；滑块关态左偏移 3px（1px 边框＋2px padding）、开态 19px，行程 16px，走 CSS `translate` 属性（`transform` 恒为 `none`），上下偏移恒为 3px | 同心圆角：轨道与滑块圆角均为 0，天然一致；这里的刻意选择是方形，不是胶囊 |
| **Hover 差值** | 零差值（开态实测＋关态克隆实测；CSS 模块无相关规则） | 无位移、无变色、无透明度变化 |
| **Active 差值** | 零差值（开态 mousedown 实测；无 `:active` 规则） | 同上 |
| **开 ↔ 关差值** | 轨道背景 白 ↔ `#0A0A0A`；滑块背景 `#0A0A0A` ↔ 白；滑块 `translate` 0 ↔ 16px；只动这两个维度 | 轨道过渡 `background-color 0.15s ease`；滑块过渡 `translate 0.15s ease, background-color 0.15s ease` |
| **Focus/边缘态** | focus-visible（键盘实测，`:focus-visible` 命中）：`outline: 2px solid #0A0A0A`，`outline-offset: 2px`（暗色下描边变白）；disabled（克隆实测）：零视觉差值，`opacity` 仍为 1，颜色不变；暗色模式（`prefers-color-scheme: dark` 仿真实测）：严格黑白反转——开态轨道白底＋白边框、滑块 `#0A0A0A`，关态轨道 `#0A0A0A` 底＋白边框、滑块白 | 光标 `default`；disabled 态演示样式未给任何视觉处理；暗色值走仿真实测，非原生切换采样 |

---

## 4. 沙盒临摹与视觉纠偏记录 (Critic Loop)
* **Round 1 发现的失真问题**：
1. 无返工——无上下文盲审结论：，四项挑刺（边框是否偏重显廉价、padding 是否偏窄、字重字距是否偏粗偏挤、阴影高光是否过猛）均未命中，初稿直接通过。
2. 唯一观察：对比拼图中有 1px 截图对齐抖动，属截图裁切/抗锯齿层面，肉眼不可见，不建议改，未作修改。
* **最终视觉复核结论**：临摹与原版在方形几何、黑白反转、克制气质上对齐；开/关两态与 focus-visible 态并排对照均通过，结构层可入库。

过程事实：monolith 本地档未水合（file:// 与 HTTP 服务下点击均不能切换，`aria-checked` 恒为 true），关态/disabled/暗色值改走 CSS 模块考古＋`data-checked` 克隆实测＋`prefers-color-scheme` 仿真验证，并用截图对照；页内仅 1 个 live 开关（hero 演示），无第二种 live 变体可对照。

---

## 5. 提取的视觉禁忌 (Anti-Patterns)
* ❌ 严禁把方形开关默认改成胶囊——方形是本对象刻意选择的身份，改成胶囊即丢掉其全部克制逻辑
* ❌ 严禁给无 hover 反馈的开关加 hover 变色——原版 hover/active 零差值是设计决定，不是遗漏；自行加 hover 会破坏「开关不能喧宾夺主」的退让
* ❌ 严禁给 disabled 开关只降透明度了事而不补其他区分——原版 disabled 本就零视觉差值（演示样式未处理），照搬时若不补处理，禁用态与可用态将无法区分；要么明确补上禁用处理，要么不在需要区分禁用态的场景用此样式

---

## 6. 已验证的纯净代码 (Verified Code)
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Base UI Switch — clean replica (measured tokens)</title>
<style>
/* Tokens measured from the Base UI docs demo switch (light mode).
Track: 36x20, 1px solid near-black, 2px padding, radius 0, shadow none.
Thumb: 14x14, translate 16px when checked. Transitions 0.15s ease. */
:root { --ink: oklch(14.5% 0 0);}
* { box-sizing: border-box;}
body { margin: 0; background: #fff; color: #2e2e2e;
font: 14px/1.5 system-ui, sans-serif;}
.stage { display: flex; gap: 48px; align-items: center;
padding: 60px;}
figure { margin: 0; text-align: center;}
figcaption { margin-top: 12px; font-size: 12px; color: #666;}

.switch {
display: flex; flex-shrink: 0;
width: 36px; height: 20px; padding: 2px;
border: 1px solid var(--ink);
border-radius: 0; box-shadow: none;
background: #fff; cursor: default;
transition: background-color 0.15s ease;
}
.switch[data-checked] { background: var(--ink);}
.switch:focus-visible {
outline: 2px solid var(--ink); outline-offset: 2px;
}
.switch .thumb {
width: 14px; height: 14px; border-radius: 0;
background: var(--ink);
transition: translate 0.15s ease, background-color 0.15s ease;
}
.switch[data-checked] .thumb { translate: 16px 0; background: #fff;}

@media (prefers-color-scheme: dark) {
body { background: var(--ink); color: #fff;}
figcaption { color: #aaa;}
.switch { border-color: #fff; background: var(--ink);}
.switch[data-checked] { background: #fff;}
.switch:focus-visible { outline-color: #fff;}
.switch .thumb { background: #fff;}
.switch[data-checked] .thumb { background: var(--ink);}
}
</style>
</head>
<body>
<div class="stage">
<figure>
<button type="button" class="switch" id="sw-checked" role="switch"
aria-checked="true" data-checked aria-label="Notifications">
<span class="thumb"></span>
</button>
<figcaption>checked</figcaption>
</figure>
<figure>
<button type="button" class="switch" id="sw-unchecked" role="switch"
aria-checked="false" aria-label="Notifications">
<span class="thumb"></span>
</button>
<figcaption>unchecked</figcaption>
</figure>
<figure>
<button type="button" class="switch" id="sw-disabled" role="switch"
aria-checked="true" data-checked disabled data-disabled aria-label="Notifications">
<span class="thumb"></span>
</button>
<figcaption>disabled (checked)</figcaption>
</figure>
</div>
<script>
// Minimal behaviour only: toggle data-checked / aria-checked on click.
document.querySelectorAll('.switch:not([disabled])').forEach(function (sw) {
sw.addEventListener('click', function () {
var on = sw.getAttribute('aria-checked') === 'true';
sw.setAttribute('aria-checked', String(!on));
sw.toggleAttribute('data-checked',!on);
});
});
</script>
</body>
</html>
```
