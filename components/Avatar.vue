<template>
  <div
    :style="{background: color}"
    :class="`h-${size} w-${size}`"
    class="avatar
      rounded-full
      flex items-center justify-center"
  >
    <div>
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'red'
    },
    size: {
      type: String,
      default: '16'
    }
  },
  data () {
    return {
    }
  }
}
</script>

<style>
</style>
