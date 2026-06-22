# 页面搭建工作流

基于 9.0 规范在 Figma 中搭建页面的标准流程。

---

## 🚦 出图标准检查清单（每次出图必须按顺序执行）

### 阶段一：出图前准备

- [ ] **00. 素材完整性校验（最先执行，不可跳过）**：拿到压缩包/素材后，先核对完整性再动手。**禁止只跑一次 `unzip` 就凭 stdout 判断**。
  - 用清单核对：`python3 -c "import zipfile; [print(i.file_size, i.filename.encode('cp437').decode('utf-8')) for i in zipfile.ZipFile('x.zip').infolist()]"`，比对**实际解出的文件数与字节大小**。
  - 含中文名的包必须用 Python `zipfile` 按 `cp437→utf8` 解码解压，`unzip` 易截断/乱码。
  - **任何 0 字节文件 = 解压失败信号**，必须重解，不得当作"空文件/缺失"。
  - 逐一确认 `SKILL.md` 资产索引、page 规范、`assets/fonts/`、`assets/images/` 中**每个被引用的文件都存在且非空**；若某个被引用文件"缺失/为空"，**先怀疑解压不完整并重解**，不要直接下结论说素材缺失或让用户重传。

- [ ] **0. 输出内容确认表并等待用户确认**，再写任何代码。确认表必须包含以下三部分，用户只需确认内容正确，无需了解组件细节；内部同步完成组件映射：

  **第一部分：页面清单（产出几个页面，每个页面有哪些状态）**
  ```
  ## 出图前确认

  ### 一、页面与状态

  | 页面 | 状态数 | 状态列表 |
  |------|--------|---------|
  | 免佣卡详情页 | 4 | 未开始 / 进行中 / 已结束·获奖 / 已结束·未获奖 |
  ```

  **第二部分：每个状态的模块内容**
  ```
  ### 二、各状态模块内容

  | 模块 | 未开始 | 进行中 | 已结束·获奖 | 已结束·未获奖 |
  |------|--------|--------|------------|--------------|
  | 顶部头图 | ✅ 相同 | ✅ 相同 | ✅ 相同 | ✅ 相同 |
  | 活动时间卡 | ✅ 相同 | ✅ 相同 | ✅ 相同 | ✅ 相同 |
  | 参与情况卡 | ❌ 无 | ✅ 累计生效+收益 | ✅ 获奖文案 | ✅ 遗憾文案 |
  | 活动进度 | ❌ 无 | ✅ 时间轴（有进行中） | ✅ 时间轴（全暂未开始） | ✅ 时间轴（全暂未开始） |
  | 活动权益 | ✅ 在活动概要下方 | ✅ 在活动要求后 | ✅ 在活动要求后 | ✅ 在活动要求后 |
  | 活动要求 | ✅ | ✅ | ✅ | ✅ |
  | 活动说明 | ✅ | ✅ | ✅ | ✅ |
  | 底部按钮 | ✅ 价格+立即购买（吸底） | ✅ 前往打开随心接（吸底） | ❌ 无 | ❌ 无 |
  ```

  **必确认项（确认表里必须逐条问，用户不答不出图）：**
  - **进行中默认渲染哪个子态**：未获奖（"免佣卡接单享免佣！"）/ 已获奖（累计生效+收益）/ 两者都出？
  - **已结束（获奖/未获奖）底部是否展示「前往打开随心接」**：page 规范默认无按钮，若产品要求展示需在此确认。
  - **活动进度时段数量与日期**：PRD 只给单时段就只出单时段（不得自行编造多时段日期）；如需展示多时段平铺，须用户提供真实日期。

  **第三部分：页面跳转关系**
  ```
  ### 三、页面跳转关系

  | 触发位置 | 触发动作 | 跳转目标 |
  |---------|---------|---------|
  | 活动时间卡「查看」 | 点击 | 活动区域地图弹窗 |
  | 参与情况卡「查看」 | 点击 | 订单获奖情况页 |
  | 底部「立即购买」 | 点击 | 购买确认页 |
  | 底部「前往打开随心接」 | 点击 | 随心接功能页 |
  ```

  内部同步维护的组件映射（不对用户展示）：
  ```
  活动时间卡 → 编组24切图背景 + 文字层
  参与情况卡 → 自定义卡片（无库组件对应）
  活动进度卡 → 自定义时间轴（无库组件对应）
  底部按钮   → Button Full-width Primary / 自定义价格+按钮
  ```
