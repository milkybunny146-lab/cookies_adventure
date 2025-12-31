<script setup>
import { ref, computed } from 'vue'

const currentDay = ref(5) // 當前連續登入天數
const todayClaimed = ref(false) // 今日是否已領取

const dailyRewards = ref([
  {
    day: 1,
    reward: { type: 'gold', amount: 100, icon: '💰' },
    name: '金幣',
    claimed: true
  },
  {
    day: 2,
    reward: { type: 'exp', amount: 50, icon: '⭐' },
    name: '經驗值',
    claimed: true
  },
  {
    day: 3,
    reward: { type: 'item', amount: 1, icon: '🧪', itemName: '初級生命藥水' },
    name: '初級生命藥水',
    claimed: true
  },
  {
    day: 4,
    reward: { type: 'diamond', amount: 10, icon: '💠' },
    name: '鑽石',
    claimed: true
  },
  {
    day: 5,
    reward: { type: 'gold', amount: 200, icon: '💰' },
    name: '金幣',
    claimed: false,
    isToday: true
  },
  {
    day: 6,
    reward: { type: 'item', amount: 1, icon: '💎', itemName: '強化石' },
    name: '強化石',
    claimed: false
  },
  {
    day: 7,
    reward: { type: 'special', amount: 1, icon: '🎫', itemName: '餅乾召喚券' },
    name: '餅乾召喚券',
    claimed: false,
    isSpecial: true
  }
])

const weeklyRewards = ref([
  {
    week: 1,
    rewards: [
      { type: 'gold', amount: 500, icon: '💰' },
      { type: 'diamond', amount: 50, icon: '💠' },
      { type: 'item', amount: 5, icon: '💎', itemName: '強化石' }
    ],
    claimed: false
  },
  {
    week: 2,
    rewards: [
      { type: 'gold', amount: 1000, icon: '💰' },
      { type: 'diamond', amount: 100, icon: '💠' },
      { type: 'item', amount: 1, icon: '🎫', itemName: '餅乾召喚券' }
    ],
    claimed: false
  }
])

const claimDailyReward = (day) => {
  const reward = dailyRewards.value.find(r => r.day === day)
  if (reward && !reward.claimed && reward.isToday) {
    reward.claimed = true
    todayClaimed.value = true
    alert(`領取成功！獲得：${reward.name} x${reward.reward.amount || 1}`)
    // TODO: 實際領取邏輯
  }
}

const claimWeeklyReward = (week) => {
  const weekly = weeklyRewards.value.find(w => w.week === week)
  if (weekly && !weekly.claimed) {
    weekly.claimed = true
    alert('領取成功！獲得週獎勵！')
    // TODO: 實際領取邏輯
  }
}

