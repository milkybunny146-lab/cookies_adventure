<script setup>
import { ref } from 'vue'

const currentChapter = ref(0)

const chapters = ref([
  {
    id: 1,
    title: '第一章：餅乾王國的誕生',
    content: `很久很久以前，在一個充滿魔法的世界裡，存在著一個由餅乾組成的王國。這個王國的居民都是各種各樣的餅乾，他們擁有不同的能力和特質。

有一天，黑暗勢力突然降臨，威脅著整個餅乾王國的和平。國王召集了所有勇敢的餅乾勇者，準備展開一場史詩般的冒險。

你，作為一位新晉的餅乾勇者，被選中參與這場偉大的冒險。你的任務是收集強大的餅乾夥伴，探索未知的領域，擊敗邪惡勢力，拯救餅乾王國！`,
    image: '🏰',
    unlocked: true
  },
  {
    id: 2,
    title: '第二章：初遇夥伴',
    content: `在冒險的開始，你遇到了第一位夥伴——勇敢餅乾。他是一位充滿勇氣的戰士，總是衝在最前線保護夥伴。

勇敢餅乾告訴你，要拯救王國，需要收集更多強大的餅乾勇者。每個餅乾都有自己獨特的能力和元素屬性，只有團結一致，才能戰勝強大的敵人。

你們一起踏上了尋找其他餅乾勇者的旅程，前往各個神秘的區域，探索未知的秘密。`,
    image: '⚔️',
    unlocked: true
  },
  {
    id: 3,
    title: '第三章：黑暗森林的試煉',
    content: `經過長途跋涉，你們來到了黑暗森林。這裡充滿了危險的怪物和陷阱，但同時也隱藏著珍貴的寶藏和強大的餅乾。

在森林深處，你們遇到了暗影餅乾。他是一位神秘的刺客，擅長隱匿和暗殺。雖然一開始他對你們保持警惕，但在共同面對森林中的危險後，他決定加入你們的隊伍。

黑暗森林的試煉讓你們的隊伍更加強大，也讓你們意識到，真正的冒險才剛剛開始。`,
    image: '🌲',
    unlocked: true
  },
  {
    id: 4,
    title: '第四章：冰霜洞穴的挑戰',
    content: `為了尋找傳說中的冰系餅乾，你們來到了極寒的冰霜洞穴。這裡的溫度極低，每一步都需要小心謹慎。

在洞穴深處，你們發現了守護餅乾。他是一位堅毅的坦克，擁有強大的防禦能力。他告訴你們，邪惡勢力正在集結，準備對餅乾王國發動最後的攻擊。

你們必須加快腳步，收集更多強大的餅乾，準備迎接最終的決戰。`,
    image: '❄️',
    unlocked: true
  },
  {
    id: 5,
    title: '第五章：龍族遺跡的秘密',
    content: `根據古老的傳說，你們來到了龍族遺跡。這裡是遠古龍族的棲息地，隱藏著強大的力量和秘密。

在遺跡深處，你們遇到了傳說中的龍焰餅乾。他擁有遠古龍族的血脈，戰力無雙。他告訴你們，要擊敗最終的敵人，需要集合所有傳說級餅乾的力量。

龍族遺跡的探索讓你們獲得了強大的裝備和道具，也讓你們對即將到來的決戰充滿信心。`,
    image: '🐉',
    unlocked: true
  },
  {
    id: 6,
    title: '第六章：深淵地獄的最終決戰',
    content: `最終的時刻來臨了。你們來到了深淵地獄，這裡是邪惡勢力的最後據點。黑暗領主正在這裡等待著你們。

這是一場史詩般的戰鬥。你們集結了所有收集到的餅乾勇者，使用各種強大的技能和策略，與黑暗領主展開激烈的對決。

經過艱苦的戰鬥，你們終於擊敗了黑暗領主，拯救了餅乾王國。但這只是新的開始，還有更多的冒險等待著你們...`,
    image: '💀',
    unlocked: false
  }
])

const selectChapter = (index) => {
  if (chapters.value[index].unlocked) {
    currentChapter.value = index
  }
}

const nextChapter = () => {
  if (currentChapter.value < chapters.value.length - 1) {
    currentChapter.value++
  }
}

const prevChapter = () => {
  if (currentChapter.value > 0) {
    currentChapter.value--
  }
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">📖 冒險故事</h1>

    <!-- 章節列表 -->
    <div class="chapters-sidebar">
      <h3 class="sidebar-title">章節列表</h3>
      <div class="chapter-list">
        <div
          v-for="(chapter, index) in chapters"
          :key="chapter.id"
          class="chapter-item"
          :class="{ 
            active: currentChapter === index,
            locked: !chapter.unlocked
          }"
          @click="selectChapter(index)"
        >
          <div class="chapter-number">{{ index + 1 }}</div>
          <div class="chapter-info">
            <div class="chapter-title-small">{{ chapter.title }}</div>
            <div v-if="!chapter.unlocked" class="locked-badge">🔒 未解鎖</div>
          </div>
        </div>
      </div>
    </div>

    <!-- 故事內容 -->
    <div class="story-content">
      <div class="story-header">
        <div class="chapter-icon">{{ chapters[currentChapter].image }}</div>
        <div>
          <h2 class="story-title">{{ chapters[currentChapter].title }}</h2>
          <div class="chapter-meta">
            <span>第 {{ currentChapter + 1 }} 章 / 共 {{ chapters.length }} 章</span>
          </div>
        </div>
      </div>

      <div class="story-body">
        <div class="story-text">
          <p v-for="(paragraph, index) in chapters[currentChapter].content.split('\n\n')" :key="index">
            {{ paragraph }}
          </p>
        </div>
      </div>

      <!-- 導航按鈕 -->
      <div class="story-navigation">
        <button 
          class="nav-btn prev-btn"
          :disabled="currentChapter === 0"
          @click="prevChapter"
        >
          ← 上一章
        </button>
        <div class="chapter-indicator">
          {{ currentChapter + 1 }} / {{ chapters.length }}
        </div>
        <button 
          class="nav-btn next-btn"
          :disabled="currentChapter === chapters.length - 1 || !chapters[currentChapter + 1].unlocked"
          @click="nextChapter"
        >
          下一章 →
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
  display: flex;
  gap: 2rem;
}

