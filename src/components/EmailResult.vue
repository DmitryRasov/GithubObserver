<template>
  <div class="email-wrap">
    <div 
      class="email-item"
      v-for="email in emails" 
      :key="email"
    >
      <a>{{ email }}</a>
      <img 
        src="../../public/copy.svg" 
        alt="copy icon"
        @click="copyText(email)"
        class="copy-icon"
      >
    </div>
    <div v-if="loading === true" class="loader"><div></div><div></div><div></div><div></div></div>
  </div>
</template>

<script>

export default {
  name: 'EmailResult',
  props: {
    emails: {
      required: true
    },
    loading: {
      required: true
    }
  },
  methods: {
    async copyText(text) {
    try {
      await navigator.clipboard.writeText(text);
      alert('Copied');
    } catch($e) {
      alert('Cannot copy');
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
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes loader3 {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
@keyframes loader2 {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(24px, 0);
  }
}

.copy-icon {
  filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
  cursor: pointer;
  width: 15px;
  height: 15px;
  margin-left: 15px;
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