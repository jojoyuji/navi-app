<template>
  <div
    style="max-width:500px;"
    class="container mx-auto "
  >
    <div class="flex justify-between items-center">
      <h1 class="text-2xl mx-2 my-4">
        {{ title }}
      </h1>
      <Button @clicked="newUser" class="mx-2">
        novo usuário
      </Button>
    </div>
    <div v-if="loading">
      carregando...
    </div>
    <Card
      v-for="(item, index) in users"
      :key="index"
      class="mb-2 mx-2 flex items-center justify-between"
    >
      <div
        @click="edit(item)"
        class="flex justify-start items-center"
      >
        <Avatar
          :color="item.color"
          class="m-2 mr-4"
          size="12"
        >
          {{ item.initial }}
        </Avatar>
        <div>
          <h3 class="text-base">
            {{ item.name }}
          </h3>
          <p class="text-sm" style="color: #00c4c4; text-decoration: underline;">
            {{ item.email }}
          </p>
        </div>
      </div>
      <Button
        @clicked="deleteMe(item)"
        class="mx-2"
      >
        apagar
      </Button>
    </Card>
  </div>
</template>

<script>
import Avatar from '@/components/Avatar'
import Card from '@/components/Card'
import Button from '@/components/Button'

function hashCode (str) {
  let hash = 0
  for (let i = 0; i < str.length; i++) {
    hash = str.charCodeAt(i) + ((hash << 5) - hash)
  }
  return hash
}

function intToRGB (i) {
  const c = (i & 0x00FFFFFF).toString(16).toUpperCase()
  return '00000'.substring(0, 6 - c.length) + c
}
function pickTextColorBasedOnBgColorSimple (bgColor, lightColor, darkColor) {
  const color = bgColor.charAt(0) === '#' ? bgColor.substring(1, 7) : bgColor
  const r = parseInt(color.substring(0, 2), 16) // hexToR
  const g = parseInt(color.substring(2, 4), 16) // hexToG
  const b = parseInt(color.substring(4, 6), 16) // hexToB
  return r * 0.299 + g * 0.587 + b * 0.114 > 186 ? darkColor : lightColor
}

function generateAvatar (name, max = 2) {
  const names = name.toUpperCase().split(' ')
  let rs = ''
  names.forEach(function (e) {
    rs += e[0]
  })
  const bg = '#' + intToRGB(hashCode(name))
  return {
    letters: rs.slice(0, max),
    backgroundColor: bg,
    color: pickTextColorBasedOnBgColorSimple(bg, '#fff', '#000')
  }
}

export default {
  components: {
    Avatar,
    Card,
    Button
  },
  data () {
    return {
      title: 'Usuários',
      loading: true,
      users: [ ]
    }
  },
  mounted () {
    this.loading = true
    this.$axios
      .get('http://localhost:3333/users')
      .then((response) => {
        this.loading = false
        this.users = response.data.map((i) => {
          const avatar = generateAvatar(i.name, 2)
          i.initial = avatar.letters
          i.color = avatar.backgroundColor
          return i
        })
          .sort((a, b) => {
            return (a.updatedAt > b.updatedAt) ? -1 : 1
          })
        // console.log(response.data)
      })
  },
  methods: {
    deleteMe (item) {
      this.$axios
        .delete(`http://localhost:3333/users/${item._id}`)
        .then((response) => {
          for (let i = 0; i < this.users.length; i++) {
            if (this.users[i]._id === response.data._id) {
              this.users.splice(i, 1)
            }
          }
        })
    },
    newUser () {
      // console.log(this)
      this.$router.push('/users/new')
    },
    edit (item) {
      this.$router.push(`/users/${item._id}`)
    }

  }
}
</script>

<style>
body{
  background: #ccc ;
}
</style>
