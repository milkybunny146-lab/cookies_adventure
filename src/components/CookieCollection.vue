<script setup>
import { ref, computed } from 'vue'

const currentFilter = ref('all')

const cookies = ref([
  {
    id: 1,
    name: '勇敢餅乾',
    rarity: 'common',
    rarityName: '普通',
    type: '前鋒',
    element: '🔥 火焰',
    description: '充滿勇氣的餅乾戰士，總是衝在最前線保護夥伴。',
    stats: { hp: 850, atk: 320, def: 180 },
    skill: '烈焰衝鋒'
  },
  {
    id: 2,
    name: '魔法餅乾',
    rarity: 'rare',
    rarityName: '稀有',
    type: '法師',
    element: '💜 魔法',
    description: '精通魔法的餅乾，能施放強大的範圍法術。',
    stats: { hp: 520, atk: 480, def: 120 },
    skill: '魔力風暴'
  },
  {
    id: 3,
    name: '治癒餅乾',
    rarity: 'rare',
    rarityName: '稀有',
    type: '輔助',
    element: '💚 自然',
    description: '溫柔善良的餅乾，擁有治癒夥伴的神奇力量。',
    stats: { hp: 680, atk: 180, def: 220 },
    skill: '生命之光'
  },
  {
    id: 4,
    name: '暗影餅乾',
    rarity: 'epic',
    rarityName: '史詩',
    type: '刺客',
    element: '🖤 暗影',
    description: '來自黑暗的神秘餅乾，擅長隱匿與暗殺。',
    stats: { hp: 480, atk: 520, def: 100 },
    skill: '暗影突襲'
  },
  {
    id: 5,
    name: '守護餅乾',
    rarity: 'epic',
    rarityName: '史詩',
    type: '坦克',
    element: '🛡️ 大地',
    description: '堅毅不屈的餅乾，能抵擋最猛烈的攻擊。',
    stats: { hp: 1200, atk: 150, def: 450 },
    skill: '鋼鐵壁壘'
  },
  {
    id: 6,
    name: '海洋餅乾',
    rarity: 'legendary',
    rarityName: '傳說',
    type: '法師',
    element: '💧 海洋',
    description: '傳說中來自深海的餅乾，掌控海洋之力。',
    stats: { hp: 720, atk: 580, def: 280 },
    skill: '海嘯狂潮'
  },
  {
    id: 7,
    name: '龍焰餅乾',
    rarity: 'legendary',
    rarityName: '傳說',
    type: '前鋒',
    element: '🐉 龍族',
    description: '擁有遠古龍族血脈的傳奇餅乾，戰力無雙。',
    stats: { hp: 980, atk: 620, def: 350 },
    skill: '龍息吐焰'
  },
  {
    id: 8,
    name: '星光餅乾',
    rarity: 'legendary',
    rarityName: '傳說',
    type: '輔助',
    element: '⭐ 星辰',
    description: '來自星空的神聖餅乾，擁有祝福全隊的力量。',
    stats: { hp: 800, atk: 380, def: 320 },
    skill: '星辰祝福'
  }
])

const filters = ref([
  { key: 'all', name: '全部' },
  { key: 'common', name: '普通' },
  { key: 'rare', name: '稀有' },
  { key: 'epic', name: '史詩' },
  { key: 'legendary', name: '傳說' }
])

const filteredCookies = computed(() => {
  if (currentFilter.value === 'all') {
    return cookies.value
  }
  return cookies.value.filter(c => c.rarity === currentFilter.value)
})

const getRarityClass = (rarity) => {
  return `rarity-${rarity}`
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">🍪 餅乾圖鑑</h1>
    
    <!-- 篩選器 -->
    <div class="filter-bar">
      <button
        v-for="filter in filters"
        :key="filter.key"
        class="filter-btn"
        :class="{ active: currentFilter === filter.key, [filter.key]: true }"
        @click="currentFilter = filter.key"
      >
        {{ filter.name }}
      </button>
    </div>

    <!-- 餅乾數量 -->
    <p class="cookie-count">
      顯示 {{ filteredCookies.length }} / {{ cookies.length }} 隻餅乾
    </p>

    <!-- 餅乾卡片 -->
    <div class="cookie-grid">
      <div
        v-for="cookie in filteredCookies"
        :key="cookie.id"
        class="cookie-card"
        :class="getRarityClass(cookie.rarity)"
      >
        <!-- 稀有度標籤 -->
        <div class="rarity-badge" :class="cookie.rarity">
          {{ cookie.rarityName }}
        </div>

        <!-- 餅乾圖示 -->
        <div class="cookie-avatar">
          🍪
        </div>

        <!-- 餅乾資訊 -->
        <div class="cookie-info">
          <h3 class="cookie-name">{{ cookie.name }}</h3>
          <div class="cookie-meta">
            <span class="type">{{ cookie.type }}</span>
            <span class="element">{{ cookie.element }}</span>
          </div>
          <p class="cookie-desc">{{ cookie.description }}</p>
        </div>

        <!-- 數值區 -->
        <div class="cookie-stats">
          <div class="stat">
            <span class="stat-label">❤️ HP</span>
            <div class="stat-bar">
              <div class="stat-fill hp" :style="{ width: (cookie.stats.hp / 1200 * 100) + '%' }"></div>
            </div>
            <span class="stat-value">{{ cookie.stats.hp }}</span>
          </div>
          <div class="stat">
            <span class="stat-label">⚔️ ATK</span>
            <div class="stat-bar">
              <div class="stat-fill atk" :style="{ width: (cookie.stats.atk / 620 * 100) + '%' }"></div>
            </div>
            <span class="stat-value">{{ cookie.stats.atk }}</span>
          </div>
          <div class="stat">
            <span class="stat-label">🛡️ DEF</span>
            <div class="stat-bar">
              <div class="stat-fill def" :style="{ width: (cookie.stats.def / 450 * 100) + '%' }"></div>
            </div>
            <span class="stat-value">{{ cookie.stats.def }}</span>
          </div>
        </div>

        <!-- 技能 -->
        <div class="cookie-skill">
          <span class="skill-label">必殺技</span>
          <span class="skill-name">{{ cookie.skill }}</span>
        </div>
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

.filter-btn.active.rare {
  background: #3498db;
  border-color: #3498db;
}

.filter-btn.active.epic {
  background: #9b59b6;
  border-color: #9b59b6;
}

.filter-btn.active.legendary {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  border-color: #f39c12;
}

.cookie-count {
  color: #7f8c8d;
  margin-bottom: 1.5rem;
}

/* 餅乾網格 */
.cookie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1.5rem;
}

