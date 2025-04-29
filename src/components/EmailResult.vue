<template>
  <div class="email-wrap">
    <div v-if="emails.length > 0">
      <h3>Почты привязанные к пользователю {{ currentQuery }}</h3>
    </div>
    <div 
      v-for="email in emails" 
      :key="email"
      class="email-item"
    >
      <span>{{ email }}</span>
      <img 
        src="../../public/copy.svg" 
        alt="copy icon"
        @click="copyText(email)"
        class="copy-icon"
        :class="{ 'copy-icon--clicked': copiedEmail === email }"
      >
    </div>
    <div v-if="loading" class="loader">
      <div v-for="i in 4" :key="i"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EmailResult',
  props: {
    emails: {
      type: Array,
      required: true,
      default: () => []
    },
    loading: {
      type: Boolean,
      required: true
    },
    currentQuery: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      copiedEmail: null
    }
  },
  methods: {
    async copyText(text) {
      try {
        await navigator.clipboard.writeText(text)
        this.copiedEmail = text
        setTimeout(() => {
          this.copiedEmail = null
        }, 1000)
      } catch (err) {
        console.error('Failed to copy text:', err)
      }
    }
  }
}
</script>

<style scoped>
.loader {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}

.loader div {
  position: absolute;
  top: 33px;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  background: #ffe500;
  animation-timing-function: cubic-bezier(0, 1, 1, 0);
}

.loader div:nth-child(1) {
  left: 8px;
  animation: loader1 0.6s infinite;
}

.loader div:nth-child(2) {
  left: 8px;
  animation: loader2 0.6s infinite;
}

.loader div:nth-child(3) {
  left: 32px;
  animation: loader2 0.6s infinite;
}

.loader div:nth-child(4) {
  left: 56px;
  animation: loader3 0.6s infinite;
}

@keyframes loader1 {
  0% { transform: scale(0); }
  100% { transform: scale(1); }
}

@keyframes loader3 {
  0% { transform: scale(1); }
  100% { transform: scale(0); }
}

@keyframes loader2 {
  0% { transform: translate(0, 0); }
  100% { transform: translate(24px, 0); }
}

.copy-icon {
  filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
  cursor: pointer;
  width: 15px;
  height: 15px;
  margin-left: 15px;
  transition: all 0.3s ease;
}

.copy-icon:hover {
  transform: scale(1.1);
}

.copy-icon--clicked {
  animation: copyAnimation 0.5s ease;
}

@keyframes copyAnimation {
  0% {
    transform: scale(1);
    filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
  }
  50% {
    transform: scale(1.2);
    filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(93deg) brightness(103%) contrast(103%);
  }
  100% {
    transform: scale(1);
    filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
  }
}

.email-wrap {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.email-item {
  color: #ffe500;
  display: flex;
  align-items: center;
  border-radius: 50px;
  background: #2e2e2e;  
  padding: 10px 20px;
}
</style>