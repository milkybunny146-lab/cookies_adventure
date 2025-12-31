<script setup>
import { ref, computed } from 'vue'
import StatusCard from './StatusCard.vue'

const cards = ref([
  {
    icon: '🍪',
    title: '餅乾勇者',
    description: '目前擁有 28 位餅乾勇者，其中 5 位為傳說級稀有餅乾！'
  },
  {
    icon: '💎',
    title: '王國資源',
    description: '金幣 125,430 | 鑽石 892 | 體力 80/120'
  },
  {
    icon: '⚔️',
    title: '冒險進度',
    description: '已通關至第 12 章「黑暗森林」，戰鬥力評分 48,720。'
  },
  {
    icon: '🎯',
    title: '每日任務',
    description: '已完成 3/5 個每日任務，還差 2 個即可領取寶箱！'
  }
])

// 儀表板數據
const dashboardData = ref({
  power: 48720,
  powerTarget: 50000,
  powerHistory: [32000, 35000, 38000, 42000, 45000, 48720],
  resources: {
    gold: 125430,
    diamond: 892,
    energy: 80,
    energyMax: 120
  },
  cookies: {
    total: 28,
    common: 8,
    rare: 10,
    epic: 5,
    legendary: 5
  },
  dailyProgress: {
    tasks: 3,
    tasksTotal: 5,
    loginDays: 5,
    loginTarget: 7
  },
  achievements: {
    completed: 1,
    total: 12
  }
})

// 計算百分比
const powerPercentage = computed(() => {
  return Math.min((dashboardData.value.power / dashboardData.value.powerTarget) * 100, 100)
})

const energyPercentage = computed(() => {
  return (dashboardData.value.resources.energy / dashboardData.value.resources.energyMax) * 100
})

const tasksPercentage = computed(() => {
  return (dashboardData.value.dailyProgress.tasks / dashboardData.value.dailyProgress.tasksTotal) * 100
})

const loginPercentage = computed(() => {
  return (dashboardData.value.dailyProgress.loginDays / dashboardData.value.dailyProgress.loginTarget) * 100
})

const achievementPercentage = computed(() => {
  return (dashboardData.value.achievements.completed / dashboardData.value.achievements.total) * 100
})

