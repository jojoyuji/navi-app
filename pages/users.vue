<template>
  <div class="name">
    users!
    <span v-if="loading"> carregando...</span>
    <ul v-else>
      <li v-for="(i, index) in users" :key="index">
        {{ i }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'Users',
  data () {
    return {
      loading: true,
      users: []
    }
  },
  mounted () {
    this.loading = true
    this.$axios.get('http://localhost:8080/users')
      .then((e) => {
        console.log(e)
        this.loading = false
        this.users = e.data
      })
  }

}
</script>
