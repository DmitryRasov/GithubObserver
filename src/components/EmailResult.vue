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
  </div>
</template>

<script>
export default {
  name: 'EmailResult',
  props: {
    emails: {
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