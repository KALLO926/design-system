# 组件/动效做法卡 · 2026-11-20

来源：21st.dev 头部库与优设美地 Studio 的代表组件，逐个打开官网 Docs/Manual 源码或开源仓库核对。
用途：以后做页面要某个效果，照卡实现——机制、关键参数、代码骨架都在，不用现查。
注：21st.dev 单组件页直链当时 404/重定向，实际出处是各库官网与 GitHub 仓库；标「任务给定」的数字来自优设文章转述、非源码原文。
全六批共 78 张（2026-11-20 至 11-21）。整卡推断：40、76；局部推断处卡内均有标注。

## 分类索引（卡号）
- 文字与排版动效（9）：6 闪光、7 字符解码、14 打字机、15 翻词、16 逐词浮现、39 可变字体、40 滚动显影（推断）、65 文字描边光效、74 SplitText 逐行入场
- 背景与氛围（12）：3 极光、17 流星、18 粒子、22 点阵涟漪、25 半调圆点、42 光束背景、43 波浪、45 透视网格、57 渐变团、59 点阵揭示、61 漩涡、75 噪点
- 滚动叙事（9）：27 视差长廊、28 滚动换卡、29 开盖、41 3D 板转正、49 时间轴、66 GSAP 横滚、67 Lenis 平滑滚动、68 路径绘制、78 进度指示
- 悬停与光标（7）：1 Dock、8 流体鼠标、9 磁吸、11 聚光/倾斜、56 高亮揭示、60 跟随标签、72 流体溅射
- 提示与浮层（2）：48 Tooltip（Radix 包装）、62 头像提示
- 布局与容器（3）：20 Bento、52 拖拽画布、63 拖拽分栏
- 切换与转场（8）：12 FLIP 展开、23 对比滑块、24 胶囊标签、38 定价切换、47 手风琴、64 手势抽屉、71 多页转场、77 主题扩散
- 按钮与材质（2）：10 液态玻璃、21 弹性开关
- 展示与轮播（8）：4 跑马灯、19 无限横滚、30 台灯光锥、31 轨道环绕、32 地球、46 评价轮播、58 全屏图切换、70 头像组
- 数字与数据（2）：5 数字滚动、69 环形进度
- 反馈与加载（4）：13 AI 思考态、34 Toast、37 骨架屏、54 多步加载
- AI 界面（8）：26 流式呈现、33 命令面板、35 思考折叠、36 提示词输入框、50 代码块、51 工具状态卡、53 粒子消散输入、55 终端序列
- 连线与图形（3）：2 边框光束、44 节点连线光束、73 SVG 变形
- 开场（1）：76 Preloader（推断）

## 1. Floating Dock（Aceternity）——图标随鼠标距离放大
- 机制：父容器一个 MotionValue 存鼠标 x；每图标算中心到鼠标的距离 → 映射成宽高 → spring 平滑。
- 参数：距离 [-150,0,150] → 宽高 [40,80,40]、内图标 [20,40,20]；spring mass 0.1 / stiffness 150 / damping 12；容器 h-16、gap-4、圆角 2xl；移动端展开 stagger 0.05s/个。
- 骨架：onMouseMove 写 mouseX（离开置 Infinity）→ useTransform 算 distance（getBoundingClientRect）→ useTransform 映射 → useSpring → motion.div style{width,height}；悬停提示 AnimatePresence。
- 依赖：motion/react。出处：ui.aceternity.com/components/floating-dock（Manual 源码）。

## 2. Border Beam（Magic UI）——光束沿边框跑
- 机制：不画边框——一段渐变小条沿 offset-path 矩形路径跑，offset-distance 0%→100% 循环。
- 参数：size 50、duration 6s、colorFrom #ffaa40 → colorTo #9c40ff、borderWidth 1；offset-path: rect(...)；linear 无限循环。
- 骨架：motion.div animate offsetDistance 0%→100%，style 里 offsetPath + linear-gradient(to left, from, to, transparent)；容器 pointer-events-none、overflow-hidden。
- 依赖：motion/react。出处：magicui.design/docs/components/border-beam。