- [ ] **1. 阅读 PRD，整理文案清单**：把页面所有需要显示的文案逐条列出，后续每加一个文字节点都要核对，没有的文案一律不加

- [ ] **1.2 判断出图来源，每次出图必须明确声明，禁止不说明就直接复用旧 HTML**：

  | 情况 | 正确做法 |
  |------|---------|
  | skill 里有该玩法的 page 规范，且 PRD 文案与已有 HTML **逐条核查后完全一致** | 明确说明"复用已有 HTML，已逐条核查文案无差异"，再出图 |
  | skill 里有该玩法的 page 规范，但 PRD 文案**有差异**（新活动名/新区域/新文案等） | 读取 page 规范 `.md` + PRD 文案，逐条替换后出图 |
  | skill 里**没有**该玩法的 page 规范（全新玩法） | 读取 Figma 设计稿 + PRD，从头按规范生成 HTML，并建立新的 page 规范文档 |

  ⚠️ **禁止的做法**：
  - 不声明来源，直接复用旧 HTML
  - 说"文案完全匹配"但没有逐条核查
  - 用旧 HTML 而非 page 规范作为出图依据

  ⚠️ **复用参考稿的核查范围 = 文案 + 关键交互元素（按钮/CTA/链接）的"可见性与 token"**：
  - 禁止假设"旧稿既然存在就一定渲染正常"——参考稿可能携带潜伏 bug。
  - 反例（2026-06 复盘）：免佣卡全状态展示页的「立即购买」按钮 `background:none` + 依赖一张空 SVG 切图撑底，导致整颗按钮在白色吸底栏上"白字透明底"隐形；当时只核查了文案没核查按钮可见性，把雷一并沿用。复用时务必对每个按钮/CTA 跑「资产与可见性门」。
- [ ] **1.1 PRD 有歧义或信息缺失时，必须向用户二次确认，等待明确答复后再继续**：禁止自行推测或补全 PRD 未明确的内容；确认问题须一次性列出，不可逐条打断用户
- [ ] **2. 去规范库验证 skill**：打开规范库文件（`sxQXbLdcTfITwow8YCnmOJ`）查询当前组件列表，与 `components.md` 对比，发现不一致立即更新 skill，再开始出图
- [ ] **3. 做组件映射表**：把页面每个模块对应的库组件列出来，只有确认库里不存在对应组件时，才手动创建

### 阶段二：出图中规范
- [ ] **4. App bars 尺寸确认**：原始尺寸 375×88px，禁止 resize 高度，内容区起始 y 固定为 88，不使用 `instance.height` 推算
- [ ] **5. 按 CellCard 标准操作流程**（见下方）
- [ ] **6. 所有 Frame 通过 `createAutoFrame` 封装函数创建，禁止直接调用 `figma.createFrame()`**：
  ```js
  function createAutoFrame(name, layoutMode = 'VERTICAL') {
    const f = figma.createFrame();
    f.name = name;
    f.layoutMode = layoutMode;
    f.primaryAxisSizingMode = 'AUTO';
    f.counterAxisSizingMode = 'FIXED';
    f.fills = [];
    return f;
  }
  ```
- [ ] **7. 组件报错必须继续排查原因，不得降级为手动拼装**：Key 失效 → 尝试其他导入方式；setProperties 报错 → 检查属性值大小写和可选值
- [ ] **8. `visible=false` 节点若需脱离布局流，必须检查 `layoutPositioning`**：实例内部节点无法直接修改时，记录为已知限制，不绕过
- [ ] **9. 每完成一个模块立即截图验证**：不要等全部完成再看，发现问题及时修复

### 阶段三：出图后核对
- [ ] **10. 对照 PRD 文案清单逐条核对**：删除所有不在 PRD 里的文案和内容
- [ ] **11. 检查所有节点类型**：确认没有违规的手动 Frame/Text 拼装库组件

---

## ✅ 交付前自查清单（输出前逐项确认，不可跳过）

