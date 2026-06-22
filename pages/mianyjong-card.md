# 免佣卡 Page

固定玩法页面规范。Claude 出图时必须完整读取本文档后再动手。

## 资产基础 URL

所有资产统一使用 GitHub raw URL（Agent 沙箱中无本地资产文件）：

```
https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/
```

**字体 @font-face 声明（HTML 输出时必须包含）：**
```css
@font-face {
  font-family: 'MFYuanHei';
  src: url('https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/fonts/MFYuanHei-Boldtalic.ttf') format('truetype');
  font-weight: bold;
  font-style: italic;
}
```

---

## 基本信息

| 项目 | 值 |
|------|---|
| 玩法名称 | 免佣卡 |
| Figma 文件 | `wGKbcvDNuk8BFGMYGSixkZ` |
| 设备 | iPhone 14，375px 宽，逻辑像素 |
| 状态数 | 4（未开始 / 进行中 / 已结束·获奖 / 已结束·未获奖）|

---

## 页面状态定义

| 状态 | Figma Node | 说明 |
|------|-----------|------|
| 未开始 | `19:204` | 底部显示价格 + 立即购买按钮 |
| 进行中 | `20:349` | 显示司机参与数据 + 活动进度时间轴 + 默认文案按钮 |
| 已结束·获奖 | `23:877` | 显示获奖插画 + 累计收益 + 无购买按钮 |
| 已结束·未获奖 | `28:359` | 显示遗憾插画 + 无购买按钮 |

---

## 页面模块结构（从上到下）

### 所有状态共有模块

#### 1. App Bar（top:0 h:88px）
- 背景透明，叠在头图之上
- 状态栏：`PingFang SC Semibold 15px #222`，图标深色
- 标题：`PingFang SC Semibold 18px #222` 居中
- 返回箭头：`#222`

#### 2. 头图背景（top:0 h:485.667px）
- 资产：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/hero-bg.png`（Figma: `img1`）
- z-index: 0，铺满宽度

#### 3. 主标题（top:96px left:18px w:260px h:72px）
- 字体：`MFYuanHei Bold Italic 26px`，行高 `36px`
- 第一行："北京主城热区"，颜色 `#49220a`
- 第二行："特惠快车单单免佣"，颜色 `#ff2008`
- z-index: 1

#### 4. 奖励标识（top:118px left:265px 110×110px）
- 资产：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/badge-free.png`（Figma: `img5`）
- **z-index: 2，必须在编组24之上**

#### 5. 编组24 活动时间卡（top:188px left:12px w:351px h:77px）
- 资产：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/meta-card-bg.png`（Figma node `33:324`，`img241`）
- z-index: 1（低于奖励标识）
- 内部布局（Figma node `33:328`）：
  - `padding: 15px 12px 16px 14px`
  - 活动时间行：`PingFang SC Regular 14px #222`，`line-height:normal`
  - 活动区域行：`margin-top:6px`（视觉间距 6px）
  - "特定区域"：`PingFang SC Semibold 14px #FF6435`
  - "查看"胶囊：`background:#f8e0c1 border-radius:8.5px h:17px px:6px`，`Regular 12px #222`

---

### 未开始专属模块（top:275px 往下）

#### 6. 活动权益卡（top:275px left:12px w:351px h:250px）
- Figma node: `13:240`
- 背景：`#fff border-radius:16px shadow: 0 4px 12px rgba(15,40,77,0.06)`
- **标题区（h:52px）**：
  - 底纹图：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/title-bg.png`，`bottom:8px 居中 w:152px h:15px`
  - "免佣卡"：`MFYuanHei Bold Italic 19.8px #49220b letter-spacing:0.7071px`
  - "3"：`Barlow Semi Condensed Bold 30px #ff2008 letter-spacing:1.0714px`
  - "项权益"：`MFYuanHei Bold Italic 19.8px #ff2008 letter-spacing:0.7071px`
