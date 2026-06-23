---
name: reward-skill
description: 奖励页面设计与出图技能。用于将奖励类PRD文档转化为H5 HTML设计稿，或通过Figma MCP输出到Figma。每当用户提到「出图」「奖励页面」「免佣卡」「冲单奖励」「奖励PRD」「活动页面」「奖励活动」等关键词时必须触发此skill。输出前必须按workflow.md执行出图前确认流程，等待用户确认后再动手。
---

# 奖励 Skill

## 概述

本 skill 服务于司机端奖励激励页面的设计出图工作。
- **输入**：奖励活动 PRD 文档
- **输出**：H5 HTML 设计稿 / Figma 设计文件（由用户指定）
- **设备基准**：iPhone 14，375×844px（逻辑像素）

---

## 出图流程（每次必须严格执行）

详细流程见 `references/workflow.md`，核心步骤：

1. **阅读 PRD** → 整理所有文案清单
2. **输出确认表** → 等待用户确认内容与状态
3. **有歧义必须二次确认** → 一次性列出所有问题，等待答复
4. **确认后开始出图** → 按规范逐模块搭建
5. **出图后核对** → 对照 PRD 文案逐条检查

---

## 资产基础 URL

所有资产统一使用 GitHub raw URL 前缀，Agent 无需依赖本地文件：

```
https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/
```

---

## 资产索引

### 字体
| 文件 | 字族名 | 用途 | URL |
|------|--------|------|-----|
| `assets/fonts/MFYuanHei-Boldtalic.ttf` | MFYuanHei | 页面大标题 / 活动权益标题 / 底部金额数字 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/fonts/MFYuanHei-Boldtalic.ttf` |

### 切图（assets/images/）
| 文件名 | 用途 | URL |
|--------|------|-----|
| `hero-bg.png` | 页面顶部头图背景 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/hero-bg.png` |
| `badge-free.png` | 右上角"免"字奖励标识贴纸 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/badge-free.png` |
| `title-bg.png` | 活动权益区块标题底纹 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/title-bg.png` |
| `icon-benefit-1.png` | 权益图标（亏必补） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-1.png` |
| `icon-benefit-2.png` | 权益图标（免佣保底） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-2.png` |
| `icon-benefit-3.png` | 权益图标（100%给到司机） | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-benefit-3.png` |
| `driver-thumb.png` | 进行中/获奖状态司机插画 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/driver-thumb.png` |
| `driver-sad.png` | 已结束未获奖状态插画 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/driver-sad.png` |
| `icon-clock.png` | 时间轴时钟图标 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/icon-clock.png` |
| `map-area.png` | 活动区域地图 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/map-area.png` |
| `meta-card-bg.png` | 活动时间卡背景 | `https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/meta-card-bg.png` |

---

## 规范文档索引（references/）

| 文件 | 内容 |
|------|------|
| `colors.md` | 色彩体系，主色 #FF6435，所有颜色必须从此文件读取 |
| `typography.md` | 字体规范，含 MFYuanHei 补充定义 |
| `spacing.md` | 间距与圆角，页面宽375px，卡片宽351px |
| `shadows.md` | 5级阴影体系 |
| `components.md` | 组件规范（Button、App bar、CellCard等） |
| `custom-rules.md` | 自定义规则，最高优先级 |
| `workflow.md` | 出图标准流程与自查清单 |

**读取顺序**：custom-rules.md（最高优先级）→ colors.md → spacing.md → typography.md → 其他

> ⚠️ 生成全状态展示页（all-states.html）前必须先读 `references/workflow.md` 中的「全状态展示页生成规范」章节
---

## 固定玩法 Page 索引

| Page | 文档路径 | 状态数 | 说明 |
|------|---------|--------|------|
| 免佣卡 | `pages/mianyjong-card/mianyjong-card.md` | 4 | 未开始/进行中/已结束-获奖/已结束-未获奖 |

**交付产物说明：**

单页 HTML（未开始/进行中/已结束·获奖/已结束·未获奖）是最终交付给工程师的产物，**不存于 skill**，每次按 page 规范 + PRD 重新生成后单独交付。

全状态展示页（设计评审用）按需生成，生成规范见 `references/workflow.md` 「全状态展示页生成规范」章节。

> 收到免佣卡相关 PRD 时，必须先读 `pages/mianyjong-card/mianyjong-card.md`，再按 workflow.md 流程出图。
> 新增固定玩法时，在 `pages/` 下新建同名 `.md`，并在此表登记。
---

## MFYuanHei 字体补充定义

在 `references/typography.md` 的基础上，补充 MFYuanHei 的使用规范：

| 场景 | 字族 | 字重 | 字号 | 颜色 |
|------|------|------|------|------|
| 页面大标题（活动名称） | MFYuanHei | Bold Italic | 28-32px | #FFFFFF |
| 活动权益区块标题 | MFYuanHei | Bold Italic | 18px | 主色/金色 |
| 底部按钮区金额数字 | MFYuanHei | Bold Italic | 36px | #222222 |

---

## 输出方式说明

### HTML 模式
- **所有资产路径必须使用 GitHub raw URL**（因为 Agent 运行在隔离沙箱中，本地无资产文件）
- 图片示例：`https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/images/hero-bg.png`
- 字体通过 `@font-face` 引入远程 URL：
  ```css
  @font-face {
    font-family: 'MFYuanHei';
    src: url('https://raw.githubusercontent.com/abbiehappy123-lang/Rewardsmall/main/assets/fonts/MFYuanHei-Boldtalic.ttf') format('truetype');
    font-weight: bold;
    font-style: italic;
  }
  ```
- 数字字体 Barlow Semi Condensed 通过 Google Fonts 引入（价格、翻牌格、时间轴日期等所有数字元素均需此字体）：
  ```html
  <link href="https://fonts.googleapis.com/css2?family=Barlow+Semi+Condensed:wght@600;700&display=swap" rel="stylesheet">
  ```
- 4种状态通过 URL 参数切换：`?state=pending`（未开始）/ `active`（进行中）/ `ended-win`（已结束获奖）/ `ended-lose`（已结束未获奖）
- ⚠️ **禁止使用相对路径**（如 `../../assets/images/xxx.png`），Agent 沙箱中无法解析

### Figma 模式
- 执行前先读 `references/workflow.md` 的完整 Figma 操作流程
- 组件库 key：`sxQXbLdcTfITwow8YCnmOJ`
- 切图通过 `figma.createImage()` 导入为 ImagePaint
- 字体回退策略：PingFang SC → Noto Sans SC（见 workflow.md）
