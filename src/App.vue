<template>
  <div>
    <email-result :emails="emails"/>
    <form @click.prevent>
      <input
          type="text"
          v-model="query"
      >
      <button @click="getRepos(query)">Find</button>
    </form>
  </div>
</template>

<script>
import EmailResult from "@/components/EmailResult.vue";

export default {
  components: {
    EmailResult

  },
  data() {
    return {
      title: 'hello there',
      query: "",
      repository: '',
      emails: new Set(),
    }
  },
  methods: {
    async getRepos(userName){
      const result = await fetch(`https://api.github.com/users/${userName}/repos`).then(res => res.json())
      for (let i = 0; i < result.length; i++) {
        if (result[i]?.fork === false) {
          this.repository = result[i]?.name
          this.getCommits(userName, this.repository)
        }
      }
    },
    async getCommits(userName, repo){
      const commits = await fetch(`https://api.github.com/repos/${userName}/${repo}/commits`).then(res => res.json())
      console.log(commits[0]?.commit?.author?.email)
      if (!commits[0]?.commit?.author?.email?.includes('noreply')) {
        this.emails.add(commits[0]?.commit?.author?.email)
      }
    }
  }
}

</script>

<style>

</style>