- **权益列表（top:65px left:16px，flex column gap:8px）**：
  - 每条：`w:319px h:50px background:#f9f8f5 border-radius:12px padding:0 16px gap:12px`
  - 图标：20×20px
  - 文字：`PingFang SC Semibold 16px`，部分文字橙色 `#fa6400`
  - 权益1图标：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-3.png`（img21）→ 乘客实付金额100%<橙>给到司机
  - 权益2图标：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-2.png`（img11）→ 免佣保底: <橙>免佣金额≥劳动报酬10%
  - 权益3图标：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-1.png`（img4）→ 亏必补！没回本即<橙>返</>购卡金额<橙>价差

#### 7. 活动要求卡（top:535px left:12px w:351px h:368px）
- Figma node: `16:263`
- `padding:16px 16px 20px`，`display:flex flex-direction:column align-items:center gap:12px`
- **标题行**（Figma node `16:260`，宽 233px）：
  - 左渐变线：`72.5px h:0.5px from:#fff to:#c8c8c8`
  - 标题文字：`PingFang SC Semibold 16px #222`，`flex:1 text-align:center`
  - 右渐变线：`72.5px h:0.5px from:#c8c8c8 to:#fff`
- **活动区域行**：`PingFang SC Regular 14px #222`，"主城及黄岛区" `#ff6435 Semibold`，"（青岛市）" `#556b9f`
- **地图区**：`w:319px h:158px border-radius:24px overflow:hidden background:#e8edf2`，固定切图 `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/map-area.png`（编组_3），直接引用不手绘，图片本身也加 `border-radius:24px`
- **活动区域行**：`text-align:left`（左对齐，非居中），`PingFang SC Regular 14px #222`，"主城及黄岛区" `#ff6435 Semibold`，"（青岛市）" `#556b9f`
- **标签行**（三行分组，每行 = 字段标签 + tags 横排）：
  - 行结构：`display:flex; align-items:flex-start; gap:8px; width:100%`，行间距 `12px`
  - 字段标签：`PingFang SC Regular 14px #222 white-space:nowrap line-height:27px`（与 tag 垂直居中）
  - 标签样式：`border:0.5px solid #8a99bc border-radius:5px padding:4px font-size:13px color:#556b9f Semibold`
  - 三行内容：
    - 订单类型：特惠快车 / 特惠快车仅轻快单
    - 接单方式：实时单
    - 顺路类型：非顺路单

#### 8. 活动说明卡（top:912px left:10px w:351px）
- Figma node: `19:207`
- `padding:16px`（四边均16px）
- 标题与正文**无间距**（`.desc-card .sec-title-row { margin-bottom:0 }`，gap:0）
- 正文：`PingFang SC Regular 14px #4b4b4d line-height:1.6 w:319px`
- 距底部按钮：`10px`（由 phone `padding-bottom:105px` 保证）

#### 9. 底部按钮区（fixed bottom）
- Figma node: `23:876`
- 容器：`h:75px background:#fff border-radius:16px 16px 0 0 shadow:0 -2.5px 2.5px rgba(0,0,0,0.05) padding:6px 20px`
- **价格区**（左侧，grid叠加结构）：
  - `¥`：`Barlow Semi Condensed Bold 18px #222`，`align-self:flex-end`（无左侧margin）
  - `1`：`Barlow Semi Condensed Bold 36px #222 line-height:35px`
  - `.45`：`Barlow Semi Condensed Bold 22.5px #222 line-height:35px`，`position:relative; top:6px`（下移6px对齐整数）
  - **三者底部对齐**（`align-items:flex-end`），小数部分额外下移 6px
  - 停止购买：`PingFang SC Regular 14px #757575 margin-top:4px`
- **立即购买按钮**（右侧）：
  - `w:135px h:50px border-radius:25px`
  - 背景：切图（Figma `img`，渐变橙色按钮）
  - ⚠️ **底色不得仅靠切图**：渐变切图缺失/为占位时，必须回退 `--primary` 实色 `#FF6435`（见 `components.md` Button Primary），**禁止用空 SVG / 透明图占位**，否则按钮会"白字透明底"隐形（见 workflow「资产与可见性门」复盘案例）。
  - 文字：`PingFang SC Semibold 18px #fff 居中`
