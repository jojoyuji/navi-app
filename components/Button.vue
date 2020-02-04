<template>
  <button
    @click="emitClick"
    class="rounded bg-black p-2 text-white"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: 'Button',
  methods: {
    emitClick () {
      this.$emit('clicked')
    }
  }
}
</script>

<style>
</style>
