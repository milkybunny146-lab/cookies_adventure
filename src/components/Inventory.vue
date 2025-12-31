<script setup>
import { ref, computed } from 'vue'

const currentCategory = ref('all')
const selectedItem = ref(null)

const items = ref([
  {
    id: 1,
    name: '初級生命藥水',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'common',
    rarityName: '普通',
    icon: '🧪',
    description: '恢復 200 點生命值',
    quantity: 15,
    effect: { type: 'heal', value: 200 }
  },
  {
    id: 2,
    name: '中級生命藥水',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '💉',
    description: '恢復 500 點生命值',
    quantity: 8,
    effect: { type: 'heal', value: 500 }
  },
  {
    id: 3,
    name: '高級生命藥水',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'epic',
    rarityName: '史詩',
    icon: '⚗️',
    description: '恢復 1000 點生命值',
    quantity: 3,
    effect: { type: 'heal', value: 1000 }
  },
  {
    id: 4,
    name: '體力藥劑',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '⚡',
    description: '恢復 20 點體力',
    quantity: 5,
    effect: { type: 'energy', value: 20 }
  },
  {
    id: 5,
    name: '強化石',
    category: 'material',
    categoryName: '材料',
    rarity: 'common',
    rarityName: '普通',
    icon: '💎',
    description: '用於強化裝備的基礎材料',
    quantity: 42,
    effect: { type: 'enhance', value: 1 }
  },
  {
    id: 6,
    name: '精煉石',
    category: 'material',
    categoryName: '材料',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '✨',
    description: '用於精煉裝備的高級材料',
    quantity: 18,
    effect: { type: 'refine', value: 1 }
  },
  {
    id: 7,
    name: '傳說強化石',
    category: 'material',
    categoryName: '材料',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '🌟',
    description: '極其稀有的強化材料，可大幅提升裝備屬性',
    quantity: 2,
    effect: { type: 'enhance', value: 5 }
  },
  {
    id: 8,
    name: '餅乾召喚券',
    category: 'special',
    categoryName: '特殊',
    rarity: 'epic',
    rarityName: '史詩',
    icon: '🎫',
    description: '可以召喚一位隨機餅乾勇者',
    quantity: 5,
    effect: { type: 'summon', value: 1 }
  },
  {
    id: 9,
    name: '經驗書',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'common',
    rarityName: '普通',
    icon: '📚',
    description: '使用後獲得 1000 點經驗值',
    quantity: 12,
    effect: { type: 'exp', value: 1000 }
  },
  {
    id: 10,
    name: '高級經驗書',
    category: 'consumable',
    categoryName: '消耗品',
    rarity: 'rare',
    rarityName: '稀有',
    icon: '📖',
    description: '使用後獲得 5000 點經驗值',
    quantity: 4,
    effect: { type: 'exp', value: 5000 }
  },
  {
    id: 11,
    name: '龍族碎片',
    category: 'material',
    categoryName: '材料',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '🐉',
    description: '遠古龍族的碎片，用於合成傳說裝備',
    quantity: 1,
    effect: { type: 'craft', value: 1 }
  },
  {
    id: 12,
    name: '深淵結晶',
    category: 'material',
    categoryName: '材料',
    rarity: 'legendary',
    rarityName: '傳說',
    icon: '💠',
    description: '來自深淵的神秘結晶，蘊含強大力量',
    quantity: 1,
    effect: { type: 'craft', value: 1 }
  }
])

const categories = ref([
  { key: 'all', name: '全部' },
  { key: 'consumable', name: '消耗品' },
  { key: 'material', name: '材料' },
  { key: 'special', name: '特殊' }
])