// 戰力趨勢圖的最大值（用於縮放）
const maxPower = computed(() => {
  return Math.max(...dashboardData.value.powerHistory, dashboardData.value.powerTarget)
})
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">王國總覽</h1>
    
    <!-- 狀態卡片 -->
    <div class="card-grid">
      <StatusCard
        v-for="card in cards"
        :key="card.title"
        :icon="card.icon"
        :title="card.title"
        :description="card.description"
      />
    </div>

    <!-- 儀表板區域 -->
    <div class="dashboard-section">
      <h2 class="dashboard-title">📊 每日數據</h2>
      
      <div class="dashboard-grid">
        <!-- 戰力進度儀表盤 -->
        <div class="dashboard-card">
          <h3 class="dashboard-card-title">⚔️ 戰力進度</h3>
          <div class="gauge-container">
            <div class="gauge-wrapper">
              <div class="gauge">
                <div class="gauge-inner">
                  <div class="gauge-value">
                    <span class="value-number">{{ dashboardData.power.toLocaleString() }}</span>
                    <span class="value-label">/ {{ dashboardData.powerTarget.toLocaleString() }}</span>
                  </div>
                </div>
                <svg class="gauge-svg" viewBox="0 -10 200 110">
                  <path
                    class="gauge-background"
                    d="M 20 80 A 80 80 0 0 1 180 80"
                    fill="none"
                    stroke="#ecf0f1"
                    stroke-width="12"
                  />
                  <path
                    class="gauge-fill"
                    d="M 20 80 A 80 80 0 0 1 180 80"
                    fill="none"
                    stroke="#667eea"
                    stroke-width="12"
                    stroke-linecap="round"
                    :style="{ 
                      strokeDasharray: Math.PI * 80,
                      strokeDashoffset: Math.PI * 80 * (1 - powerPercentage / 100)
                    }"
                  />
                </svg>
              </div>
            </div>
            <p class="gauge-desc">距離目標還差 {{ (dashboardData.powerTarget - dashboardData.power).toLocaleString() }} 戰力</p>
          </div>
        </div>

        <!-- 資源分佈 -->
        <div class="dashboard-card">
          <h3 class="dashboard-card-title">💎 資源分佈</h3>
          <div class="resource-list">
            <div class="resource-item">
              <div class="resource-icon">💰</div>
              <div class="resource-info">
                <span class="resource-name">金幣</span>
                <span class="resource-value">{{ dashboardData.resources.gold.toLocaleString() }}</span>
              </div>
            </div>
            <div class="resource-item">
              <div class="resource-icon">💠</div>
              <div class="resource-info">
                <span class="resource-name">鑽石</span>
                <span class="resource-value">{{ dashboardData.resources.diamond.toLocaleString() }}</span>
              </div>
            </div>
            <div class="resource-item">
              <div class="resource-icon">⚡</div>
              <div class="resource-info">
                <span class="resource-name">體力</span>
                <div class="resource-progress">
                  <div class="progress-bar">
                    <div 
                      class="progress-fill energy" 
                      :style="{ width: energyPercentage + '%' }"
                    ></div>
                  </div>
                  <span class="resource-value">{{ dashboardData.resources.energy }} / {{ dashboardData.resources.energyMax }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 餅乾分佈 -->
        <div class="dashboard-card">
          <h3 class="dashboard-card-title">🍪 餅乾分佈</h3>
          <div class="cookie-distribution">
            <div class="distribution-item">
              <div class="distribution-bar">
                <div 
                  class="distribution-fill common" 
                  :style="{ width: (dashboardData.cookies.common / dashboardData.cookies.total * 100) + '%' }"
                ></div>
              </div>
              <div class="distribution-label">
                <span>普通</span>
                <span>{{ dashboardData.cookies.common }}</span>
              </div>
            </div>
            <div class="distribution-item">
              <div class="distribution-bar">
                <div 
                  class="distribution-fill rare" 
                  :style="{ width: (dashboardData.cookies.rare / dashboardData.cookies.total * 100) + '%' }"
                ></div>
              </div>
              <div class="distribution-label">
                <span>稀有</span>
                <span>{{ dashboardData.cookies.rare }}</span>
              </div>
            </div>
            <div class="distribution-item">
              <div class="distribution-bar">
                <div 
                  class="distribution-fill epic" 
                  :style="{ width: (dashboardData.cookies.epic / dashboardData.cookies.total * 100) + '%' }"
                ></div>
              </div>
              <div class="distribution-label">
                <span>史詩</span>
                <span>{{ dashboardData.cookies.epic }}</span>
              </div>
            </div>
            <div class="distribution-item">
              <div class="distribution-bar">
                <div 
                  class="distribution-fill legendary" 
                  :style="{ width: (dashboardData.cookies.legendary / dashboardData.cookies.total * 100) + '%' }"
                ></div>
              </div>
              <div class="distribution-label">
                <span>傳說</span>
                <span>{{ dashboardData.cookies.legendary }}</span>
              </div>
            </div>
          </div>
        </div>

        <!-- 戰力趨勢圖 -->
        <div class="dashboard-card chart-card">
          <h3 class="dashboard-card-title">📈 戰力趨勢</h3>
          <div class="chart-container">
            <div class="chart-bars">
              <div
                v-for="(value, index) in dashboardData.powerHistory"
                :key="index"
                class="chart-bar"
                :style="{ 
                  height: (value / maxPower * 100) + '%',
                  '--index': index
                }"
              >
                <span class="bar-value">{{ (value / 1000).toFixed(0) }}k</span>
              </div>
            </div>
            <div class="chart-labels">
              <span v-for="(_, index) in dashboardData.powerHistory" :key="index">
                {{ index + 1 }}週前
              </span>
            </div>
          </div>
        </div>

        <!-- 每日進度 -->
        <div class="dashboard-card">
          <h3 class="dashboard-card-title">📅 每日進度</h3>
          <div class="progress-items">
            <div class="progress-item">
              <div class="progress-header">
                <span>每日任務</span>
                <span>{{ dashboardData.dailyProgress.tasks }} / {{ dashboardData.dailyProgress.tasksTotal }}</span>
              </div>
              <div class="progress-bar">
                <div 
                  class="progress-fill tasks" 
                  :style="{ width: tasksPercentage + '%' }"
                ></div>
              </div>
            </div>
            <div class="progress-item">
              <div class="progress-header">
                <span>連續登入</span>
                <span>{{ dashboardData.dailyProgress.loginDays }} / {{ dashboardData.dailyProgress.loginTarget }} 天</span>
              </div>
              <div class="progress-bar">
                <div 
                  class="progress-fill login" 
                  :style="{ width: loginPercentage + '%' }"
                ></div>
              </div>
            </div>
          </div>
        </div>

        <!-- 成就進度 -->
        <div class="dashboard-card">
          <h3 class="dashboard-card-title">🏆 成就進度</h3>
          <div class="achievement-progress">
            <div class="achievement-circle">
              <svg class="progress-ring" width="120" height="120">
                <circle
                  class="progress-ring-background"
                  cx="60"
                  cy="60"
                  r="50"
                ></circle>
                <circle
                  class="progress-ring-fill"
                  cx="60"
                  cy="60"
                  r="50"
                  :style="{
                    strokeDasharray: `${2 * Math.PI * 50}`,
                    strokeDashoffset: `${2 * Math.PI * 50 * (1 - achievementPercentage / 100)}`
                  }"
                ></circle>
              </svg>
              <div class="achievement-center">
                <span class="achievement-count">{{ dashboardData.achievements.completed }}</span>
                <span class="achievement-total">/ {{ dashboardData.achievements.total }}</span>
              </div>
            </div>
            <p class="achievement-desc">已完成 {{ Math.round(achievementPercentage) }}% 的成就</p>
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

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