- **Home Indicator**：`w:134px h:4px background:#000 opacity:0.18 border-radius:100px`，容器 h:20px

---

### 进行中专属模块

> 头图区域（App Bar / 头图背景 / 主标题 / 奖励标识 / 编组24活动时间卡）与未开始完全一致。

**布局策略：**
- `hero-area` 容器：`position:relative height:275px`（meta-card bottom:265 + 间距10px）
- 所有卡片放入 `.cards` 文档流容器，`padding:0 12px gap:10px`
- driver-card 是 cards 第一个子元素，紧接 hero-area，间距=10px ✓

---

#### 司机参与情况卡（Figma node: 23:657）
- 容器：`padding:20px 16px 15px background:#fff border-radius:16px`
- **两列数据区**（`align-items:flex-start justify-content:space-around margin-bottom:20px`）：
  - 标签行：`height:14px display:flex align-items:center gap:4px margin-bottom:8px`，`PingFang SC Regular 14px #757575`
  - 左列 — 累计生效：数字 `Barlow Semi Condensed Bold 30px #222 line-height:1`，单位"单" `PingFang SC Regular 14px #444`，`align-items:flex-end`
  - 右列 — 累计收益：标签旁 info 图标 `13×13px`；翻牌格 `w:25px h:36px background:rgba(253,225,193,0.68) border:0.5px solid rgba(166,66,1,0.05) border-radius:2px margin-right:3px`；数字 `Barlow Semi Condensed Bold 30px #752e00`；"." `PingFang SC Semibold 16px #752e00 top:2px`；".00" `Barlow Semi Condensed Bold 16px #752e00`；"元" `PingFang SC Semibold 14px #752e00 top:2px`
- **分割线**：`width:319px height:0.5px background:#EBECF2 margin-bottom:14.5px`
- **订单链接行**：左"订单获奖情况" + 右"查看"+箭头图标，`PingFang SC Regular 14px #757575`

#### 活动进度卡（Figma node: 23:689）
- 容器：`padding:16px background:#fff border-radius:16px`，**高度随内容自适应（不写死）**
- 标题：`PingFang SC Semibold 16px #222 width:224px margin:0 auto 16px text-align:center`
- **时间轴**（`position:relative height:248px width:319px margin:0 auto`，绝对坐标）：
  - 3个时段 top 分别：`0 / 88px / 176px`
  - 圆点（激活）：Figma URL `22110183`，16×16px，`top:1.5px`
  - 圆点（未激活）：Figma URL `c6de2519`，16×16px，`top:1px`
  - 竖线：Figma URL `3399d499`（橙色虚线切图），`position:absolute left:7px width:1px height:68.5px`；时段1 `top:18.5px`，时段2 `top:17px`；最后一段无竖线
  - 时间行：`position:absolute top:0 left:20px`（圆点16px+间距4px）
  - 胶囊：`background:#fde1c1 border:0.6px solid rgba(166,66,1,0.05) border-radius:9.5px height:19px padding:1px 5px 1px 1px`
  - 时钟图标：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-clock.png`（透明背景PNG切图），`width:17px height:17px display:block flex-shrink:0`
  - 日期文字：`Barlow Semi Condensed Bold 16px #222 margin-left:5px`
  - 时间范围：`Barlow Semi Condensed Bold 16px #222`
  - 进行中描述框：`position:absolute top:27px left:20px right:0`，`background:linear-gradient(to right,#ff6435,#ff9550) border-radius:8px min-height:45px padding:12px 16px`，文字 `PingFang SC Semibold 16px #fff`
  - 未开始描述框：`background:#f9f7f9`，文字 `PingFang SC Semibold 16px #999`

#### 活动要求卡 / 活动权益卡 / 活动说明卡
完全复用未开始规范，在 `.cards` 文档流里排列，`gap:10px` 统一间距。