> 参考 br-fintech p0-rules 机制引入。每次生成完毕、截图/交付前，必须逐项过一遍。有任何一项为「否」，立即修复再交付。

### 渲染截图门（最高优先级，先于一切交付）
- [ ] **是否真实渲染并目视核对**？仅通过文案/assert 校验**不等于**视觉正确（黑线、卡片高度、节点样式、间距等只有渲染才能发现）。
- [ ] **若当前环境无法渲染**（沙箱无浏览器 / headless 渲染器崩溃 / Chrome 扩展未连接）：**必须显式声明"未做像素级自查"，并把视觉确认责任明确交回用户**，不得把"结构校验通过"当作"已完成视觉核对"。
- [ ] 渲染后逐项确认：头图无黑边/裂图、四（或多）个状态正常显示、进度轴节点（生效中 vs 未开始 颜色不同）、间距、吸底栏与末卡间距正常。
- [ ] **无法渲染时，交付消息里必须出现一句强制声明**（缺这句即视为流程未完成），例如：「当前环境无浏览器/未连 Chrome 扩展，未做像素级渲染自查，视觉确认请你打开文件目视核对」。

### 资产与可见性门（无需浏览器即可执行，先于交付）

> 渲染门被环境卡住时，这一门**仍必须跑**——它能在无浏览器情况下抓住"按钮隐形/裂图/幻觉色"这类问题。

- [ ] **空 / 占位资源校验**：所有被引用切图均非 1×1、非空 body SVG（`viewBox="0 0 1 1"` 无子节点）、非全透明像素；命中即判缺失资产并反查依赖元素。
- [ ] **按钮可见性**：每个按钮/CTA 底色来自品牌 token 实色（`#FF6435`），不依赖背景图；不存在"文字色==背景色"或文字叠在透明底上。
- [ ] **颜色幻觉**：页面出现的所有 hex 都能在 `colors.md` 找到，没有凭记忆/临时造的值（**含 bug 修复时新加的**）。

可直接套用的自查脚本（任一断言失败即不得交付）：

```python
import re
html = open('xxx.html', encoding='utf-8').read()
# 1) 空 SVG 占位（本玩法历史上立即购买按钮即栽在这里）
assert not re.search(r'viewBox="0 0 1 1"\s*>\s*</svg>', html), "存在空SVG占位资源"
# 2) 按钮底色幻觉/依赖图片：人工确认每个 .btn* 底色来自 token，而非 background:none + <img>
# 3) 颜色幻觉：页面 hex 必须都在 colors.md 的 token 集合内
tokens = {h.upper() for h in re.findall(r'#[0-9a-fA-F]{6}', open('references/colors.md', encoding='utf-8').read())}
unknown = {h.upper() for h in re.findall(r'#[0-9a-fA-F]{6}', html)} - tokens
assert not unknown, f"非规范颜色(疑似幻觉): {unknown}"
print("资产与可见性门通过")
```

### Token 正确性
- [ ] 所有颜色值是否来自 `colors.md` 中的变量，没有凭记忆写入的 hex？
- [ ] 是否使用了 `--text-primary: #222222`，没有出现 `#000000` 纯黑？
- [ ] 页面底色是否为 `#F0F1F5`，卡片色是否为 `#FFFFFF`，没有混用？
- [ ] 间距和圆角值是否与 `spacing.md` 中的 4px 阶梯吻合？

### 视觉质量（对应 custom-rules.md「视觉质量量化铁律」）
- [ ] 按钮/CTA 是否底色为 `#FF6435` 实色、文字清晰可见，没有依赖切图撑底导致隐形？
- [ ] 是否存在文字色与底色相同 / 文字叠透明底 的"看不见"元素？
- [ ] 是否所有切图都非空、非占位、非全透明？

### 组件规范
- [ ] 所有 UI 组件是否优先使用了库实例，没有手动拼装库中已有的组件？
- [ ] 所有组件实例是否已完成「组件三问」（文本填写 / 多余元素隐藏 / 属性修改）？
- [ ] 没有占位文（「正文内容示意」「副标题信息内容示意」等）残留在画面中？

### 文案与内容
- [ ] 页面内所有文案是否均来自 PRD 文案清单，没有 AI 自行补全的内容？
- [ ] 空状态是否只显示 PRD 指定文案，没有自行添加副文案？

