<template>
    <email-result
        :emails="emails"
    />
    <email-form
        @clear="clear"
        @addToEmails="addToEmails"
    />
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

</style>