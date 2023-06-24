<template>
  <div class="wrapper">
    <a v-for="email in emails" :key="email">
      {{ email }}
    </a>
  </div>
</template>

<script>
export default {
  name: 'EmailResult',
  props: {
    emails: {
      required: true
    }
  }
}
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
}
</style>