<script setup>
import { ref } from 'vue'

// 設定狀態
const settings = ref({
  // 遊戲設定
  soundEnabled: true,
  musicEnabled: true,
  soundVolume: 80,
  musicVolume: 60,
  autoPlay: false,
  notifications: true,
  
  // 顯示設定
  language: 'zh-TW',
  theme: 'light',
  fontSize: 'medium',
  animationEnabled: true,
  
  // 帳號設定
  username: '餅乾勇者',
  email: 'player@example.com',
  autoSave: true,
  cloudSync: true
})

const languages = ref([
  { value: 'zh-TW', label: '繁體中文' },
  { value: 'zh-CN', label: '簡體中文' },
  { value: 'en', label: 'English' },
  { value: 'ja', label: '日本語' }
])

const themes = ref([
  { value: 'light', label: '淺色模式' },
  { value: 'dark', label: '深色模式' },
  { value: 'auto', label: '跟隨系統' }
])

const fontSizes = ref([
  { value: 'small', label: '小' },
  { value: 'medium', label: '中' },
  { value: 'large', label: '大' }
])

const saveSettings = () => {
  // TODO: 實際保存邏輯
  alert('設定已保存！')
}

const resetSettings = () => {
  if (confirm('確定要重置所有設定嗎？此操作無法復原。')) {
    // TODO: 重置邏輯
    alert('設定已重置為預設值')
  }
}

const exportData = () => {
  alert('遊戲數據導出功能開發中...')
  // TODO: 導出邏輯
}

const importData = () => {
  alert('遊戲數據導入功能開發中...')
  // TODO: 導入邏輯
}

const deleteAccount = () => {
  if (confirm('警告：此操作將永久刪除您的帳號和所有遊戲數據，且無法復原。確定要繼續嗎？')) {
    if (confirm('請再次確認：您真的要刪除帳號嗎？')) {
      alert('帳號刪除功能開發中...')
      // TODO: 刪除邏輯
    }
  }
}
</script>

