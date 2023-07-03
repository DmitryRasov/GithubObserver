<template>
  <div class="wrapper">
    <email-form
        @clear="clear"
        @addToEmails="addToEmails"
        @loading="load"
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
    load() {
      this.loading = false
      console.log('false')
    },
    clear() {
      this.emails.clear()
      this.loading = true
      console.log('true')
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
  margin-top: 150px;
  font-family: 'Roboto', sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: #f0f0ef;
}
.wrapper {
  display: flex;
  flex-direction: column;
  margin: 0 50px;
  align-items: center;
}
</style>