## 3. Aurora Background（Aceternity）——极光背景
- 机制：纯 CSS。超大重复线性渐变（background-size 300%）用 background-position 关键帧横移造流动；径向渐变遮罩收边。
- 参数：60s linear infinite；position 50%→350%；repeating-linear-gradient(100deg, #3b82f6 10%, #a5b4fc 15%, #38bdf8 20%, #67e8f9 25%, #22d3ee 30%)；mask radial-gradient(ellipse at 100% 0%, black 10%, transparent 70%)。
- 骨架：外层 relative → 绝对层 overflow-hidden → after 伪元素挂动画与渐变 → 内容 z-10；另叠白/暗渐变层。
- 依赖：仅 Tailwind 关键帧，无 JS。出处：ui.aceternity.com/components/aurora-background。

## 4. Marquee（Magic UI）——无缝跑马灯
- 机制：内容复制 repeat 份排成一排，整排 translateX(-100% - gap) 无限线性位移，移出一份正好接上下一份。
- 参数：repeat 默认 4；--duration 40s（演示 20s）、--gap 1rem；reverse / pauseOnHover 开关。
- 骨架：外层 group flex overflow-hidden → 内层 shrink-0 animate-marquee → Array.from({length: repeat}) 渲染；竖版换 translateY。
- 依赖：仅 CSS 关键帧。出处：magicui.design/docs/components/marquee。

## 5. Number Ticker（Magic UI）——数字滚动
- 机制：MotionValue → spring 插值 → 订阅 change 事件，每帧把格式化数字直接写 span.textContent（不走 React 重渲染）。
- 参数：spring damping 60 / stiffness 100；useInView once 进视口才启动；delay 秒级延迟；tabular-nums 防抖。
- 骨架：useMotionValue(起始值) → useSpring → inView 后 setTimeout 设目标值 → spring.on('change') 写 Intl.NumberFormat 格式化结果。
- 依赖：motion/react。出处：magicui.design/docs/components/number-ticker。

## 6. Text Shimmer（Motion Primitives）——文字闪光扫过
- 机制：文字 bg-clip-text 透明，底是 250% 宽线性渐变（中间亮带、两边透明），动画 backgroundPosition 100%→0% 循环，亮带横扫。
- 参数：duration 2s、spread 2，亮带宽 = 字数 × spread px（--spread）；浅色底 #a1a1aa / 亮带 #000，暗色底 #71717a / 亮带 #fff；linear 无限。
- 骨架：motion 组件拼 className（bg-clip-text、text-transparent、CSS 变量）→ animate backgroundPosition → style 注入 --spread 与双层 backgroundImage。
- 依赖：motion/react。出处：GitHub ibelick/motion-primitives，components/core/text-shimmer.tsx（官网当时被挡，走仓库）。

## 7. ScrambledText 字符解码（React Bits）
- 机制：GSAP SplitText 拆单字 span，ScrambleTextPlugin 逐字乱码→收敛；另加鼠标物理：按字符到鼠标距离，近处上浮/模糊/放大。
- 参数：radius 100、duration 1.2、speed 0.5、乱码字符集 '.:'；衰减 falloff=(1-(d/radius)²)²；上浮 0.75×字号、模糊 0.24×字号、放大 1+0.15e。
- 骨架：registerPlugin → SplitText.create(chars) → gsap.to(scrambleText)；mousemove 逐字算距离 → gsap.to(y/filter/opacity/scale, 0.5s, overwrite auto)；离开恢复；入场先跑一次全串解码。
- 依赖：GSAP + Club 插件 SplitText/ScrambleTextPlugin。出处：GitHub DavidHDev/react-bits，ScrambledText.jsx（站内页渲染空白，走仓库）。

## 8. BlobCursor 流体鼠标（React Bits）
- 机制：WebGL 全屏透明画布 + GLSL SDF：主球拖一串弹簧跟随的球，SDF 平滑并集让球之间"黏"成流体；另有水滴粒子。
- 参数：主球半径 0.7、融合 0.6、拖尾上限 16、水滴 96；弹簧刚度按 lag/stretch 在 160–30000 间插值；悬停可交互元素球放大 1.3×、按下缩 0.86×；物理子步 1/240s。
- 骨架：Three WebGLRenderer(alpha) → pointer 事件更新目标 → 每帧弹簧积分 → 球心/半径写 uniform 数组 → 片元着色器 SDF 并集 + 法线/反射上色 → rAF。
- 依赖：three + 自定义 GLSL。出处：GitHub DavidHDev/react-bits，BlobCursor.jsx。

## 9. Magnetic 磁吸（Motion Primitives）
- 机制：全局 mousemove 算鼠标相对元素中心的偏移，按"越远吸力越弱"线性衰减乘强度，写 MotionValue 再 spring 平滑；出范围回 0。
- 参数：spring stiffness 26.7 / damping 4.1 / mass 0.2；intensity 0.6、range 100；作用域 self/parent/global。
- 骨架：useMotionValue x/y → useSpring → mousemove 里算 scale=(1 - 距离/range)，x.set(偏移 × intensity × scale) → motion.div style{x,y}。
- 依赖：motion/react。出处：GitHub ibelick/motion-primitives，components/core/magnetic.tsx。

## 10. 液态玻璃按钮（两条路线，复刻二选一）
- 路线 A · 真折射（Aceternity liquid-glass）：Canvas 按斯涅尔定律（折射率 1.5）逐像素算位移图（R 横向弯折/G 纵向/B 边缘高光/A 遮罩），包成 SVG filter（feImage+feDisplacementMap+三通道色散 feBlend+feGaussianBlur），挂 backdrop-filter: url(#id)，背景被真实折射。参数：scale 48、aberration 4（演示 14）、bevelDepth 14、frost 0.4、tint 0.12；折射态 saturate 1.85；仅 Chromium 支持，Safari/Firefox 降级 blur(20px) saturate(1.4)+白底 70%。光晕来自位移图 B 通道高光 + 内阴影，不是另贴渐变。
- 路线 B · 轻量毛玻璃（优设 Lucian 版，参数为站内实测、实现为对应做法）：backdrop-filter: blur(18px) + 半透明底 + 渐变光晕（#8b6cff/#33b6ff），圆角 39px，float 6s 上下漂浮。
- 依赖：A 为原生 Canvas+SVG 滤镜；B 为纯 CSS。出处：ui.aceternity.com/components/liquid-glass。

## 11. Card Spotlight / Tilt 聚光倾斜卡片（Aceternity）
- 聚光机制：鼠标位置写 MotionValue，useMotionTemplate 拼 radial-gradient(350px circle at x y, white, transparent 80%) 当彩色层的 mask-image，悬停淡入（300ms）——光斑只是一层被遮罩裁出的颜色。可叠 CanvasRevealEffect（Three.js 点阵，dotSize 3、速度 5、蓝紫双色）。
- 倾斜机制：纯 CSS 3D——外层 perspective 1000px + preserve-3d，偏移/25 得 rotateY/rotateX，子元素 translateZ 分层（50/60/100）浮起，离开归零，200ms linear。
- 依赖：聚光 motion（+three 可选）；倾斜仅 React+CSS。出处：ui.aceternity.com/components/card-spotlight、/3d-card-effect。

## 12. FLIP 卡片展开（View Transitions API）
- 机制：不用手算 FLIP——document.startViewTransition(改DOM回调)，浏览器对改前/改后各截快照，按 view-transition-name 配对同名元素，自动做位移插值+交叉淡入；同名元素同一时刻只能有一个。
- 参数（任务给定，源自优设文章转述）：进入 0.7s / 退出 0.6s、cubic-bezier(0.32, 0.72, 0, 1)，写在 ::view-transition-old/new(name) 的 animation 上，新旧两层可分别给时长。
- 骨架：卡片与展开态同名 view-transition-name: card-<id> → 点击 startViewTransition(换 DOM) → CSS 定 group/old/new 动画 → 关闭反向再调一次；无 API 时直接改 DOM 降级。
- 依赖：原生 View Transitions API（Chromium 稳定，其他浏览器用时查支持）。出处：MDN View Transition API（机制）；数字为任务给定。

## 13. AI 思考/加载状态（SmoothUI AI Loader + Siri Orb）
- AI Loader 机制：三变体共用 1.2s 周期做透明度/位移波形；明确"不定等待不伪造进度"，只显示真实已耗时间（useAnimationFrame 累计、0.1s 变化才更新、显示 xx.x s）。
- 参数：Dots 3 点 size-1.5、opacity [0.25,1,0.25]、延迟 i×0.2s；Bar 亮段 w-1/3、x -100%→200%、时长 1.68s；Grid 3×3、对角延迟表 [0,1,2,1,2,3,2,3,4]×0.15s；共用缓动 [0.645,0.045,0.355,1]；role=status + aria-live；减动效时静态。
- Siri Orb 机制（球体思考态）：6 层 conic-gradient 叠加，@property 注册 --angle 驱动旋转，振幅用 MotionValue 在渲染外 60fps 更新；按 idle/listening/thinking/streaming/done/error 切转速/大小/饱和度——thinking 转速 2.2× 不改尺寸，error 去饱和+<200ms 抖动，done 一次回弹 settle。
- 依赖：motion/react；Orb 另需 CSS @property。出处：smoothui.dev/docs/components/ai-loader、/siri-orb。注：SmoothUI 无 "Thinking Orb" 同名组件，球体态按 Siri Orb 对应（推断）；优设 Thinking Orb 未单独核对。

---

# 第二批 · 2026-11-20（编号续前）

## 14. Typewriter 打字机（Aceternity）
- 机制：词拆成字符 span（初始隐藏），入视口后 useAnimate 逐字点亮；光标是独立 span 无限反向闪烁。Smooth 版是容器宽度 0→fit-content 展开。
- 参数：字符 stagger 0.1s、单字 0.3s easeInOut；光标 0.8s opacity 0↔1 reverse 无限；Smooth 版 2s linear、delay 1s。
- 骨架：split("")→motion.span(hidden) → animate("span",{display,opacity},{stagger(0.1)}) → 光标 motion.span animate opacity[0,1]。
- 依赖：motion/react。出处：ui.aceternity.com/components/typewriter-effect。

## 15. Flip Words 翻词（Aceternity）
- 机制：AnimatePresence 按 key=当前词 整词换：容器 spring 翻入，词内字母逐个浮现；旧词退场上移+放大+模糊。
- 参数：换词间隔 3s（退场完成才放行下一轮）；容器 spring stiffness 100/damping 10；入场 y 10→0；退场 y -40、x 40、blur 8px、scale 2；字母延迟 词序×0.3+字序×0.05、0.2s。
- 骨架：AnimatePresence onExitComplete → motion.div key=word（initial/animate/exit）→ 字母 motion.span 逐个 blur 8→0。
- 依赖：motion/react。出处：ui.aceternity.com/components/flip-words。

## 16. Text Generate Effect 逐词浮现（Aceternity）
- 机制：按空格分词，每词初始 opacity 0 + blur(10px)，挂载即全量 stagger 点亮（不看视口，见即播）。
- 参数：stagger 0.2s；单词时长默认取 1s（参数默认 0.5）；filter 开关默认开。
- 骨架：split(" ")→motion.span(opacity-0, blur) → useEffect animate("span",{opacity:1,filter:blur(0)},{stagger(0.2)})。
- 依赖：motion/react。出处：ui.aceternity.com/components/text-generate-effect。

## 17. Meteors 流星（Magic UI）
- 机制：纯 CSS 动画 + JS 随机化：每颗 = 圆点头 + 50px 渐变尾，随机落点/延迟/时长，沿 -215° 平移 500px，70% 后才淡出。
- 参数：默认 20 颗；延迟 0.2–1.2s、时长 2–10s 随机；头 size-0.5、尾 h-px w-50px。
- 骨架：useEffect 生成随机样式数组 → span animate-meteor（rotate(var(--angle))）内嵌尾巴 span（gradient to transparent）。
- 依赖：Tailwind 自定义关键帧。出处：magicui.design/docs/components/meteors。

## 18. Sparkles 粒子（Aceternity）
- 机制：tsParticles 画布粒子：随机分布、慢漂移、透明度呼吸；加载完 Motion 淡入。
- 参数：fps 上限 120；数量默认 120（密度区 400×400）；大小 1–3；速度 0.1–1；透明度 0.1–1 呼吸速度 4；点击 push 4 个；碰撞关、Retina 开。
- 骨架：initParticlesEngine(loadSlim) → <Particles options={密度/大小/速度/透明度动画}>。
- 依赖：@tsparticles/react+engine+slim、motion/react。出处：ui.aceternity.com/components/sparkles。

## 19. Infinite Moving Cards 无限横滚（Aceternity）
- 机制：JS 把子节点 cloneNode 复制一份（状态门控，复制完才开动画防闪），轨道平移 -50% 无缝循环；速度/方向写 CSS 变量。
- 参数：三档速 20s/40s/80s；keyframes 到 translate(-50% - 0.5rem)（补 gap）；悬停暂停默认开；两侧渐隐遮罩（mask 20%/80%）；卡片定宽 350px。
- 与 Marquee 区别：同为"复制+平移 -50%"；此为卡片专用版（JS 设变量、三档速、补 gap、渐隐罩），Marquee 是通用版（--duration/--gap、无卡片结构；对照句为推断）。
- 依赖：Tailwind。出处：ui.aceternity.com/components/infinite-moving-cards。

## 20. Bento Grid 便当格（Aceternity）
- 机制：组件本身几乎无 JS——Tailwind 网格 + 插槽：Grid 只定列/行高/间距，Item 只定样式与 hover 位移；每格动效全由调用方放进 header 插槽（演示里各格是不同的 Skeleton 动效）。
- 参数：grid-cols-3、auto-rows 18rem、gap-4；Item 圆角 xl、hover shadow-xl；图标 hover translate-x-2（200ms）；跨格靠调用方 className（col-span-2 等）。
- 骨架：BentoGrid(div grid) + BentoGridItem(header 插槽 + 图标/标题/描述)。
- 依赖：Tailwind。出处：ui.aceternity.com/components/bento-grid。

## 21. 弹性开关 Switch（React Bits SquishSwitch，同类；Lucian 原版无开源）
- 机制：不用 CSS transition——Motion 弹簧链：拇指位置的速度 → 一根"流弹簧"换算拉伸量，再叠一根"膨胀弹簧"做回弹挤压；可拖动，松手弹簧吸到最近端。
- 参数：流弹簧 stiffness 320/damping 40/mass 0.6；膨胀弹簧 520/34/0.6；最大拉伸 0.4、拉伸速度标尺 600；默认 76×38、圆角 19。
- 骨架：useVelocity(x) → useSpring(流） → scaleX=useTransform（速度→[1.4,1,1.4]）；useSpring(1，膨胀）管 scaleY；指针 down/move/up 拖动、松手按位置判定开关。
- 依赖：motion/react。出处：GitHub DavidHDev/react-bits，SquishSwitch.tsx（文件树定位）。

## 22. 点阵涟漪（React Bits RippleGrid，同类）
- 机制：原生 WebGL 着色器网格（非 DOM 点阵）：最多 12 个涟漪源 {x,y,born,strength}，鼠标/点击/定时自动产生，片元着色器按距离算波包叠加到网格亮度。
- 参数：涟漪上限 12；格 48px；强度 0.5、速度 1、大小 4；自动涟漪间隔 1600ms（中心起波）；variant 可选 lines/dots/cross。
- 骨架：涟漪数组作 uniform → 片元 wave=sin(归一化距离-时间)*波包*扩散*衰减 → 每帧更新 uTime。
- 注：DOM 点阵版（逐点 div 按距离延迟）为同类推断，未核对。依赖：原生 WebGL。出处：GitHub DavidHDev/react-bits，RippleGrid.tsx。

## 23. 昼夜对比滑块 Compare（Aceternity，同类）
- 机制：两图叠放，上层用 clip-path: inset 裁掉右侧只露左半；百分比状态同时驱动裁剪量与分隔线位置；坐标经 getBoundingClientRect 换算、rAF 节流。
- 参数：初始 50%；模式 hover（移入跟随、移出回 50）/ drag（按住拖）；自动播放 5s 三角波往返、鼠标进入暂停；分隔线 w-px、手柄 h-5 w-5。
- 骨架：handleMove(clientX)→percent→rAF setState → 上层 motion.div clipPath inset(0 ${100-p}% 0 0)，下层全幅垫底。
- 依赖：motion/react。出处：ui.aceternity.com/components/compare。

## 24. 滑动胶囊标签 Tabs（Motion layoutId）
- 机制：共享布局动画——每个标签里条件渲染同一个 layoutId 的胶囊元素（只在选中项存在），Motion 测新旧包围盒用 transform 补间，胶囊就"滑"过去；本质是 FLIP。
- 参数：默认 layout 时长 0.3s；spring 模式 visualDuration 0.4、bounce 0.2；layout="position" 只动位置不缩放。
- 骨架：按钮 relative → {selected && <motion.span layoutId="tab-pill" className="absolute inset-0 rounded-full"/>} → 文字 z-10；切换 selected 即滑动（需 AnimatePresence 配合离场）。
- 依赖：motion/react。出处：motion.dev/docs/react-layout-animations（胶囊写法为同机制应用）。

## 25. 半调圆点背景（Magic UI Dot Pattern，同类；RicoUI 原版未找到开源）
- 机制（已核对做法）：JS 按容器尺寸算点数逐个生成 SVG circle（等大），渐变靠外层 mask；glow 模式每点随机延迟呼吸。
- 参数：间距 16px、半径 1；glow 时 opacity [0.4,1,0.4]、scale [1,1.5,1]、延迟随机 0–5s、时长 2–5s。
- 三种做法结论：①等大点+mask 渐变（本卡）；②真半调——点半径随图像亮度变（canvas 采样，推断未核对）；③CSS radial-gradient 平铺等大点。
- 骨架：Array.from（行×列）算坐标 → <svg absolute inset-0> 逐点 <Dot>；resize 时重算。
- 依赖：motion/react。出处：magicui.design/docs/components/dot-pattern。注：ricoui.dev 域名无法解析，未找到 RicoUI 开源实现。

## 26. AI 聊天流式呈现（AI Elements / Vercel）
- 机制：不是逐字打字机——token 流经 useChat 累加成"不断变长的整段 Markdown 字符串"（此环为推断，未逐行核对 useChat 源码），每次更新整段交给 Streamdown 增量解析（容错未闭合的代码块/表格）；自动滚动由 use-stick-to-bottom 接管（到底才跟随，上翻看历史不抢）。
- 关键点：MessageResponse = memo 包 Streamdown + cjk/code/math/mermaid 插件；Conversation 用 StickToBottom（initial/resize 均 smooth）；未到底时才显示回到底部按钮。
- 骨架：<Conversation><Content>{messages.map(<Message><MessageResponse>{m.text}</MessageResponse></Message>)}</Content><ScrollButton/></Conversation>。
- 依赖：streamdown 及插件、use-stick-to-bottom、AI SDK。出处：GitHub vercel/ai-elements，message.tsx / conversation.tsx（Response 已改名 MessageResponse）。

---

# 第三批 · 2026-11-20（编号续前）

## 27. Hero Parallax 视差长廊（Aceternity）
- 机制：useScroll 拿整段滚动进度，三行产品图各自不同方向横移，整组再联动 rotateX/rotateZ/translateY/opacity，拼成透视长廊；全部过 spring 平滑。
- 参数：三行 translateX [0,1]→[0,1000]/[0,-1000]/[0,1000]；rotateX 前 20% [15,0]、rotateZ [20,0]、translateY [0,-100]、opacity [0.2,1]；spring stiffness 100/damping 30；每行 5 张、卡 40rem×30rem。
- 骨架：useScroll({target, offset:["start start","end start"]}) → 各 useTransform → useSpring 包裹 → motion.div style{rotateX,rotateZ,translateY,opacity} 内三行交替方向。
- 依赖：motion/react。出处：ui.aceternity.com/components/hero-parallax。

## 28. Sticky Scroll Reveal 滚动换卡（Aceternity）
- 机制：左侧文字列是独立滚动容器（h-30rem、overflow-y-auto），onScroll 按 scrollTop 找最近断点换 activeCard；右侧卡 sticky 钉住，只换背景渐变不跟滚。
- 参数：断点 = 段序/总段数；激活段 opacity 1、其余 0.3；背景/渐变各 3 色 %3 循环；卡 h-60 w-80、sticky top-10；段间距 my-20。
- 骨架：onScroll 算 closestIndex → setActiveCard → 右侧 motion.div animate{background: 渐变[active%3]}。
- 依赖：motion/react。出处：ui.aceternity.com/components/sticky-scroll-reveal。

## 29. Macbook Scroll 滚动开盖（Aceternity）
- 机制：200vh 长段滚动驱动：屏幕先放大下沉，再 rotateX 从 -28° 开盖到 0°；标题同步上移淡出；机身靠 Lid(-25°) + Screen 双层 perspective 拼。
- 参数：scaleX [0,0.3]→[1.2,1.5]、scaleY→[0.6,1.5]、translate [0,1]→[0,1500]、rotate 分段 [0.1,0.12,0.3]→[-28,-28,0]；标题 opacity 前 20% [1,0]；perspective 800、屏幕 22rem×32rem。
- 骨架：div min-h-200vh → motion.div style{scaleX,scaleY,translate} → Lid/Screen(style rotateX)/Base。
- 依赖：motion/react。出处：ui.aceternity.com/components/macbook-scroll。

## 30. Lamp Effect 台灯光锥（Aceternity）
- 机制：光锥不是图——两片 conic-gradient（from 70°/290°、透明→青→透明）拼成 V 形，下叠 blur 光晕 + 一条横线，whileInView 时宽度展开。
- 参数：锥宽 15rem→30rem、opacity 0.5→1、delay 0.3、0.8s easeInOut；光晕 blur-2xl 宽 8→16rem；容器 h-40rem 深底。
- 骨架：左右 motion.div（--conic-position 不同）initial{width 15rem} whileInView{30rem} + 光晕 div + 横线 + 文字 -translate-y-80 压上。
- 依赖：motion/react。出处：ui.aceternity.com/components/lamp-effect。

## 31. Orbiting Circles 轨道环绕（Magic UI）
- 机制：纯 CSS：外环容器匀速自转，图标用 rotate(角) translateX(半径) rotate(-角) 定位并抵消自转、保持正立；反向环 animation-direction: reverse。
- 参数：--duration 20s、--radius 160、图标 30；角度 = 360/个数×序号；path 开时画虚线圆（dasharray 4 4）。
- 骨架：容器 animate-orbit → SVG 虚线圆 → children.map 按角度 transform 定位。
- 依赖：仅 CSS 关键帧。出处：magicui.design/docs/components/orbiting-circles。

## 32. Globe 点阵地球（Magic UI / cobe）
- 机制：cobe 在 canvas 画点阵地球；rAF 每帧 phi += 0.005 自转；拖拽位移/1400 换算成目标角度，spring 平滑跟随；容器尺寸变则重建。
- 参数：拖拽阻尼 1400；theta 0.3；画布 800×800、DPR 2；mapSamples 16000、亮度 1.2、diffuse 0.4；标记橙 [251,100,21]；入场淡入 500ms。
- 骨架：createGlobe(canvas, {…, onRender: state => state.phi += 0.005}) + pointer 事件改目标 phi/theta。
- 依赖：cobe。出处：magicui.design/docs/components/globe。

## 33. 命令面板 CmdK（cmdk）
- 机制：单文件状态机：输入存 search 状态，command-score 给每项打分过滤排序；键盘只改当前 value，选中项靠 aria-selected 的 DOM 节点反查；空分组自动隐藏。
- 参数：搜索后微任务选中首项并 scrollIntoView(block nearest)；loop 开时首尾回绕；shouldFilter 默认开、可换自定义 filter。
- 骨架：<Command><Input/><List><Empty/><Group heading><Item value onSelect/></Group></List></Command>；内部 store→filterItems→sort→selectFirst。
- 依赖：command-score、React（Dialog 版另需 Radix Dialog）。出处：github.com/dip/cmdk（原 pacocoursey/cmdk 已迁移）。

## 34. Toast 通知（Sonner）
- 机制：Toaster 维护数组与每条实测高度；未展开只露前 3 条、后条按序号缩放下沉，hover/Alt+T 展开成列；关闭靠滑动位移+速度判定；到期 200ms 后卸载。
- 参数：可见 3 条、寿命 4000ms、宽 356、间距 14；滑动阈值 45px 或速度 >0.11；Escape 收起。
- 骨架：toast(msg, {description, action}) 触发；内部 CSS 变量 --index/--offset/--swipe-amount 驱动堆叠与滑动。
- 依赖：无运行时依赖。出处：sonner.emilkowal.ski / github.com/emilkowalski/sonner。

## 35. Reasoning 思考折叠（AI Elements）
- 机制：Collapsible 包裹：isStreaming 为真自动展开、结束自动折叠；标题行按状态换文案/图标；时长取服务端 Trace 元数据（非本地计时）。
- 参数：内容高度过渡 200ms ease-out；文案 streaming=「Thinking…」、完成=「Thought for Xs」（1 位小数）、error 专属文案；箭头随展开 rotate-90。
- 骨架：<Reasoning isStreaming><Trigger/><Content/></Reasoning>；useEffect 监听 isStreaming 跟随开关。
- 依赖：Radix Collapsible、AI SDK reasoning 流。出处：ui.shadcn.com/docs/ai-elements/base/reasoning（新版基于 Trace）。

## 36. PromptInput 提示词输入框（AI Elements）
- 机制：form + InputGroup 组合：Textarea 靠 CSS field-sizing-content 自适应高度（最小 3 行）；Enter 提交、Shift+Enter 换行；附件走隐藏 file input，选中后以芯片列在输入框顶部；提交时文本+文件一起交 sendMessage。
- 参数：min-h-24、resize-none；canSend = 有文本或有附件；附件示例 accept image/*。
- 骨架：<PromptInput onSubmit={({text,files})=>sendMessage({text,files})}><Header 附件芯片/><Body Textarea onKeyDown/><Footer 工具菜单 + Submit disabled={!canSend}/></PromptInput>。
- 依赖：AI SDK、shadcn InputGroup 等。出处：ui.shadcn.com/docs/ai-elements/base/prompt-input。

## 37. 骨架屏 Skeleton（shadcn/ui）
- 机制：官方本体只有脉冲式：animate-pulse 色块占位（2s 循环、opacity 1↔0.5）；"流光"不是骨架变体，是独立的 shimmer 文本工具（background-clip:text 让高光带扫过文字）。
- 参数：shimmer 默认 2s、spread 3ch+40px、角度 20°；有 once/reverse/变色/改时长等变体；减动效自动停。骨架块上做流光官方无现成，需自加渐变层（推断）。
- 骨架：<Skeleton className="h-12 w-12 rounded-full"/> 等拼形；流光 <p className="shimmer">Generating…</p>。
- 依赖：Tailwind + shadcn 样式包。出处：ui.shadcn.com/docs/components/base/skeleton、/docs/utils/shimmer。

## 38. 定价月/年切换（Efferd Pricing 4；Kokonut 免费版无此组件，404 已核）
- 机制：frequency 状态三处联动——切换器滑块按实测按钮位置（offsetLeft/Width）移动；价格用 NumberFlow 滚动数字；折扣徽章 AnimatePresence 淡换。
- 参数：滑块 spring duration 0.4；徽章过渡 0.15s；折扣 = (月-年)/月 取整%；示例价 Basic 7/6、Pro 17/14、Business 49/40（月/年美元），Pro 卡 scale-105 高亮。
- 骨架：useLayoutEffect 量位置 → motion.span animate{left,width} + mix-blend-difference → <NumberFlow value format=currency suffix="/month"/>。
- 依赖：motion/react、@number-flow/react。出处：efferd.com/blocks/pricing。

## 39. 可变字体随鼠标（Fancy Components）
- 机制：文本拆字母 span，鼠标坐标存 ref（不触发重渲染）；每帧算每字母到鼠标的距离→衰减强度→在 font-variation-settings 各轴 from/to 间插值，直接写 DOM 样式；出半径复位。
- 参数：radius 默认 50（演示 200）；衰减三式 linear(1-d/r)、exponential(平方)、gaussian；演示 wght 400→900、slnt 0→-10；另置 sr-only 原文保无障碍。
- 骨架：useMousePositionRef + useAnimationFrame 遍历 letterRefs → 算 d 与 f → 写 style.fontVariationSettings。
- 依赖：motion/react + 可变字体文件。出处：fancycomponents.dev/docs/components/text/variable-font-cursor-proximity。

---

# 第四批 · 2026-11-20（编号续前）

## 40. Text Reveal 滚动逐词显影（Aceternity）——整卡推断
- 说明：官网页与仓库路径均 404（站内只有 Text Reveal Card 与 Text Generate Effect，非此物），未找到源码，整卡为机制推断、未核对。
- 机制（推断）：整段拆词，每词按词序均分滚动区间 [i/n,(i+1)/n]，useScroll 进度映射每词 opacity 0.1→1，滚动逐词显影；无时长缓动、滚动直驱。
- 骨架（推断）：useScroll({target}) → words.map 每词 useTransform(p,[i/n,(i+1)/n],[0.1,1]) → motion.span style{opacity}。

## 41. Container Scroll 3D 板转正（Aceternity）
- 机制：useScroll 读容器进度，一条映射同时驱动卡片 rotateX/scale/translate：倾斜的大板滚着转正。
- 参数：rotateX [0,1]→[20,0]；scale 桌面 [1.05,1]、移动 [0.7,0.9]；translate→[0,-100]；perspective 1000；容器高 60rem（md 80rem）、卡高 30/40rem、圆角 30px；多层大阴影写死在 style。
- 骨架：useScroll({target:containerRef}) → 三个 useTransform → motion.div style{rotateX,scale,translate,boxShadow}；isMobile 靠 innerWidth<768 监听。
- 依赖：motion/react。出处：ui.aceternity.com/components/container-scroll-animation。

## 42. Background Beams 光束背景（Aceternity）
- 机制：约 51 条同形 SVG 路径作底纹（间隔 7px 递增平移），另挑一条路径用可动 linearGradient 描边——动画的是渐变的坐标不是路径本身，光带就沿线跑。
- 参数：底纹 strokeOpacity 0.05、宽 0.5；渐变 x1 0→100%、x2 0→95%、y2 到 93+随机8%；时长随机 10–20s、easeInOut、无限、延迟随机；色 #18CCFC→#6344F5→#AE48FF（两端透明）。
- 骨架：motion.linearGradient animate{x1,x2,y1,y2} + 底纹 path 群 + motion.path stroke=url(#grad)；React.memo。
- 依赖：motion/react。出处：ui.aceternity.com/components/background-beams。

## 43. Wavy Background 波浪背景（Aceternity）
- 机制：canvas 上用 simplex 3D 噪声逐帧画 5 条波浪线，时间维递增即流动，blur 10 柔化。
- 参数：5 条、线宽默认 50；x 步进 5，y=noise(x/800, 0.3i, nt)×100；速度 slow 0.001/fast 0.002（每帧 nt 增量）；透明度 0.5；默认 5 色蓝紫青；Safari 无 ctx.filter 时改 CSS blur 兜底。
- 骨架：createNoise3D → rAF 循环 clearRect+逐条 lineTo → nt+=speed；resize 重置画布。
- 依赖：simplex-noise。出处：ui.aceternity.com/components/wavy-background。

## 44. Animated Beam 节点连线光束（Magic UI）
- 机制：量两节点中心相对容器的坐标，算一条二次贝塞尔路径；底色路径常驻，另一条同路径用渐变描边，动画渐变 x1/x2 让光沿线流动；ResizeObserver 重算。
- 参数：duration 5s、缓动 [0.16,1,0.3,1]、无限无间隔；curvature 控弯（控制点 y = 起点 y - curvature）；底路径 opacity 0.2、宽 2；渐变 #ffaa40→#9c40ff。
- 骨架：三 ref（from/to/container）→ getBoundingClientRect 算起终点 → pathD=M…Q… → 两条 path + defs 里 motion.linearGradient animate{x1:[10%,110%],x2:[0%,100%]}。
- 依赖：motion/react。出处：magicui.design/docs/components/animated-beam。

## 45. Retro Grid 透视网格（Magic UI）——现版是 WebGL
- 机制：当前版不是 CSS 动画——WebGL 片元着色器做射线-平面求交画透视网格，u_time 推动向地平线滚动，远处自动换粗网格 LOD；无 WebGL 时才降级 CSS 版（perspective+rotateX+背景平移）。
- 参数：angle 65（1–89）、格 60、15s 滚半个网格高；线宽 0.92px + smoothstep 抗锯齿；DPR 上限 2；离屏暂停、减动效停时、顶部渐变淡出。
- 骨架：canvas getContext(webgl) → 编译 shader → rAF 写 u_time drawArrays；三个 Observer 管尺寸/可见/亮暗。
- 依赖：仅 React。出处：magicui.design/docs/components/retro-grid。

## 46. Animated Testimonials 评价轮播（Aceternity）
- 机制：图片与文字各一个 AnimatePresence，active 索引取模循环；图片带随机倾斜入场、文字逐词 blur 入场。
- 参数：autoplay 开时 5s 一换；图片入场 scale 0.9/rotate 随机 -10°~10°/z -100、0.4s easeInOut，非激活 opacity 0.7；文字词延迟 0.02×序、blur 10→0、0.2s。
- 骨架：setInterval(handleNext,5000) → AnimatePresence 内 map 图片 motion.div（animate 按 i===active 分支）+ 文字 motion.div key=active 逐词 span。
- 依赖：motion/react。出处：ui.aceternity.com/components/animated-testimonials。

## 47. Accordion 手风琴（Motion Primitives）
- 机制：height auto 的解法——不写 CSS 高度动画，variants 里 expanded 直接写 height:"auto"，Motion 运行时量真实高度插值；AnimatePresence 管进出。Radix 版是另一路：用它给的 --radix-accordion-content-height 变量做关键帧。
- 参数：默认 0.3s easeInOut；collapsed {height:0, opacity:0}；点已展开项则关闭（单开）；图标随展开 -rotate-180。
- 骨架：Context 存 expandedValue → Content 仅展开时渲染 motion.div（initial collapsed/animate expanded/exit collapsed、overflow-hidden）。
- 依赖：motion/react。出处：motion-primitives.com/docs/accordion。

## 48. Tooltip（Motion Primitives 仓库版）——实测为 Radix 包装、非跟随
- 机制：官网无此组件（404）；仓库里的是 shadcn 风 Radix Tooltip 包装：Provider/Root/Trigger/Portal Content，入场靠 Tailwind animate-in（fade+zoom+按方向滑入 2px）；定位防溢出全由 Radix 内部处理，不跟随鼠标——要跟随效果须另包 motion 或改用光标类实现，不能照抄此文件。
- 参数：delayDuration 0、sideOffset 0；Content z-50、px-3 py-1.5、text-xs；箭头 size-2.5 rotate-45。
- 依赖：@radix-ui/react-tooltip。出处：github.com/ibelick/motion-primitives，components/ui/tooltip.tsx。

## 49. Timeline 滚动时间轴（Aceternity）
- 机制：useScroll 读整条轴的滚动进度（offset start 10%→end 50%），映射成一根渐变竖线的高度，线随滚动生长；标题 sticky 钉在 top-40。
- 参数：线宽 2px；生长线渐变紫→蓝→透明；高度挂载时量一次容器高（无 resize 监听，是已知短板）；圆点 h-10 内圆 h-4；生长线 opacity 前 10% [0,1]。
- 骨架：useScroll({target, offset}) → heightTransform [0,1]→[0,height] → 底线 div 内 motion.div style{height:heightTransform}。
- 依赖：motion/react。出处：ui.aceternity.com/components/timeline。

## 50. CodeBlock 代码块（AI Elements）
- 机制：Shiki 在浏览器端把代码算成带色 token 行：先用无色"原始 token"占位防闪，highlighter 按语言缓存建好后替换；复制按钮是独立组合子组件；暗色靠 .dark 类 + shiki CSS 变量切换。
- 参数：highlighterCache 按语言 Map 缓存 Promise；主题 github-light/github-dark；pre p-4 text-sm；容器 contentVisibility 做长页性能优化；CopyButton 的 timeout 默认值本轮未核对。
- 骨架：useState(createRawTokens) → useEffect getHighlighter(lang).then(codeToTokens) → useMemo 渲染逐行逐 token span + 可选行号列。
- 依赖：shiki。出处：elements.ai-sdk.dev/components/code-block（仓库 main 路径已变，以文档 Manual 为准）。另注：AI Elements 整体将并入 shadcn/ui、2026 年 11 月底弃用（与 batch 2 的 AI Elements 迁移注记互证）。

## 51. Tool 工具状态卡（AI Elements）
- 机制：就是 Collapsible 换皮：Tool/ToolHeader/ToolContent 对应 Collapsible 三件；状态只决定标题徽章的图标+文案+颜色；完成/出错默认展开，内容里参数 JSON 走 CodeBlock 高亮、结果走 MessageResponse。
- 参数：7 状态映射——input-streaming Pending（灰圆）、input-available Running（时钟+脉冲）、approval-requested Awaiting（黄）、approval-responded（蓝勾）、output-available Completed（绿勾）、output-error Error（红叉）、output-denied Denied（橙叉）；徽章 rounded-full text-xs。
- 骨架：getStatusBadge(state) 查表 → <Tool defaultOpen><ToolHeader type state/><ToolContent><ToolInput/><ToolOutput/></ToolContent></Tool>。
- 依赖：shadcn Badge/Collapsible、ai 类型。出处：elements.ai-sdk.dev/components/tool。

## 52. 可拖拽画布画廊（Motion drag；无限画布为组合推断）
- 机制：Motion drag 是物理拖拽：拖动改 x/y、松手按速度惯性滑行；dragConstraints 只给边界（像素对象或另一元素 ref 的包围盒），本身不提供"无限画布"——无限感靠内层大画布拖动 + 坐标取模回绕/动态补图组合（推断，非内置 API）。
- 参数：dragElastic 0–1（例 0.1，越界弹性回拉）；dragMomentum 默认开、惯性物理可改 bounceStiffness 600/damping 10；whileDrag 可放大加阴影；dragDirectionLock 锁首方向；事件 info 含 point/delta/offset/velocity。
- 骨架：视口 div ref 作 constraints → 内层 motion.div drag dragConstraints={ref} dragElastic={0.1}；手柄触发用 useDragControls.start。坑：父级有 transform 时拖距错位需 transformPagePoint 校正；img 子元素加 draggable={false}。
- 依赖：motion/react。出处：motion.dev/docs/react-drag、/react-gestures。

---

# 第五批 · 2026-11-20/21（编号续前）

## 53. Vanish Input 粒子消散输入框（Aceternity）
- 机制：占位词 AnimatePresence 轮播（3s 一换、切页暂停）；提交时把输入文字画到离屏 canvas、逐像素取样成粒子，rAF 逐帧随机散开消失。
- 参数：占位进场 y 5→0、离场 y -15+淡出 0.3s linear、mode="wait"；取样 canvas 800×800、字号 ×2；粒子每帧位移 -8、抖动 ±1、半径每帧 -0.05×随机。
- 骨架：setInterval 换占位 → draw() getImageData 扫像素生成粒子数组 → animate() 逐粒子画小方块 → 提交时先 draw+animate 再清空 value。
- 依赖：motion/react。出处：ui.aceternity.com/components/placeholders-and-vanish-input。

## 54. Multi-step Loader 多步加载（Aceternity）
- 机制：数组 + 下标的状态机，setTimeout 链逐项推进（每步默认 2s，loop 时末项归零）；每项按与当前项的距离衰减透明度并整体上移，完成变实心勾、当前项文字变绿。
- 参数：opacity = max(1-|i-当前|×0.2, 0)；y = -(当前×40)；过渡 0.5s；遮罩 fixed 全屏 + backdrop-blur-2xl、z-100。
- 骨架：useEffect setTimeout 推下标 → map 每项 motion.div style{opacity} + 图标按 i 与当前比大小换空心/实心勾。
- 依赖：motion/react。出处：ui.aceternity.com/components/multi-step-loader。

## 55. Terminal 终端序列（Magic UI）
- 机制：序号串联——Context 存 activeIndex，只有轮到自己的行才开始（打字或淡入），完成回调把序号 +1；startOnView 时等进视口 30% 才启动。
- 参数：打字默认 60ms/字；外壳 max-h 400、圆角 xl；顶栏三色点 h-2 w-2。
- 骨架：Children.toArray 逐个包 ItemIndexContext → TypingAnimation 用 setInterval 切片 slice(0,i+1)、写满清 interval 并 completeItem。
- 依赖：motion/react。出处：magicui.design/docs/components/terminal（仓库 terminal.tsx）。

## 56. Hero Highlight（Aceternity）
- 机制：鼠标位置 MotionValue 拼 radial-gradient(200px) 当 mask，只在光标处揭示一层点阵；Highlight 文字靠 backgroundSize 0%→100% 的渐变底"划线"。
- 参数：mask 半径 200px、hover 层 300ms 淡入；划线 2s linear、延迟 0.5s、靛→紫渐变；标题入场 y [20,-5,0] 0.5s。
- 骨架：onMouseMove 写 mouseX/Y（减 rect）→ useMotionTemplate 拼 maskImage → 点阵层（SVG data-uri，亮暗各一）→ motion.span 控 backgroundSize。
- 依赖：motion/react。出处：ui.aceternity.com/components/hero-highlight。

## 57. Background Gradient Animation 渐变团（Aceternity）
- 机制：5 个径向渐变色团全用 hard-light 混合，各跑 CSS 关键帧缓慢游走（竖移/绕圈/横移，20–40s）；第 6 个交互团不用关键帧，鼠标目标位每帧 cur += (目标-cur)/20 追赶。
- 参数：色团尺寸 80%；transform-origin 各偏移 ±200–800px 造大半径绕行；底层 SVG goo 滤镜（blur 10 + colorMatrix)；容器底 40° 线性渐变深紫蓝。
- 骨架：body.style 写 --first-color 等变量 → 5 团 radial-gradient + animate-* 类 → mousemove 记目标 → rAF/effect 平滑追赶写 transform。
- 依赖：CSS 关键帧（需在 tailwind.config 注入）。出处：ui.aceternity.com/components/background-gradient-animation。

## 58. Images Slider 全屏图切换（Aceternity）
- 机制：先 Promise.all 预加载全部图才渲染；当前图 AnimatePresence 做 rotateX 翻入、按方向 y ±150% 滑出；自动、键盘、手柄共用同一组 next/prev。
- 参数：自动 5s（默认开）；入场 scale 0/rotateX 45→1/0、0.5s 缓动 [0.645,0.045,0.355,1]；滑出 1s；perspective 1000；遮罩 bg-black/60。
- 骨架：preload.then(setLoaded) → motion.img key=current variants（visible/upExit/downExit）→ keydown 左右箭头监听。
- 依赖：motion/react。出处：ui.aceternity.com/components/images-slider。

## 59. Canvas Reveal 点阵揭示（Aceternity，独立版）
- 机制：WebGL（react-three/fiber + ShaderMaterial）全屏平面：GLSL 按网格取随机点，u_time 驱动从中心向外扩散入场，点透明度查 opacities 数组表；帧率节流 60fps。
- 参数：animationSpeed 默认 0.4（演示用到 5.1）；opacities 10 档 [0.3×3,0.5×3,0.8×3,1]；点阵版 totalSize 4、dotSize 2；入场偏移 = 距中心距离×0.01 + 随机×0.15。
- 骨架：<Canvas><ShaderMaterial uniforms{u_time,u_opacities,u_colors,…}/><planeGeometry 2×2/></Canvas>；useFrame 节流写 u_time。
- 依赖：three、@react-three/fiber。出处：ui.aceternity.com/components/canvas-reveal-effect（灵感 Clerk）。

## 60. Following Pointer 跟随标签（Aceternity）
- 机制：卡片内 mousemove 直接把光标坐标写进 MotionValue，标签硬跟随（无 spring）；仅 isInside 时挂载，16px 圆点 + 倾斜箭头 + 随机色标签；容器 cursor-none。
- 参数：点 h-4 w-4；箭头 -rotate-70°；标签入场 scale 0.5→1；颜色每次渲染从 7 色随机；rect 只在挂载时量一次（滚动靠 +scrollX/Y 补偿）。
- 骨架：onMouseMove x.set(clientX-rect.left+scrollX) → motion.div style{top:y,left:x} absolute z-50 pointer-events-none。
- 依赖：motion/react。出处：ui.aceternity.com/components/following-pointer。

## 61. Vortex 漩涡粒子（Aceternity）
- 机制：2D canvas + simplex 噪声：700 粒子各 9 个 float 存一个大数组，噪声角定方向、速度 lerp 平滑转向，画带生命淡入淡出的短线；整张 canvas 再两次 blur+brightness+lighter 自叠做辉光。
- 参数：粒子 700、寿命 50+随机 150、速度 0–1.5、半径 1–3、色相 220+随机 100；噪声步进 x/y 0.00125、z 0.0005；辉光 blur 8px/4px + brightness 200%。
- 骨架：initParticle 写数组 → update 里 noise3D 算角 → lerp 速度 → lineTo 短线 → drawGlow 自绘两次 → rAF。
- 依赖：simplex-noise。出处：ui.aceternity.com/components/vortex。

## 62. Animated Tooltip 头像组提示（Aceternity）
- 机制：头像 -mr-4 重叠；悬停时提示的横移与倾斜由同一根 x 运动值映射后各包一层 spring 驱动；mousemove 先经 rAF 节流再写 x。
- 参数：跟随 spring 100/15；rotate [-100,100]→[-45,45]、translateX→[-50,50]；提示入场 spring 260/10（y 20、scale 0.6 起）；头像 h-14 圆、白边 2。
- 骨架：rAF 节流 x.set(offsetX - 宽/2) → rotate/translateX 两个 useSpring(useTransform) → AnimatePresence 按 hoveredIndex 渲染提示。
- 依赖：motion/react。出处：ui.aceternity.com/components/animated-tooltip。

## 63. Resizable Panels 拖拽分栏（react-resizable-panels）
- 机制：布局 = 一份「Panel id → 百分比」映射由 Group 持有；拖 Separator 时按指针增量成对调整相邻两 Panel 并夹紧 min/max，约束失败则整次拖拽撤销弹回。
- 参数：现版 4.24.1，API 为 Group/Panel/Separator（v2 的 PanelGroup 命名已旧）；defaultSize 数字=px、字符串=百分比；Separator 双击恢复默认；resizePreviewMode 可选"只动分隔条、松手生效"；onLayout 指针拖拽时松手才回调（适合存 storage）。
- 骨架：<Group onLayout><Panel id defaultSize minSize maxSize/><Separator/><Panel/></Group>；命令式句柄 getLayout/setLayout。
- 依赖：仅 React。出处：github.com/bvaughn/react-resizable-panels（README，main）。

## 64. Drawer 手势抽屉（Vaul）
- 机制：Radix Dialog 底座 + 拖拽直接改 transform 跟手；松手按速度或距离判定关闭/回弹/吸附 snap 点；开抽屉时页面 wrapper 同步缩小下移（背景联动）。
- 参数：时长 0.5s、缓动 [0.32,0.72,0,1]（与 FLIP 卡同一条曲线）；速度阈值 0.4、关闭距离阈值 25% 高；背景 scale=(屏宽-26)/屏宽、圆角 8；snapPoints 例 [0.2,0.5,0.8]。松手判定分支为推断、常量已核对。
- 骨架：onDrag 位移 dampen 后写 transform + 同步遮罩透明度 → onRelease 比阈值 close()/snapToPoint（useSnapPoints 管偏移）。
- 依赖：@radix-ui/react-dialog。出处：github.com/emilkowalski/vaul（src/constants.ts 等）。

## 65. Text Hover Effect 文字描边光效（Aceternity）
- 机制：同一段文字叠三层 SVG text：底层灰描边（悬停才显）、中层描边做 dashoffset 1000→0 的 4s 描画入场、上层彩色渐变描边套以光标为圆心的径向渐变 mask，只在光标附近显色。
- 参数：viewBox 300×100、描边宽 0.3；mask 半径 20%、位置按光标百分比算（cx/cy%）；渐变 5 色标（黄红蓝青紫）；源码注释留有 spring 备选 300/50。
- 骨架：svg onMouseMove setCursor → defs 里 motion.radialGradient animate 到 mask 位置 → mask 套上层 text（stroke=url(#渐变）)。
- 依赖：motion/react。出处：ui.aceternity.com/components/text-hover-effect。

---

# 第六批 · 2026-11-21（扫尾批，编号续前）

## 66. GSAP ScrollTrigger 横向滚动
- 机制：纵向滚动区间内 pin 钉住外层，scrub 把滚动进度直映射成内层轨道 x 平移，滚完才放行。
- 参数：pin true、start "top top"、end "+=轨道超宽"、scrub 1（1s 追上滚动；0.5 更跟手）、anticipatePin 1；横向位移量 = 轨道宽-视口宽（通式为推断、配置项已核对）。
- 骨架：gsap.to(track, {x: () => -(scrollWidth-innerWidth), ease:"none", scrollTrigger:{trigger, pin, scrub:1, start:"top top", end:() => "+=" + 超宽， invalidateOnRefresh}})。
- 依赖：gsap + ScrollTrigger。出处：gsap.com/docs/v3/Plugins/ScrollTrigger。

## 67. Lenis 平滑滚动
- 机制：不直接改 scrollTop——拦截 wheel/touch 的目标值，每帧 lerp 逼近后写回原生滚动，所以 ScrollTrigger 读到的仍是真实位置。
- 参数：duration 1.2、lerp 0.1、默认缓动指数衰减式；syncTouchLerp 0.075、触摸惯性倍率 35；autoRaf 默认关需手动 raf。
- 骨架：new Lenis({autoRaf:true})；lenis.on('scroll', ScrollTrigger.update)；gsap.ticker.add(t => lenis.raf(t*1000))；lagSmoothing(0)。
- 依赖：lenis（v1.3.17+）。出处：github.com/darkroomengineering/lenis。

## 68. 滚动驱动 SVG 路径绘制
- 机制：useScroll 的 0→1 进度直驱 motion.path 的 pathLength（归一化属性，不用算总长），线随滚动"画"出来；可再包 spring 平滑（100/30）。
- 参数：offset ["start end","end start"]；pathLength 0→1。
- 骨架：const {scrollYProgress} = useScroll({target, offset}) → <motion.path style={{pathLength: scrollYProgress}}/>。
- 依赖：motion/react。出处：motion.dev/docs/react-scroll-animations、/react-svg-animation。

## 69. 环形进度 + 迷你折线（Magic UI 环形实测；折线为推断）
- 机制：环形 = 圆周长 2πr 按百分比截 strokeDasharray 显弧长，rotate(-90°) 从顶部起画；折线 = pathLength 生长（与卡 68 同机制，折线部分未在同一组件核对、属推断）。
- 参数：r 45、周长 2π×45、dasharray `${百分比弧长} ${周长}`、过渡 stroke-dasharray 1s ease、线宽默认 10、尺寸 100×100；挂载后 100ms 写值触发过渡。
- 骨架：<circle r={45} strokeDasharray transform="rotate(-90 50 50)" style={{transition}}/>。
- 依赖：React。出处：magicui.design/docs/components/animated-circular-progress-bar。

## 70. 头像堆叠组 Avatar Circles（Magic UI）
- 机制：flex 横排 + -space-x-4 负间距重叠，白描边分隔，超出收进 "+N" 徽章。注：该源码无悬停展开，上浮/展开间距是通用推断、未核对。
- 参数：头像 h-10 w-10、border-2（dark 边 gray-800）；numPeople 默认 99。
- 骨架：<div flex -space-x-4>{avatars.map(<Avatar h-10 w-10 border-2><AvatarImage/></Avatar>)}<Avatar>+{N}</Avatar></div>。
- 依赖：shadcn Avatar。出处：magicui.design/docs/components/avatar-circles（SmoothUI 同名页 404，改用此卡）。

## 71. 多页页面转场 @view-transition
- 机制：新旧两页 CSS 各写 @view-transition {navigation: auto}，同源跨页导航时浏览器自动截快照做过渡，零 JS；不支持的浏览器忽略该规则按普通跳转（降级为推断）。
- 参数：types 可分组（如 slide）；仅同源、无跨域重定向；兼容 Chrome/Edge 126+、Safari 18.2+、Firefox 仅 Nightly。
- 骨架：@view-transition{navigation:auto} ::view-transition-old(root){animation: move-out .3s both} ::view-transition-new(root){animation: move-in .3s both}。
- 依赖：纯 CSS。出处：MDN @view-transition。

## 72. SplashCursor 流体溅射（React Bits）
- 机制：全屏 WebGL2 实时流体模拟（Navier-Stokes）：指针产生 splat 注入染料+速度，每帧 curl→涡量→散度→压力（Jacobi 迭代 20 次）→减梯度→平流，最后 bloom 输出。是 78 张里最重的一张。
- 参数（高档）：模拟分辨率 128、染料 1440、压力迭代 20、CURL 30×swirl、SPLAT_FORCE 6000×force、dt 钳 1/60；bloom 分辨率 256、强度 0.8×glow、阈值 0.3；开场 1100ms 两次 burst。
- 骨架：pointermove → splat(x,y,dx×F,dy×F)；帧循环各 pass 依次跑手写着色器。
- 依赖：WebGL2 手写 shader（无 three）。出处：reactbits.dev/animations/splash-cursor。

## 73. SVG 形状变形 Morph
- 机制：对两条 path 的 d 插值。Motion 要求两路径指令数量与类型相同才可直插；不同则 flubber 先重采样成等点数、按最短距离对齐子路径并旋转起点，再逐点插值。
- 参数：flubber interpolate(from,to,{maxSegmentLength, single})；interpolateAll 管多子路径。
- 骨架：<motion.path animate={{d:"…"}}/> 或 const f = interpolate(d1,d2); d = f(t)。
- 依赖：motion 或 flubber。出处：motion.dev/docs/react-svg-animation、github.com/veltman/flubber。

## 74. GSAP SplitText 逐行入场
- 机制：文本拆行/词/字三层 span，每行外包 overflow:hidden 的 mask，行内从 y:100% 升到 0、stagger 错开——"逐行升起"。mask 是真裁切（与 visibility 隐藏不同）。
- 参数：type 默认 "chars,words,lines"；mask:"lines"；示例 gsap.from(lines, {y:100, autoAlpha:0, duration:1, stagger:0.05})；autoSplit 响应式重拆。SplitText 自 GSAP 3.13 起并入核心免费（与卡 7 的 Club 口径已更新：现为免费）。
- 骨架：SplitText.create(".quote", {type:"words,lines", mask:"lines", autoSplit:true}) → gsap.from(split.lines, …)。
- 依赖：gsap。出处：gsap.com/docs/v3/Plugins/SplitText。

## 75. 噪点 Grain 覆盖层
- 机制：静态版 = SVG feTurbulence 生成 Perlin 噪声纹理当全屏覆盖层；动画版 = 不停跳换噪声位置造胶片闪烁（React Bits 版用 rAF 随机位移，steps 跳帧写法为通用推断）。
- 参数：feTurbulence baseFrequency 0.9、numOctaves 2、stitchTiles stitch，去色后 opacity 约 0.06；React Bits Noise：瓦片 256、默认 opacity 0.2、fps 24、blend overlay、离屏暂停、减动效停。
- 骨架：<svg><filter id="n"><feTurbulence …/><feColorMatrix saturate 0/></filter></svg> + .grain{position:fixed; inset:0; filter:url(#n); pointer-events:none}。
- 依赖：无（纯 SVG/CSS）。出处：MDN feTurbulence、reactbits.dev/animations/noise。

## 76. Preloader 开场加载——整卡推断
- 说明：未找到可逐行核对的单一开源实现（GitHub 搜索受限、候选仓库 404），整卡为通用做法推断，参数非实测。
- 机制（推断）：全屏遮罩 + 计数 0→100（补间或按真实加载进度），到 100 后幕布上揭（yPercent -100 或 clip-path），同时解锁滚动、接首屏入场。
- 参数（推断）：计数 1.5–2.5s、揭开 0.8–1s、到 100 后延迟 0.1–0.3s；期间 body overflow hidden。
- 骨架（推断）：gsap.to(obj{v:0}→100, onUpdate 写百分比， onComplete 幕布 yPercent:-100）。

## 77. 亮暗主题圆形扩散切换（Magic UI Animated Theme Toggler）
- 机制：记点击坐标，startViewTransition 包住换主题，ready 后给新快照加 clip-path circle 从点击点 0 扩到覆盖全屏半径（hypot 斜边）；无 API 时同步直切。
- 参数：默认 400ms（示例 600）；variant 可选 circle/rectangle/polygon/blur/gif；shape 7 种；fromCenter 默认关（从点击点起）。
- 骨架：const t = document.startViewTransition(() => setTheme(next)); t.ready.then(() => documentElement.animate({clipPath:[circle(0 at x y), circle(R at x y)]}, {duration, pseudoElement:"::view-transition-new(root)"}))。
- 依赖：next-themes + View Transition API。出处：magicui.design/docs/components/animated-theme-toggler。

## 78. 滚动进度指示（Motion 官方）
- 机制：同一个 scrollYProgress 驱动两形态：顶条 scaleX（origin-left）；环形按钮用同进度驱 dashoffset 并点按回顶（环形为与卡 69 公式的组合推断）。
- 参数：顶条 useSpring(100/30, restDelta 0.001)、fixed top-0 h-1 origin-left；旋转变体 useTransform [0,1]→[0,360]。
- 骨架：const scaleX = useSpring(useScroll().scrollYProgress, {stiffness:100, damping:30}) → <motion.div style={{scaleX}} className="fixed inset-x-0 top-0 origin-left"/>。
- 依赖：motion/react。出处：motion.dev/docs/react-scroll-animations（Magic UI scroll-progress 同款佐证）。
