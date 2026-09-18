<script setup lang="ts">
import { ref, computed } from 'vue';

/**
 * -----------------------------------------------------------------------------
 * 1. 类型定义 (TypeScript Interfaces)
 * -----------------------------------------------------------------------------
 */
export interface CardItem {
  id: string;
  iconSrc: string;
  title: string;
  linkText: string;
  url?: string;
  action?: string;
  disabled?: boolean;
}

export interface ProductBanner {
  id: string;
  name: string;
  desc: string;
  keywords: string;
  link?: string;
}

export interface StreamItem {
  id: string;
  title: string;
  category?: string;
  updateTime?: string;
}

/**
 * -----------------------------------------------------------------------------
 * 2. 响应式状态与数据源 (Reactive State & Mock Data)
 * -----------------------------------------------------------------------------
 */

// 主题切换 (浅色 / 深色)
const isDark = ref(false);
const toggleTheme = () => {
  isDark.value = !isDark.value;
  if (typeof document !== 'undefined') {
    document.documentElement.setAttribute('data-theme', isDark.value ? 'dark' : 'light');
  }
};

// 角色选择
const currentRole = ref<'business' | 'tech' | 'admin'>('tech');

// 检索与 Agent 选择状态
const searchQuery = ref('');
const activeAgent = ref('智能取数 Agent');

const agentList = [
  { name: '智能取数 Agent', img: new URL('./assets/images/agent-tag-fetch@2x.png', import.meta.url).href },
  { name: '智能问数 Agent', img: new URL('./assets/images/agent-tag-ask@2x.png', import.meta.url).href },
  { name: '模板取数 Agent', img: new URL('./assets/images/agent-tag-template@2x.png', import.meta.url).href }
];

const inputPlaceholder = computed(() => {
  return `已切换为 [${activeAgent.value}]，请输入您的问题、业务口径或数据集需求...`;
});

const selectAgent = (agentName: string) => {
  activeAgent.value = agentName;
};

// 弹窗状态
const modalVisible = ref(false);
const modalTitle = ref('');
const modalContent = ref('');

const openModal = (title: string, content: string) => {
  modalTitle.value = title;
  modalContent.value = content;
  modalVisible.value = true;
};

const closeModal = () => {
  modalVisible.value = false;
};

// 触发 AI 检索
const handleSearch = () => {
  const query = searchQuery.value.trim() || '查询近7日零售客户交易活跃度前 10 排名与异常趋势';
  openModal(
    '✨ AI 智能问数解析结果',
    `<div class="search-result-dialog">
      <p><strong>🎯 执行 Agent：</strong><span style="color:#256DF8;">${activeAgent.value}</span></p>
      <p><strong>💬 查询内容：</strong>${query}</p>
      <div style="margin-top:12px; padding:10px; background:rgba(37,109,248,0.06); border-radius:6px; font-size:13px;">
        已自动关联核心资产 <code>@dept/retail_cust_360</code> 与实时交易流水表。数据质量健康度 <strong>99.8%</strong>，权限校验通过（L0 公开级）。
      </div>
    </div>`
  );
};

// 打开用数助手
const openAssistant = () => {
  openModal(
    '🤖 专属用数助手 Agent',
    `<p>您好！我是全天候智能用数助手，支持以下高频操作：</p>
    <ul style="margin: 10px 0 0 18px; line-height: 1.8; font-size: 13px; color: #4B5563;">
      <li>“帮我查询本月普惠金融贷款逾期率与环比变化”</li>
      <li>“导出供应链拓客评分高于 80 分的企业清单”</li>
      <li>“分析零售月报中财富管理客户增长归因”</li>
    </ul>`
  );
};

// 模式切换：技术模式 (tech) vs 业务模式 (biz)
const currentMode = ref<'tech' | 'biz'>('tech');

