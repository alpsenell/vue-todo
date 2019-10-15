<template>
    <div>
      <div class="check-all-container">
        <input type="checkbox" :checked="!anyRemaining" @change="allChecked">Check All
      </div>
    </div>
</template>

<script>
    export default {
      name: 'todo-check-all',
      props: {
          anyRemaining: {
            type: Boolean,
            required: true
          }
      },
      methods: {
        allChecked() {
          eventBus.$emit('checkAllChanged', this.anyRemaining)
        }
      }
    }
</script>

<style scoped>

</style>
