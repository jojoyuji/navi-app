<template>
  <div
    style="max-width:500px;"
    class="container mx-auto "
  >
    <div class="flex flex-col">
      <label class="mb-2">
        <div>Nome:</div>
        <input
          v-model="name"
          class="rounded h-8 px-2 w-full"
          name="name"
          type="text"
          placeholder="Seu nome aqui..."
        >
      </label>
      <label class="mb-2">
        <div>Email: </div>
        <input
          v-model="email"
          class="rounded h-8 px-2 w-full"
          name="email"
          type="email"
          placeholder="meu_email@email.com"
        >
      </label>
      <Button @clicked="save" class="mt-8">
        Salvar
      </Button>
    </div>
  </div>
</template>

<script>
import Button from '@/components/Button'
export default {
  components: {
    Button
  },
  data () {
    return {
      name: '',
      email: ''
    }
  },
  mounted () {
    this.$axios
      .get('http://localhost:3333/users/' + this.$route.params.id)
      .then((result) => {
        this.name = result.data.name
        this.email = result.data.email
      })
  },
  methods: {
    post () {
      const toCreate = {
        name: this.name,
        email: this.email
      }
      this.$axios.post('http://localhost:3333/users', toCreate)
        .then((result) => {
          this.$router.push('/users')
        })
    },
    put () {
      const toCreate = {
        name: this.name,
        email: this.email
      }
      this.$axios.put(`http://localhost:3333/users/${this.$route.params.id}`, toCreate)
        .then((result) => {
          this.$router.push('/users')
        })
    },
    save () {
      if (this.$route.params.id === 'new') {
        this.post()
      } else {
        this.put()
      }
    }

  }
}
</script>

<style>
body{
  background: #ccc ;
}
</style>
