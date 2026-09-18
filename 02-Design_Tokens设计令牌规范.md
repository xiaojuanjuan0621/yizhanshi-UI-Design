# 02 - Design Tokens 设计令牌规范 (Design Tokens Specification)

> **适用系统**：`一站式数据服务平台` (`OneData Platform`)  
> **实现形态**：原生 CSS 自定义属性（CSS Variables），100% 对齐 Pixso Local Styles 与 Variables。

---

## 🎨 一、色彩系统 (Color System - 源自 Pixso 色板)

### 1. 品牌色与渐变 (Brand & Gradients)
| Token 变量名 | Pixso 原生值 | 语义与场景 |
| :--- | :--- | :--- |
| `--brand` | `#256DF8` / `#3B66F1` | 按钮主色、主标高亮、激活态背景 |
| `--brand-hover` | `#1A57D0` | 主按钮悬浮加深态 |
| `--accent-info` | `#1B7DEA` | Pixso 功能辅助色/信息辅助 |
| `--accent-purple`| `#593AC4` | Pixso 功能辅助色/颜色补充、AI 标签紫 |
| `--accent-orange`| `#F5A216` | 浦惠暖橙、高亮标签 |
| `--grad-ai` | `linear-gradient(135deg, #3B66F1 0%, #8B5CF6 50%, #00B4D8 100%)` | 3D 水晶球、AI Agent 机器人球体光晕 |
| `--grad-banner` | `linear-gradient(135deg, #EEF4FF 0%, #F5F3FF 50%, #E8F7FF 100%)` | 数据产品专区（浦查查）全息柔光底 |

---

### 2. Pixso 中性色与表层阶梯 (Neutral & Surface Scale)

| 语义层级 | Pixso 原生色值 | 浅色模式角色 | 深色模式角色 |
| :--- | :--- | :--- | :--- |
| **背景/页面** | `#F9F9FC` | `--bg-page` 页面底色 | 极高亮反差文字 |
| **背景/基础容器** | `#FFFFFF` | `--bg-card` 卡片纯白底 | `--bg-page` 页面底色 (`#0C121E`) |
| **背景/隔行次级** | `#F5F7FA` | `--bg-subtle` 表格/次级浅底 | `--bg-card` 卡片容器 (`#151F32`) |
| **边框 1** | `#E8EEF8` | `--border` 1px 精细分界线 | `--border` 暗色边框 (`#24344E`) |
| **边框 2 (次级)** | `#D3D7DD` | `--border-hover` 交互边框 | `--border-hover` (`#3D5377`) |
| **文本/标题** | `#303133` | `--text-primary` 主要标题正文 | `--text-primary` (`#F8FAFC`) |
| **文本/正文** | `#4B5563` | `--text-regular` 标准正文阅读 | `--text-secondary` (`#94A3B8`) |
| **文本/次要** | `#6B7280` | `--text-secondary` 辅助说明 | `--text-muted` (`#64748B`) |
| **文本/占位符** | `#9397A4` | `--text-placeholder` 占位文本 | 弱化占位符 |
| **文本/置灰未选** | `#B5B8BF` | `--text-disabled` 禁用态文本 | 禁用态文本 |

---

### 3. L0~L3 风险与状态语义色 (Semantic & Risk Levels)

| 风险等级 / 状态 | Pixso 原生色值 | 背景 Token (10% Alpha) | 适用含义 |
| :--- | :--- | :--- | :--- |
| **L0 基础公开 (Public)** | `#00A20B` (成功绿) | `rgba(0, 162, 11, 0.1)` | 官方标准、公开可信、健康度正常 |
| **L1 内部共享 (Internal)** | `#1B7DEA` (信息蓝) | `rgba(27, 125, 234, 0.1)` | 条线内部共享、标准审批流通过 |
| **L2 敏感受控 (Sensitive)** | `#F5A216` (警告黄) | `rgba(245, 162, 22, 0.1)` | 敏感字段、需动态脱敏与双人复核 |
| **L3 绝密隔离 (Restricted)**| `#F24B5C` (危险红) | `rgba(242, 75, 92, 0.1)` | 监管特权、绝密数据沙箱阻断 |

---

## 📐 二、间距、圆角与布局令牌 (Spacing & Layout Tokens)

### 1. 通用间距 (Spacing)
* `--space-4: 4px;`
* `--space-8: 8px;`
* `--space-10: 10px;`
* `--space-16: 16px;`
* `--space-24: 24px;`

### 2. 几何圆角 (Radius)
* `--r-2: 2px;`
* `--r-4: 4px;` (小型标签)
* `--r-8: 8px;` (输入框、普通按键)
* `--r-10: 10px;` (小卡片容器)
* `--r-16: 16px;` (核心功能卡片、大面板)
* `--r-pill: 9999px;` (模式切换器、胶囊标签、搜索栏发送键)

### 3. 页面布局骨架 (Page Layout Dimensions)
* **顶部导航布局高度**：`80px`（内容区 `54px`）
* **侧边导航布局宽度**：`200px`
* **标准版心宽度**：`1200px` / 宽屏工作台 `1600px`
* **页面左右内边距**：`16px` / `36px`
* **页面上下内边距**：`24px`

---

## 🔤 三、排版字体系统 (Typography Scale)

| 级别 | 字号 (Pixso Variable) | 字重规范 | 适用场景 |
| :--- | :--- | :--- | :--- |
| **平台大标题 / H1** | `24px` / `20px` | Bold (`700`) | 页面主标、Hero 标语、专区大词 |
| **模块标题 / H2** | `18px` | Bold / Medium | 核心功能卡片标题、数据产品专区标题 |
| **正文默认 (Body)** | `14px` / `16px` | Regular (`400`) | 列表项、表单输入、长篇描述文本 |
| **辅助说明 (Small)** | `12px` | Regular / Medium | 提示注释、时间戳、换一换、胶囊标签 |
| **机器流 / Mono** | `13px` / `14px` | Medium (`500`) | `@dept/dataset` 命名空间、SQL 指令、调用量数值 |
