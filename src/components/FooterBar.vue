<script setup>
import { ref } from 'vue'

const footerLinks = ref([
  { name: '遊戲公告', href: '#' },
  { name: '玩家社群', href: '#' },
  { name: '客服中心', href: '#' }
])

const currentYear = new Date().getFullYear()
</script>

<template>
  <footer class="footer">
    <div class="footer-content">
      <div class="copyright">
        &copy; {{ currentYear }} 餅乾大冒險王國. 勇敢的餅乾們，冒險永不停歇！
      </div>
      <div class="footer-links">
        <a v-for="link in footerLinks" :key="link.name" :href="link.href">
          {{ link.name }}
        </a>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: #2c3e50;
  color: #bdc3c7;
  padding: 1.5rem 2rem;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.footer-links {
  display: flex;
  gap: 1.5rem;
}

.footer-links a {
  color: #bdc3c7;
  text-decoration: none;
  transition: color 0.3s;
}

.footer-links a:hover {
  color: white;
}

@media (max-width: 768px) {
  .footer-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
}
</style>
