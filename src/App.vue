<template>
  <div class="app">
    <img 
      src="../public/mlablogo.svg" 
      alt="MLab logo"
      class="logo"
    >
    <div class="wrapper">
      <email-form
        @clear="clearEmails"
        @addToEmails="addToEmails"
        @startLoading="startLoading"
        @stopLoading="stopLoading"
        @update-query="updateQuery"
        :loading="loading"
      />
      <email-result
        :emails="emailList"
        :loading="loading"
        :current-query="lastSearchedQuery"
      />
    </div>
    <search-history
      :history="searchHistory"
      @select-history="loadHistoryItem"
      @delete-history="deleteHistoryItem"
    />
  </div>
</template>

<script>
import EmailResult from "@/components/EmailResult.vue";
import EmailForm from "@/components/EmailForm.vue";
import SearchHistory from "@/components/SearchHistory.vue";

const STORAGE_KEY = 'github-email-finder-history';
const MAX_HISTORY_ITEMS = 10;

export default {
  name: 'App',
  components: {
    EmailResult,
    EmailForm,
    SearchHistory
  },
  data() {
    return {
      emails: new Set(),
      loading: false,
      currentQuery: '',
      lastSearchedQuery: '',
      searchHistory: []
    }
  },
  computed: {
    emailList() {
      return Array.from(this.emails).filter(Boolean)
    }
  },
  created() {
    this.loadHistory()
  },
  methods: {
    startLoading() {
      this.loading = true
      this.lastSearchedQuery = this.currentQuery
    },
    stopLoading() {
      this.loading = false
      if (this.emailList.length > 0) {
        this.saveToHistory()
      }
    },
    clearEmails() {
      this.emails.clear()
    },
    updateQuery(query) {
      this.currentQuery = query
    },
    addToEmails(email) {
      if (email) {
        this.emails.add(email)
      }
    },
    saveToHistory() {
      const historyItem = {
        query: this.lastSearchedQuery,
        emails: this.emailList,
        timestamp: Date.now()
      }

      this.searchHistory.unshift(historyItem)
      
      if (this.searchHistory.length > MAX_HISTORY_ITEMS) {
        this.searchHistory.pop()
      }

      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.searchHistory))
    },
    loadHistory() {
      const savedHistory = localStorage.getItem(STORAGE_KEY)
      if (savedHistory) {
        this.searchHistory = JSON.parse(savedHistory)
      }
    },
    loadHistoryItem(item) {
      this.emails.clear()
      item.emails.forEach(email => this.emails.add(email))
      this.lastSearchedQuery = item.query
    },
    deleteHistoryItem(index) {
      this.searchHistory.splice(index, 1)
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.searchHistory))
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

body {
  font-family: 'Roboto', sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: #2e2e2e;
  margin: 0;
  min-height: 100vh;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  position: relative;
}

.wrapper {
  border-radius: 15px;
  padding: 20px 20px 45px 20px;
  background: #f0f0ef;
  display: flex;
  flex-direction: column;
  margin: 20px auto;
  max-width: 800px;
  width: 100%;
  align-items: center;
}

.logo {
  width: 250px;
  height: 80px;
  margin-bottom: 20px;
}

@media (max-width: 768px) {
  .wrapper {
    margin: 20px;
    padding: 15px;
  }
}
</style>