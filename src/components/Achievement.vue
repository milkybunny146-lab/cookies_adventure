<script setup>
import { ref, computed } from 'vue'

const currentFilter = ref('all')
const selectedAchievement = ref(null)

const achievements = ref([
  {
    id: 1,
    name: '初出茅廬',
    description: '完成第一次戰鬥',
    category: 'battle',
    categoryName: '戰鬥',
    rarity: 'common',
    rarityName: '普通',
    icon: '🎯',
    progress: 1,
    target: 1,
    completed: true,
    reward: { gold: 100, exp: 50 },
    unlockedAt: '2024-01-15'
  },
  {
    id: 2,
    name: '百戰不殆',
    description: '完成 100 場戰鬥',
    category: 'battle',
    categoryName: '戰鬥',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '⚔️',
    progress: 87,
    target: 100,
    completed: false,
    reward: { gold: 1000, exp: 500, items: ['強化石 x10'] }
  },
  {
    id: 3,
    name: '收藏家',
    description: '收集 10 位餅乾勇者',
    category: 'collection',
    categoryName: '收集',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '🍪',
    progress: 8,
    target: 10,
    completed: false,
    reward: { gold: 800, exp: 400 }
  },
  {
    id: 4,
    name: '傳說收集者',
    description: '收集 5 位傳說級餅乾',
    category: 'collection',
    categoryName: '收集',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '🌟',
    progress: 3,
    target: 5,
    completed: false,
    reward: { gold: 5000, exp: 2000, items: ['傳說強化石 x5'] }
  },
  {
    id: 5,
    name: '副本征服者',
    description: '通關所有普通難度副本',
    category: 'dungeon',
    categoryName: '副本',
    rarity: 'epic',
    rarityName: '史詩',
    icon: '🏰',
    progress: 2,
    target: 4,
    completed: false,
    reward: { gold: 2000, exp: 1000, items: ['精煉石 x20'] }
  },
  {
    id: 6,
    name: '深淵挑戰者',
    description: '完成深淵地獄副本',
    category: 'dungeon',
    categoryName: '副本',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '💀',
    progress: 0,
    target: 1,
    completed: false,
    reward: { gold: 10000, exp: 5000, items: ['神話裝備', '深淵結晶 x3'] }
  },
  {
    id: 7,
    name: '道具大師',
    description: '使用 50 個道具',
    category: 'item',
    categoryName: '道具',
    rarity: 'common',
    rarityName: '普通',
    icon: '🎒',
    progress: 32,
    target: 50,
    completed: false,
    reward: { gold: 500, exp: 250 }
  },
  {
    id: 8,
    name: '強化達人',
    description: '強化裝備 20 次',
    category: 'item',
    categoryName: '道具',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '💎',
    progress: 12,
    target: 20,
    completed: false,
    reward: { gold: 1500, exp: 750, items: ['精煉石 x10'] }
  },
  {
    id: 9,
    name: '每日冒險家',
    description: '連續登入 7 天',
    category: 'daily',
    categoryName: '每日',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '📅',
    progress: 5,
    target: 7,
    completed: false,
    reward: { gold: 700, exp: 350, items: ['餅乾召喚券 x2'] }
  },
  {
    id: 10,
    name: '忠實玩家',
    description: '連續登入 30 天',
    category: 'daily',
    categoryName: '每日',
    rarity: 'epic',
    rarityName: '史詩',
    icon: '👑',
    progress: 5,
    target: 30,
    completed: false,
    reward: { gold: 3000, exp: 1500, items: ['傳說強化石 x3', '餅乾召喚券 x5'] }
  },
  {
    id: 11,
    name: '戰力突破',
    description: '總戰力達到 50000',
    category: 'power',
    categoryName: '戰力',
    rarity: 'epic',
    rarityName: '史詩',
    icon: '💪',
    progress: 48720,
    target: 50000,
    completed: false,
    reward: { gold: 2500, exp: 1200, items: ['高級經驗書 x5'] }
  },
  {
    id: 12,
    name: '無敵戰神',
    description: '總戰力達到 100000',
    category: 'power',
    categoryName: '戰力',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '🔥',
    progress: 48720,
    target: 100000,
    completed: false,
    reward: { gold: 10000, exp: 5000, items: ['神話裝備', '傳說強化石 x10'] }
  }
])