const techCards: CardItem[] = [
  { id: 't1', iconSrc: new URL('./assets/images/tech/icon-report@2x.png', import.meta.url).href, title: '看报表', linkText: '查看 →' },
  { id: 't2', iconSrc: new URL('./assets/images/tech/icon-ask@2x.png', import.meta.url).href, title: '问数', linkText: '查看 →', action: 'ask' },
  { id: 't3', iconSrc: new URL('./assets/images/tech/icon-explore@2x.png', import.meta.url).href, title: '探数', linkText: '查看 →', action: 'explore' },
  { id: 't4', iconSrc: new URL('./assets/images/tech/icon-analyze@2x.png', import.meta.url).href, title: '析数', linkText: '查看 →', action: 'analyze' },
  { id: 't5', iconSrc: new URL('./assets/images/tech/icon-extract@2x.png', import.meta.url).href, title: '取数', linkText: '查看 →' },
  { id: 't6', iconSrc: new URL('./assets/images/tech/icon-building@2x.png', import.meta.url).href, title: '更多建设中...', linkText: '敬请期待', disabled: true }
];

const bizCards: CardItem[] = [
  { id: 'b1', iconSrc: new URL('./assets/images/biz/icon-super-prod@2x.png', import.meta.url).href, title: '超级产品', linkText: '查看 →' },
  { id: 'b2', iconSrc: new URL('./assets/images/biz/icon-puhui@2x.png', import.meta.url).href, title: '浦惠来了', linkText: '查看 →' },
  { id: 'b3', iconSrc: new URL('./assets/images/biz/icon-tech-fin@2x.png', import.meta.url).href, title: '科技金融', linkText: '查看 →' },
  { id: 'b4', iconSrc: new URL('./assets/images/biz/icon-inclusive-fin@2x.png', import.meta.url).href, title: '普惠金融', linkText: '查看 →' },
  { id: 'b5', iconSrc: new URL('./assets/images/biz/icon-cross-border@2x.png', import.meta.url).href, title: '跨境金融', linkText: '查看 →' },
  { id: 'b6', iconSrc: new URL('./assets/images/biz/icon-treasury-fin@2x.png', import.meta.url).href, title: '财资金融', linkText: '查看 →' }
];

const displayedCards = computed(() => {
  return currentMode.value === 'tech' ? techCards : bizCards;
});

const isCardFlipping = ref(false);
const rotateBiz = () => {
  isCardFlipping.value = true;
  setTimeout(() => {
    isCardFlipping.value = false;
  }, 200);
};

const handleCardClick = (card: CardItem) => {
  if (card.disabled) return;
  if (card.action === 'ask') {
    searchQuery.value = '';
    const el = document.getElementById('framework-ai-input');
    if (el) el.focus();
    return;
  }
  openModal(`正在进入【${card.title}】`, `<p>已为您准备好【${card.title}】的专属指标大屏与多维下钻工作流。</p>`);
};

// 数据产品专区轮播
const productBanners: ProductBanner[] = [
  { id: 'p1', name: '浦查查', desc: '我们自己的企业查询和拓客平台', keywords: '查企业 ｜ 查产业 ｜ 查园区' },
  { id: 'p2', name: '数智星看板', desc: '全行业务核心指标多维可视化工作台', keywords: '指标雷达 ｜ 实时监控 ｜ 异常预警' },
  { id: 'p3', name: '风控哨兵', desc: '对公与零售信贷毫秒级反欺诈拦截中台', keywords: '规则引擎 ｜ 实时流计算 ｜ 智能阻断' }
];
const currentBannerIdx = ref(0);
const currentBanner = computed(() => productBanners[currentBannerIdx.value]);

const slideBanner = (step: number) => {
  currentBannerIdx.value = (currentBannerIdx.value + step + productBanners.length) % productBanners.length;
};

// 底部三列动态数据流 (支持换一换)
const isRefreshingHistory = ref(false);
const isRefreshingFavorite = ref(false);
const isRefreshingRecommend = ref(false);

const historyPool: StreamItem[][] = [
  [
    { id: 'h1', title: '供应链风险总结分析一站式报表' },
    { id: 'h2', title: '超级产品客群画像分析' },
    { id: 'h3', title: '总结零售月报智能总结分析' },
    { id: 'h4', title: '浦惠重点商户交易监测表' }
  ],
  [
    { id: 'h5', title: '对公信贷逾期预警特征集' },
    { id: 'h6', title: '财富管理中产客群资产异动监控' },
    { id: 'h7', title: '全行网点吞吐量与效能排行榜' },
    { id: 'h8', title: '信用卡分期意向模型实时评分流' }
  ]
];
const historyBatch = ref(0);
const historyList = computed(() => historyPool[historyBatch.value]);

