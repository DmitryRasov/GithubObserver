<template>
  <div v-for="email in emails" :key="email">
    <h1>{{ email }}</h1>
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

<style lang="scss" scoped>

</style>