const getRewardClass = (reward) => {
  if (reward.claimed) return 'claimed'
  if (reward.isToday) return 'today'
  if (reward.isSpecial) return 'special'
  return 'upcoming'
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">🎁 每日獎勵</h1>

    <!-- 連續登入資訊 -->
    <div class="login-info">
      <div class="info-card">
        <div class="info-icon">📅</div>
        <div class="info-content">
          <div class="info-label">連續登入</div>
          <div class="info-value">{{ currentDay }} 天</div>
        </div>
      </div>
      <div class="info-card">
        <div class="info-icon">⏰</div>
        <div class="info-content">
          <div class="info-label">下次領取</div>
          <div class="info-value">明天 00:00</div>
        </div>
      </div>
    </div>

    <!-- 每日簽到 -->
    <div class="reward-section">
      <h2 class="section-title">📆 每日簽到</h2>
      <p class="section-desc">連續登入 7 天可獲得特殊獎勵！</p>
      
      <div class="daily-rewards-grid">
        <div
          v-for="reward in dailyRewards"
          :key="reward.day"
          class="reward-card"
          :class="getRewardClass(reward)"
          @click="!reward.claimed && reward.isToday && claimDailyReward(reward.day)"
        >
          <!-- 日期標籤 -->
          <div class="day-label">第 {{ reward.day }} 天</div>
          
          <!-- 獎勵圖示 -->
          <div class="reward-icon">
            {{ reward.reward.icon }}
          </div>
          
          <!-- 獎勵資訊 -->
          <div class="reward-info">
            <div class="reward-name">{{ reward.name }}</div>
            <div class="reward-amount">x{{ reward.reward.amount || 1 }}</div>
          </div>

          <!-- 狀態標記 -->
          <div v-if="reward.claimed" class="status-badge claimed-badge">
            ✅ 已領取
          </div>
          <div v-else-if="reward.isToday" class="status-badge today-badge">
            🎯 今日可領
          </div>
          <div v-else-if="reward.isSpecial" class="status-badge special-badge">
            ⭐ 特殊獎勵
          </div>
          <div v-else class="status-badge upcoming-badge">
            🔒 未解鎖
          </div>
        </div>
      </div>
    </div>

    <!-- 週獎勵 -->
    <div class="reward-section">
      <h2 class="section-title">📦 週獎勵</h2>
      <p class="section-desc">每週完成所有每日簽到可領取額外獎勵！</p>
      
      <div class="weekly-rewards">
        <div
          v-for="weekly in weeklyRewards"
          :key="weekly.week"
          class="weekly-card"
          :class="{ claimed: weekly.claimed }"
        >
          <div class="weekly-header">
            <h3 class="weekly-title">第 {{ weekly.week }} 週獎勵</h3>
            <button
              v-if="!weekly.claimed"
              class="claim-weekly-btn"
              @click="claimWeeklyReward(weekly.week)"
            >
              領取
            </button>
            <div v-else class="claimed-tag">已領取</div>
          </div>
          
          <div class="weekly-rewards-list">
            <div
              v-for="(reward, index) in weekly.rewards"
              :key="index"
              class="weekly-reward-item"
            >
              <span class="reward-icon-small">{{ reward.icon }}</span>
              <span class="reward-text">
                {{ reward.itemName || (reward.type === 'gold' ? '金幣' : reward.type === 'diamond' ? '鑽石' : '道具') }}
                x{{ reward.amount }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 獎勵規則 -->
    <div class="rules-section">
      <h3 class="rules-title">📋 獎勵規則</h3>
      <ul class="rules-list">
        <li>每日 00:00 重置，請記得準時領取</li>
        <li>連續登入 7 天可獲得特殊獎勵（餅乾召喚券）</li>
        <li>中斷連續登入會重置簽到進度</li>
        <li>週獎勵需要完成當週所有每日簽到才能領取</li>
        <li>所有獎勵會自動發送到背包</li>
      </ul>
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

/* 登入資訊 */
.login-info {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.info-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  gap: 1rem;
}

.info-icon {
  font-size: 2.5rem;
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.info-content {
  flex: 1;
}

.info-label {
  font-size: 0.85rem;
  color: #7f8c8d;
  margin-bottom: 0.3rem;
}

.info-value {
  font-size: 1.3rem;
  font-weight: bold;
  color: #2c3e50;
}

/* 獎勵區塊 */
.reward-section {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  margin-bottom: 2rem;
}

.section-title {
  font-size: 1.5rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.section-desc {
  color: #7f8c8d;
  margin-bottom: 1.5rem;
}

/* 每日獎勵網格 */
.daily-rewards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 1rem;
}

.reward-card {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 1.5rem 1rem;
  text-align: center;
  position: relative;
  transition: all 0.3s;
  border: 3px solid transparent;
}

.reward-card.claimed {
  opacity: 0.7;
  background: #ecf0f1;
}

.reward-card.today {
  background: linear-gradient(135deg, #667eea15 0%, #764ba215 100%);
  border-color: #667eea;
  cursor: pointer;
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.reward-card.today:hover {
  transform: scale(1.08);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4);
}

.reward-card.special {
  background: linear-gradient(135deg, #f39c1215 0%, #e74c3c15 100%);
  border-color: #f39c12;
}

.reward-card.upcoming {
  opacity: 0.6;
}

.day-label {
  font-size: 0.75rem;
  color: #7f8c8d;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.reward-icon {
  font-size: 3rem;
  margin: 0.5rem 0;
}

.reward-info {
  margin-top: 0.5rem;
}

.reward-name {
  font-size: 0.9rem;
  color: #2c3e50;
  font-weight: 500;
  margin-bottom: 0.2rem;
}

.reward-amount {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.status-badge {
  position: absolute;
  top: 8px;
  right: 8px;
  padding: 0.25rem 0.6rem;
  border-radius: 12px;
  font-size: 0.7rem;
  font-weight: bold;
}

.claimed-badge {
  background: #95a5a6;
  color: white;
}

.today-badge {
  background: #667eea;
  color: white;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.7;
  }
}

.special-badge {
  background: linear-gradient(135deg, #f39c12, #e74c3c);
  color: white;
}

.upcoming-badge {
  background: #ecf0f1;
  color: #7f8c8d;
}

/* 週獎勵 */
.weekly-rewards {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.weekly-card {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 1.5rem;
  border: 2px solid #ecf0f1;
  transition: all 0.3s;
}

.weekly-card.claimed {
  opacity: 0.7;
  background: #ecf0f1;
}

.weekly-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid #ecf0f1;
}

.weekly-title {
  font-size: 1.2rem;
  color: #2c3e50;
  margin: 0;
}

.claim-weekly-btn {
  padding: 0.6rem 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.claim-weekly-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.claimed-tag {
  padding: 0.6rem 1.5rem;
  background: #95a5a6;
  color: white;
  border-radius: 8px;
  font-weight: bold;
  font-size: 0.9rem;
}

.weekly-rewards-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.weekly-reward-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.6rem 1rem;
  background: white;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #2c3e50;
}

.reward-icon-small {
  font-size: 1.2rem;
}

/* 規則區 */
.rules-section {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.rules-title {
  font-size: 1.2rem;
  color: #2c3e50;
  margin-bottom: 1rem;
}

.rules-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.rules-list li {
  padding: 0.5rem 0;
  color: #7f8c8d;
  line-height: 1.6;
  padding-left: 1.5rem;
  position: relative;
}

.rules-list li::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #667eea;
  font-weight: bold;
  font-size: 1.2rem;
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

  .login-info {
    grid-template-columns: 1fr;
    gap: 0.8rem;
    margin-bottom: 1.5rem;
  }

  .info-card {
    padding: 1.2rem;
  }

  .info-icon {
    width: 50px;
    height: 50px;
    font-size: 2rem;
  }

  .info-value {
    font-size: 1.2rem;
  }

  .reward-section {
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .section-title {
    font-size: 1.3rem;
  }

  .section-desc {
    font-size: 0.9rem;
    margin-bottom: 1.2rem;
  }

  .daily-rewards-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 0.8rem;
  }

  .reward-card {
    padding: 1rem 0.8rem;
  }

  .reward-icon {
    font-size: 2.5rem;
    margin: 0.3rem 0;
  }

  .reward-name {
    font-size: 0.85rem;
  }

  .reward-amount {
    font-size: 0.8rem;
  }

  .weekly-card {
    padding: 1.2rem;
  }

  .weekly-title {
    font-size: 1.1rem;
  }

  .rules-section {
    padding: 1.2rem;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .info-card {
    padding: 1rem;
  }

  .info-icon {
    width: 45px;
    height: 45px;
    font-size: 1.8rem;
  }

  .info-value {
    font-size: 1.1rem;
  }

  .reward-section {
    padding: 1rem;
  }

  .section-title {
    font-size: 1.2rem;
  }

  .daily-rewards-grid {
    grid-template-columns: 1fr;
    gap: 0.8rem;
  }

  .reward-card {
    padding: 1rem;
  }

  .reward-icon {
    font-size: 2.2rem;
  }

  .day-label {
    font-size: 0.7rem;
  }

  .status-badge {
    font-size: 0.65rem;
    padding: 0.2rem 0.5rem;
  }

  .weekly-card {
    padding: 1rem;
  }

  .weekly-title {
    font-size: 1rem;
  }

  .claim-weekly-btn {
    padding: 0.5rem 1.2rem;
    font-size: 0.9rem;
  }

  .weekly-reward-item {
    padding: 0.5rem 0.8rem;
    font-size: 0.85rem;
  }
}
</style>