const refreshHistory = () => {
  isRefreshingHistory.value = true;
  setTimeout(() => {
    historyBatch.value = (historyBatch.value + 1) % historyPool.length;
    isRefreshingHistory.value = false;
  }, 350);
};

const favoriteList = ref<StreamItem[]>([
  { id: 'f1', title: '超级产品客群画像分析' },
  { id: 'f2', title: '供应链风险总结分析一站式报表' },
  { id: 'f3', title: '对公信贷逾期预警特征集' },
  { id: 'f4', title: '总结零售月报智能总结分析' }
]);

const refreshFavorite = () => {
  isRefreshingFavorite.value = true;
  setTimeout(() => {
    favoriteList.value = [...favoriteList.value].reverse();
    isRefreshingFavorite.value = false;
  }, 350);
};

const recommendList = ref<StreamItem[]>([
  { id: 'r1', title: '2026年二季度制造业普惠贷款贴息明细' },
  { id: 'r2', title: '跨境电商外汇实时清算看板' },
  { id: 'r3', title: '同业理财穿透式底层资产合规排查' },
  { id: 'r4', title: '绿色金融ESG重点标的碳足迹分析' }
]);

const refreshRecommend = () => {
  isRefreshingRecommend.value = true;
  setTimeout(() => {
    recommendList.value = [...recommendList.value].reverse();
    isRefreshingRecommend.value = false;
  }, 350);
};

const handleItemClick = (item: StreamItem) => {
  openModal(`📄 ${item.title}`, `<p>已定位该数据集/报表，支持一键在报表大屏中打开并下钻维度。</p>`);
};
</script>