### 布局结构
- [ ] Phone frame 宽度是否为 375px，没有使用 390px？
- [ ] 卡片宽度是否为 351px，左右各 12px margin？
- [ ] Home Indicator 颜色是否与背景匹配（浅背景→黑色，深背景→白色）？

---

## ⚠️  执行前必读

**在写任何 use_figma 代码前，必须完成 Step 1-2（检查文件 + 搜索导入组件）。**
**违反此流程的后果：手动拼装、字体不匹配、无法复用更新。**

## 开始前必读 ⚠️

**在写任何 use_figma 代码前，必须回答这些问题：**

- [ ] 我列出了页面需要的所有组件了吗？
- [ ] 我查过 `components.md` 索引表了吗？
- [ ] 我用 `search_design_system` 搜索过所有需要的组件了吗？
- [ ] 我导入了所有能用的库组件了吗？
- [ ] 我现在要 `createFrame` / `createText` / `createRectangle` 的东西，**确定库里没有**吗？

**如果任何一个答案是"否"，立即停下来，先完成它。**

违反这个检查清单的后果：
- 手动拼装的组件缺少系统图标、插图、正确的分割线实现
- 字体、字重、颜色不匹配规范
- 无法复用设计系统的更新
- 增加维护成本

## 核心原则

**必须优先使用组件库实例，禁止手动拼装已有库组件。**

手动用 Frame/Rectangle/Text 拼装会导致：
- 状态栏缺少系统图标（时间、电池、WiFi、信号）
- 分割线实现方式错误（矩形 vs inner shadow）
- 图片/插图丢失（灰色占位 vs 实际插图）
- 图标细节不准确（手绘 vector vs 原始图标）
- 字体/字重不匹配

## 标准流程

### Step 1：检查目标文件

```js
// 查看文件现有页面和内容
const pages = figma.root.children.map(p => `${p.name} id=${p.id}`);
return pages;
```

### Step 2：搜索并导入组件库

**这一步是强制性的，不可跳过。**

#### 2.1 列出页面所需的所有组件

在写任何代码前，先列出页面需要的所有 UI 元素：
- 导航栏 → App bars
- 按钮 → Button / Button Full-width
- 卡片 → card / CellCard
- 标签页 → tabs
- 标签 → Tag
- 提示条 → Alert
- 状态栏 → Status
- 返回按钮 → 返回
- 文字链接 → TextLink / ExpandLink
- 图标 → arrow
- 状态蒙层 → StatusOverlay

#### 2.2 查找 Component Key

**先查 `components.md` 中的 Component Key 索引表**，直接使用已知的 Key 导入。

如果索引表中没有需要的组件，再用 `search_design_system` 搜索：

```
search_design_system(
  query: "组件名",
  fileKey: "目标文件Key",
  includeLibraryKeys: ["lk-2da8834b5155e15c43aaff36eb3d6dfd723be0037508c3655ce7ee6ff6915f7514fbedede0db3112d0b39eec678b7cb800311cfbc5084939677f5fa9b42fff39"]
)
```

#### 2.3 一次性导入所有组件

**不要边写边导入**，先把所有需要的组件都导入好：

```js
// 一次性导入所有需要的组件
const appBarCompSet = await figma.importComponentSetByKeyAsync("c921a6bd...");
const buttonCompSet = await figma.importComponentSetByKeyAsync("cc277429...");
const cardComp = await figma.importComponentByKeyAsync("75e370d8...");
const tabsCompSet = await figma.importComponentSetByKeyAsync("tabs_key...");
// ... 导入所有需要的组件

// 然后再开始创建实例
```

### Step 3：导入组件

```js
// 组件集（有变体）
const compSet = await figma.importComponentSetByKeyAsync("component_key");
const variant = compSet.children.find(c => c.name.includes("Flat"));
const instance = variant.createInstance();

// 单一组件
const comp = await figma.importComponentByKeyAsync("component_key");
const instance = comp.createInstance();
```

### Step 4：创建页面框架

