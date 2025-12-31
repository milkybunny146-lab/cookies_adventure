<script setup>
import { ref, computed } from 'vue'

const selectedChapter = ref(null)
const currentDifficulty = ref('normal')

const chapters = ref([
  {
    id: 1,
    name: '新手村',
    description: '適合初學者的簡單副本，可以熟悉戰鬥機制',
    difficulty: 'easy',
    recommendedPower: 5000,
    rewards: { gold: 100, exp: 50, items: ['初級藥水'] },
    stages: 5,
    completed: 5,
    unlocked: true
  },
  {
    id: 2,
    name: '黑暗森林',
    description: '充滿危險的森林，需要強大的隊伍才能通過',
    difficulty: 'normal',
    recommendedPower: 15000,
    rewards: { gold: 300, exp: 150, items: ['中級藥水', '強化石'] },
    stages: 8,
    completed: 8,
    unlocked: true
  },
  {
    id: 3,
    name: '冰霜洞穴',
    description: '極寒之地，敵人擁有強大的冰系攻擊',
    difficulty: 'hard',
    recommendedPower: 30000,
    rewards: { gold: 600, exp: 300, items: ['高級藥水', '稀有裝備'] },
    stages: 10,
    completed: 3,
    unlocked: true
  },
  {
    id: 4,
    name: '龍族遺跡',
    description: '遠古龍族的遺跡，隱藏著強大的寶藏',
    difficulty: 'expert',
    recommendedPower: 50000,
    rewards: { gold: 1200, exp: 600, items: ['傳說裝備', '龍族碎片'] },
    stages: 12,
    completed: 0,
    unlocked: true
  },
  {
    id: 5,
    name: '深淵地獄',
    description: '最危險的副本，只有最強的勇者才能挑戰',
    difficulty: 'nightmare',
    recommendedPower: 80000,
    rewards: { gold: 2000, exp: 1000, items: ['神話裝備', '深淵結晶'] },
    stages: 15,
    completed: 0,
    unlocked: false
  }
])

const difficulties = ref([
  { key: 'easy', name: '簡單', color: '#27ae60' },
  { key: 'normal', name: '普通', color: '#3498db' },
  { key: 'hard', name: '困難', color: '#e67e22' },
  { key: 'expert', name: '專家', color: '#9b59b6' },
  { key: 'nightmare', name: '噩夢', color: '#e74c3c' }
])

const filteredChapters = computed(() => {
  if (currentDifficulty.value === 'all') {
    return chapters.value
  }
  return chapters.value.filter(c => c.difficulty === currentDifficulty.value)
})

const getDifficultyInfo = (difficulty) => {
  return difficulties.value.find(d => d.key === difficulty) || difficulties.value[1]
}

const selectChapter = (chapter) => {
  selectedChapter.value = chapter
}

const startBattle = () => {
  if (selectedChapter.value) {
    alert(`開始挑戰：${selectedChapter.value.name}\n推薦戰力：${selectedChapter.value.recommendedPower}`)
    // TODO: 實際戰鬥邏輯
  }
}

