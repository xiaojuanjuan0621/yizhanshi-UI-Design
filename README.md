# 📘 一站式数据服务平台 UI 设计规范与交互原型 (yizhanshi-UI-Design)

> **版本**：v2.0.0-Pixso-Aligned  
> **定位**：融合金融科技严谨度与 AI 智能体质感的一站式数据中台设计系统与高保真交互原型套件。  
> **核心原则**：Token-First（设计令牌优先）、Atomic Design（原子设计）、全流程“看-问-探-析-取”业务闭环、1px 精细工艺。

---

## 🖥 核心交互原型清单 (HTML Prototypes)

本项目包含可直接在现代浏览器中独立运行的高保真前端交互原型页面：

| 原型页面 | 文件路径 | 核心业务功能与交互亮点 |
| :--- | :--- | :--- |
| **服务中心** | [`service-center.html`](./service-center.html) | 服务目录树、搜索过滤、3D业务概览横幅、卡片详情抽屉、**服务链路实时防抖拓扑图**（支持4层全链路下钻、系统/服务双维度切换、实时调用量流动展示）、申请调用闭环 |
| **新建服务配置** | [`service-config.html`](./service-config.html) | 服务全生命周期注册向导（业务信息设定、表名映射、数据敏感等级分级、更新频率设置、入参/出参业务与技术双口径定义、SQL探数与接口联调） |
| **服务监控大屏** | [`service-monitor.html`](./service-monitor.html) | 金融级 API 服务监控看板、拓扑全景大屏、SLA 与吞吐量指标卡、调用耗时分析、节点状态健康排查 |
| **服务工作台** | [`service-workbench.html`](./service-workbench.html) | 我发布的服务、我调用的服务、调用统计报表、告警提醒与流控配额看板 |
| **我的审批中心** | [`my-approvals.html`](./my-approvals.html) | 待办审批流、敏感权限分级审批（L0~L3级）、变更申请对比与审计追溯 |
| **平台首页** | [`home.html`](./home.html) | 门户大厅导航、智能问数搜索框、常用服务与应用磁贴、个人资产与数据概览 |
| **应用中心** | [`app-center.html`](./app-center.html) | 全行应用集市、分类检索、应用接入向导与授权管理 |
| **设计系统规范** | [`design-system.html`](./design-system.html) | Living Style Guide，包含色彩系统、字体梯度、按钮、标签、卡片等所有原子组件展示 |

---

## 📚 设计规范体系文档 (Specifications)

套件包含 8 套模块化设计系统标准规范：

1. [00-README_使用指南与全局变量字典](./00-README_使用指南与全局变量字典.md)
2. [01-设计基调与设计原则](./01-设计基调与设计原则.md)
3. [02-Design_Tokens设计令牌规范](./02-Design_Tokens设计令牌规范.md)
4. [03-组件库规范与设计模式](./03-组件库规范与设计模式.md)
5. [04-布局骨架与页面模板规范](./04-布局骨架与页面模板规范.md)
6. [05-业务场景与高阶交互规范](./05-业务场景与高阶交互规范.md)
7. [06-前端工程落地与StyleGuide代码模版](./06-前端工程落地与StyleGuide代码模版.md)
8. [07-AI驱动一键生成Prompt模版](./07-AI驱动一键生成Prompt模版.md)

---

## 🎨 静态资产与元数据

- **`assets/`**：包含所有高保真 3D 渲染图（API Cube、SLA Shield、Mesh Gateway Banner）、业务状态图标及切图素材。
- **`pixso_*.json` / `tech_*_dsl.json`**：Pixso 设计稿导出的 Design Tokens DSL、上下文与变量配置。
- **`HomeView.vue`**：Vue 3 页面骨架实现。

---

## 🚀 快速预览与使用

无需复杂的构建环境，直接使用任何静态服务器或直接双击用 Chrome / Edge / Safari 浏览器打开上述 HTML 文件即可体验完整交互体验。

```bash
# 启动本地轻量预览服务
npx serve .
# 或使用 Python
python3 -m http.server 8080
```