const filteredItems = computed(() => {
  if (currentCategory.value === 'all') {
    return items.value
  }
  return items.value.filter(item => item.category === currentCategory.value)
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

const selectItem = (item) => {
  selectedItem.value = item
}

const useItem = (item) => {
  if (item.quantity > 0) {
    alert(`使用 ${item.name}\n效果：${item.description}`)
    // TODO: 實際使用邏輯
  }
}

const totalItems = computed(() => {
  return items.value.reduce((sum, item) => sum + item.quantity, 0)
})
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">🎒 背包道具</h1>

    <!-- 統計資訊 -->
    <div class="stats-bar">
      <div class="stat-item">
        <span class="stat-label">總道具數</span>
        <span class="stat-value">{{ totalItems }}</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">道具種類</span>
        <span class="stat-value">{{ items.length }}</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">當前分類</span>
        <span class="stat-value">{{ categories.find(c => c.key === currentCategory)?.name }}</span>
      </div>
    </div>

    <!-- 分類篩選 -->
    <div class="filter-bar">
      <button
        v-for="category in categories"
        :key="category.key"
        class="filter-btn"
        :class="{ active: currentCategory === category.key }"
        @click="currentCategory = category.key"
      >
        {{ category.name }}
      </button>
    </div>

    <!-- 道具數量 -->
    <p class="item-count">
      顯示 {{ filteredItems.length }} / {{ items.length }} 種道具
    </p>

    <!-- 道具網格 -->
    <div class="item-grid">
      <div
        v-for="item in filteredItems"
        :key="item.id"
        class="item-card"
        :class="{ 
          selected: selectedItem?.id === item.id,
          [getRarityClass(item.rarity)]: true
        }"
        @click="selectItem(item)"
      >
        <!-- 稀有度邊框 -->
        <div 
          class="rarity-border"
          :style="{ borderColor: getRarityColor(item.rarity) }"
        ></div>

        <!-- 稀有度標籤 -->
        <div 
          class="rarity-badge"
          :style="{ backgroundColor: getRarityColor(item.rarity) }"
        >
          {{ item.rarityName }}
        </div>

        <!-- 道具圖示 -->
        <div class="item-icon">
          {{ item.icon }}
        </div>

        <!-- 道具資訊 -->
        <div class="item-info">
          <h3 class="item-name">{{ item.name }}</h3>
          <span class="item-category">{{ item.categoryName }}</span>
          <p class="item-desc">{{ item.description }}</p>
        </div>

        <!-- 數量 -->
        <div class="item-quantity">
          <span class="quantity-label">數量</span>
          <span class="quantity-value">{{ item.quantity }}</span>
        </div>

        <!-- 使用按鈕 -->
        <button 
          v-if="item.category === 'consumable' && item.quantity > 0"
          class="use-btn"
          @click.stop="useItem(item)"
        >
          使用
        </button>
      </div>
    </div>

    <!-- 選中道具詳情 -->
    <div v-if="selectedItem" class="item-detail">
      <h3>📋 {{ selectedItem.name }} - 詳細資訊</h3>
      <div class="detail-content">
        <div class="detail-row">
          <span class="detail-label">分類</span>
          <span class="detail-value">{{ selectedItem.categoryName }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">稀有度</span>
          <span 
            class="detail-value"
            :style="{ color: getRarityColor(selectedItem.rarity) }"
          >
            {{ selectedItem.rarityName }}
          </span>
        </div>
        <div class="detail-row">
          <span class="detail-label">數量</span>
          <span class="detail-value">{{ selectedItem.quantity }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">效果</span>
          <span class="detail-value">{{ selectedItem.description }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">效果類型</span>
          <span class="detail-value">{{ selectedItem.effect.type }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">效果數值</span>
          <span class="detail-value">{{ selectedItem.effect.value }}</span>
        </div>
      </div>
      <div v-if="selectedItem.category === 'consumable' && selectedItem.quantity > 0" class="action-section">
        <button class="action-btn" @click="useItem(selectedItem)">
          使用道具
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

/* 統計欄 */
.stats-bar {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
  flex-wrap: wrap;
}

.stat-item {
  flex: 1;
  min-width: 150px;
  background: white;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
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

.item-count {
  color: #7f8c8d;
  margin-bottom: 1.5rem;
}

/* 道具網格 */
.item-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}

/* 道具卡片 */
.item-card {
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

.item-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.item-card.selected {
  border-color: #667eea;
  box-shadow: 0 4px 16px rgba(102, 126, 234, 0.3);
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

/* 道具圖示 */
.item-icon {
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

.item-card.rarity-legendary .item-icon {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  box-shadow: 0 0 20px rgba(243, 156, 18, 0.4);
}

/* 道具資訊 */
.item-info {
  text-align: center;
  margin-bottom: 1rem;
}

.item-name {
  font-size: 1.1rem;
  color: #2c3e50;
  margin-bottom: 0.3rem;
}

.item-category {
  display: inline-block;
  background: #ecf0f1;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  font-size: 0.75rem;
  color: #7f8c8d;
  margin-bottom: 0.5rem;
}

.item-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  line-height: 1.5;
  margin: 0.5rem 0 0;
}

/* 數量 */
.item-quantity {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.8rem;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.quantity-label {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.quantity-value {
  font-size: 1.2rem;
  font-weight: bold;
  color: #2c3e50;
}

/* 使用按鈕 */
.use-btn {
  width: 100%;
  padding: 0.7rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.use-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

/* 道具詳情 */
.item-detail {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  margin-top: 2rem;
}

.item-detail h3 {
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

.action-section {
  margin-top: 1rem;
}

.action-btn {
  width: 100%;
  padding: 0.8rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
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
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
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

  .stats-bar {
    flex-direction: column;
    gap: 0.8rem;
  }

  .stat-item {
    min-width: auto;
  }

  .filter-bar {
    gap: 0.3rem;
    margin-bottom: 0.8rem;
  }

  .filter-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }

  .item-count {
    font-size: 0.85rem;
    margin-bottom: 1rem;
  }

  .item-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .item-card {
    padding: 1rem;
  }

  .item-icon {
    width: 60px;
    height: 60px;
    font-size: 2rem;
  }

  .item-name {
    font-size: 1rem;
  }

  .item-detail {
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

  .item-card {
    padding: 0.8rem;
  }

  .item-icon {
    width: 50px;
    height: 50px;
    font-size: 1.8rem;
  }

  .item-name {
    font-size: 0.95rem;
  }

  .item-desc {
    font-size: 0.8rem;
  }

  .use-btn {
    padding: 0.6rem;
    font-size: 0.85rem;
  }
}
</style>