/* 儀表板區域 */
.dashboard-section {
  margin-top: 2rem;
}

.dashboard-title {
  font-size: 1.5rem;
  color: #2c3e50;
  margin-bottom: 1.5rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 1.5rem;
}

.dashboard-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s, box-shadow 0.3s;
  overflow: visible;
}

.dashboard-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
}

.dashboard-card-title {
  font-size: 1.1rem;
  color: #2c3e50;
  margin-bottom: 1rem;
  font-weight: 600;
}

/* 戰力儀表盤 */
.gauge-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  padding-top: 1rem;
}

.gauge-wrapper {
  width: 100%;
  display: flex;
  justify-content: center;
  overflow: visible;
}

.gauge {
  width: 200px;
  height: 110px;
  position: relative;
  overflow: visible;
  padding-top: 10px;
}

.gauge-svg {
  width: 100%;
  height: 100%;
  position: absolute;
  top: -10px;
  left: 0;
  overflow: visible;
}

.gauge-background {
  transition: all 0.3s;
}

.gauge-fill {
  transition: stroke-dashoffset 0.8s ease;
}

.gauge-inner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -20%);
  text-align: center;
  z-index: 2;
}

.gauge-value {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.value-number {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2c3e50;
  line-height: 1.2;
}

.value-label {
  font-size: 0.85rem;
  color: #7f8c8d;
  margin-top: 0.2rem;
}

.gauge-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  text-align: center;
  margin: 0;
}

/* 資源列表 */
.resource-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.resource-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.resource-icon {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  flex-shrink: 0;
}

