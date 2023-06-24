<template>
  <form @click.prevent>
    <input
        type="text"
        v-model="query"
    >
    <button @click="getRepos(query)">Find</button>
  </form>
</template>

<script>
export default {
  name: "EmailForm",
  props: {

  },
  data() {
    return {
      repository: "",
      query: "",
    }
  },
  methods: {
    async getRepos(userName){
      this.query = ''
      this.$emit('clear')
      const result = await fetch(`https://api.github.com/users/${userName}/repos`).then(res => res.json())
      for (let i = 0; i < result.length; i++) {
        if (result[i]?.fork === false) {
          this.repository = result[i]?.name
          await this.getCommits(userName, this.repository)
        }
      }
    },
    async getCommits(userName, repo){
      const commits = await fetch(`https://api.github.com/repos/${userName}/${repo}/commits`).then(res => res.json())
      if (!commits[0]?.commit?.author?.email?.includes('noreply')) {
        this.$emit('addToEmails', commits[0]?.commit?.author?.email)
      }
    }
  }
}
</script>

<style scoped>

</style>