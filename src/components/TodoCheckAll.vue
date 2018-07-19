<template lang="html">
  <div>
    <label>
      <input
        @change="allChecked"
        :checked="!anyRemaining"
        type="checkbox" /> Check All
    </label>
  </div>
</template>

<script>
export default {
  name: 'todo-check-all',
  props: {
    anyRemaining: {
      type: Boolean,
      required: true,
    }
  },
  methods: {
    allChecked() {
      eventBus.$emit('checkAllChanged', this.anyRemaining)
    }
  }
}
</script>

<style lang="css">
</style>