```js
const phone = figma.createFrame();
phone.name = "iPhone 14 - 页面名称";
phone.resize(375, 812);
phone.fills = [{ type: 'SOLID', color: { r: 240/255, g: 241/255, b: 245/255 } }]; // #F0F1F5
phone.layoutMode = 'VERTICAL';
phone.clipsContent = true;
```

### Step 5：放置库组件实例

按页面结构从上到下放置：

```js
// 1. App bars（导入 Flat 变体）
phone.appendChild(appBarInstance);
appBarInstance.layoutSizingHorizontal = 'FILL';

// 2. 内容区域（手动创建，因为是自定义布局）
const content = figma.createFrame();
content.layoutMode = 'VERTICAL';
phone.appendChild(content);
content.layoutSizingHorizontal = 'FILL';
content.layoutSizingVertical = 'FILL';

// 3. 底部按钮（导入 Button Full-width Primary Default 变体）
phone.appendChild(buttonInstance);
```

**⚠️ 关键检查点：**

在这一步，每次要 `createFrame()` / `createText()` / `createRectangle()` 前，问自己：
- **这个元素库里有现成的组件吗？**
- **我是不是应该用库组件实例而不是手动创建？**

如果不确定，回到 Step 2.1 的组件清单，或者重新搜索 `search_design_system`。

### Step 6：定制实例内容

```js
// 修改文本 — 先查找文本节点，加载字体后再修改
const titleNode = instance.findOne(n => n.type === 'TEXT' && n.characters === '标题文案');
// 先尝试加载原字体
try {
  await figma.loadFontAsync(titleNode.fontName);
} catch(e) {
  // 原字体不可用时替换为 Noto Sans SC
  await figma.loadFontAsync({ family: "Noto Sans SC", style: "Bold" });
  titleNode.fontName = { family: "Noto Sans SC", style: "Bold" };
}
titleNode.characters = "新标题";

// 切换变体
instance.setProperties({ "Property 1": "Flat" });
```

### Step 7：手动构建自定义部分

**只有组件库中不存在的部分才手动构建**，例如：
- CellCard 内的 Body 信息网格（自定义键值对布局）
- 自定义的内容组合
- 特定业务逻辑的排列

**再次确认：**
- 我要手动创建的这个元素，**真的**在组件库里找不到吗？
- 我检查过 `components.md` 的完整列表了吗？
- 我用 `search_design_system` 搜索过了吗？

如果以上任何一个答案是"否"，回到 Step 2。

### Step 8：截图验证

```
get_screenshot(fileKey, nodeId)
```

## 字体回退策略

9.0 规范指定字体为 PingFang SC，但该字体在部分 Figma 环境中不可用。

| 规范字体 | 回退字体 |
|---------|---------|
| PingFang SC Semibold | Noto Sans SC Bold |
| PingFang SC Regular | Noto Sans SC Regular |
| Barlow Semi Condensed | Barlow Semi Condensed（通常可用） |

回退步骤：
1. 先尝试 `loadFontAsync({ family: "PingFang SC", style: "Semibold" })`
2. 若失败，使用 `loadFontAsync({ family: "Noto Sans SC", style: "Bold" })`
3. 对库组件实例中的文本，需先设 `fontName` 再改 `characters`

## 常见错误清单

| 错误做法 | 正确做法 |
|---------|---------|
| 手动用 Frame 拼装 App bars | 导入库组件 Flat 变体实例 |
| App bars 之外额外单独放 Status 实例 | App bars 内已内嵌 Status，只放 App bars 一个实例即可 |
| 空白 Frame 做状态栏 | 使用库中的 Status 组件（已含时间/电池/WiFi/信号） |
| 矩形做底部分割线 | App bars Flat 变体自带 inner shadow 分割线 |
| 手绘 vector 做返回按钮 | 库中的 App bars 已包含标准返回图标 |
| 手动构建 Button Full-width | 导入库组件 Primary/Default 变体实例 |
| iPhone 框架高度被 HUG 压缩 | `resize(375, 812)` 后设 `primaryAxisSizingMode = 'FIXED'` |
| HORIZONTAL 布局行高默认 100px | 设 `counterAxisSizingMode = 'AUTO'` 使高度 HUG 内容 |
| 直接调用 `figma.createFrame()` | 必须用 `createAutoFrame()` 封装函数，默认 `primaryAxisSizingMode = 'AUTO'` |
| 组件 Key 失效或 setProperties 报错就改为手动拼装 | 必须继续排查：检查 Key 是否过期、属性值大小写、可选值枚举 |
| `visible=false` 认为不占布局空间 | `visible=false` 在 AutoLayout 中仍占位，需检查 `layoutPositioning` |
| 用库中语义不符的组件凑合（如用 Alert warning 表达 error） | 库中无合适变体时手动构建，手动构建是合规的最后手段 |