<template>
  <div class="home-container" :class="{ 'dark-theme': isDark }">
    <!-- 1. 顶部全局导航栏 -->
    <header class="app-header">
      <div class="header-left">
        <div class="brand-logo">
          <img src="./assets/images/logo.png" alt="Logo" class="logo-img" />
          <span class="brand-title">一站式数据服务平台</span>
        </div>

        <nav class="nav-links">
          <a href="javascript:void(0)" class="nav-item active">首页工作台</a>
          <a href="javascript:void(0)" class="nav-item">资产大厅</a>
          <a href="javascript:void(0)" class="nav-item">指标中心</a>
          <a href="javascript:void(0)" class="nav-item">数据字典</a>
          <a href="javascript:void(0)" class="nav-item">设计规范</a>
          <a href="javascript:void(0)" class="nav-item">平台管理</a>
        </nav>
      </div>

      <div class="header-right">
        <span class="user-greeting">您好，<b>张科技 (科技人员)</b></span>
        <button class="feedback-btn" @click="openModal('在线支持', '数据服务支持团队将在1小时内响应您的反馈。')">
          反馈
        </button>
        <button class="theme-btn" @click="toggleTheme" title="切换深/浅色模式">
          {{ isDark ? '🌙' : '☀️' }}
        </button>
      </div>
    </header>

    <!-- 2. 主页面内容 -->
    <main class="main-content">
      <!-- 顶层 AI 智能问数与 Agent 检索栏 -->
      <section class="ai-search-card">
        <div class="search-inner-box">
          <div class="search-left-panel">
            <input
              id="framework-ai-input"
              v-model="searchQuery"
              type="text"
              class="search-text-input"
              :placeholder="inputPlaceholder"
              @keydown.enter="handleSearch"
            />

            <!-- Agent 胶囊选择器 (支持点击切换) -->
            <div class="agent-capsules">
              <div
                v-for="agent in agentList"
                :key="agent.name"
                class="agent-pill-btn"
                :class="{ active: activeAgent === agent.name }"
                @click="selectAgent(agent.name)"
              >
                <img :src="agent.img" :alt="agent.name" class="agent-img" />
              </div>
            </div>
          </div>

          <!-- 发送按钮 -->
          <button class="search-action-btn" @click="handleSearch" title="发送 AI 查询">
            <img src="./assets/images/search-arrow-btn@2x.png" alt="发送" />
          </button>
        </div>

        <!-- 右侧用数助手入口机器人 -->
        <div class="assistant-entry-badge" @click="openAssistant">
          <span class="star-sparkle">✦</span>
          <img src="./assets/images/ai-agent-robot@2x.png" alt="用数助手" class="robot-avatar" />
          <div class="assistant-text">
            <div class="primary-text">用数助手</div>
            <div class="sub-text">Agent ✦</div>
          </div>
        </div>
      </section>

      <!-- 中部主工作台：左侧功能卡片 + 右侧数据产品专区 -->
      <section class="workspace-layout">
        <!-- 左侧 6 功能卡片区 -->
        <div class="feature-panel">
          <div class="mode-switch-container">
            <div class="mode-segmented-control">
              <button
                class="mode-btn"
                :class="{ active: currentMode === 'biz' }"
                @click="currentMode = 'biz'"
              >
                业务模式
              </button>
              <button
                class="mode-btn"
                :class="{ active: currentMode === 'tech' }"
                @click="currentMode = 'tech'"
              >
                技术模式
              </button>
            </div>
          </div>

          <!-- 业务模式左右切页箭头 -->
          <button
            v-if="currentMode === 'biz'"
            class="slider-arrow arrow-left"
            @click="rotateBiz"
          >
            ❮
          </button>
          <button
            v-if="currentMode === 'biz'"
            class="slider-arrow arrow-right"
            @click="rotateBiz"
          >
            ❯
          </button>

          <!-- 6 宫格卡片网格 -->
          <div class="cards-grid" :class="{ 'grid-flip': isCardFlipping }">
            <div
              v-for="card in displayedCards"
              :key="card.id"
              class="feature-card"
              :class="{ disabled: card.disabled }"
              @click="handleCardClick(card)"
            >
              <img :src="card.iconSrc" :alt="card.title" class="card-icon" />
              <div class="card-text-wrapper">
                <div class="card-title">{{ card.title }}</div>
                <div class="card-link" :class="{ 'link-disabled': card.disabled }">
                  {{ card.linkText }}
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 右侧数据产品专区 (浦查查 3D 背景卡片) -->
        <div class="product-panel">
          <div class="panel-header">
            <img src="./assets/images/product/puchacha-icon@2x.png" alt="数据产品" class="header-icon" />
            <span class="header-label">数据产品专区</span>
          </div>

          <div class="banner-box">
            <button class="banner-nav nav-prev" @click="slideBanner(-1)">❮</button>
            <button class="banner-nav nav-next" @click="slideBanner(1)">❯</button>

            <!-- 铺底 3D 渲染图 -->
            <img
              src="./assets/images/product/puchacha-bg-full@2x.png"
              alt="浦查查全景切图"
              class="banner-bg-img"
            />

            <!-- 浮层文字信息 -->
            <div class="banner-content">
              <div>
                <h2 class="banner-title">{{ currentBanner.name }}</h2>
                <p class="banner-desc">{{ currentBanner.desc }}</p>
                <div class="banner-tags">{{ currentBanner.keywords }}</div>
              </div>
              <div>
                <button class="explore-btn" @click="openModal(currentBanner.name, '正在进入该产品的全景工作台...')">
                  立即探索 →
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 底部三列动态数据流 -->
      <section class="stream-columns">
        <!-- 历史访问 -->
        <div class="stream-card">
          <div class="stream-card-header">
            <div class="header-title">
              <img src="./assets/images/stream/icon-history@2x.png" alt="历史访问" class="stream-icon" />
              <span>历史访问</span>
            </div>
            <button class="refresh-btn" @click="refreshHistory">
              换一换
              <span class="refresh-icon" :class="{ rotating: isRefreshingHistory }">↻</span>
            </button>
          </div>
          <ul class="stream-list">
            <li
              v-for="item in historyList"
              :key="item.id"
              class="stream-item"
              @click="handleItemClick(item)"
            >
              <span class="bullet-dot"></span>
              <span class="item-name">{{ item.title }}</span>
            </li>
          </ul>
        </div>

        <!-- 我的收藏 -->
        <div class="stream-card">
          <div class="stream-card-header">
            <div class="header-title">
              <img src="./assets/images/stream/icon-favorite@2x.png" alt="我的收藏" class="stream-icon" />
              <span>我的收藏</span>
            </div>
            <button class="refresh-btn" @click="refreshFavorite">
              换一换
              <span class="refresh-icon" :class="{ rotating: isRefreshingFavorite }">↻</span>
            </button>
          </div>
          <ul class="stream-list">
            <li
              v-for="item in favoriteList"
              :key="item.id"
              class="stream-item"
              @click="handleItemClick(item)"
            >
              <span class="bullet-star">★</span>
              <span class="item-name">{{ item.title }}</span>
            </li>
          </ul>
        </div>

        <!-- 猜你想看 -->
        <div class="stream-card">
          <div class="stream-card-header">
            <div class="header-title">
              <img src="./assets/images/stream/icon-recommend@2x.png" alt="猜你想看" class="stream-icon" />
              <span>猜你想看</span>
            </div>
            <button class="refresh-btn" @click="refreshRecommend">
              换一换
              <span class="refresh-icon" :class="{ rotating: isRefreshingRecommend }">↻</span>
            </button>
          </div>
          <ul class="stream-list">
            <li
              v-for="item in recommendList"
              :key="item.id"
              class="stream-item"
              @click="handleItemClick(item)"
            >
              <span class="bullet-tag">AI</span>
              <span class="item-name">{{ item.title }}</span>
            </li>
          </ul>
        </div>
      </section>
    </main>

    <!-- 3. 模态反馈弹窗 (Vue 响应式，无原生 alert 阻塞) -->
    <Teleport to="body">
      <div v-if="modalVisible" class="modal-backdrop" @click="closeModal">
        <div class="modal-card" @click.stop>
          <div class="modal-header">
            <h3 class="modal-title">{{ modalTitle }}</h3>
            <button class="modal-close-btn" @click="closeModal">✕</button>
          </div>
          <div class="modal-body" v-html="modalContent"></div>
          <div class="modal-footer">
            <button class="modal-primary-btn" @click="closeModal">确认</button>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<style scoped>