.page-title {
  position: absolute;
  top: 2rem;
  left: 2rem;
  font-size: 1.8rem;
  color: #2c3e50;
  margin: 0;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #667eea;
}

/* 章節側邊欄 */
.chapters-sidebar {
  width: 300px;
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  height: fit-content;
  position: sticky;
  top: 2rem;
  flex-shrink: 0;
}

.sidebar-title {
  font-size: 1.1rem;
  color: #2c3e50;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.chapter-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.chapter-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.8rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
  border: 2px solid transparent;
}

.chapter-item:hover {
  background: #f8f9fa;
}

.chapter-item.active {
  background: linear-gradient(135deg, #667eea15 0%, #764ba215 100%);
  border-color: #667eea;
}

.chapter-item.locked {
  opacity: 0.6;
  cursor: not-allowed;
}

.chapter-number {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  flex-shrink: 0;
}

.chapter-item.locked .chapter-number {
  background: #95a5a6;
}

.chapter-info {
  flex: 1;
}

.chapter-title-small {
  font-size: 0.9rem;
  color: #2c3e50;
  font-weight: 500;
  margin-bottom: 0.2rem;
}

.locked-badge {
  font-size: 0.75rem;
  color: #7f8c8d;
}

/* 故事內容 */
.story-content {
  flex: 1;
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.story-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.chapter-icon {
  font-size: 4rem;
  width: 100px;
  height: 100px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.story-title {
  font-size: 1.8rem;
  color: #2c3e50;
  margin: 0 0 0.5rem 0;
}

.chapter-meta {
  font-size: 0.9rem;
  color: #7f8c8d;
}

.story-body {
  margin-bottom: 2rem;
}

.story-text {
  line-height: 2;
  color: #2c3e50;
}

.story-text p {
  margin-bottom: 1.5rem;
  font-size: 1.05rem;
  text-align: justify;
}

/* 導航按鈕 */
.story-navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 2rem;
  border-top: 2px solid #ecf0f1;
}

.nav-btn {
  padding: 0.8rem 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.nav-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.nav-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.chapter-indicator {
  font-size: 0.9rem;
  color: #7f8c8d;
  font-weight: 500;
}

/* RWD */
@media (max-width: 1024px) {
  .main-content {
    flex-direction: column;
  }
  
  .chapters-sidebar {
    width: 100%;
    position: static;
  }
  
  .page-title {
    position: static;
    margin-bottom: 1.5rem;
  }
}

@media (max-width: 1024px) {
  .main-content {
    flex-direction: column;
    padding: 1rem;
  }
  
  .chapters-sidebar {
    width: 100%;
    position: static;
    margin-bottom: 1.5rem;
  }
  
  .page-title {
    position: static;
    margin-bottom: 1rem;
  }
}

@media (max-width: 768px) {
  .main-content {
    padding: 1rem;
  }

  .page-title {
    font-size: 1.5rem;
  }

  .chapters-sidebar {
    padding: 1rem;
  }

  .chapter-item {
    padding: 0.6rem;
  }

  .chapter-number {
    width: 35px;
    height: 35px;
    font-size: 0.9rem;
  }

  .chapter-title-small {
    font-size: 0.85rem;
  }

  .story-content {
    padding: 1.5rem;
  }

  .story-header {
    flex-direction: column;
    text-align: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .chapter-icon {
    width: 80px;
    height: 80px;
    font-size: 3rem;
  }

  .story-title {
    font-size: 1.5rem;
  }

  .story-text {
    font-size: 1rem;
    line-height: 1.8;
  }

  .story-text p {
    margin-bottom: 1rem;
  }
  
  .story-navigation {
    flex-direction: column;
    gap: 1rem;
    padding-top: 1.5rem;
  }
  
  .nav-btn {
    width: 100%;
    padding: 0.7rem;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .chapters-sidebar {
    padding: 0.8rem;
  }

  .chapter-item {
    padding: 0.5rem;
  }

  .chapter-number {
    width: 30px;
    height: 30px;
    font-size: 0.8rem;
  }

  .story-content {
    padding: 1rem;
  }

  .chapter-icon {
    width: 60px;
    height: 60px;
    font-size: 2.5rem;
  }

  .story-title {
    font-size: 1.3rem;
  }

  .story-text {
    font-size: 0.95rem;
  }

  .nav-btn {
    padding: 0.6rem;
    font-size: 0.9rem;
  }
}
</style>

