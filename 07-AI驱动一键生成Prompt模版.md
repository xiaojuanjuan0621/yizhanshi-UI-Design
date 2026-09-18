# 07 - AI 驱动一键生成 Prompt 模版 (AI Prompt Recipes)

> **使用场景**：将以下 Prompt 复制并直接投喂给任意顶级大模型（如 Claude 3.7 / GPT-4o / DeepSeek），只需填入业务关键词，即可在几秒内生成专业级的前端页面或完整设计系统！

---

## 🎯 Prompt 模版 1：一键生成全量 Living Style Guide 页面 (`design-system.html`)

```markdown
请为我设计并生成一套名为「{{PRODUCT_NAME}} ({{PRODUCT_CN_NAME}})」的高保真前端 Living Style Guide 页面（design-system.html）。

【业务背景与定位】
1. 产品定位：面向「{{TARGET_USERS}}」的「{{PRODUCT_SLOGAN}}」。
2. 核心资产形态：{{CORE_ASSET_NAME}}，具备「{{ASSET_NAMESPACE_PREFIX}}/name」命名空间体系。
3. 风险与治理：具备从 {{STATUS_L0_NAME}} 到 {{STATUS_L3_NAME}} 的严格风险分级管控。

【设计风格与参考标杆】
- 融合 GitHub Primer、Tailwind Catalyst、Vercel Geist 与 Linear 的设计精髓。
- 采用 1px 精细边框（1px border）、4px 栅格间距节奏与 3 档几何圆角（8px, 12px, 9999px）。
- 采用双排版引擎：正文采用现代无衬线体（Inter / PingFang SC），代码、哈希、版本号与指标强制采用等宽字体（JetBrains Mono）。
- 中性色采用冷调 Slate 灰阶梯队（Slate 50~950），内置完备的浅色/深色（Light/Dark）无缝切换机制。

【必须包含的页面模块】
1. 顶部 Header：产品 Logo、版本号徽章、浅/深色主题切换按钮、进入首页快捷入口。
2. 左侧目录导航：支持快速锚点平滑滚动定位到各模块。
3. 主体规范展示区：
   - Section 1: 色彩与 Design Tokens（品牌色、强调色、Slate灰阶对比、语义风险色）。
   - Section 2: 字体排印体系（Display、Heading、Body、Mono Code、Metric 数值）。
   - Section 3: 原子组件（{{STATUS_L0_NAME}}~{{STATUS_L3_NAME}} 风险徽章、数据药丸胶囊、各尺寸按钮）。
   - Section 4: 核心资产卡片（网格 Card 与紧凑 List 两种形态展示，含微位移 Hover 动效）。
   - Section 5: 企业级权限矩阵表（PermissionMatrixTable，展示 {{ROLE_USER}} / {{ROLE_ADMIN}} / {{ROLE_AUDITOR}} 的操作权限差异）。
   - Section 6: 三引擎安全合规扫描卡片（SAST、密钥防泄露、许可证合规）。
   - Section 7: 右侧滑出式抽屉（RoleAssignmentDrawer）与提级二次确认弹窗。

【输出要求】
- 产出单文件完整 HTML（内嵌完整、优雅、语义化的 CSS 与 Vanilla JS 交互）。
- 严禁任何外部重型框架依赖，保证直接在浏览器中打开即可完美运行和交互。
```

---

## 🎯 Prompt 模版 2：一键生成具体的业务页面（如资产市场首页 `home.html`）

```markdown
请基于「{{PRODUCT_NAME}}」设计规范，为我生成「{{PRODUCT_CN_NAME}} - 资产探索与治理市场首页 (home.html)」。

【页面核心需求】
1. 布局骨架：
   - 顶部主导航（TopNav 56px）：Logo、全局命令面板搜索框（Cmd+K 提示）、通知铃铛、主题切换、用户头像。
   - 二级业务导航（SubNav 44px）：探索市场（激活）、审计中心、监控指标、权限配置、右侧高亮操作按钮「+ 新建{{CORE_ASSET_NAME}}」。
2. Hero 概览区：
   - 居中醒目标题与一句话 Slogan：「{{PRODUCT_SLOGAN}}」。
   - 全局大搜索栏与热门标签过滤药丸（如 #核心认证、#网关安全、#数据处理）。
   - 4 项核心态势指标卡（总资产数、本周调用量、通过率、待审提级数）。
3. 资产探索主体区：
   - 过滤工具栏：分类 Tab（全部/官方/自建）、风险下拉选择器（{{STATUS_L0_NAME}}~{{STATUS_L3_NAME}}）、排序器、网格/列表视图切换开关。
   - 资产网格列表：至少展示 6 个具有代表性的真实业务卡片（包含命名空间、图标、中文名、双行简介、版本号、{{METRIC_1}}、操作按键）。
   - 分页器与加载更多。
4. 交互与动效：
   - 点击「网格/列表」开关可平滑切换展示形态。
   - 点击主题按钮可在浅色（#F8FAFC）与深色（#0F172A）间平滑切换。
   - 右下角常驻多角色切换浮窗（可在 {{ROLE_USER}}、{{ROLE_ADMIN}}、{{ROLE_AUDITOR}} 之间切换并模拟界面权限变化）。

【输出要求】
- 输出一份完整的单文件 HTML 源码，包含完整的 CSS Tokens 与 JS 交互逻辑，设计质感对标 Linear 与 GitHub。
```