#### 底部按钮（进行中）
- `height:75px background:#fff border-radius:16px 16px 0 0 padding:0 20px`
- 按钮：`width:335px height:50px background:#FF6435 border-radius:1000px`，`PingFang SC Semibold 18px #fff`

### 已结束专属模块

> 布局策略与进行中一致：`hero-area height:275px` + `.cards` 文档流 `gap:10px`，**无底部按钮**，页面 `padding-bottom:50px`（home indicator 34px + 间距 16px）。

#### 结果卡（替换进行中的司机参与情况卡）
- 容器：`padding:20px 16px 15px background:#fff border-radius:16px`
- 内部行（`display:flex align-items:center gap:12px margin-bottom:20px`）：
  - 插画：`width:57px height:77px object-fit:contain`
  - 文案：`PingFang SC Semibold 16px #222 line-height:1.5`
- **已结束·未获奖**：插画 `img121`（Figma URL `6227992b`），文案："很遗憾未获得奖励，下次记得早点参与～"
- **已结束·获奖**：插画 `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/driver-thumb.png`，文案："恭喜您累计获得**4单免佣**，累计收益**30元**"（数字橙色 `#FF6435`）
- 分割线：`width:319px height:0.5px background:#EBECF2 margin-bottom:14.5px`
- 订单链接行：复用进行中样式

#### 活动进度卡（已结束）
- 完全复用进行中的时间轴结构
- **所有时段改为暂未开始**：3个时段均用 `tl-desc-pending`（`background:#f9f7f9 color:#999`）
- 圆点全部用未激活图（Figma URL `c6de2519`）

#### 活动要求 / 活动权益 / 活动说明
完全复用进行中规范，`gap:10px` 间距，活动说明标题与正文 `gap:12px`，底部 `padding:16px`。

---

## 资产对照表

| 用途 | 本地文件 | Figma 变量名 | 说明 |
|------|---------|------------|------|
| 头图背景 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/hero-bg.png` | `img1` | 头图渐变大图 |
| 活动时间卡背景 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/meta-card-bg.png` | `img241` / `33:324` | 编组24切图 |
| 奖励标识 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/badge-free.png` | `img5` | "免"字贴纸 |
| 权益标题底纹 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/title-bg.png` | `img3` | 标题背景装饰 |
| 权益图标1（100%给到司机） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-3.png` | `img21` | 奖励详情-免-2 |
| 权益图标2（免佣保底） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-2.png` | `img11` | 奖励详情-免-1 |
| 权益图标3（亏必补） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-1.png` | `img4` | 奖励详情-免 |
| 司机插画（获奖）| `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/driver-thumb.png` | — | 小滴竖大拇指 |
| 司机插画（未获奖）| `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/driver-sad.png` | — | 分组12 |
| 时间轴节点（激活）| `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/timeline-dot.png` | `imgGroup24` | 编组24节点，进行中时段 |
| 时间轴节点（未激活）| Figma URL `img22` | `img22` | 未开始时段节点 |
| 时间轴竖线 | Figma URL `img` | `img` | 连接节点的竖线 |
| 时间图标 | Figma URL `imgIcon` | `imgIcon` | 日期胶囊内图标 |
| 时间图标路径 | Figma URL `img5` | `img5` | 时间图标子元素 |
| 累计收益 info 图标 | Figma URL `imgIconFunFillCircleInfo` | `imgIconFunFillCircleInfo` | 累计收益标签旁 |
| 分割线 | Figma URL `img26` | `img26` | 司机卡内水平分割线 |
| 箭头图标 | Figma URL `imgArrowIconGreySizeLIconArrow` | `imgArrowIconGreySizeLIconArrow` | 查看箭头 |
| 地图切图 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/map-area.png` | `编组_3` | 活动区域地图，圆角24px |
| 字体 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/fonts/MFYuanHei-Boldtalic.ttf` | — | 标题/数字专用字体 |

---

## 出图注意事项

1. **z-index 顺序**：头图(0) < 编组24活动时间卡(1) < 奖励标识(2) < AppBar(20)
2. **编组24** 是活动时间卡的背景切图，时间轴节点图标另见 `timeline-dot.png`，两者不要混淆
3. **活动时间卡内边距**：`pt:15px pl:14px pr:12px pb:16px`，两行间距 `margin-top:6px`（视觉间距6px）
4. **标题行宽度**：固定 `233px`，左右渐变线各 `72.5px`，文字 `flex:1 text-align:center`
5. **底部数字**：`¥` + 整数 + 小数三者 `align-items:flex-end` 底部对齐，小数 `position:relative top:6px` 下移，`¥` 无左侧margin
6. **活动说明卡**：标题行与正文间距 **18px**（`.desc-card .sec-title-row { margin-bottom:18px }`），底部 `padding:16px`
7. **地图切图**：`border-radius:24px`，容器和图片均设置，`overflow:hidden`，`background:#e8edf2`
8. **布局策略**：
   - **绝对定位元素（仅限 hero 区）**：头图背景(z:0)、主标题(z:1)、奖励标识(z:2)、meta 活动时间卡(z:1)，共 4 个，均相对 `.phone` 定位
   - **文档流**：App Bar(88px) + hero-spacer(**187px** = 275 − 88) = 275px，之后所有卡片（权益卡、活动要求卡、说明卡、司机卡、结果卡等）统一放入 `.cards` 容器走文档流，`padding:0 12px; gap:10px`
   - **⚠️ hero-spacer 高度计算公式**：`hero视觉高度(275) − app-bar高度(88) = 187px`，因为 app-bar 在文档流中已占 88px，spacer 只需补齐剩余高度
   - **禁止**对 `.cards` 内的卡片使用 `position:absolute`，避免 iframe 嵌套时的定位失效问题