/* ==========================================================================
   CSS TOKENS & 样式封装
   ========================================================================== */
.home-container {
  --brand: #256DF8;
  --brand-hover: #1A57D0;
  --bg-page: #F3F5FA;
  --bg-card: #FFFFFF;
  --text-main: #303133;
  --text-sub: #606266;
  --text-muted: #9397A4;
  --border-line: #E5EBF5;
  --radius-sm: 8px;
  --radius-md: 14px;
  --radius-lg: 20px;

  min-height: 100vh;
  background-color: var(--bg-page);
  background-image: url('./assets/images/page-bg-mesh@2x.png');
  background-repeat: no-repeat;
  background-size: 100% 640px;
  color: var(--text-main);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Microsoft YaHei", sans-serif;
  box-sizing: border-box;
}

/* 深色模式适配 */
.home-container.dark-theme {
  --bg-page: #0C121E;
  --bg-card: #151F32;
  --text-main: #F8FAFC;
  --text-sub: #94A3B8;
  --text-muted: #64748B;
  --border-line: #24344E;
}

/* 顶部导航 */
.app-header {
  height: 64px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border-line);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 32px;
  position: sticky;
  top: 0;
  z-index: 50;
}
.dark-theme .app-header {
  background: rgba(21, 31, 50, 0.92);
}

.header-left {
  display: flex;
  align-items: center;
  gap: 40px;
}
.brand-logo {
  display: flex;
  align-items: center;
  gap: 10px;
}
.logo-img {
  height: 28px;
  object-fit: contain;
}
.brand-title {
  font-size: 16px;
  font-weight: 700;
  color: #1A365D;
}
.dark-theme .brand-title {
  color: #F1F5F9;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 24px;
}
.nav-item {
  font-size: 14px;
  color: var(--text-sub);
  text-decoration: none;
  padding: 6px 4px;
  transition: all 0.2s;
  position: relative;
}
.nav-item.active {
  color: var(--brand);
  font-weight: 600;
}
.nav-item.active::after {
  content: '';
  position: absolute;
  bottom: -18px;
  left: 0;
  right: 0;
  height: 3px;
  background: var(--brand);
  border-radius: 3px 3px 0 0;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 16px;
}
.user-greeting {
  font-size: 13px;
  color: var(--text-sub);
}
.feedback-btn, .theme-btn {
  background: #F1F5F9;
  border: 1px solid var(--border-line);
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  color: var(--text-main);
  transition: background 0.2s;
}
.dark-theme .feedback-btn, .dark-theme .theme-btn {
  background: #1E293B;
}

