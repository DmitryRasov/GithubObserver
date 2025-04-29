<template>
  <div>
    <img 
        src="../public/mlablogo.svg" 
        alt="copy icon"
        class="logo"
      >
  </div>
  <div class="wrapper">
    <email-form
        @clear="clear"
        @addToEmails="addToEmails"
        @startLoading="startLoad"
        @stopLoading="stopLoad"
    />
    <email-result
        :emails="emails"
        :loading="loading"
    />
  </div>
</template>

<script>
import EmailResult from "@/components/EmailResult.vue";
import EmailForm from "@/components/EmailForm.vue";

export default {
  components: {
    EmailResult,
    EmailForm
  },
  data() {
    return {
      emails: new Set(),
      loading: false
    }
  },
  methods: {
    stopLoad() {
      this.loading = false
      console.log('false')
    },
    startLoad() {
      this.loading = true
      console.log('true')
    },
    clear() {
      this.emails.clear()
    },
    addToEmails(data){
      this.emails.add(data)
      this.emails.forEach(email => {
        if (email === undefined) {
          this.emails.delete(email)
        }
      })
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
}
.wrapper {
  border-radius: 15px;
  padding: 20px 20px 45px 20px;
  background: #f0f0ef;
  display: flex;
  flex-direction: column;
  margin: 0 450px;
  align-items: center;
}
.logo {
  width: 250px;
  height: 80px;
}

</style>