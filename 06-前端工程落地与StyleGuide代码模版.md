# 06 - 前端工程落地与 Style Guide 代码模版 (Living Style Guide Template)

> **适用系统**：`{{PRODUCT_NAME}}` (`{{PRODUCT_CN_NAME}}`)  
> **技术选型**：纯原生 HTML5 + 现代化 CSS3（CSS Variables） + 极简原生 Vanilla JS（无任何厚重依赖，开箱即用，易于迁移至 React / Vue / Svelte）

---

## 📁 一、推荐工程目录组织结构

在实际前端项目中，推荐将通用设计资产与业务页面按如下结构组织：

```text
├── assets/
│   ├── css/
│   │   ├── tokens.css              # 全局 Design Tokens (色彩/间距/圆角/主题)
│   │   ├── base.css                # 重置样式与全局排版规则 (Inter + JetBrains Mono)
│   │   ├── components.css          # 原子与复合组件样式 (Badge/Button/Card/Table/Drawer)
│   │   ├── layout.css              # 双层导航与响应式栅格
│   │   └── nav-shared.css          # 顶部与二级导航复用样式
│   └── js/
│       ├── theme.js                # 深浅主题切换与持久化逻辑
│       ├── nav-shared.js           # 导航交互、搜索、通知中心
│       ├── data-table.js           # 表格分页、排序、空态交互
│       └── demo-role-widget.js     # 多角色（用户/管理员/审查员）切换模拟器
├── pages/
│   ├── index.html                  # 全景设计规范索引入口
│   ├── design-system.html          # Living Style Guide 活体规范展示页
│   ├── home.html                   # 资产市场首页
│   ├── asset-detail.html           # 资产详情与代码接入页
│   ├── publish.html                # 资产发布向导
│   └── security-audit.html         # 安全与权限审计中心
```

---

## 💻 二、完整可运行的 `design-system.html` 核心模板骨架

将以下代码保存为 `design-system.html`，即可作为你产品专属的设计系统活体规范页：

