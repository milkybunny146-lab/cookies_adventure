<script setup>
import { ref, inject } from 'vue'

const currentPage = inject('currentPage')
const activeIndex = ref(0)

const mainMenu = ref([
  { icon: '🏰', name: '王國總覽', page: 'home' },
  { icon: '🍪', name: '我的餅乾', page: 'collection' },
  { icon: '⚔️', name: '戰鬥副本', page: 'battle' },
  { icon: '🎒', name: '背包道具', page: 'inventory' },
  { icon: '🏆', name: '成就系統', page: 'achievement' }
])

const otherMenu = ref([
  { icon: '🎁', name: '每日獎勵', page: 'daily' },
  { icon: '⚙️', name: '設定', page: 'settings' }
])

const setActive = (index, page) => {
  activeIndex.value = index
  currentPage.value = page
}
</script>

<template>
  <aside class="sidebar">
    <div class="menu-title">冒險選單</div>
    <div
      v-for="(item, index) in mainMenu"
      :key="item.name"
      class="menu-item"
      :class="{ active: activeIndex === index }"
      @click="setActive(index, item.page)"
    >
      <span class="menu-icon">{{ item.icon }}</span>
      <span>{{ item.name }}</span>
    </div>

    <div class="menu-title">其他</div>
    <div
      v-for="(item, index) in otherMenu"
      :key="item.name"
      class="menu-item"
      :class="{ active: activeIndex === mainMenu.length + index }"
      @click="setActive(mainMenu.length + index, item.page)"
    >
      <span class="menu-icon">{{ item.icon }}</span>
      <span>{{ item.name }}</span>
    </div>
  </aside>
</template>

<style scoped>
.sidebar {
  width: 250px;
  background: #2c3e50;
  color: white;
  padding: 1.5rem 0;
  flex-shrink: 0;
}

.menu-title {
  padding: 0 1.5rem 1rem;
  font-size: 0.85rem;
  text-transform: uppercase;
  color: #95a5a6;
  border-bottom: 1px solid #34495e;
  margin-bottom: 1rem;
}

.menu-title:not(:first-child) {
  margin-top: 1rem;
}

.menu-item {
  padding: 0.8rem 1.5rem;
  cursor: pointer;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 0.8rem;
  border-left: 3px solid transparent;
}

.menu-item:hover {
  background: #34495e;
  border-left-color: #667eea;
}

.menu-item.active {
  background: #34495e;
  border-left-color: #667eea;
}

.menu-icon {
  width: 20px;
  text-align: center;
}

@media (max-width: 768px) {
  .sidebar {
    width: 100%;
    padding: 1rem 0;
    max-height: 200px;
    overflow-y: auto;
  }

  .menu-title {
    padding: 0 1rem 0.8rem;
    font-size: 0.75rem;
    margin-bottom: 0.8rem;
  }

  .menu-item {
    padding: 0.6rem 1rem;
    font-size: 0.9rem;
  }

  .menu-icon {
    width: 18px;
    font-size: 1rem;
  }
}

@media (max-width: 480px) {
  .sidebar {
    max-height: 150px;
  }

  .menu-item {
    padding: 0.5rem 0.8rem;
    font-size: 0.85rem;
  }

  .menu-item span:last-child {
    font-size: 0.8rem;
  }
}
</style>
