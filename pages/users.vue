<template>
  <div style="max-width:500px" class="container mx-auto">
    <h1 class="text-2xl">
      Usuários
    </h1>
    <Avatar
      v-for="(item) in avatars"
      :color="item.color"
    >
      {{ item.initial }}
    </Avatar>

    {{ avatars }}
  </div>
</template>

<script>
import Avatar from '@/components/Avatar'

export default {
  components: {
    Avatar
  },
  data () {
    return {
      avatars: [
        { initial: 'JH', color: '#bada55' },
        { initial: 'YK', color: 'red' },
        { initial: 'KT', color: '#FC0' },
        { initial: 'GT', color: '#00c4c4' },
        { initial: 'YK', color: 'teal' }
      ]
    }
  }
}
</script>

<style>
</style>