/* 餅乾卡片 */
.cookie-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s, box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}

.cookie-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

/* 稀有度邊框 */
.cookie-card.rarity-common {
  border-top: 3px solid #95a5a6;
}

.cookie-card.rarity-rare {
  border-top: 3px solid #3498db;
}

.cookie-card.rarity-epic {
  border-top: 3px solid #9b59b6;
}

.cookie-card.rarity-legendary {
  border-top: 3px solid #f39c12;
  background: linear-gradient(135deg, #fffdf7 0%, #fff9e6 100%);
}

/* 稀有度標籤 */
.rarity-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  padding: 0.25rem 0.6rem;
  border-radius: 10px;
  font-size: 0.75rem;
  font-weight: bold;
}

.rarity-badge.common {
  background: #ecf0f1;
  color: #7f8c8d;
}

.rarity-badge.rare {
  background: #e8f4fc;
  color: #3498db;
}

.rarity-badge.epic {
  background: #f3e8fc;
  color: #9b59b6;
}

.rarity-badge.legendary {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  color: white;
}

/* 餅乾頭像 */
.cookie-avatar {
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

.rarity-legendary .cookie-avatar {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  box-shadow: 0 0 20px rgba(243, 156, 18, 0.4);
}

/* 餅乾資訊 */
.cookie-info {
  text-align: center;
  margin-bottom: 1rem;
}

.cookie-name {
  font-size: 1.2rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.cookie-meta {
  display: flex;
  justify-content: center;
  gap: 0.8rem;
  margin-bottom: 0.8rem;
}

.type {
  background: #ecf0f1;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  font-size: 0.8rem;
  color: #7f8c8d;
}

.element {
  font-size: 0.85rem;
}

.cookie-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  line-height: 1.5;
}

/* 數值條 */
.cookie-stats {
  margin: 1rem 0;
}

.stat {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
}

.stat-label {
  width: 60px;
  font-size: 0.8rem;
}

.stat-bar {
  flex: 1;
  height: 8px;
  background: #ecf0f1;
  border-radius: 4px;
  overflow: hidden;
}

.stat-fill {
  height: 100%;
  border-radius: 4px;
  transition: width 0.5s ease;
}

.stat-fill.hp {
  background: linear-gradient(90deg, #e74c3c, #c0392b);
}

.stat-fill.atk {
  background: linear-gradient(90deg, #e67e22, #d35400);
}

.stat-fill.def {
  background: linear-gradient(90deg, #3498db, #2980b9);
}

.stat-value {
  width: 45px;
  text-align: right;
  font-size: 0.85rem;
  font-weight: bold;
  color: #2c3e50;
}

/* 技能 */
.cookie-skill {
  background: #f8f9fa;
  padding: 0.8rem;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.skill-label {
  font-size: 0.8rem;
  color: #95a5a6;
}

.skill-name {
  font-weight: bold;
  color: #667eea;
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

  .filter-bar {
    gap: 0.3rem;
    margin-bottom: 0.8rem;
  }

  .filter-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }

  .cookie-count {
    font-size: 0.85rem;
    margin-bottom: 1rem;
  }

  .cookie-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .cookie-card {
    padding: 1rem;
  }

  .cookie-avatar {
    width: 60px;
    height: 60px;
    font-size: 2rem;
  }

  .cookie-name {
    font-size: 1.1rem;
  }

  .cookie-stats {
    margin: 0.8rem 0;
  }

  .stat-label {
    width: 50px;
    font-size: 0.75rem;
  }

  .stat-value {
    width: 40px;
    font-size: 0.8rem;
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
  }

  .cookie-card {
    padding: 0.8rem;
  }

  .cookie-avatar {
    width: 50px;
    height: 50px;
    font-size: 1.8rem;
  }

  .cookie-name {
    font-size: 1rem;
  }

  .cookie-desc {
    font-size: 0.8rem;
  }
}
</style>
