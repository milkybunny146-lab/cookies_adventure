<script setup>
import { ref, provide } from 'vue'
import HeaderNav from './components/HeaderNav.vue'
import SideMenu from './components/SideMenu.vue'
import MainContent from './components/MainContent.vue'
import CookieCollection from './components/CookieCollection.vue'
import BattleDungeon from './components/BattleDungeon.vue'
import Inventory from './components/Inventory.vue'
import Achievement from './components/Achievement.vue'
import AdventureStory from './components/AdventureStory.vue'
import Contact from './components/Contact.vue'
import DailyReward from './components/DailyReward.vue'
import Settings from './components/Settings.vue'
import FooterBar from './components/FooterBar.vue'

const currentPage = ref('home')

// 提供給子元件使用
provide('currentPage', currentPage)
</script>

<template>
  <div class="app-container">
    <HeaderNav />
    <div class="container">
      <SideMenu />
      <!-- 根據 currentPage 切換顯示內容 -->
      <MainContent v-if="currentPage === 'home'" />
      <CookieCollection v-else-if="currentPage === 'collection'" />
      <BattleDungeon v-else-if="currentPage === 'battle'" />
      <Inventory v-else-if="currentPage === 'inventory'" />
      <Achievement v-else-if="currentPage === 'achievement'" />
      <AdventureStory v-else-if="currentPage === 'story'" />
      <Contact v-else-if="currentPage === 'contact'" />
      <DailyReward v-else-if="currentPage === 'daily'" />
      <Settings v-else-if="currentPage === 'settings'" />
    </div>
    <FooterBar />
  </div>
</template>

<style scoped>
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  display: flex;
  flex: 1;
}

@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .app-container {
    overflow-x: hidden;
  }

  .container {
    flex-direction: column;
    min-height: auto;
  }
}
</style>