```html
<!DOCTYPE html>
<html lang="zh-CN" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{PRODUCT_NAME}} - Design System & Living Style Guide</title>
  <!-- Google Fonts: Inter & JetBrains Mono -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
  
  <style>
    /* =========================================================
       1. DESIGN TOKENS
       ========================================================= */
    :root {
      --brand: {{BRAND_COLOR_PRIMARY}};
      --brand-hover: #162C48;
      --accent: {{BRAND_COLOR_ACCENT}};
      --accent-subtle: rgba(59, 130, 246, 0.1);

      --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", sans-serif;
      --font-mono: 'JetBrains Mono', Consolas, monospace;

      --r-sm: 8px;
      --r-md: 12px;
      --r-pill: 9999px;

      --space-1: 4px;
      --space-2: 8px;
      --space-3: 12px;
      --space-4: 16px;
      --space-6: 24px;
      --space-8: 32px;

      --shadow-sm: 0 1px 3px rgba(0,0,0,0.06);
      --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.08);
      --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.1);
      
      --ease: 150ms cubic-bezier(0.4, 0, 0.2, 1);
    }

    :root[data-theme="light"] {
      --bg-page: #F8FAFC;
      --bg-card: #FFFFFF;
      --bg-subtle: #F1F5F9;
      --border: #E2E8F0;
      --border-hover: #CBD5E1;
      --text-primary: #1E293B;
      --text-secondary: #64748B;
      --text-muted: #94A3B8;
    }

    :root[data-theme="dark"] {
      --bg-page: #0F172A;
      --bg-card: #1E293B;
      --bg-subtle: #334155;
      --border: #334155;
      --border-hover: #475569;
      --text-primary: #F8FAFC;
      --text-secondary: #94A3B8;
      --text-muted: #64748B;
    }

    /* =========================================================
       2. BASE STYLES
       ========================================================= */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: var(--font-sans);
      background: var(--bg-page);
      color: var(--text-primary);
      line-height: 1.5;
      transition: background var(--ease), color var(--ease);
    }
    .font-mono { font-family: var(--font-mono); }

    /* =========================================================
       3. LIVING STYLE GUIDE LAYOUT
       ========================================================= */
    .styleguide-header {
      background: var(--bg-card);
      border-bottom: 1px solid var(--border);
      padding: 16px 32px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .styleguide-body {
      display: flex;
      max-width: 1440px;
      margin: 0 auto;
    }
    .styleguide-nav {
      width: 240px;
      padding: 32px 16px;
      border-right: 1px solid var(--border);
      position: sticky;
      top: 65px;
      height: calc(100vh - 65px);
      overflow-y: auto;
    }
    .styleguide-nav a {
      display: block;
      padding: 8px 12px;
      color: var(--text-secondary);
      text-decoration: none;
      font-size: 14px;
      border-radius: var(--r-sm);
    }
    .styleguide-nav a:hover {
      background: var(--bg-subtle);
      color: var(--text-primary);
    }
    .styleguide-content {
      flex: 1;
      padding: 32px 48px;
    }
    .sys-section {
      margin-bottom: 56px;
      scroll-margin-top: 80px;
    }
    .sys-section-title {
      font-size: 20px;
      font-weight: 700;
      margin-bottom: 8px;
      padding-bottom: 8px;
      border-bottom: 1px solid var(--border);
    }
    .sys-section-desc {
      color: var(--text-secondary);
      font-size: 14px;
      margin-bottom: 24px;
    }

    /* =========================================================
       4. COMPONENTS STYLES
       ========================================================= */
    /* Badges */
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 3px 10px;
      font-size: 12px;
      font-weight: 600;
      border-radius: var(--r-pill);
    }
    .badge-dot { width: 6px; height: 6px; border-radius: 50%; }
    .badge-l0 { background: #ECFDF5; color: #059669; border: 1px solid #A7F3D0; }
    .badge-l0 .badge-dot { background: #10B981; }
    .badge-l1 { background: #EFF6FF; color: #2563EB; border: 1px solid #BFDBFE; }
    .badge-l1 .badge-dot { background: #3B82F6; }
    .badge-l2 { background: #FFFBEB; color: #D97706; border: 1px solid #FDE68A; }
    .badge-l2 .badge-dot { background: #F59E0B; }
    .badge-l3 { background: #FEF2F2; color: #DC2626; border: 1px solid #FECACA; }
    .badge-l3 .badge-dot { background: #EF4444; }

    /* Buttons */
    .btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      height: 36px;
      padding: 0 16px;
      font-size: 14px;
      font-weight: 500;
      border-radius: var(--r-sm);
      cursor: pointer;
      border: 1px solid transparent;
      transition: var(--ease);
    }
    .btn-primary { background: var(--brand); color: #FFF; }
    .btn-primary:hover { background: var(--brand-hover); }
    .btn-secondary { background: var(--bg-card); color: var(--text-primary); border-color: var(--border); }
    .btn-secondary:hover { background: var(--bg-subtle); border-color: var(--border-hover); }
    .btn-accent { background: var(--accent); color: #FFF; }

    /* Cards */
    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
      gap: 20px;
    }
    .asset-card {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--r-md);
      padding: 20px;
      display: flex;
      flex-direction: column;
      gap: 12px;
      transition: var(--ease);
      box-shadow: var(--shadow-sm);
    }
    .asset-card:hover {
      border-color: var(--border-hover);
      transform: translateY(-2px);
      box-shadow: var(--shadow-md);
    }
  </style>
</head>
<body>

  <!-- 顶部工具栏 -->
  <header class="styleguide-header">
    <div style="display:flex; align-items:center; gap:12px;">
      <span style="font-size:20px;">📘</span>
      <h1 style="font-size:18px; font-weight:700;">{{PRODUCT_NAME}} 设计规范系统</h1>
      <span class="badge badge-l0">v2.0.0</span>
    </div>
    <div style="display:flex; align-items:center; gap:16px;">
      <button class="btn btn-secondary" onclick="toggleTheme()">🌓 切换主题</button>
      <a href="home.html" class="btn btn-primary">进入{{PRODUCT_CN_NAME}} ➔</a>
    </div>
  </header>

  <div class="styleguide-body">
    <!-- 左侧目录导航 -->
    <nav class="styleguide-nav">
      <a href="#section-colors">1. 色彩与 Tokens</a>
      <a href="#section-typography">2. 字体与排版</a>
      <a href="#section-badges">3. 风险徽章 (Badges)</a>
      <a href="#section-buttons">4. 按钮体系 (Buttons)</a>
      <a href="#section-cards">5. 资产卡片 (Cards)</a>
      <a href="#section-tables">6. 权限矩阵表 (Tables)</a>
    </nav>

    <!-- 主展示区 -->
    <main class="styleguide-content">
      
      <!-- Section 1: 色彩与 Tokens -->
      <section id="section-colors" class="sys-section">
        <h2 class="sys-section-title">1. 色彩与 Tokens 系统</h2>
        <p class="sys-section-desc">基于 Slate 灰度阶梯与高对比度语义风险分级色彩。</p>
        <div style="display:flex; gap:12px; flex-wrap:wrap;">
          <div style="padding:16px; background:var(--brand); color:#FFF; border-radius:var(--r-sm); width:140px;">
            <div style="font-size:12px;">Brand Primary</div>
            <div class="font-mono" style="font-weight:700;">{{BRAND_COLOR_PRIMARY}}</div>
          </div>
          <div style="padding:16px; background:var(--accent); color:#FFF; border-radius:var(--r-sm); width:140px;">
            <div style="font-size:12px;">Accent High</div>
            <div class="font-mono" style="font-weight:700;">{{BRAND_COLOR_ACCENT}}</div>
          </div>
          <div style="padding:16px; background:var(--bg-card); border:1px solid var(--border); border-radius:var(--r-sm); width:140px;">
            <div style="font-size:12px;">Card Surface</div>
            <div class="font-mono" style="font-size:12px;">var(--bg-card)</div>
          </div>
        </div>
      </section>

      <!-- Section 2: 风险徽章 -->
      <section id="section-badges" class="sys-section">
        <h2 class="sys-section-title">2. 风险与状态徽章 (Badges)</h2>
        <p class="sys-section-desc">在深浅双主题下均保持最高安全可识别度。</p>
        <div style="display:flex; gap:16px; align-items:center;">
          <span class="badge badge-l0"><span class="badge-dot"></span>{{STATUS_L0_NAME}}</span>
          <span class="badge badge-l1"><span class="badge-dot"></span>{{STATUS_L1_NAME}}</span>
          <span class="badge badge-l2"><span class="badge-dot"></span>{{STATUS_L2_NAME}}</span>
          <span class="badge badge-l3"><span class="badge-dot"></span>{{STATUS_L3_NAME}}</span>
        </div>
      </section>

      <!-- Section 3: 按钮体系 -->
      <section id="section-buttons" class="sys-section">
        <h2 class="sys-section-title">3. 操作按钮组 (Buttons)</h2>
        <p class="sys-section-desc">包含主操作、次操作、强调操作与微动效交互。</p>
        <div style="display:flex; gap:12px; align-items:center;">
          <button class="btn btn-primary">主要操作 (Primary)</button>
          <button class="btn btn-secondary">次要操作 (Secondary)</button>
          <button class="btn btn-accent">强调操作 (Accent)</button>
        </div>
      </section>

      <!-- Section 4: 核心资产卡片 -->
      <section id="section-cards" class="sys-section">
        <h2 class="sys-section-title">4. 核心资产卡片 (AssetCard)</h2>
        <p class="sys-section-desc">1px 极细边框，悬浮微位移与清晰的信息层次。</p>
        <div class="card-grid">
          
          <div class="asset-card">
            <div style="display:flex; justify-content:space-between; align-items:flex-start;">
              <div>
                <span class="font-mono" style="font-size:12px; color:var(--accent);">{{ASSET_NAMESPACE_PREFIX}}/core-auth</span>
                <h3 style="font-size:16px; font-weight:600; margin-top:4px;">企业级身份认证网关</h3>
              </div>
              <span class="badge badge-l0">{{STATUS_L0_NAME}}</span>
            </div>
            <p style="font-size:13px; color:var(--text-secondary); line-height:1.5;">
              支持多租户单点登录、OIDC 协议对接与企业级访问令牌动态签发。
            </p>
            <div style="margin-top:auto; padding-top:12px; border-top:1px solid var(--border); display:flex; justify-content:space-between; align-items:center;">
              <span class="font-mono" style="font-size:12px; color:var(--text-muted);">18.4k {{METRIC_1}}</span>
              <button class="btn btn-secondary" style="height:28px; padding:0 10px; font-size:12px;">接入配置</button>
            </div>
          </div>

          <div class="asset-card">
            <div style="display:flex; justify-content:space-between; align-items:flex-start;">
              <div>
                <span class="font-mono" style="font-size:12px; color:var(--accent);">{{ASSET_NAMESPACE_PREFIX}}/data-masker</span>
                <h3 style="font-size:16px; font-weight:600; margin-top:4px;">敏感数据动态脱敏引擎</h3>
              </div>
              <span class="badge badge-l2">{{STATUS_L2_NAME}}</span>
            </div>
            <p style="font-size:13px; color:var(--text-secondary); line-height:1.5;">
              针对手机号、身份证、银行卡等个人隐私信息执行实时动态脱敏掩码。
            </p>
            <div style="margin-top:auto; padding-top:12px; border-top:1px solid var(--border); display:flex; justify-content:space-between; align-items:center;">
              <span class="font-mono" style="font-size:12px; color:var(--text-muted);">6.1k {{METRIC_1}}</span>
              <button class="btn btn-secondary" style="height:28px; padding:0 10px; font-size:12px;">申请提级</button>
            </div>
          </div>

        </div>
      </section>

    </main>
  </div>

  <script>
    // 深浅主题切换脚本
    function toggleTheme() {
      const current = document.documentElement.getAttribute('data-theme') || 'light';
      const target = current === 'light' ? 'dark' : 'light';
      document.documentElement.setAttribute('data-theme', target);
      localStorage.setItem('theme', target);
    }
    // 读取持久化主题
    const savedTheme = localStorage.getItem('theme');
    if (savedTheme) {
      document.documentElement.setAttribute('data-theme', savedTheme);
    }
  </script>
</body>
</html>
```