---

## 全状态展示页生成规范（iframe + srcdoc）

> 每次生成 `mianyjong-all-states.html` 前必须按此规范执行，不可跳过。

### ⛔ 资产与 iframe 铁律（2026-06 复盘新增，违反必返工）

1. **严禁残留 Figma 临时链接**：`https://www.figma.com/api/mcp/asset/...` 这类地址会过期且需登录，离线/预览环境一律裂图。所有切图必须内嵌为 base64（或本地相对路径）。生成后用 `grep -c 'figma.com/api/mcp'` 自查，**结果必须为 0**。
2. **iframe 一律用 `srcdoc`，禁止 `src="data:text/html;base64,..."`**：预览器/artifact 沙箱会拦截 iframe 的 `data:` 跨源导航，导致整页空白。把单状态 HTML 转义后放进 `srcdoc`。
3. **改写 iframe 标签时，尾部属性必须留在 `srcdoc` 之外**：`width/height/frameborder/style` 等若被正则吞进 srcdoc 内容，会以纯文字形式渲染出来。用带属性捕获的模式 `<iframe srcdoc="(.*?)"((?: [^>]*)?)></iframe>`，重建时 `'<iframe srcdoc="%s"%s></iframe>' % (escaped_inner, trailing_attrs)`。
4. **没有切图的小元素用 CSS/内联 SVG，不要置空**：信息图标 ⓘ、查看箭头 ›、进度轴圆点、连接虚线、按钮底色等是 Figma 矢量，置成透明会消失或裂开。这些用 inline SVG / CSS 绘制；按钮底色直接用品牌色 `#FF6435`，不依赖图片底。
5. **占位"透明"像素必须经校验**：常见 1×1 PNG 可能是 gray+alpha 的**不透明**像素，叠在有色背景上会盖住颜色。要隐藏元素就 `display:none`，需要透明就用确认过 alpha=0 的像素。
6. **交付前必须真实渲染截图自查**：不能只比对文案/代码。渲染后确认无裂图、四个 iframe 正常显示、进度轴/图标/按钮等关键组件可见，再交付。

### 进度轴组件规格（固定值，出图直接套用）

| 元素 | 规格 |
|------|------|
| 选中圆点 | 外圈 16px `#FFE0D7`，内圈 6px `#FF6435` |
| 未选中圆点 | 外圈 16px `#FFFFFF`，内圈 6px `#FFCBBB` |
| 连接线 | 1px 虚线，色值 `#FF6435` |
| 日期药丸时钟 | 用 `assets/images/icon-时间.png` |

> 「选中」= 含 `tl-desc-active`（生效中）的那一段；其余段用未选中样式。

### 核心原则

**单页预览文件**（`index.html`）和**全状态展示页**（`all-states.html`）是两套不同的 CSS 策略，不能混用。

| 文件类型 | bottom-bar | phone | 用途 |
|---------|-----------|-------|------|
| 单页预览 `index.html` | `position:fixed; bottom:0; left:0` | `padding-bottom:111px`，高度自适应 | 开发/真机使用 |
| 全状态展示 `all-states.html` | `position:sticky; bottom:0; left:0` | `height:844px; overflow-y:auto; padding-bottom:0` | 设计评审 |

### iframe 内 CSS position 行为对照

| 方案 | 在 iframe 内的行为 | 是否可用 |
|------|-----------------|---------|
| `position:fixed` + `transform:translateX(-50%)` | transform 创建新 stacking context，fixed 失效 | ❌ |
| `position:fixed` + `left:0` | 预览器/artifact 环境可能拦截 | ⚠️ 不稳定 |
| `position:absolute` + `bottom:0` | 若 phone 高度自适应，按钮跑到页面最底部（iframe 外） | ❌ |
| `position:sticky` + `bottom:0` | 在 `overflow-y:auto` 滚动容器内始终贴底 | ✅ |

