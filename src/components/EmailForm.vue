<template>
  <form @submit.prevent="handleSearch">
    <input
      placeholder="Юзернейм с гитхаба"
      class="email-input"
      type="text"
      v-model="query"
      :disabled="loading"
      @input="updateQuery"
    >
    <button 
      class="btn" 
      :disabled="!query.trim() || loading" 
    >
      {{ loading ? 'Поиск...' : 'Найти' }}
    </button>
  </form>
  <div v-if="error" class="error-message">{{ error }}</div>
</template>

<script>
export default {
  name: "EmailForm",
  props: {
    loading: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      query: "",
      error: null,
      currentRequest: null
    }
  },
  methods: {
    updateQuery() {
      this.$emit('update-query', this.query)
    },
    async handleSearch() {
      if (!this.query.trim()) return
      
      if (this.currentRequest) {
        this.currentRequest.abort()
      }

      const currentQuery = this.query
      this.$emit('startLoading')
      
      try {
        await this.getRepos(currentQuery)
      } finally {
        this.query = ''
        this.updateQuery()
      }
    },
    async getRepos(userName) {
      this.error = null
      this.$emit('clear')
      
      try {
        const controller = new AbortController()
        this.currentRequest = controller
        
        const response = await fetch(`https://api.github.com/users/${userName}/repos`, {
          signal: controller.signal
        })
        
        if (!response.ok) throw new Error(`Ошибка запроса: ${response.status}`)
        const result = await response.json()
        if (result.length === 0) throw new Error('Репозитории не найдены')

        const nonForkRepos = result.filter(repo => !repo.fork)
        for (const repo of nonForkRepos) {
          await this.getCommits(userName, repo.name)
        }
      } catch (e) {
        if (e.name !== 'AbortError') {
          this.error = e.message
        }
      } finally {
        this.currentRequest = null
        this.$emit('stopLoading')
      }
    },
    async getCommits(userName, repo) {
      try {
        const response = await fetch(`https://api.github.com/repos/${userName}/${repo}/commits`)
        if (!response.ok) throw new Error(`Ошибка запроса коммитов: ${response.status}`)
        
        const commits = await response.json()
        const email = commits[0]?.commit?.author?.email
        
        if (email && !email.includes('noreply') && !email.includes('undefined')) {
          this.$emit('addToEmails', email)
        }
      } catch (e) {
        if (e.name !== 'AbortError') {
          this.error = e.message
        }
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