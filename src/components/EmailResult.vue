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
      >
    </div>
    <div v-if="loading === true" class="loader"><div></div><div></div><div></div></div>
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
  display: inline-block;
  position: absolute;
  left: 8px;
  width: 16px;
  background: #8fc9fb;
  animation: loader 1.2s cubic-bezier(0, 0.5, 0.5, 1) infinite;
}
.loader div:nth-child(1) {
  left: 8px;
  animation-delay: -0.24s;
}
.loader div:nth-child(2) {
  left: 32px;
  animation-delay: -0.12s;
}
.loader div:nth-child(3) {
  left: 56px;
  animation-delay:  0s;
}
@keyframes loader {
  0% {
    top: 8px;
    height: 64px;
  }
  50%, 100% {
    top: 24px;
    height: 32px;
  }
}

img { 
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
  display: flex;
  align-items: center;
  border-radius: 50px;
  background: #8fc9fb;  
  padding: 10px 20px;
}
</style>