<template>
  <div class="wrapper">
    <email-form
        @clear="clear"
        @addToEmails="addToEmails"
    />
    <email-result
        :emails="emails"
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
    }
  },
  methods: {
    clear() {
      this.emails.clear()
    },
    addToEmails(data){
      this.emails.add(data)
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
  background-color: lightslategrey;
}
.wrapper {

  display: flex;
  flex-direction: column;
  border: 1px solid red;
  margin: 0 50px;
  align-items: center;
}
</style>