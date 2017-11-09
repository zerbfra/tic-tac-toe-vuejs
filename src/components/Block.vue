<template>
  <div class="block">
    <span v-if="figure > -1">{{ fig }}</span>
  </div>
</template>

<script>
export default {
  name: 'block',
  template: '#block',
  props: {
    figure: {
      type: Number,
      default: -1
    }
  },
  computed: {
    fig () {
      return this.figure === 0 ? 'O' : 'X'
    }
  }
}
</script>
