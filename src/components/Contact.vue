<script setup>
import { ref } from 'vue'

const formData = ref({
  name: '',
  email: '',
  subject: '',
  message: '',
  type: 'suggestion'
})

const contactTypes = ref([
  { value: 'suggestion', label: '建議與反饋', icon: '💡' },
  { value: 'bug', label: '問題回報', icon: '🐛' },
  { value: 'cooperation', label: '合作提案', icon: '🤝' },
  { value: 'other', label: '其他', icon: '📝' }
])

const socialLinks = ref([
  { name: '官方網站', url: '#', icon: '🌐' },
  { name: 'Facebook', url: '#', icon: '📘' },
  { name: 'Twitter', url: '#', icon: '🐦' },
  { name: 'Discord', url: '#', icon: '💬' },
  { name: 'YouTube', url: '#', icon: '📺' }
])

const faqItems = ref([
  {
    question: '如何獲得傳說級餅乾？',
    answer: '傳說級餅乾可以通過完成高難度副本、使用餅乾召喚券，或參與特殊活動獲得。建議多完成每日任務和成就來獲得召喚券。'
  },
  {
    question: '體力用完了怎麼辦？',
    answer: '體力會隨時間自動恢復，也可以使用體力藥劑來立即恢復。完成每日任務也會獲得體力獎勵。'
  },
  {
    question: '如何提升戰力？',
    answer: '提升戰力的方法包括：強化餅乾等級、裝備更好的裝備、提升餅乾品質、完成成就獲得屬性加成等。'
  },
  {
    question: '遊戲數據會保存嗎？',
    answer: '是的，所有遊戲數據都會自動保存到雲端。即使更換設備，只要使用相同的帳號登入，就能繼續你的冒險。'
  }
])

const expandedFaq = ref(null)

const toggleFaq = (index) => {
  expandedFaq.value = expandedFaq.value === index ? null : index
}

