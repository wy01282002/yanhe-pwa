# 素材清单

开屏与书房现已默认改用**你提供的整图**：开屏是 `public/opening-full.webp`（含桂花树与灯盏的完整场景），书房是 `public/assets/study/study-bg.webp`。代码里的 SVG 叠层（`OsmanthusTree.tsx` 树、`StudyRoom.tsx` 线稿）默认关闭（见 `src/pages/SplashPage.tsx` 的 `SPLASH_USE_SVG_TREE`、`src/pages/StudyPage.tsx` 的 `STUDY_USE_SVG`，设为 `true` 可恢复）。
下面按文件名列出规格，放进对应目录即可自动生效（代码里已留好读取路径与失败兜底：文件缺失时开屏回退墨绿渐变、书房隐藏 image 仅剩热区，均不报错）。

## 0. 开屏完整场景图（含桂花树与灯盏 · 必放）

| 项 | 值 |
| --- | --- |
| 路径 | `public/opening-full.webp`（旧的 `opening-bg.webp` 不再使用） |
| 尺寸 | **1080 × 1920 px（9:16）**，任意比例也能放（见下「坐标规则」） |
| 格式 / 体积 | WebP q88，建议 ≤ 1.5 MB |
| 内容 | **整张画完的夜晚场景**：夜空与星、右上月亮、三道远山、雾带、**桂花树、燃着暖光的灯盏**、落花与暗角。树与灯画在图上即可，代码不再叠加 SVG 树 |
| 坐标规则 | 画框在手机上铺满全屏、宽视口居中成 9:16 竖幅，底图用 `cover` 铺（不会拉变形）。所有叠层坐标都写在**图片自身的百分比**上（左上 0,0 / 右下 100,100），由 `src/lib/scene/cover.ts` 换算进画框，换比例、换机型都不跑位。**灯芯位置**：改 `src/pages/SplashPage.tsx` 顶部 `LAMP_POS = { xPct, yPct }`（当前 59 / 53），暖光落在这里，镜头推进也朝这里放大 |
| 失效兜底 | 文件缺失 / 加载失败 → 显示 CSS 墨绿渐变（不报错、不空屏），点任意位置照常推进书房 |

想恢复原来的「树从虚无生长 + 点火」动画：把树上/灯从图里去掉并放回 `opening-bg.webp`，再把 `SPLASH_USE_SVG_TREE` 设回 `true`。

## 1. 书房背景（必放）

| 项 | 值 |
| --- | --- |
| 路径 | `public/assets/study/study-bg.webp`（兜底 `study-bg.jpg`） |
| 尺寸 | 1080 × 1920 px（9:16，当前用的就是这张），或 1080 × 2340 等任意竖图 |
| 格式 | WebP，质量 78~82；备一张 JPG |
| 体积 | ≤ 1.5 MB |
| 内容 | 深墨绿书房：左侧月光窗，中间书桌，桌面左为日记、中为音乐、右为一盏**常亮暖光**的铜座灯；右侧墙上挂画，中间墙上铜镜 |
| 关键点位 | 热区坐标是**图片自身的百分比**（`src/pages/StudyPage.tsx` 的 HOTSPOTS）：日记 (25.1, 65.6)、音乐 (52.0, 66.8)、聊天 (74.5, 56.4)、铜镜 (51.5, 42.3)。这些只是出厂默认值 —— **换图后不用改代码**：手机上长按画面空白处（别按热区）约 0.65 秒进入校准模式，手指把四个圈拖到物件正中，松手即存进 localStorage（key `yanhe.study.hotspots.v1`），点「完成」退出；点「还原」回到出厂点位。校准模式下会显示 10% 网格，工具条实时显示当前圆点的图片百分比，方便记录 |
| 暗角 | 请自带四周暗角（vignette）；图上再叠一层极淡的灯位暖光 `.study-lamp-glow`（跟随灯热区，可整体调淡） |
| 失效兜底 | 文件缺失 → image 自动隐藏，热区照常可点 |

想恢复原 SVG 线稿：把 `src/pages/StudyPage.tsx` 的 `STUDY_USE_SVG` 设回 `true`。

## 2. 开屏音效（可选，缺省静音）

| 路径 | 用途 | 规格 |
| --- | --- | --- |
| `public/assets/audio/lamp-ignite.mp3` | 点亮灯盏时的「擦火 / 灯火升起」 | 单声道 44.1 kHz，≤ 1.2 s，≤ 80 KB，末尾自然衰减 |
| `public/assets/audio/night-ambient.mp3` | 开屏环境底噪（夜风、远虫鸣） | 单声道 44.1 kHz，10~20 s 可循环，≤ 400 KB |

> 移动端浏览器要求**用户手势后**才能播放音频。代码在「点击灯盏」时才尝试播放，未放文件不会报错。

## 3. 应用图标（PWA）

| 路径 | 尺寸 |
| --- | --- |
| `public/icons/icon-192.png` | 192 × 192 |
| `public/icons/icon-512.png` | 512 × 512 |
| `public/icons/icon-maskable-512.png` | 512 × 512，主体留 20% 安全边距 |

底色 `#14201B`，图案建议用 `public/icons/favicon.svg` 的灯盏（旧金 #B8955A）。

## 4. 聊天背景（阶段 2）

| 路径 | 规格 |
| --- | --- |
| 用户上传，存 IndexedDB | 建议最长边 ≤ 1600 px，WebP/JPG，≤ 800 KB；设置页可调模糊 / 暗化 |
