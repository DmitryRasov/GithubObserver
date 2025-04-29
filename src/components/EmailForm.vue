<template>
  <form @submit.prevent>
    <input
        placeholder="Юзернейм с гитхаба"
        class="email-input"
        type="text"
        v-model="query"
    >
    <button class="btn" :disabled="!query.trim()" @click="getRepos(query)">Найти</button>
  </form>
  <div v-if="error" class="error-message">{{ error }}</div>
</template>

<script>
export default {
  name: "EmailForm",
  data() {
    return {
      repository: "",
      query: "",
      error: null
    }
  },
  methods: {
    async getRepos(userName){
      this.query = ''
      this.error = null
      this.$emit('clear')
      try {
        const response = await fetch(`https://api.github.com/users/${userName}/repos`)
        if (!response.ok) throw new Error(`Ошибка запроса: ${response.status}`)
        const result = await response.json()
        if (result.length === 0) throw new Error('Репозитории не найдены')

        for (let i = 0; i < result.length; i++) {
          if (result[i]?.fork === false) {
            this.repository = result[i]?.name
            await this.getCommits(userName, this.repository)
              .then(this.$emit('loading'))
          }
        }
      } catch (e) {
        this.error = e.message
      }
    },
    async getCommits(userName, repo){
      this.$emit('startLoading')
      try {
        const response = await fetch(`https://api.github.com/repos/${userName}/${repo}/commits`)
        if (!response.ok) throw new Error(`Ошибка запроса коммитов: ${response.status}`)
        const commits = await response.json()
        if (!commits[0]?.commit?.author?.email?.includes('noreply') && !commits[0]?.commit?.author?.email?.includes('undefined')) {
          this.$emit('addToEmails', commits[0]?.commit?.author?.email)
        }
      } catch (e) {
        this.error = e.message
      } finally {
        this.$emit('stopLoading')
      }
    }
  }
}
</script>

<style scoped>
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
  background-color: #2e2e2e;
  background-image: url(../../public/backgroundbtn.jpg);
  border-top-right-radius: 35px;
  border-bottom-right-radius: 35px;
  padding: 23px 48px;
  border: none;
  font-size: 32px;
  cursor: pointer;
  color: #ffe500;
  transition: .2s ease-in-out;
}
.btn:hover:enabled {
  background: #2e2e2e;
}
.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
.error-message {
  color: #ff4d4f;
  margin-top: 10px;
  font-size: 18px;
  font-family: 'Roboto', sans-serif;
}
</style>