> iframe 承载方式：用 `srcdoc`（✅）承载单状态 HTML；`src="data:text/html;base64"`（❌）会被沙箱拦截导致空白。

### 标准生成流程（必须严格执行）

每个单状态 HTML 先做 CSS 适配，再 `html.escape` 后放进 iframe 的 `srcdoc`：

```python
import html as _html, re

def adapt_state_css(html):
    # 1. phone 固定高度，内容区滚动
    html = re.sub(r'(\.phone\s*\{)', r'\1height:844px;overflow-y:auto;overflow-x:hidden;', html)
    # 2. padding-bottom 清零（sticky 不需要）
    html = re.sub(r'padding-bottom:\d+px', 'padding-bottom:0', html)
    # 3. bottom-bar 改 sticky
    html = re.sub(
        r'position:fixed[^;]*;[^;]*bottom:0[^;]*;[^;]*left:[^;]*;[^;]*(?:transform:[^;]*;)?',
        'position:sticky;bottom:0;left:0;',
        html
    )
    # 4. 资产铁律：确认无 figma 临时链接残留（应已全部内嵌 base64）
    assert 'figma.com/api/mcp' not in html, "存在未内嵌的 Figma 临时链接"
    return html

def make_iframe(state_html, attrs='width="375" height="844" frameborder="0" scrolling="no" style="display:block;vertical-align:top;border:none;"'):
    inner = adapt_state_css(state_html)
    # 用 srcdoc，禁止 src="data:text/html;base64"；尾部属性留在 srcdoc 之外
    return '<iframe srcdoc="%s" %s></iframe>' % (_html.escape(inner, quote=True), attrs)
```

### ⚠️ 修改后必须验证 iframe 内容

修改源文件后，展示页里的 iframe 内容是旧的，**必须重新生成展示页**，并反转义验证：

```python
import html as _html, re
with open('mianyjong-all-states.html') as f: doc = f.read()
# 禁止出现 data: URI 的 iframe
assert 'src="data:text/html' not in doc, "仍在用 data: URI，必须改 srcdoc"
assert 'figma.com/api/mcp' not in doc, "存在未内嵌的 Figma 临时链接"
for m in re.finditer(r'<iframe srcdoc="(.*?)"(?: [^>]*)?></iframe>', doc, re.S):
    decoded = _html.unescape(m.group(1))
    assert decoded.rstrip().endswith('</html>'), "srcdoc 内容异常（尾部属性可能被吞入）"
    assert 'height:844px' in decoded, "phone 高度未设置"
    assert 'position:sticky' in decoded, "bottom-bar 未改为 sticky"
print("验证通过")
```

> 最后一步仍需**真实渲染截图**确认无裂图、组件可见——通过 assert 不等于视觉正确。

---

## 出图常见错误清单（每次出图后必须对照自查）

### 一、资产相关

| 错误 | 根本原因 | 正确做法 |
|------|---------|---------|
| 切图有黑色背景 | 用了 JPEG 格式切图（文件头 `FFD8FF`），无透明通道 | 检查文件头，JPEG 必须换成透明 PNG 或 SVG；用户提供切图时直接用，不自己渲染 |
| 时钟图标模糊/黑底 | 用了 Figma 截图 API 导出的小尺寸 PNG，或 JPEG 切图 | 优先用用户提供的 PNG 切图；无切图时用 SVG inline 矢量绘制，不用 JPEG |
| 图标双层叠加出现白色遮罩 | Figma `imgIcon` 底层图自带 `opacity:0.5` 白色矩形遮罩，单独渲染变白色块 | 不用 `imgIcon` 做底层；底层用纯色 SVG 圆，路径层用 Figma `img` URL |
| 地图区域多余图标 | 手动加了 `MAP_ICON` 定位图标，设计稿里没有 | 按设计稿严格还原，没有的元素不加 |
| 切图全部裂图/加载不出 | 引用了 `figma.com/api/mcp/asset/...` 临时链接，过期且需登录 | 全部内嵌 base64 或本地相对路径；`grep -c 'figma.com/api/mcp'` 必须为 0 |
| 小图标置空后消失/裂开 | 信息图标、箭头、进度轴圆点/虚线本是矢量，没切图就设成透明 | 用 inline SVG / CSS 绘制，不当切图处理 |
| "透明"占位像素盖住按钮颜色 | 用的 1×1 占位 PNG 是 gray+alpha 不透明像素，object-fit 铺满盖住底色 | 隐藏用 `display:none`；透明用确认 alpha=0 的像素；按钮底色用 `#FF6435` 不依赖图片 |