const submitForm = () => {
  if (!formData.value.name || !formData.value.email || !formData.value.message) {
    alert('請填寫所有必填欄位')
    return
  }
  
  alert('感謝您的聯絡！我們會盡快回覆您。')
  // TODO: 實際提交邏輯
  formData.value = {
    name: '',
    email: '',
    subject: '',
    message: '',
    type: 'suggestion'
  }
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">💬 聯絡勇者</h1>

    <div class="contact-container">
      <!-- 聯絡表單 -->
      <div class="contact-form-section">
        <h2 class="section-title">📧 聯絡我們</h2>
        <p class="section-desc">有任何問題、建議或想法？歡迎隨時與我們聯繫！</p>
        
        <form @submit.prevent="submitForm" class="contact-form">
          <div class="form-group">
            <label for="name">姓名 <span class="required">*</span></label>
            <input
              id="name"
              v-model="formData.name"
              type="text"
              placeholder="請輸入您的姓名"
              required
            />
          </div>

          <div class="form-group">
            <label for="email">電子郵件 <span class="required">*</span></label>
            <input
              id="email"
              v-model="formData.email"
              type="email"
              placeholder="example@email.com"
              required
            />
          </div>

          <div class="form-group">
            <label for="type">聯絡類型</label>
            <div class="type-selector">
              <button
                v-for="type in contactTypes"
                :key="type.value"
                type="button"
                class="type-btn"
                :class="{ active: formData.type === type.value }"
                @click="formData.type = type.value"
              >
                <span class="type-icon">{{ type.icon }}</span>
                <span>{{ type.label }}</span>
              </button>
            </div>
          </div>

          <div class="form-group">
            <label for="subject">主題</label>
            <input
              id="subject"
              v-model="formData.subject"
              type="text"
              placeholder="請輸入主題（選填）"
            />
          </div>

          <div class="form-group">
            <label for="message">訊息內容 <span class="required">*</span></label>
            <textarea
              id="message"
              v-model="formData.message"
              rows="6"
              placeholder="請詳細描述您的問題或建議..."
              required
            ></textarea>
          </div>

          <button type="submit" class="submit-btn">
            發送訊息
          </button>
        </form>
      </div>

      <!-- 其他資訊 -->
      <div class="contact-info-section">
        <!-- 社群連結 -->
        <div class="info-card">
          <h3 class="card-title">🌐 關注我們</h3>
          <div class="social-links">
            <a
              v-for="link in socialLinks"
              :key="link.name"
              :href="link.url"
              class="social-link"
            >
              <span class="social-icon">{{ link.icon }}</span>
              <span>{{ link.name }}</span>
            </a>
          </div>
        </div>

        <!-- 常見問題 -->
        <div class="info-card">
          <h3 class="card-title">❓ 常見問題</h3>
          <div class="faq-list">
            <div
              v-for="(item, index) in faqItems"
              :key="index"
              class="faq-item"
              :class="{ expanded: expandedFaq === index }"
            >
              <div class="faq-question" @click="toggleFaq(index)">
                <span>{{ item.question }}</span>
                <span class="faq-icon">{{ expandedFaq === index ? '−' : '+' }}</span>
              </div>
              <div v-if="expandedFaq === index" class="faq-answer">
                {{ item.answer }}
              </div>
            </div>
          </div>
        </div>

        <!-- 聯絡資訊 -->
        <div class="info-card">
          <h3 class="card-title">📞 聯絡資訊</h3>
          <div class="contact-info">
            <div class="info-item">
              <span class="info-icon">📧</span>
              <div>
                <div class="info-label">電子郵件</div>
                <div class="info-value">support@cookie-adventure.com</div>
              </div>
            </div>
            <div class="info-item">
              <span class="info-icon">⏰</span>
              <div>
                <div class="info-label">服務時間</div>
                <div class="info-value">週一至週五 09:00 - 18:00</div>
              </div>
            </div>
            <div class="info-item">
              <span class="info-icon">⚡</span>
              <div>
                <div class="info-label">回覆時間</div>
                <div class="info-value">通常在 24-48 小時內回覆</div>
              </div>
            </div>
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

.contact-container {
  display: grid;
  grid-template-columns: 1.5fr 1fr;
  gap: 2rem;
}

/* 聯絡表單 */
.contact-form-section {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.section-title {
  font-size: 1.5rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.section-desc {
  color: #7f8c8d;
  margin-bottom: 2rem;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.9rem;
  color: #2c3e50;
  font-weight: 500;
}

.required {
  color: #e74c3c;
}

.form-group input,
.form-group textarea {
  padding: 0.8rem;
  border: 2px solid #ecf0f1;
  border-radius: 8px;
  font-size: 1rem;
  font-family: inherit;
  transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #667eea;
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
}

.type-selector {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.5rem;
}

.type-btn {
  padding: 0.8rem;
  border: 2px solid #ecf0f1;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-size: 0.9rem;
}

.type-btn:hover {
  border-color: #667eea;
  background: #f8f9fa;
}

.type-btn.active {
  border-color: #667eea;
  background: linear-gradient(135deg, #667eea15 0%, #764ba215 100%);
  color: #667eea;
}

.type-icon {
  font-size: 1.2rem;
}

.submit-btn {
  padding: 1rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
  margin-top: 0.5rem;
}

.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

/* 資訊區 */
.contact-info-section {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.info-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.card-title {
  font-size: 1.2rem;
  color: #2c3e50;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

/* 社群連結 */
.social-links {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

.social-link {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.8rem;
  border-radius: 8px;
  text-decoration: none;
  color: #2c3e50;
  transition: all 0.3s;
  border: 2px solid transparent;
}

.social-link:hover {
  background: #f8f9fa;
  border-color: #667eea;
  transform: translateX(4px);
}

.social-icon {
  font-size: 1.5rem;
  width: 30px;
  text-align: center;
}

/* 常見問題 */
.faq-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.faq-item {
  border: 2px solid #ecf0f1;
  border-radius: 8px;
  overflow: hidden;
  transition: all 0.3s;
}

.faq-item.expanded {
  border-color: #667eea;
}

.faq-question {
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  background: #f8f9fa;
  font-weight: 500;
  color: #2c3e50;
  transition: background 0.3s;
}

.faq-question:hover {
  background: #ecf0f1;
}

.faq-icon {
  font-size: 1.5rem;
  color: #667eea;
  font-weight: bold;
}

.faq-answer {
  padding: 1rem;
  color: #7f8c8d;
  line-height: 1.6;
  background: white;
}

/* 聯絡資訊 */
.contact-info {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.info-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
}

.info-icon {
  font-size: 1.5rem;
  width: 30px;
  text-align: center;
}

.info-label {
  font-size: 0.85rem;
  color: #7f8c8d;
  margin-bottom: 0.2rem;
}

.info-value {
  font-size: 0.95rem;
  color: #2c3e50;
  font-weight: 500;
}

/* RWD */
@media (max-width: 1024px) {
  .contact-container {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  
  .type-selector {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .main-content {
    padding: 1rem;
  }

  .page-title {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  .contact-form-section {
    padding: 1.5rem;
  }

  .section-title {
    font-size: 1.3rem;
  }

  .section-desc {
    font-size: 0.9rem;
    margin-bottom: 1.5rem;
  }

  .contact-form {
    gap: 1.2rem;
  }

  .form-group input,
  .form-group textarea {
    padding: 0.7rem;
    font-size: 0.95rem;
  }

  .setting-input {
    width: 100%;
  }

  .info-card {
    padding: 1.2rem;
  }

  .card-title {
    font-size: 1.1rem;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .contact-form-section {
    padding: 1rem;
  }

  .section-title {
    font-size: 1.2rem;
  }

  .form-group input,
  .form-group textarea {
    padding: 0.6rem;
    font-size: 0.9rem;
  }

  .type-btn {
    padding: 0.7rem;
    font-size: 0.85rem;
  }

  .submit-btn {
    padding: 0.9rem;
    font-size: 0.95rem;
  }

  .info-card {
    padding: 1rem;
  }

  .social-link {
    padding: 0.7rem;
    font-size: 0.9rem;
  }

  .faq-question {
    padding: 0.8rem;
    font-size: 0.9rem;
  }

  .faq-answer {
    padding: 0.8rem;
    font-size: 0.85rem;
  }
}
</style>