const filters = ref([
  { key: 'all', name: '全部' },
  { key: 'battle', name: '戰鬥' },
  { key: 'collection', name: '收集' },
  { key: 'dungeon', name: '副本' },
  { key: 'item', name: '道具' },
  { key: 'daily', name: '每日' },
  { key: 'power', name: '戰力' }
])

const filteredAchievements = computed(() => {
  if (currentFilter.value === 'all') {
    return achievements.value
  }
  return achievements.value.filter(a => a.category === currentFilter.value)
})

const completedCount = computed(() => {
  return achievements.value.filter(a => a.completed).length
})

const totalCount = computed(() => {
  return achievements.value.length
})

const completionRate = computed(() => {
  return Math.round((completedCount.value / totalCount.value) * 100)
})

const getRarityClass = (rarity) => {
  return `rarity-${rarity}`
}

const getRarityColor = (rarity) => {
  const colors = {
    common: '#95a5a6',
    rare: '#3498db',
    epic: '#9b59b6',
    legendary: '#f39c12'
  }
  return colors[rarity] || colors.common
}

const getProgressPercentage = (achievement) => {
  return Math.min((achievement.progress / achievement.target) * 100, 100)
}

const selectAchievement = (achievement) => {
  selectedAchievement.value = achievement
}

const claimReward = (achievement) => {
  if (achievement.completed) {
    alert(`領取獎勵：${achievement.name}\n金幣：${achievement.reward.gold}\n經驗：${achievement.reward.exp}`)
    // TODO: 實際領取邏輯
  }
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">🏆 成就系統</h1>

    <!-- 成就統計 -->
    <div class="stats-section">
      <div class="stat-card">
        <div class="stat-icon">✅</div>
        <div class="stat-content">
          <span class="stat-label">已完成</span>
          <span class="stat-value">{{ completedCount }} / {{ totalCount }}</span>
        </div>
      </div>
      <div class="stat-card">
        <div class="stat-icon">📊</div>
        <div class="stat-content">
          <span class="stat-label">完成率</span>
          <span class="stat-value">{{ completionRate }}%</span>
        </div>
      </div>
      <div class="stat-card">
        <div class="stat-icon">🎁</div>
        <div class="stat-content">
          <span class="stat-label">可領取</span>
          <span class="stat-value">{{ achievements.filter(a => a.completed).length }}</span>
        </div>
      </div>
    </div>

    <!-- 分類篩選 -->
    <div class="filter-bar">
      <button
        v-for="filter in filters"
        :key="filter.key"
        class="filter-btn"
        :class="{ active: currentFilter === filter.key }"
        @click="currentFilter = filter.key"
      >
        {{ filter.name }}
      </button>
    </div>

    <!-- 成就數量 -->
    <p class="achievement-count">
      顯示 {{ filteredAchievements.length }} / {{ achievements.length }} 個成就
    </p>

    <!-- 成就列表 -->
    <div class="achievement-grid">
      <div
        v-for="achievement in filteredAchievements"
        :key="achievement.id"
        class="achievement-card"
        :class="{ 
          selected: selectedAchievement?.id === achievement.id,
          completed: achievement.completed,
          [getRarityClass(achievement.rarity)]: true
        }"
        @click="selectAchievement(achievement)"
      >
        <!-- 完成標記 -->
        <div v-if="achievement.completed" class="completed-badge">
          ✅
        </div>

        <!-- 稀有度邊框 -->
        <div 
          class="rarity-border"
          :style="{ borderColor: getRarityColor(achievement.rarity) }"
        ></div>

        <!-- 稀有度標籤 -->
        <div 
          class="rarity-badge"
          :style="{ backgroundColor: getRarityColor(achievement.rarity) }"
        >
          {{ achievement.rarityName }}
        </div>

        <!-- 成就圖示 -->
        <div class="achievement-icon">
          {{ achievement.icon }}
        </div>

        <!-- 成就資訊 -->
        <div class="achievement-info">
          <h3 class="achievement-name">{{ achievement.name }}</h3>
          <span class="achievement-category">{{ achievement.categoryName }}</span>
          <p class="achievement-desc">{{ achievement.description }}</p>
        </div>

        <!-- 進度條 -->
        <div class="progress-section">
          <div class="progress-info">
            <span>{{ achievement.progress }} / {{ achievement.target }}</span>
            <span>{{ Math.round(getProgressPercentage(achievement)) }}%</span>
          </div>
          <div class="progress-bar">
            <div 
              class="progress-fill"
              :style="{ 
                width: getProgressPercentage(achievement) + '%',
                backgroundColor: achievement.completed ? '#27ae60' : getRarityColor(achievement.rarity)
              }"
            ></div>
          </div>
        </div>

        <!-- 獎勵預覽 -->
        <div class="rewards-preview">
          <div class="reward-item">
            <span>💰</span>
            <span>{{ achievement.reward.gold }}</span>
          </div>
          <div class="reward-item">
            <span>⭐</span>
            <span>{{ achievement.reward.exp }}</span>
          </div>
          <div v-if="achievement.reward.items" class="reward-item">
            <span>🎁</span>
            <span>{{ achievement.reward.items.length }} 件</span>
          </div>
        </div>

        <!-- 領取按鈕 -->
        <button 
          v-if="achievement.completed"
          class="claim-btn"
          @click.stop="claimReward(achievement)"
        >
          領取獎勵
        </button>
      </div>
    </div>

    <!-- 選中成就詳情 -->
    <div v-if="selectedAchievement" class="achievement-detail">
      <h3>📋 {{ selectedAchievement.name }} - 詳細資訊</h3>
      <div class="detail-content">
        <div class="detail-row">
          <span class="detail-label">分類</span>
          <span class="detail-value">{{ selectedAchievement.categoryName }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">稀有度</span>
          <span 
            class="detail-value"
            :style="{ color: getRarityColor(selectedAchievement.rarity) }"
          >
            {{ selectedAchievement.rarityName }}
          </span>
        </div>
        <div class="detail-row">
          <span class="detail-label">進度</span>
          <span class="detail-value">{{ selectedAchievement.progress }} / {{ selectedAchievement.target }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">狀態</span>
          <span 
            class="detail-value"
            :style="{ color: selectedAchievement.completed ? '#27ae60' : '#7f8c8d' }"
          >
            {{ selectedAchievement.completed ? '已完成' : '進行中' }}
          </span>
        </div>
        <div v-if="selectedAchievement.unlockedAt" class="detail-row">
          <span class="detail-label">完成時間</span>
          <span class="detail-value">{{ selectedAchievement.unlockedAt }}</span>
        </div>
      </div>
      <div class="rewards-detail">
        <h4>🎁 獎勵內容</h4>
        <div class="rewards-list">
          <div class="reward-detail-item">
            <span>💰 金幣</span>
            <span>{{ selectedAchievement.reward.gold }}</span>
          </div>
          <div class="reward-detail-item">
            <span>⭐ 經驗值</span>
            <span>{{ selectedAchievement.reward.exp }}</span>
          </div>
          <div v-if="selectedAchievement.reward.items" class="reward-detail-item">
            <span>📦 道具</span>
            <span>{{ selectedAchievement.reward.items.join(', ') }}</span>
          </div>
        </div>
      </div>
      <div v-if="selectedAchievement.completed" class="action-section">
        <button class="action-btn" @click="claimReward(selectedAchievement)">
          領取獎勵
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped>
.main-content {
  flex: 1;
  padding: 2rem;
  background-color: #f5f5f5;
}

.page-title {
  font-size: 1.8rem;
  color: #2c3e50;
  margin-bottom: 1.5rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #667eea;
}

/* 統計區 */
.stats-section {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  border-radius: 12px;
  padding: 1.2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  gap: 1rem;
}

.stat-icon {
  font-size: 2rem;
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stat-content {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.stat-label {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.stat-value {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2c3e50;
}

/* 篩選器 */
.filter-bar {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 0.5rem 1.2rem;
  border: 2px solid #ddd;
  border-radius: 20px;
  background: white;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.3s;
}

.filter-btn:hover {
  border-color: #667eea;
}

.filter-btn.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}

.achievement-count {
  color: #7f8c8d;
  margin-bottom: 1.5rem;
}

/* 成就網格 */
.achievement-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1.5rem;
}

/* 成就卡片 */
.achievement-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s;
  position: relative;
  cursor: pointer;
  border: 2px solid transparent;
  overflow: hidden;
}

.achievement-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.achievement-card.selected {
  border-color: #667eea;
  box-shadow: 0 4px 16px rgba(102, 126, 234, 0.3);
}

.achievement-card.completed {
  background: linear-gradient(135deg, #f0fdf4 0%, #ffffff 100%);
}

/* 完成標記 */
.completed-badge {
  position: absolute;
  top: 12px;
  left: 12px;
  font-size: 1.5rem;
  z-index: 5;
}

/* 稀有度邊框 */
.rarity-border {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
}

/* 稀有度標籤 */
.rarity-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  padding: 0.25rem 0.6rem;
  border-radius: 10px;
  color: white;
  font-size: 0.75rem;
  font-weight: bold;
}

/* 成就圖示 */
.achievement-icon {
  width: 80px;
  height: 80px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  margin: 0 auto 1rem;
}

.achievement-card.rarity-legendary .achievement-icon {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  box-shadow: 0 0 20px rgba(243, 156, 18, 0.4);
}

.achievement-card.completed .achievement-icon {
  background: linear-gradient(135deg, #27ae60, #229954);
}

/* 成就資訊 */
.achievement-info {
  text-align: center;
  margin-bottom: 1rem;
}

.achievement-name {
  font-size: 1.2rem;
  color: #2c3e50;
  margin-bottom: 0.3rem;
}

.achievement-category {
  display: inline-block;
  background: #ecf0f1;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  font-size: 0.75rem;
  color: #7f8c8d;
  margin-bottom: 0.5rem;
}

.achievement-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  line-height: 1.5;
  margin: 0.5rem 0 0;
}

/* 進度條 */
.progress-section {
  margin: 1rem 0;
}

.progress-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
  font-size: 0.85rem;
  color: #7f8c8d;
}

.progress-bar {
  height: 10px;
  background: #ecf0f1;
  border-radius: 5px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  border-radius: 5px;
  transition: width 0.5s ease;
}

/* 獎勵預覽 */
.rewards-preview {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
  justify-content: center;
}

.reward-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.85rem;
  color: #7f8c8d;
}