.resource-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.resource-name {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.resource-value {
  font-size: 1.1rem;
  font-weight: bold;
  color: #2c3e50;
}

.resource-progress {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  width: 100%;
}

.progress-bar {
  flex: 1;
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

.progress-fill.energy {
  background: linear-gradient(90deg, #3498db, #2980b9);
}

.progress-fill.tasks {
  background: linear-gradient(90deg, #27ae60, #229954);
}

.progress-fill.login {
  background: linear-gradient(90deg, #f39c12, #e67e22);
}

/* 餅乾分佈 */
.cookie-distribution {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

.distribution-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.distribution-bar {
  height: 24px;
  background: #ecf0f1;
  border-radius: 12px;
  overflow: hidden;
  position: relative;
}

.distribution-fill {
  height: 100%;
  border-radius: 12px;
  transition: width 0.5s ease;
}

.distribution-fill.common {
  background: linear-gradient(90deg, #95a5a6, #7f8c8d);
}

.distribution-fill.rare {
  background: linear-gradient(90deg, #3498db, #2980b9);
}

.distribution-fill.epic {
  background: linear-gradient(90deg, #9b59b6, #8e44ad);
}

.distribution-fill.legendary {
  background: linear-gradient(90deg, #f39c12, #e67e22);
}

.distribution-label {
  display: flex;
  justify-content: space-between;
  font-size: 0.85rem;
  color: #7f8c8d;
}

/* 戰力趨勢圖 */
.chart-card {
  grid-column: span 2;
}

.chart-container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.chart-bars {
  display: flex;
  align-items: flex-end;
  justify-content: space-around;
  height: 200px;
  gap: 0.5rem;
  padding: 1rem 0;
}

.chart-bar {
  flex: 1;
  background: linear-gradient(180deg, #667eea 0%, #764ba2 100%);
  border-radius: 4px 4px 0 0;
  position: relative;
  min-width: 40px;
  transition: height 0.5s ease;
  animation: growBar 0.8s ease forwards;
  animation-delay: calc(var(--index, 0) * 0.1s);
}

@keyframes growBar {
  from {
    height: 0;
  }
}

.bar-value {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.75rem;
  color: #7f8c8d;
  white-space: nowrap;
}

.chart-labels {
  display: flex;
  justify-content: space-around;
  font-size: 0.75rem;
  color: #7f8c8d;
  padding-top: 0.5rem;
}

/* 每日進度 */
.progress-items {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.progress-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.progress-header {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: #2c3e50;
}

/* 成就進度圓環 */
.achievement-progress {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.achievement-circle {
  position: relative;
  width: 120px;
  height: 120px;
}

.progress-ring {
  transform: rotate(-90deg);
}

.progress-ring-background {
  fill: none;
  stroke: #ecf0f1;
  stroke-width: 8;
}

.progress-ring-fill {
  fill: none;
  stroke: #667eea;
  stroke-width: 8;
  stroke-linecap: round;
  transition: stroke-dashoffset 0.8s ease;
}

.achievement-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.achievement-count {
  font-size: 1.8rem;
  font-weight: bold;
  color: #2c3e50;
  display: block;
}

.achievement-total {
  font-size: 0.9rem;
  color: #7f8c8d;
}

.achievement-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
  margin: 0;
  text-align: center;
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

  .card-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .dashboard-section {
    margin-top: 1.5rem;
  }

  .dashboard-title {
    font-size: 1.3rem;
  }

  .dashboard-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
  
  .chart-card {
    grid-column: span 1;
  }
  
  .chart-bars {
    height: 150px;
  }

  .stats-bar {
    flex-direction: column;
    gap: 0.8rem;
  }

  .stat-item {
    min-width: auto;
  }

  .gauge {
    width: 180px;
    height: 100px;
  }

  .gauge-wrapper {
    padding: 0.5rem 0;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .dashboard-title {
    font-size: 1.1rem;
  }

  .gauge {
    width: 160px;
    height: 90px;
  }

  .value-number {
    font-size: 1.2rem;
  }

  .value-label {
    font-size: 0.75rem;
  }

  .chart-bars {
    height: 120px;
    gap: 0.3rem;
  }

  .bar-value {
    font-size: 0.7rem;
  }

  .chart-labels {
    font-size: 0.7rem;
  }
}
</style>