/* 主内容容器 */
.main-content {
  max-width: 1320px;
  margin: 0 auto;
  padding: 24px 24px 60px;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* AI 检索栏 */
.ai-search-card {
  display: flex;
  align-items: center;
  gap: 16px;
}
.search-inner-box {
  flex: 1;
  background: #FFFFFF;
  border-radius: 28px;
  padding: 8px 10px 8px 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 14px 36px 0 rgba(192, 205, 253, 0.38);
  border: 1px solid rgba(255, 255, 255, 0.8);
}
.dark-theme .search-inner-box {
  background: #1E293B;
  box-shadow: 0 14px 36px 0 rgba(0, 0, 0, 0.4);
}
.search-left-panel {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 16px;
}
.search-text-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 15px;
  color: var(--text-main);
  background: transparent;
}
.search-text-input::placeholder {
  color: var(--text-muted);
}
.agent-capsules {
  display: flex;
  align-items: center;
  gap: 8px;
}
.agent-pill-btn {
  cursor: pointer;
  transition: transform 0.2s, filter 0.2s;
  border-radius: 16px;
}
.agent-pill-btn:hover {
  transform: translateY(-1px);
}
.agent-pill-btn.active {
  box-shadow: 0 0 0 2px var(--brand);
}
.agent-img {
  height: 32px;
  display: block;
}
.search-action-btn {
  border: none;
  background: transparent;
  cursor: pointer;
  padding: 0;
  display: flex;
  align-items: center;
}
.search-action-btn img {
  height: 44px;
  width: 44px;
}

/* 用数助手入口 */
.assistant-entry-badge {
  background: linear-gradient(135deg, #4A6CF7 0%, #7B61FF 100%);
  border-radius: 28px;
  padding: 8px 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  color: #FFFFFF;
  box-shadow: 0 8px 20px rgba(99, 102, 241, 0.35);
  transition: transform 0.2s;
}
.assistant-entry-badge:hover {
  transform: translateY(-2px);
}
.robot-avatar {
  height: 36px;
  width: 36px;
  object-fit: contain;
}
.assistant-text .primary-text {
  font-size: 14px;
  font-weight: 700;
}
.assistant-text .sub-text {
  font-size: 12px;
  opacity: 0.9;
}
.star-sparkle {
  color: #FFD166;
  font-size: 14px;
}

/* 工作台栅格 */
.workspace-layout {
  display: grid;
  grid-template-columns: 1fr 475px;
  gap: 24px;
}
@media (max-width: 1100px) {
  .workspace-layout {
    grid-template-columns: 1fr;
  }
}

/* 左侧 6 卡片面板 */
.feature-panel {
  background: var(--bg-card);
  border-radius: var(--radius-lg);
  padding: 24px;
  border: 1px solid var(--border-line);
  position: relative;
}
.mode-switch-container {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 20px;
}
.mode-segmented-control {
  background: #EDF2F9;
  border-radius: 20px;
  padding: 3px;
  display: inline-flex;
}
.dark-theme .mode-segmented-control {
  background: #24344E;
}
.mode-btn {
  border: none;
  background: transparent;
  padding: 6px 18px;
  border-radius: 16px;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  color: var(--text-sub);
  transition: all 0.2s;
}
.mode-btn.active {
  background: #FFFFFF;
  color: var(--brand);
  font-weight: 600;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
}
.dark-theme .mode-btn.active {
  background: #151F32;
  color: #60A5FA;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  transition: opacity 0.2s;
}
.cards-grid.grid-flip {
  opacity: 0.4;
}

.feature-card {
  background: #F8FAFD;
  border: 1px solid var(--border-line);
  border-radius: var(--radius-md);
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 14px;
  cursor: pointer;
  transition: all 0.2s;
}
.dark-theme .feature-card {
  background: #1A273D;
}
.feature-card:hover:not(.disabled) {
  transform: translateY(-2px);
  border-color: #3B82F6;
  box-shadow: 0 6px 16px rgba(59, 130, 246, 0.12);
}
.feature-card.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
.card-icon {
  width: 48px;
  height: 48px;
  object-fit: contain;
}
.card-title {
  font-size: 15px;
  font-weight: 600;
  color: var(--text-main);
  margin-bottom: 4px;
}
.card-link {
  font-size: 12px;
  color: var(--brand);
}
.card-link.link-disabled {
  color: var(--text-muted);
}

.slider-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.9);
  border: 1px solid var(--border-line);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  z-index: 10;
}
.arrow-left { left: 8px; }
.arrow-right { right: 8px; }