.reward-item span:first-child {
  font-size: 1.2rem;
}

/* 領取按鈕 */
.claim-btn {
  width: 100%;
  padding: 0.7rem;
  background: linear-gradient(135deg, #27ae60 0%, #229954 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
  margin-top: 1rem;
}

.claim-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(39, 174, 96, 0.4);
}

/* 成就詳情 */
.achievement-detail {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  margin-top: 2rem;
}

.achievement-detail h3 {
  color: #2c3e50;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.detail-content {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  margin-bottom: 1.5rem;
}

.detail-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #ecf0f1;
}

.detail-label {
  font-size: 0.9rem;
  color: #7f8c8d;
}

.detail-value {
  font-size: 0.95rem;
  font-weight: bold;
  color: #2c3e50;
}

.rewards-detail h4 {
  color: #2c3e50;
  margin-bottom: 0.8rem;
}

.rewards-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.reward-detail-item {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem;
  background: #f8f9fa;
  border-radius: 6px;
  font-size: 0.9rem;
}

.action-section {
  margin-top: 1rem;
}

.action-btn {
  width: 100%;
  padding: 0.8rem;
  background: linear-gradient(135deg, #27ae60 0%, #229954 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.action-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(39, 174, 96, 0.4);
}

/* RWD */
@media (max-width: 768px) {
  .main-content {
    padding: 1rem;
  }

  .page-title {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  .stats-section {
    grid-template-columns: 1fr;
    gap: 0.8rem;
    margin-bottom: 1rem;
  }

  .stat-card {
    padding: 1rem;
  }

  .stat-icon {
    width: 50px;
    height: 50px;
    font-size: 1.5rem;
  }

  .stat-value {
    font-size: 1.3rem;
  }

  .filter-bar {
    gap: 0.3rem;
    margin-bottom: 0.8rem;
    flex-wrap: wrap;
  }

  .filter-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }

  .achievement-count {
    font-size: 0.85rem;
    margin-bottom: 1rem;
  }

  .achievement-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .achievement-card {
    padding: 1rem;
  }

  .achievement-icon {
    width: 60px;
    height: 60px;
    font-size: 2rem;
  }

  .achievement-name {
    font-size: 1.1rem;
  }

  .achievement-detail {
    margin-top: 1.5rem;
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .filter-btn {
    padding: 0.35rem 0.7rem;
    font-size: 0.75rem;
    flex: 1;
    min-width: calc(50% - 0.15rem);
  }

  .achievement-card {
    padding: 0.8rem;
  }

  .achievement-icon {
    width: 50px;
    height: 50px;
    font-size: 1.8rem;
  }

  .achievement-name {
    font-size: 1rem;
  }

  .achievement-desc {
    font-size: 0.8rem;
  }

  .claim-btn {
    padding: 0.6rem;
    font-size: 0.85rem;
  }
}
</style>

