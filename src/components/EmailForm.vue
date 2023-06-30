<template>
  <form @click.prevent>
    <input
        class="email-input"
        type="text"
        v-model="query"
    >
    <button class="btn" @click="getRepos(query)">Find</button>
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
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

form {
  margin-bottom: 20px;
  font-family: 'Roboto', sans-serif;
}
.email-input {
  border-top-left-radius: 35px;
  border-bottom-left-radius: 35px;
  padding: 23px 48px;
  border: none;
  outline: none;
  font-size: 32px;
}
.btn {
  font-family: inherit;
  background-color: rgb(37, 37, 37);
  border-top-right-radius: 35px;
  border-bottom-right-radius: 35px;
  padding: 23px 48px;
  border: none;
  font-size: 32px;
  cursor: pointer;
  color: #fff;
}
</style>