9. **权益卡条目**：用 `flex column` 流式布局，`benefits-list { position:absolute top:65px }`，不用 `nth-child`
10. **头图裁切（防黑线）**：头图图片自然高 485.667px，但**必须裁切到头图区高度 275px**——`.hero-bg-wrap { height:275px; overflow:hidden }` + `.hero-bg { height:485.667px }`（显式自然高，不用 `height:100%` 配 cover 以免缩放）。头图底边本身偏深色，若不裁切会露在下方卡片背后形成"黑线/粉色角"。可再加 `.hero-fade`（头图底部向 `#F0F1F5` 渐隐）让接缝更干净。
11. **进度轴节点样式（必查表，勿凭记忆）**：
   - 生效中：外圈 `16×16px background:#FFE0D7` **无边框无描边** + 内圈 `6×6px #FF6435 居中(top:5px left:5px)`
   - 未开始/已结束：外圈 `16×16px background:#FFFFFF` **无边框无描边** + 内圈 `6×6px #FFCBBB 居中(top:5px left:5px)`
   - 内圈居中公式：`(外圈size − 内圈size) / 2 = (16−6)/2 = 5px`
   - 两者外圈颜色不同（`#FFE0D7` vs `#FFFFFF`），切勿都用 `#FFE0D7`，否则未开始与生效中难以区分。
12. **单时段渲染规范**：当奖励时段只有 1 个时，进度轴**不要沿用 3 段的固定绝对坐标**（`height:248px`、`top:0/88/176`），否则卡片高度错乱。改用**流式自适应**：进度卡高度随内容 hug，单段用 flex 行（左节点、右侧日期行+描述框上下排）。多段时用 flex column + 虚线 rail 连接（`border-left:1px dashed #FF6435`）。
13. **吸底栏间距（已结束有按钮时）**：吸底栏用 `position:sticky; bottom:0`，**不需为其预留整高 padding**（预留 110px 会导致末卡到按钮间距过大）；`.cards` 仅留 `padding-bottom:10px`（末卡到按钮 10px，符合规范），与"进行中"底部一致。
14. **滚动条**：全状态展示页 iframe 内 phone 滚动时可能露出深色滚动条（看似右侧黑线），用 `.phone::-webkit-scrollbar{display:none} / scrollbar-width:none` 隐藏。
