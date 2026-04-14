<template>
  <div class="coming-soon-container" :style="{ backgroundImage: `url(${currentBackground})` }">
    <!-- 背景遮罩层，让文字更清晰 -->
    <div class="overlay"></div>
    
    <!-- 语言切换下拉框 -->
    <div class="lang-switch-container">
      <select v-model="currentLang" class="lang-select">
        <option v-for="(msg, key) in messages" :key="key" :value="key">
          {{ msg.langName }}
        </option>
      </select>
    </div>

    <div class="content">
      <h1 class="title">{{ t.title }}</h1>
      <p class="subtitle">{{ t.subtitle }}</p>
      <div class="loader"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';

// --- 多语言配置 ---
type LangType = 'zh' | 'en' | 'ja' | 'ko' | 'fr' | 'es' | 'de';
const currentLang = ref<LangType>('en');

const messages = {
  zh: {
    title: '即将上线',
    subtitle: '我们正在努力准备一些激动人心的内容。敬请期待！',
    langName: '简体中文'
  },
  en: {
    title: 'Coming Soon',
    subtitle: 'We are working hard to bring you something amazing. Stay tuned!',
    langName: 'English'
  },
  ja: {
    title: 'まもなく公開',
    subtitle: '素晴らしいコンテンツを準備中です。ご期待ください！',
    langName: '日本語'
  },
  ko: {
    title: '곧 공개됩니다',
    subtitle: '멋진 콘텐츠를 준비 중입니다. 기대해 주세요!',
    langName: '한국어'
  },
  fr: {
    title: 'Bientôt disponible',
    subtitle: 'Nous préparons quelque chose d\'incroyable. Restez à l\'écoute !',
    langName: 'Français'
  },
  es: {
    title: 'Próximamente',
    subtitle: 'Estamos preparando algo increíble. ¡Mantente al tanto!',
    langName: 'Español'
  },
  de: {
    title: 'Demnächst',
    subtitle: 'Wir bereiten etwas Großartiges vor. Bleiben Sie dran!',
    langName: 'Deutsch'
  }
};

const t = computed(() => messages[currentLang.value]);

// --- 随机背景配置 ---
const currentBackground = ref('');
const backgrounds = [
  'https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=1920&auto=format&fit=crop', // 科技/地球
  'https://images.unsplash.com/photo-1507608616759-54f48f0af0ee?q=80&w=1920&auto=format&fit=crop', // 自然/雨滴
  'https://images.unsplash.com/photo-1497366216548-37526070297c?q=80&w=1920&auto=format&fit=crop', // 极简/建筑
  'https://images.unsplash.com/photo-1478760329108-5c3ed9d495a0?q=80&w=1920&auto=format&fit=crop', // 暗色系纹理
  'https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=1920&auto=format&fit=crop', // 电路板/科技
  'https://images.unsplash.com/photo-1464802686167-b939a6910659?q=80&w=1920&auto=format&fit=crop'  // 星空
];

onMounted(() => {
  // 1. 初始化随机背景
  const randomIndex = Math.floor(Math.random() * backgrounds.length);
  currentBackground.value = backgrounds[randomIndex] || '';

  // 2. 根据浏览器语言设置默认语言
  const browserLang = navigator.language.toLowerCase();
  if (browserLang.startsWith('zh')) {
    currentLang.value = 'zh';
  } else if (browserLang.startsWith('ja')) {
    currentLang.value = 'ja';
  } else if (browserLang.startsWith('ko')) {
    currentLang.value = 'ko';
  } else if (browserLang.startsWith('fr')) {
    currentLang.value = 'fr';
  } else if (browserLang.startsWith('es')) {
    currentLang.value = 'es';
  } else if (browserLang.startsWith('de')) {
    currentLang.value = 'de';
  } else {
    currentLang.value = 'en'; // 默认回退到英文
  }
});
</script>

<style>
/* 基础重置，确保占满全屏 */
body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: 'PingFang SC', 'Helvetica Neue', Helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', Arial, sans-serif;
  background-color: #1a1a2e; /* 背景加载前的默认颜色 */
}

#app {
  width: 100%;
  height: 100vh;
}
</style>

<style scoped>
.coming-soon-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  color: #333;
  text-align: center;
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: background-image 0.5s ease-in-out;
}

/* 深色半透明遮罩，确保文字在亮色背景下也能看清 */
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  z-index: 1;
}

.lang-switch-container {
  position: absolute;
  top: 2rem;
  right: 2rem;
  z-index: 10;
}

.lang-select {
  appearance: none;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: #fff;
  padding: 8px 32px 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 0.9rem;
  outline: none;
  transition: all 0.3s ease;
  /* 自定义下拉箭头 */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23ffffff' d='M2.146 4.646a.5.5 0 0 1 .708 0L6 7.793l3.146-3.147a.5.5 0 0 1 .708.708l-3.5 3.5a.5.5 0 0 1-.708 0l-3.5-3.5a.5.5 0 0 1 0-.708z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 12px center;
}

.lang-select:hover, .lang-select:focus {
  background-color: rgba(255, 255, 255, 0.3);
}

.lang-select option {
  background-color: #2c3e50;
  color: #fff;
}

.content {
  position: relative;
  z-index: 2; /* 确保内容在遮罩层之上 */
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(15px);
  padding: 3rem 4rem;
  border-radius: 16px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  max-width: 600px;
  width: 85%;
  animation: fadeIn 1s ease-out;
}

.title {
  font-size: 2.8rem;
  margin-bottom: 1rem;
  color: #2c3e50;
  font-weight: bold;
  letter-spacing: 2px;
}

.subtitle {
  font-size: 1.2rem;
  line-height: 1.8;
  color: #4a5568;
  margin-bottom: 2.5rem;
}

.loader {
  border: 4px solid rgba(0, 0, 0, 0.05);
  border-top: 4px solid #667eea;
  border-radius: 50%;
  width: 45px;
  height: 45px;
  animation: spin 1s linear infinite;
  margin: 0 auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 480px) {
  .title {
    font-size: 2.2rem;
  }
  .content {
    padding: 2.5rem 2rem;
  }
  .lang-switch-container {
    top: 1rem;
    right: 1rem;
  }
}
</style>