<template>
  <main class="main-content">
    <h1 class="page-title">⚙️ 設定</h1>

    <div class="settings-container">
      <!-- 遊戲設定 -->
      <div class="settings-section">
        <h2 class="section-title">🎮 遊戲設定</h2>
        
        <div class="setting-group">
          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">🔊</span>
              <div>
                <div class="setting-name">音效</div>
                <div class="setting-desc">開啟/關閉遊戲音效</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.soundEnabled" />
              <span class="toggle-slider"></span>
            </label>
          </div>

          <div class="setting-item" v-if="settings.soundEnabled">
            <div class="setting-label">
              <span class="setting-icon">📊</span>
              <div>
                <div class="setting-name">音效音量</div>
                <div class="setting-desc">{{ settings.soundVolume }}%</div>
              </div>
            </div>
            <input
              type="range"
              v-model="settings.soundVolume"
              min="0"
              max="100"
              class="volume-slider"
            />
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">🎵</span>
              <div>
                <div class="setting-name">背景音樂</div>
                <div class="setting-desc">開啟/關閉背景音樂</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.musicEnabled" />
              <span class="toggle-slider"></span>
            </label>
          </div>

          <div class="setting-item" v-if="settings.musicEnabled">
            <div class="setting-label">
              <span class="setting-icon">📊</span>
              <div>
                <div class="setting-name">音樂音量</div>
                <div class="setting-desc">{{ settings.musicVolume }}%</div>
              </div>
            </div>
            <input
              type="range"
              v-model="settings.musicVolume"
              min="0"
              max="100"
              class="volume-slider"
            />
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">▶️</span>
              <div>
                <div class="setting-name">自動戰鬥</div>
                <div class="setting-desc">自動進行戰鬥（跳過動畫）</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.autoPlay" />
              <span class="toggle-slider"></span>
            </label>
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">🔔</span>
              <div>
                <div class="setting-name">通知提醒</div>
                <div class="setting-desc">接收遊戲通知和提醒</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.notifications" />
              <span class="toggle-slider"></span>
            </label>
          </div>
        </div>
      </div>

      <!-- 顯示設定 -->
      <div class="settings-section">
        <h2 class="section-title">🎨 顯示設定</h2>
        
        <div class="setting-group">
          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">🌐</span>
              <div>
                <div class="setting-name">語言</div>
                <div class="setting-desc">選擇顯示語言</div>
              </div>
            </div>
            <select v-model="settings.language" class="setting-select">
              <option v-for="lang in languages" :key="lang.value" :value="lang.value">
                {{ lang.label }}
              </option>
            </select>
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">🎨</span>
              <div>
                <div class="setting-name">主題</div>
                <div class="setting-desc">選擇介面主題</div>
              </div>
            </div>
            <select v-model="settings.theme" class="setting-select">
              <option v-for="theme in themes" :key="theme.value" :value="theme.value">
                {{ theme.label }}
              </option>
            </select>
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">📝</span>
              <div>
                <div class="setting-name">字體大小</div>
                <div class="setting-desc">調整文字大小</div>
              </div>
            </div>
            <select v-model="settings.fontSize" class="setting-select">
              <option v-for="size in fontSizes" :key="size.value" :value="size.value">
                {{ size.label }}
              </option>
            </select>
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">✨</span>
              <div>
                <div class="setting-name">動畫效果</div>
                <div class="setting-desc">開啟/關閉介面動畫</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.animationEnabled" />
              <span class="toggle-slider"></span>
            </label>
          </div>
        </div>
      </div>

      <!-- 帳號設定 -->
      <div class="settings-section">
        <h2 class="section-title">👤 帳號設定</h2>
        
        <div class="setting-group">
          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">👤</span>
              <div>
                <div class="setting-name">使用者名稱</div>
                <div class="setting-desc">您的遊戲暱稱</div>
              </div>
            </div>
            <input
              type="text"
              v-model="settings.username"
              class="setting-input"
              placeholder="輸入使用者名稱"
            />
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">📧</span>
              <div>
                <div class="setting-name">電子郵件</div>
                <div class="setting-desc">用於帳號恢復和通知</div>
              </div>
            </div>
            <input
              type="email"
              v-model="settings.email"
              class="setting-input"
              placeholder="輸入電子郵件"
            />
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">💾</span>
              <div>
                <div class="setting-name">自動儲存</div>
                <div class="setting-desc">自動儲存遊戲進度</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.autoSave" />
              <span class="toggle-slider"></span>
            </label>
          </div>

          <div class="setting-item">
            <div class="setting-label">
              <span class="setting-icon">☁️</span>
              <div>
                <div class="setting-name">雲端同步</div>
                <div class="setting-desc">同步遊戲數據到雲端</div>
              </div>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" v-model="settings.cloudSync" />
              <span class="toggle-slider"></span>
            </label>
          </div>
        </div>
      </div>

      <!-- 數據管理 -->
      <div class="settings-section">
        <h2 class="section-title">💾 數據管理</h2>
        
        <div class="action-buttons">
          <button class="action-btn export-btn" @click="exportData">
            📤 導出數據
          </button>
          <button class="action-btn import-btn" @click="importData">
            📥 導入數據
          </button>
          <button class="action-btn reset-btn" @click="resetSettings">
            🔄 重置設定
          </button>
        </div>
      </div>

      <!-- 危險操作 -->
      <div class="settings-section danger-section">
        <h2 class="section-title danger-title">⚠️ 危險操作</h2>
        
        <div class="danger-actions">
          <button class="danger-btn" @click="deleteAccount">
            🗑️ 刪除帳號
          </button>
          <p class="danger-warning">警告：刪除帳號將永久移除所有遊戲數據，且無法復原。</p>
        </div>
      </div>

      <!-- 保存按鈕 -->
      <div class="save-section">
        <button class="save-btn" @click="saveSettings">
          💾 保存設定
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

.settings-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  max-width: 900px;
}

/* 設定區塊 */
.settings-section {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.section-title {
  font-size: 1.3rem;
  color: #2c3e50;
  margin-bottom: 1.5rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #ecf0f1;
}

.setting-group {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.setting-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  transition: background 0.3s;
}

.setting-item:hover {
  background: #ecf0f1;
}

.setting-label {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex: 1;
}

.setting-icon {
  font-size: 1.5rem;
  width: 40px;
  text-align: center;
}

.setting-name {
  font-size: 1rem;
  color: #2c3e50;
  font-weight: 500;
  margin-bottom: 0.2rem;
}

.setting-desc {
  font-size: 0.85rem;
  color: #7f8c8d;
}

/* 開關按鈕 */
.toggle-switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 26px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: 0.3s;
  border-radius: 26px;
}

