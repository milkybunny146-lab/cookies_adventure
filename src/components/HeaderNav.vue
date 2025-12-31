<script setup>
import { ref, inject, computed } from 'vue'

const currentPage = inject('currentPage')

const navItems = ref([
  { name: '首頁', page: 'home', icon: '🏰' },
  { name: '冒險故事', page: 'story', icon: '📖' },
  { name: '餅乾圖鑑', page: 'collection', icon: '🍪' },
  { name: '聯絡勇者', page: 'contact', icon: '💬' }
])

const navigateTo = (page) => {
  currentPage.value = page
}
</script>

<template>
  <header class="header">
    <div class="logo" @click="navigateTo('home')">🍪 餅乾大冒險</div>
    <nav>
      <ul class="nav-list">
        <li 
          v-for="item in navItems" 
          :key="item.name"
          class="nav-item"
          :class="{ active: currentPage === item.page }"
        >
          <a @click.prevent="navigateTo(item.page)" href="#">
            <span class="nav-icon">{{ item.icon }}</span>
            <span>{{ item.name }}</span>
          </a>
        </li>
      </ul>
    </nav>
  </header>
</template>

<style scoped>
.header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: opacity 0.3s;
}

.logo:hover {
  opacity: 0.9;
}

.nav-list {
  display: flex;
  list-style: none;
  gap: 0.5rem;
  margin: 0;
  padding: 0;
}

.nav-item {
  margin: 0;
}

.nav-item a {
  color: white;
  text-decoration: none;
  padding: 0.6rem 1.2rem;
  border-radius: 6px;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  position: relative;
}

.nav-item a:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

.nav-item.active a {
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

.nav-item.active a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60%;
  height: 2px;
  background: white;
  border-radius: 2px;
}

.nav-icon {
  font-size: 1.1rem;
}

@media (max-width: 768px) {
  .header {
    padding: 0.8rem 1rem;
    flex-wrap: wrap;
  }

  .logo {
    font-size: 1.2rem;
    flex: 1;
    min-width: 0;
  }

  .nav-list {
    gap: 0.3rem;
    flex-wrap: wrap;
    width: 100%;
    margin-top: 0.5rem;
  }

  .nav-item {
    flex: 1;
    min-width: calc(50% - 0.15rem);
  }

  .nav-item a {
    padding: 0.5rem 0.6rem;
    font-size: 0.85rem;
    justify-content: center;
    gap: 0.4rem;
  }

  .nav-icon {
    font-size: 1rem;
    flex-shrink: 0;
  }

  .nav-item.active a::after {
    display: none;
  }
}

@media (max-width: 480px) {
  .header {
    padding: 0.7rem 0.8rem;
    flex-direction: column;
    align-items: stretch;
  }

  .logo {
    font-size: 1.1rem;
    text-align: center;
    margin-bottom: 0.5rem;
  }

  .nav-list {
    margin-top: 0;
    gap: 0.25rem;
  }

  .nav-item {
    flex: 1;
    min-width: calc(50% - 0.125rem);
  }

  .nav-item a {
    padding: 0.5rem 0.4rem;
    font-size: 0.75rem;
    justify-content: center;
    gap: 0.3rem;
    flex-direction: column;
    align-items: center;
  }

  .nav-icon {
    font-size: 1.1rem;
  }

  .nav-item a span:last-child {
    display: block;
    font-size: 0.7rem;
    line-height: 1.2;
    text-align: center;
  }
}
</style>