/* 右侧数据产品专区 */
.product-panel {
  background: var(--bg-card);
  border-radius: var(--radius-lg);
  padding: 24px;
  border: 1px solid var(--border-line);
  display: flex;
  flex-direction: column;
}
.panel-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 16px;
}
.header-icon {
  width: 22px;
  height: 22px;
}
.header-label {
  font-size: 16px;
  font-weight: 700;
  color: var(--text-main);
}
.banner-box {
  position: relative;
  flex: 1;
  min-height: 230px;
  border-radius: var(--radius-md);
  overflow: hidden;
}
.banner-bg-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
}
.banner-content {
  position: relative;
  z-index: 2;
  height: 100%;
  padding: 24px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-sizing: border-box;
}
.banner-title {
  font-size: 24px;
  font-weight: 800;
  color: #1E293B;
  margin: 0 0 6px 0;
}
.banner-desc {
  font-size: 13px;
  color: #475569;
  margin: 0 0 10px 0;
}
.banner-tags {
  font-size: 12px;
  color: #64748B;
}
.explore-btn {
  background: linear-gradient(135deg, #4A6CF7 0%, #256DF8 100%);
  color: #FFFFFF;
  border: none;
  padding: 8px 18px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(37, 109, 248, 0.3);
}
.banner-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.85);
  border: none;
  cursor: pointer;
  z-index: 5;
  display: flex;
  align-items: center;
  justify-content: center;
}
.nav-prev { left: 8px; }
.nav-next { right: 8px; }

/* 底部三列动态流 */
.stream-columns {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
@media (max-width: 900px) {
  .stream-columns {
    grid-template-columns: 1fr;
  }
}
.stream-card {
  background: var(--bg-card);
  border-radius: var(--radius-md);
  padding: 20px;
  border: 1px solid var(--border-line);
}
.stream-card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
  padding-bottom: 8px;
  border-bottom: 1px solid var(--border-line);
}
.header-title {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 15px;
  font-weight: 700;
  color: var(--text-main);
}
.stream-icon {
  width: 20px;
  height: 20px;
}
.refresh-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  font-size: 12px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  gap: 4px;
}
.refresh-btn:hover {
  color: var(--brand);
}
.refresh-icon.rotating {
  display: inline-block;
  animation: spin 0.6s linear infinite;
}
@keyframes spin {
  100% { transform: rotate(360deg); }
}
.stream-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.stream-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 13px;
  color: var(--text-sub);
  cursor: pointer;
  transition: color 0.15s;
}
.stream-item:hover {
  color: var(--brand);
}
.bullet-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--brand);
}
.bullet-star {
  color: #F59E0B;
  font-size: 14px;
}
.bullet-tag {
  background: rgba(37, 109, 248, 0.1);
  color: var(--brand);
  font-size: 10px;
  padding: 1px 4px;
  border-radius: 3px;
  font-weight: 700;
}
.item-name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 模态弹窗 */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}
.modal-card {
  background: #FFFFFF;
  border-radius: 16px;
  width: 460px;
  max-width: 90vw;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  overflow: hidden;
}
.modal-header {
  padding: 16px 20px;
  border-bottom: 1px solid #E2E8F0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.modal-title {
  margin: 0;
  font-size: 16px;
  font-weight: 700;
}
.modal-close-btn {
  background: transparent;
  border: none;
  font-size: 16px;
  cursor: pointer;
  color: #94A3B8;
}
.modal-body {
  padding: 20px;
  font-size: 14px;
  color: #334155;
  line-height: 1.6;
}
.modal-footer {
  padding: 12px 20px;
  background: #F8FAFC;
  display: flex;
  justify-content: flex-end;
}
.modal-primary-btn {
  background: var(--brand);
  color: #FFFFFF;
  border: none;
  padding: 6px 18px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
}
</style>