.toggle-slider:before {
  position: absolute;
  content: "";
  height: 20px;
  width: 20px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: 0.3s;
  border-radius: 50%;
}

.toggle-switch input:checked + .toggle-slider {
  background-color: #667eea;
}

.toggle-switch input:checked + .toggle-slider:before {
  transform: translateX(24px);
}

/* 滑桿 */
.volume-slider {
  width: 150px;
  height: 6px;
  border-radius: 3px;
  background: #ecf0f1;
  outline: none;
  -webkit-appearance: none;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: #667eea;
  cursor: pointer;
}

.volume-slider::-moz-range-thumb {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: #667eea;
  cursor: pointer;
  border: none;
}

/* 選擇框和輸入框 */
.setting-select,
.setting-input {
  padding: 0.6rem 1rem;
  border: 2px solid #ecf0f1;
  border-radius: 8px;
  font-size: 0.95rem;
  color: #2c3e50;
  background: white;
  min-width: 200px;
  transition: border-color 0.3s;
}

.setting-select:focus,
.setting-input:focus {
  outline: none;
  border-color: #667eea;
}

.setting-input {
  width: 250px;
}

/* 操作按鈕 */
.action-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.action-btn {
  padding: 0.8rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.export-btn {
  background: linear-gradient(135deg, #3498db, #2980b9);
  color: white;
}

.import-btn {
  background: linear-gradient(135deg, #27ae60, #229954);
  color: white;
}

.reset-btn {
  background: linear-gradient(135deg, #e67e22, #d35400);
  color: white;
}

.action-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

/* 危險操作區 */
.danger-section {
  border: 2px solid #e74c3c;
  background: #fff5f5;
}

.danger-title {
  color: #e74c3c;
  border-bottom-color: #e74c3c;
}

.danger-actions {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.danger-btn {
  padding: 1rem 2rem;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.danger-btn:hover {
  background: #c0392b;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(231, 76, 60, 0.4);
}

.danger-warning {
  color: #e74c3c;
  font-size: 0.9rem;
  margin: 0;
  padding: 0.8rem;
  background: rgba(231, 76, 60, 0.1);
  border-radius: 6px;
}

/* 保存按鈕 */
.save-section {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}

.save-btn {
  padding: 1rem 3rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.save-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4);
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

  .settings-container {
    gap: 1.5rem;
  }

  .settings-section {
    padding: 1.5rem;
  }

  .section-title {
    font-size: 1.2rem;
    margin-bottom: 1.2rem;
  }

  .setting-group {
    gap: 1.2rem;
  }

  .setting-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
    padding: 0.8rem;
  }

  .setting-label {
    width: 100%;
  }

  .setting-icon {
    width: 35px;
    font-size: 1.3rem;
  }

  .setting-select,
  .setting-input {
    width: 100%;
    min-width: auto;
  }

  .volume-slider {
    width: 100%;
  }
  
  .action-buttons {
    flex-direction: column;
    gap: 0.8rem;
  }
  
  .action-btn {
    width: 100%;
    padding: 0.7rem 1.2rem;
    font-size: 0.95rem;
  }

  .danger-btn {
    padding: 0.9rem 1.5rem;
    font-size: 0.95rem;
  }

  .save-btn {
    padding: 0.9rem 2rem;
    font-size: 1rem;
    width: 100%;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 0.8rem;
  }

  .page-title {
    font-size: 1.3rem;
  }

  .settings-section {
    padding: 1rem;
  }

  .section-title {
    font-size: 1.1rem;
  }

  .setting-item {
    padding: 0.7rem;
  }

  .setting-icon {
    width: 30px;
    font-size: 1.2rem;
  }

  .setting-name {
    font-size: 0.95rem;
  }

  .setting-desc {
    font-size: 0.8rem;
  }

  .toggle-switch {
    width: 45px;
    height: 24px;
  }

  .toggle-slider:before {
    height: 18px;
    width: 18px;
  }

  .toggle-switch input:checked + .toggle-slider:before {
    transform: translateX(21px);
  }

  .action-btn {
    padding: 0.6rem 1rem;
    font-size: 0.9rem;
  }

  .danger-btn {
    padding: 0.8rem 1.2rem;
    font-size: 0.9rem;
  }

  .save-btn {
    padding: 0.8rem 1.5rem;
    font-size: 0.95rem;
  }
}
</style>