const getProgressPercentage = (chapter) => {
  return (chapter.completed / chapter.stages) * 100
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">⚔️ 戰鬥副本</h1>

    <!-- 難度篩選 -->
    <div class="filter-bar">
      <button
        class="filter-btn"
        :class="{ active: currentDifficulty === 'all' }"
        @click="currentDifficulty = 'all'"
      >
        全部
      </button>
      <button
        v-for="diff in difficulties"
        :key="diff.key"
        class="filter-btn"
        :class="{ active: currentDifficulty === diff.key }"
        :style="{ borderColor: diff.color }"
        @click="currentDifficulty = diff.key"
      >
        {{ diff.name }}
      </button>
    </div>

    <!-- 副本列表 -->
    <div class="chapter-grid">
      <div
        v-for="chapter in filteredChapters"
        :key="chapter.id"
        class="chapter-card"
        :class="{ 
          selected: selectedChapter?.id === chapter.id,
          locked: !chapter.unlocked 
        }"
        @click="chapter.unlocked && selectChapter(chapter)"
      >
        <!-- 鎖定標記 -->
        <div v-if="!chapter.unlocked" class="lock-overlay">
          <div class="lock-icon">🔒</div>
          <p>尚未解鎖</p>
        </div>

        <!-- 難度標籤 -->
        <div 
          class="difficulty-badge"
          :style="{ backgroundColor: getDifficultyInfo(chapter.difficulty).color }"
        >
          {{ getDifficultyInfo(chapter.difficulty).name }}
        </div>

        <!-- 章節資訊 -->
        <div class="chapter-header">
          <h3 class="chapter-name">{{ chapter.name }}</h3>
          <p class="chapter-desc">{{ chapter.description }}</p>
        </div>

        <!-- 進度條 -->
        <div class="progress-section">
          <div class="progress-info">
            <span>進度</span>
            <span>{{ chapter.completed }} / {{ chapter.stages }}</span>
          </div>
          <div class="progress-bar">
            <div 
              class="progress-fill"
              :style="{ 
                width: getProgressPercentage(chapter) + '%',
                backgroundColor: getDifficultyInfo(chapter.difficulty).color
              }"
            ></div>
          </div>
        </div>

        <!-- 推薦戰力 -->
        <div class="power-requirement">
          <span class="power-label">推薦戰力</span>
          <span class="power-value">{{ chapter.recommendedPower.toLocaleString() }}</span>
        </div>

        <!-- 獎勵預覽 -->
        <div class="rewards-preview">
          <div class="reward-item">
            <span>💰</span>
            <span>{{ chapter.rewards.gold }}</span>
          </div>
          <div class="reward-item">
            <span>⭐</span>
            <span>{{ chapter.rewards.exp }}</span>
          </div>
          <div class="reward-item">
            <span>🎁</span>
            <span>{{ chapter.rewards.items.length }} 件</span>
          </div>
        </div>

        <!-- 開始按鈕 -->
        <button 
          v-if="chapter.unlocked && selectedChapter?.id === chapter.id"
          class="start-btn"
          @click.stop="startBattle"
        >
          開始挑戰
        </button>
      </div>
    </div>

    <!-- 選中章節詳情 -->
    <div v-if="selectedChapter" class="chapter-detail">
      <h3>📋 {{ selectedChapter.name }} - 詳細資訊</h3>
      <div class="detail-grid">
        <div class="detail-item">
          <span class="detail-label">難度等級</span>
          <span class="detail-value">{{ getDifficultyInfo(selectedChapter.difficulty).name }}</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">總關卡數</span>
          <span class="detail-value">{{ selectedChapter.stages }} 關</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">已完成</span>
          <span class="detail-value">{{ selectedChapter.completed }} 關</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">推薦戰力</span>
          <span class="detail-value">{{ selectedChapter.recommendedPower.toLocaleString() }}</span>
        </div>
      </div>
      <div class="rewards-detail">
        <h4>🎁 通關獎勵</h4>
        <div class="rewards-list">
          <div class="reward-detail-item">
            <span>💰 金幣</span>
            <span>{{ selectedChapter.rewards.gold }}</span>
          </div>
          <div class="reward-detail-item">
            <span>⭐ 經驗值</span>
            <span>{{ selectedChapter.rewards.exp }}</span>
          </div>
          <div class="reward-detail-item">
            <span>📦 道具</span>
            <span>{{ selectedChapter.rewards.items.join(', ') }}</span>
          </div>
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
  margin-bottom: 1.5rem;
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
  transform: translateY(-2px);
}

.filter-btn.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}

/* 章節網格 */
.chapter-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

/* 章節卡片 */
.chapter-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s;
  position: relative;
  cursor: pointer;
  border: 2px solid transparent;
}

.chapter-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.chapter-card.selected {
  border-color: #667eea;
  box-shadow: 0 4px 16px rgba(102, 126, 234, 0.3);
}

.chapter-card.locked {
  opacity: 0.6;
  cursor: not-allowed;
}

/* 鎖定覆蓋層 */
.lock-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  z-index: 10;
}

.lock-icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

/* 難度標籤 */
.difficulty-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  padding: 0.25rem 0.8rem;
  border-radius: 12px;
  color: white;
  font-size: 0.75rem;
  font-weight: bold;
}

/* 章節標題 */
.chapter-header {
  margin-bottom: 1rem;
  padding-right: 80px;
}

.chapter-name {
  font-size: 1.3rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.chapter-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  line-height: 1.5;
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
  height: 8px;
  background: #ecf0f1;
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  border-radius: 4px;
  transition: width 0.5s ease;
}

/* 推薦戰力 */
.power-requirement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.8rem;
  background: #f8f9fa;
  border-radius: 8px;
  margin: 1rem 0;
}

.power-label {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.power-value {
  font-size: 1.1rem;
  font-weight: bold;
  color: #2c3e50;
}

/* 獎勵預覽 */
.rewards-preview {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
}

.reward-item {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.85rem;
  color: #7f8c8d;
}

.reward-item span:first-child {
  font-size: 1.2rem;
}

/* 開始按鈕 */
.start-btn {
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
  margin-top: 1rem;
}

.start-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

/* 章節詳情 */
.chapter-detail {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  margin-top: 2rem;
}

.chapter-detail h3 {
  color: #2c3e50;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.detail-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.detail-item {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.detail-label {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.detail-value {
  font-size: 1.1rem;
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
    margin-bottom: 1rem;
    flex-wrap: wrap;
  }

  .filter-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }

  .chapter-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .chapter-card {
    padding: 1rem;
  }

  .chapter-name {
    font-size: 1.1rem;
  }

  .chapter-desc {
    font-size: 0.8rem;
  }

  .chapter-detail {
    margin-top: 1.5rem;
    padding: 1rem;
  }

  .detail-grid {
    grid-template-columns: 1fr;
    gap: 0.8rem;
  }

  .rewards-detail {
    margin-top: 1rem;
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

  .chapter-card {
    padding: 0.8rem;
  }

  .difficulty-badge {
    font-size: 0.7rem;
    padding: 0.2rem 0.6rem;
  }

  .start-btn {
    padding: 0.7rem;
    font-size: 0.9rem;
  }
}
</style>