### 二、布局相关

| 错误 | 根本原因 | 正确做法 |
|------|---------|---------|
| 头图背景没展示全（被裁切） | 头图背景放在设了 `overflow:hidden` 的容器里（如 `hero-area`） | 头图背景直接 `position:absolute` 到 `.phone`，不放任何会裁切的容器里 |
| 权益卡和活动要求间距过大 | `flow-pending margin-top` 值从 phone 顶部算，但 `hero-spacer` 已占了 275px，实际应减去 spacer 高度 | `flow-pending margin-top = 权益卡高度(250) + 间距(10) = 260px`，不是 535px |
| 卡片间距偏大/偏小 | hero-area 用了 `overflow:hidden` 把头图截断，导致视觉高度不对；或 margin-top 计算基准搞错 | 所有绝对定位元素的 top 值都相对 `.phone` 计算；`hero-spacer` 只作占位，不影响绝对定位元素 |
| 参与情况卡和奖励进度间距偏大 | `abs-cards` 容器包裹了 absolute 子元素，容器高度写死后 `flow-content margin-top` 叠加导致间距翻倍 | driver-card 和 progress-card 直接 absolute 定位到 phone；flow-content 紧接 hero-spacer，margin-top:10px |
| 未开始状态模块顺序/内容错误 | 直接复用进行中 HTML 改文案，没有按 page 规范重新生成 | 按 `mianyjong-card.md` 里「未开始专属」规范生成：权益卡 absolute top:275，活动要求+说明 flow-pending margin-top:260 |

### 三、文案相关

| 错误 | 根本原因 | 正确做法 |
|------|---------|---------|
| 说"文案完全匹配"但未核查 | 直接复用旧 HTML，没有逐条对照 PRD | 每次出图必须逐条核查 PRD 文案和 HTML 文案，核查完才能说"无差异" |
| 进行中描述框多余副文案 | PRD 说"只显示主文案"，但默认带了换行副文案 | 出图前确认表中明确副文案是否展示，用户说不展示就只保留主文案 |

### 四、全状态展示页相关

见上方「全状态展示页生成规范」章节（`srcdoc` + `position:sticky` 方案，含「资产与 iframe 铁律」）。

| 错误 | 根本原因 | 正确做法 |
|------|---------|---------|
| 整页空白、状态框不显示 | iframe 用了 `src="data:text/html;base64"`，被沙箱拦截 | 改用 `<iframe srcdoc="...">` |
| `width="375"...` 等属性当文字渲染出来 | 正则改 iframe 时把尾部属性吞进了 srcdoc 内容 | 用 `srcdoc="(.*?)"((?: [^>]*)?)></iframe>` 捕获，尾部属性留在 srcdoc 外 |
| 自查通过但实际仍裂图 | 只跑了文案/assert 校验，没有真实渲染 | 交付前必须渲染截图，目视确认无裂图、组件可见 |

额外注意：
- 每次修改源文件（`index.html`）后**必须重新生成展示页**，iframe 里是旧内容，不会自动更新
- 修改源文件 → 验证源文件 → **重新生成展示页** → 反转义验证 iframe 内容 → **渲染截图目视核对**，缺一不可

### 五、流程相关

| 错误 | 根本原因 | 正确做法 |
|------|---------|---------|
| 用旧 HTML 而非 page 规范出图 | 省略了步骤 1.2 出图来源判断，默认复用旧文件 | 每次出图必须声明来源（复用/基于规范替换/全新生成），不声明不出图 |
| 出图前没有等用户确认 | 跳过了确认表步骤直接出图 | 确认表（页面清单 + 模块内容 + 跳转关系）必须先发给用户确认，确认后才出图 |
