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
  </div>
</template>

<script>
import EmailResult from "@/components/EmailResult.vue";
import EmailForm from "@/components/EmailForm.vue";

export default {
  name: 'App',
  components: {
    EmailResult,
    EmailForm
  },
  data() {
    return {
      emails: new Set(),
      loading: false,
      currentQuery: '',
      lastSearchedQuery: ''
    }
  },
  computed: {
    emailList() {
      return Array.from(this.emails).filter(Boolean)
    }
  },
  methods: {
    startLoading() {
      this.loading = true
      this.lastSearchedQuery = this.currentQuery
    },
    stopLoading() {
      this.loading = false
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